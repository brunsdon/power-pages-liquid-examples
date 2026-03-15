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

## Dataverse: FetchXML and common data patterns

When you need to list Dataverse records, prefer server-side FetchXML (via Web Templates / Web Pages) or pre-populate dataset variables where possible. Keep Liquid templates focused on presentation.

### Simple FetchXML -> render pattern

```liquid
{% fetchxml cases %}
<fetch top="5">
  <entity name="incident">
    <attribute name="incidentid" />
    <attribute name="title" />
    <attribute name="ticketnumber" />
    <attribute name="createdon" />
    <filter>
      <condition attribute="statecode" operator="eq" value="0" />
    </filter>
    <order attribute="createdon" descending="true" />
  </entity>
</fetch>
{% endfetchxml %}

<div class="cases">
  {% for c in cases.entities %}
    <article class="case">
      <h4><a href="/case/{{ c.incidentid }}">{{ c.title }}</a></h4>
      <p>Ref: {{ c.ticketnumber | default: "n/a" }}</p>
      <time>{{ c.createdon | date: "%b %d, %Y" }}</time>
    </article>
  {% endfor %}
</div>
```

### Safe field access and defaults

Use `default` and `escape` to avoid empty or risky output. Example:

```liquid
<p>{{ record.description | default: "No description" | escape }}</p>
```

### Move heavy logic out of Liquid

- Use Dataverse views, calculated fields, or Power Automate to prepare datasets.
- Use server-side paging for large result sets. FetchXML supports paging cookies for this.

### Security and permissions

Portal-rendered FetchXML runs under the context of the portal user. This means results may be filtered or blocked by configured Entity Permissions and Web Roles:

- Verify Entity Permissions for the Dataverse table to ensure the portal identity (anonymous or an authenticated contact with particular web roles) has the required Read access.
- For server-to-server integrations where elevated privileges are required, consider using an Azure Function or authenticated Web API that the portal calls (avoid embedding credentials in templates).
- Test queries as both anonymous and authenticated users to reproduce permission-related differences.

Keep these considerations in mind when designing FetchXML queries and choosing where to perform data transformations.

