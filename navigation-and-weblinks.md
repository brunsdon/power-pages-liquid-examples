# Navigation and Web Links

Navigation in Power Pages is usually a combination of a web link set, the current request path, and sometimes user state. Keep the markup semantic and the active-state logic obvious.

## Navigation rendering flow

```mermaid
flowchart LR
  A[weblinks.primary] --> B[Loop each item]
  B --> C{item.url equals request.path?}
  C -->|Yes| D[Set active class and aria-current]
  C -->|No| E[Render normal link]
```

## Simple navigation list

```liquid
<ul>
  {% for item in weblinks.primary %}
    <li><a href="{{ item.url }}">{{ item.name | escape }}</a></li>
  {% endfor %}
</ul>
```

## Active class pattern

```liquid
{% for item in weblinks.primary %}
  <a href="{{ item.url }}"
     class="{% if item.url == request.path %}active{% endif %}">
    {{ item.name | escape }}
  </a>
{% endfor %}
```

## Accessible primary navigation

```liquid
<nav aria-label="Primary navigation">
  <ul>
    {% for item in weblinks.primary %}
      {% assign is_current = item.url == request.path %}
      <li>
        <a href="{{ item.url }}"
           class="{% if is_current %}active{% endif %}"
           {% if is_current %}aria-current="page"{% endif %}>
          {{ item.name | escape }}
        </a>
      </li>
    {% endfor %}
  </ul>
</nav>
```

## Auth-aware navigation branch

```liquid
<nav aria-label="Account navigation">
  <ul>
    <li><a href="/">Home</a></li>
    {% if user %}
      <li><a href="/profile">My Profile</a></li>
      <li><a href="/cases">My Cases</a></li>
    {% else %}
      <li><a href="/sign-in">Sign In</a></li>
    {% endif %}
  </ul>
</nav>
```

## Role-aware navigation helper

When navigation rules depend on more than signed-in state, compute a small capability flag first and keep the markup branch shallow.

```liquid
{% assign can_view_premium = user and user.webroles contains "PremiumUser" %}
{% assign can_view_support = user and user.webroles contains "SupportContact" %}

<nav aria-label="Portal sections">
  <ul>
    <li><a href="/">Home</a></li>
    {% if user %}
      <li><a href="/profile">My Profile</a></li>
      <li><a href="/cases">My Cases</a></li>
    {% endif %}
    {% if can_view_support %}
      <li><a href="/support">Support workspace</a></li>
    {% endif %}
    {% if can_view_premium %}
      <li><a href="/premium">Premium dashboard</a></li>
    {% endif %}
  </ul>
</nav>
```

## Hide restricted links instead of teasing them

For portal navigation, a hidden link is usually better than rendering a disabled item that the user can never access.

```liquid
{% if user and user.webroles contains "InvoiceReader" %}
  <a href="/invoices">Invoices</a>
{% endif %}
```

## New-tab external link pattern

```liquid
<a href="https://learn.microsoft.com/"
   target="_blank"
   rel="noopener noreferrer">
  Product documentation
</a>
```

## Inline breadcrumb example

```liquid
<nav aria-label="Breadcrumb">
  <ol class="breadcrumb">
    <li><a href="/">Home</a></li>
    <li><a href="/support">Support</a></li>
    <li aria-current="page">Case details</li>
  </ol>
</nav>
```

## Practical rules

- Verify which web link set a template is using before debugging markup.
- Use aria-current for the active item instead of relying on color alone.
- Keep anonymous and authenticated menus intentionally different when required.
- Compute role or capability flags before rendering the list when the menu has multiple gated items.
- Prefer descriptive link text over generic labels.