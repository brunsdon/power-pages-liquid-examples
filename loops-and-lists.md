# Loops and Lists

Looping is useful for navigation, repeated content, and collections. Below are practical patterns plus a few Dataverse-backed examples you can copy into Web Templates or Web Pages.

## Basic Liquid loops

Loop through a collection (example: web links):

```liquid
{% for item in weblinks.primary %}
  <a href="{{ item.url }}">{{ item.name }}</a>
{% endfor %}
```

Loop with an index and simple markup:

```liquid
{% for item in weblinks.primary %}
  <div class="nav-item">
    <span class="nav-index">{{ forloop.index }}.</span>
    <a href="{{ item.url }}">{{ item.name }}</a>
  </div>
{% endfor %}
```

Check first / last in a loop:

```liquid
{% for item in weblinks.primary %}
  {% if forloop.first %}
    <p>First link:</p>
  {% endif %}

  <a href="{{ item.url }}">{{ item.name }}</a>

  {% if forloop.last %}
    <p>End of list</p>
  {% endif %}
{% endfor %}
```

## Dataverse: FetchXML + render patterns

Power Pages enables server-side FetchXML queries inside Liquid (commonly via the `{% fetchxml %}` tag in Web Templates). The examples below show a typical pattern: run FetchXML then loop the results.

Example — recent active Accounts (top 10) and rendering a simple list:

```liquid
{% fetchxml accounts %}
<fetch top="10">
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="accountnumber" />
    <attribute name="createdon" />
    <filter>
      <condition attribute="statecode" operator="eq" value="0" />
    </filter>
    <order attribute="createdon" descending="true" />
  </entity>
</fetch>
{% endfetchxml %}

<ul class="accounts-list">
{% for a in accounts.entities %}
  <li class="account-item">
    <h3><a href="/account/{{ a.accountid }}">{{ a.name }}</a></h3>
    <p>Account number: {{ a.accountnumber | default: "—" }}</p>
    <small>Created: {{ a.createdon | date: "%B %d, %Y" }}</small>
  </li>
{% endfor %}
</ul>
```

Notes:
- Replace the attribute names and entity name to match your Dataverse schema.
- The `accounts` variable above is created by the `fetchxml` tag; the returned structure typically exposes an `entities` collection to loop over.

## Dataverse: render related records (contacts for an account)

Fetch contacts related to a single account and render them on an account page. This pattern is useful inside a Web Template that knows the current account id (for example, from page context or URL).

```liquid
{% fetchxml contacts %}
<fetch>
  <entity name="contact">
    <attribute name="contactid" />
    <attribute name="fullname" />
    <attribute name="emailaddress1" />
    <filter>
      <condition attribute="_parentcustomerid_value" operator="eq" value="{{ request.params.accountid }}" />
    </filter>
    <order attribute="fullname" />
  </entity>
</fetch>
{% endfetchxml %}

{% if contacts.entities.size > 0 %}
  <ul class="contacts-list">
  {% for c in contacts.entities %}
    <li>
      <strong>{{ c.fullname }}</strong>
      {% if c.emailaddress1 %}
        <a href="mailto:{{ c.emailaddress1 }}">{{ c.emailaddress1 }}</a>
      {% endif %}
    </li>
  {% endfor %}
  </ul>
{% else %}
  <p>No related contacts found.</p>
{% endif %}
```

## Practical tips for lists

- Keep markup semantic and accessible (use lists for lists).
- Avoid heavy business logic in Liquid — pre-filter in FetchXML where possible.
- Use `default`, `escape`, and `date` filters to keep output safe and consistent.
- Test empty states and paged results (use FetchXML paging for large datasets).

## FetchXML paging (paged results)

When listing large datasets you'll want to page results instead of returning everything at once. Use FetchXML `count` and `page` attributes and pass the page number via query string (or via a template param). Below is a simple pattern showing page navigation.

```liquid
{%- assign current_page = request.params.page | default: 1 | plus: 0 -%}
{%- assign page_size = 10 -%}

{% fetchxml records %}
<fetch mapping="logical" count="{{ page_size }}" page="{{ current_page }}">
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <order attribute="name" />
  </entity>
</fetch>
{% endfetchxml %}

<ul class="paged-list">
  {% for r in records.entities %}
    <li>{{ r.name }}</li>
  {% endfor %}
</ul>

{%- comment -%} simple page links {%- endcomment -%}
<nav class="pager">
  {% if current_page > 1 %}
    <a href="?page={{ current_page | minus: 1 }}">Previous</a>
  {% endif %}

  <span>Page {{ current_page }}</span>

  {% if records.entities.size == page_size %}
    <a href="?page={{ current_page | plus: 1 }}">Next</a>
  {% endif %}
</nav>

Notes:
- This pattern relies on the portal's FetchXML support for `count` and `page` attributes. For stricter control and performance, use server-side views or a custom Web API proxy.
- For true cursor-style paging you can implement FetchXML paging cookies (more advanced) to maintain consistent result sets under concurrent changes.

