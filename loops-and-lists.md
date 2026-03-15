# Loops and Lists

Looping is useful for navigation, repeated content, and collections.

## Loop Through Web Links

```liquid
{% for item in weblinks.primary %}
  <a href="{{ item.url }}">{{ item.name }}</a>
{% endfor %}
```

## Loop With Index

```liquid
{% for item in weblinks.primary %}
  <div>
    <span>{{ forloop.index }}.</span>
    <a href="{{ item.url }}">{{ item.name }}</a>
  </div>
{% endfor %}
```

## First And Last Item Checks

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

## Practical Notes

- keep loop output readable
- avoid complex business logic inside page templates
- test how empty collections behave
