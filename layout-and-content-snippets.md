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

## Reusable Dataverse list snippet

Create small, focused snippets for commonly re-used lists (e.g., a recent articles block backed by Dataverse). Store this Liquid in a Web Template or a snippet file and include it where needed.

Example — recent published articles snippet:

```liquid
{% fetchxml recent_articles %}
<fetch top="5">
  <entity name="msfp_survey"> <!-- replace with your entity -->
    <attribute name="msfp_surveyid" />
    <attribute name="msfp_name" />
    <attribute name="createdon" />
    <filter>
      <condition attribute="statuscode" operator="eq" value="1" />
    </filter>
    <order attribute="createdon" descending="true" />
  </entity>
</fetch>
{% endfetchxml %}

<aside class="recent-articles">
  <h4>Recent content</h4>
  <ul>
    {% for item in recent_articles.entities %}
      <li>
        <a href="/content/{{ item.msfp_surveyid }}">{{ item.msfp_name }}</a>
        <time>{{ item.createdon | date: "%Y-%m-%d" }}</time>
      </li>
    {% endfor %}
  </ul>
</aside>
```

Tips:
- Keep snippet inputs minimal (e.g., accept a page param for context id).
- Document required fields for maintainers.

