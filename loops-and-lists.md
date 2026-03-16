# Loops and Lists

Loops are the normal way to render navigation, cards, tables, and Dataverse result sets. The goal is to keep the loop body readable and handle first, last, and empty states intentionally.

## Loop anatomy

```mermaid
flowchart LR
  A[Collection] --> B{Has items?}
  B -->|No| C[Render empty state]
  B -->|Yes| D[Enter for loop]
  D --> E[Render each item]
  E --> F{Last item?}
  F -->|No| D
  F -->|Yes| G[Close list]
```

## Basic web link loop

```liquid
{% for item in weblinks.primary %}
  <a href="{{ item.url }}">{{ item.name | escape }}</a>
{% endfor %}
```

## Loop with index

```liquid
{% for item in weblinks.primary %}
  <div class="nav-item">
    <span class="nav-index">{{ forloop.index }}.</span>
    <a href="{{ item.url }}">{{ item.name | escape }}</a>
  </div>
{% endfor %}
```

## First and last markers

```liquid
{% for item in weblinks.primary %}
  {% if forloop.first %}
    <p>Primary links</p>
  {% endif %}

  <a href="{{ item.url }}">{{ item.name | escape }}</a>

  {% if forloop.last %}
    <p>End of navigation</p>
  {% endif %}
{% endfor %}
```

## Dataverse results loop

```liquid
{% fetchxml accounts %}
<fetch top="10">
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="accountnumber" />
    <order attribute="createdon" descending="true" />
  </entity>
</fetch>
{% endfetchxml %}

{% if accounts.entities.size > 0 %}
  <ul class="accounts-list">
    {% for account in accounts.entities %}
      <li>
        <h3>{{ account.name | default: "Unnamed account" | escape }}</h3>
        <p>Account number: {{ account.accountnumber | default: "-" | escape }}</p>
      </li>
    {% endfor %}
  </ul>
{% else %}
  <p>No accounts available.</p>
{% endif %}
```

## Group-like heading pattern

This is useful when the list has a special first record.

```liquid
{% for article in articles.entities %}
  {% if forloop.first %}
    <h2>Featured article</h2>
  {% else %}
    {% if forloop.index == 2 %}
      <h2>More articles</h2>
    {% endif %}
  {% endif %}

  <article>
    <h3>{{ article.title | escape }}</h3>
  </article>
{% endfor %}
```

## Basic page-number paging

```liquid
{% assign current_page = request.params.page | default: 1 | plus: 0 %}
{% assign page_size = 10 %}

{% fetchxml records %}
<fetch mapping="logical" count="{{ page_size }}" page="{{ current_page }}">
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <order attribute="name" />
  </entity>
</fetch>
{% endfetchxml %}

<nav class="pager">
  {% if current_page > 1 %}
    <a href="?page={{ current_page | minus: 1 }}">Previous</a>
  {% endif %}

  <span>Page {{ current_page }}</span>

  {% if records.entities.size == page_size %}
    <a href="?page={{ current_page | plus: 1 }}">Next</a>
  {% endif %}
</nav>
```

## Paging-cookie handoff

```liquid
{% if results.paging_cookie %}
  <a href="?pagingcookie={{ results.paging_cookie | url_encode }}">Next</a>
{% endif %}
```

## Practical rules

- Prefer semantic lists when rendering repeated content.
- Keep loop bodies small; assign intermediate values outside when needed.
- Always define an empty state for query-driven lists.
- Use page-number paging for simple cases and paging cookies for stable large datasets.