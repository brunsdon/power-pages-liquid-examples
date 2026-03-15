# Entity and Data Patterns

These examples are intentionally lightweight and focus on display concepts rather than heavy data logic.

## Render Simple Value Safely

```liquid
{% if user and user.fullname %}
  <p>{{ user.fullname }}</p>
{% endif %}
```

## Output Escaped Text

```liquid
<p>{{ page.title | escape }}</p>
```

## Default Value Pattern

```liquid
<p>{{ user.fullname | default: "Guest User" }}</p>
```

## Case Transformations

```liquid
<p>{{ page.title | upcase }}</p>
<p>{{ page.title | downcase }}</p>
```

## Date Formatting Pattern

```liquid
<p>{{ now | date: "%d %B %Y" }}</p>
```

## Practical Notes

- prefer simple display logic in Liquid
- move heavy processing to Dataverse, Power Automate, plugins, or integration services
- escape output where appropriate
