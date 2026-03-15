# Navigation and Web Links

Power Pages commonly uses web links and navigation structures.

## Simple Navigation Example
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

## Accessibility and markup

- Use semantic elements for navigation (`<nav>`, `<ul>`, `<li>`) to improve assistive technology support and SEO.
- Indicate the active page using `aria-current="page"` to help screen readers and navigation tools.

```liquid
<nav aria-label="Primary navigation">
  <ul>
    {% for item in weblinks.primary %}
      <li>
        <a href="{{ item.url }}" aria-current="{% if item.url == request.path %}page{% else %}false{% endif %}">
          {{ item.name }}
        </a>
      </li>
    {% endfor %}
  </ul>
</nav>
```

- Ensure link text is descriptive and avoid using non-descriptive text like "click here".
- When links open in a new tab, indicate that to the user and add `rel="noopener noreferrer"` for security.

