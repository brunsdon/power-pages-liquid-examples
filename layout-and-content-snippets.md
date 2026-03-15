# Layout and Content Snippets

Small content snippets can improve consistency across pages.

## Reusable Welcome Block Pattern

```liquid
<section class="welcome-panel">
  {% if user %}
    <h2>Welcome, {{ user.fullname }}</h2>
    <p>Use the links below to continue.</p>
  {% else %}
    <h2>Welcome</h2>
    <p>Please sign in to continue.</p>
  {% endif %}
</section>
```

## Notification Banner Pattern

```liquid
{% if request.params.notice %}
  <div class="alert alert-info">
    {{ request.params.notice | escape }}
  </div>
{% endif %}
```

## Empty State Pattern

```liquid
{% if weblinks.primary.size == 0 %}
  <p>No links are currently available.</p>
{% endif %}
```

## Practical Notes

- build small reusable patterns
- keep templates readable for future maintainers
- document any assumptions around page variables
