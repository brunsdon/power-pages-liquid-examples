# Paging demo: FetchXML + paging-cookie (full example)

This walkthrough shows a practical pattern to implement stable cursor-style paging (paging-cookie) in a Power Pages Web Template using FetchXML and a small client-side helper to navigate previous/next pages.

Notes on assumptions:
- Portal returns a paging cookie in the FetchXML result object (property name may vary by portal version; adapt if your portal uses a different key).
- The demo uses `results.paging_cookie` as the returned cookie — if your portal uses `results.pagingCookie` or another key, update accordingly.

## Server-side (Liquid) template

Copy this block into a Web Template. It reads an incoming `pagingcookie` query param, runs the FetchXML with that cookie, and renders results. It also outputs the next cookie (if present) so the client can capture it.

```liquid
{%- assign page_size = 20 -%}
{%- assign incoming_cookie = request.params.pagingcookie | default: nil -%}

{% fetchxml results %}
<fetch mapping="logical" count="{{ page_size }}" paging-cookie="{{ incoming_cookie }}">
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <order attribute="name" />
  </entity>
</fetch>
{% endfetchxml %}

<div id="results">
  <ul>
    {% for e in results.entities %}
      <li>{{ e.name }}</li>
    {% endfor %}
  </ul>
</div>

{%- comment -%}
Expose the server-provided next paging cookie in a data attribute so JS can read and push it to a client-side stack.
If the result doesn't expose a cookie, adapt to the portal's response shape.
{%- endcomment -%}

<div id="pager" data-next-cookie="{{ results.paging_cookie | escape }}">
  <button id="prevBtn" type="button" disabled>Previous</button>
  <button id="nextBtn" type="button">Next</button>
</div>

<script>
  (function(){
    // client-side helper to manage paging-cookie stack for prev/next navigation
    var PAGE_KEY = 'pp_paging_stack_v1';

    function readStack(){
      try{ return JSON.parse(sessionStorage.getItem(PAGE_KEY) || '[]'); } catch(e){ return []; }
    }
    function writeStack(s){ sessionStorage.setItem(PAGE_KEY, JSON.stringify(s)); }

    var pager = document.getElementById('pager');
    var nextCookie = pager.getAttribute('data-next-cookie');
    var prevBtn = document.getElementById('prevBtn');
    var nextBtn = document.getElementById('nextBtn');

    // current incoming cookie from the URL (if any) - used to go back
    var urlParams = new URLSearchParams(window.location.search);
    var incoming = urlParams.get('pagingcookie');

    // initialize stack: if navigating forward, push current incoming cookie (which is the cursor for current page)
    var stack = readStack();
    if (incoming) {
      // if the top of stack isn't the incoming cookie, push it (so prev will land here)
      if (stack.length === 0 || stack[stack.length - 1] !== incoming) {
        stack.push(incoming);
        writeStack(stack);
      }
    }

    // Prev: pop current cookie (we're on page that has a previous cookie in stack)
    prevBtn.addEventListener('click', function(){
      var s = readStack();
      // pop current page cookie
      s.pop();
      var prev = s.length > 0 ? s[s.length - 1] : null;
      writeStack(s);
      navigateWithCookie(prev);
    });

    // Next: navigate using the server-provided next cookie
    nextBtn.addEventListener('click', function(){
      // push next cookie onto stack for previous navigation
      if (nextCookie && nextCookie.length > 0) {
        var s = readStack();
        s.push(nextCookie);
        writeStack(s);
        navigateWithCookie(nextCookie);
      }
    });

    function navigateWithCookie(cookie){
      var url = new URL(window.location.href);
      if (cookie) url.searchParams.set('pagingcookie', cookie);
      else url.searchParams.delete('pagingcookie');
      window.location.href = url.toString();
    }

    // enable/disable prev button depending on stack
    prevBtn.disabled = readStack().length <= 1; // <=1 means no meaningful previous page
    if (!nextCookie || nextCookie.length === 0) nextBtn.disabled = true;
  })();
</script>
```

## How it works (brief)

- The server runs FetchXML with the incoming paging cookie (if any). Portal returns results and includes a new paging cookie for the *next* page.
- The template renders results and emits the next cookie in a data attribute.
- The client stores visited cookies in sessionStorage as a stack. When the user navigates Next, the client pushes the next cookie. When the user clicks Previous, the client pops the stack and navigates to the previous cookie.

## Caveats and tips

- The exact property name where the portal exposes the paging cookie may differ by portal version; inspect the `results` object in a dev portal to confirm.
- Session storage is scoped per-tab; if you need cross-tab consistency, use a server-side approach to persist cookie stacks.
- Cursor-style paging is robust under concurrent changes but assumes stable ordering (include a deterministic order clause in FetchXML).
