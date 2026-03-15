# Navigation and Web Links

Power Pages commonly uses web links and navigation structures.

## Simple Navigation Example

```liquid
{% for item in weblinks.primary %}
  <li><a href="{{ item.url }}">{{ item.name }}</a></li>
{% endfor %}
```

## Add Active Class Pattern

```liquid
{% for item in weblinks.primary %}
  <a href="{{ item.url }}"
     class="{% if item.url == request.path %}active{% endif %}">
     {{ item.name }}
  </a>
{% endfor %}
```

## Practical Notes

- confirm which web link set is being used
- keep navigation patterns consistent across the site
- test signed-in and anonymous experiences separately
