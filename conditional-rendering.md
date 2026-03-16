# Conditional Rendering

Conditional rendering is the core of most Power Pages Liquid templates. Use it to branch on authentication state, optional fields, feature flags, and query results.

## Decision flow

```mermaid
flowchart TD
  A[Start render] --> B{Is user signed in?}
  B -->|No| C[Show public content]
  B -->|Yes| D{Has required field or flag?}
  D -->|No| E[Show fallback message]
  D -->|Yes| F[Show personalized block]
```

## Basic if / else

```liquid
{% if user %}
  <p>Signed in</p>
{% else %}
  <p>Not signed in</p>
{% endif %}
```

## Check for blank values

```liquid
{% if page.title != blank %}
  <h1>{{ page.title | escape }}</h1>
{% else %}
  <h1>Untitled page</h1>
{% endif %}
```

## Multi-branch rendering with elsif

```liquid
{% assign case_status = request.params.status | default: "open" %}

{% if case_status == "open" %}
  <span class="badge badge-open">Open</span>
{% elsif case_status == "pending" %}
  <span class="badge badge-pending">Pending response</span>
{% else %}
  <span class="badge badge-closed">Closed</span>
{% endif %}
```

## Combine conditions safely

```liquid
{% if user and user.fullname != blank %}
  <p>Hello {{ user.fullname | escape }}</p>
{% endif %}
```

## Guard optional data before rendering a section

```liquid
{% if account and account.telephone1 != blank %}
  <section class="contact-summary">
    <h2>Phone</h2>
    <p>{{ account.telephone1 | escape }}</p>
  </section>
{% endif %}
```

## Personalization with a Dataverse-backed flag

```liquid
{% if user and contact and contact.new_is_beta_user == true %}
  <div class="banner banner-beta">
    Early access enabled for {{ user.fullname | escape }}
  </div>
{% endif %}
```

## Permission-aware empty state

This pattern keeps the page useful even when the user cannot see the underlying data.

```liquid
{% if cases.entities.size > 0 %}
  <ul>
    {% for case in cases.entities %}
      <li>{{ case.title | default: "Untitled case" | escape }}</li>
    {% endfor %}
  </ul>
{% elsif user %}
  <p>No visible cases were found for your account.</p>
{% else %}
  <p>Sign in to view your cases.</p>
{% endif %}
```

## Practical rules

- Prefer explicit fallbacks over silently rendering nothing.
- Keep conditional branches close to the markup they control.
- Avoid nesting many levels of if blocks; extract repeated logic into a helper snippet when needed.
- Treat empty query results and permission-trimmed results as separate user experiences.