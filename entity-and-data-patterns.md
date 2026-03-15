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

### Linked (joined) entity example

Sometimes you need attributes from related entities. Use `link-entity` in FetchXML to join related records (example: account with primary contact first/last name).

```liquid
{% fetchxml acct_contact %}
<fetch top="20">
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <link-entity name="contact" from="contactid" to="primarycontactid" alias="pc">
      <attribute name="fullname" />
      <attribute name="emailaddress1" />
    </link-entity>
  </entity>
</fetch>
{% endfetchxml %}

{% for a in acct_contact.entities %}
  <div class="account">
    <h3>{{ a.name }}</h3>
    {% if a.pc.fullname %}
      <p>Primary contact: {{ a.pc.fullname }}</p>
    {% endif %}
  </div>
{% endfor %}
```

Notes:
- Use `alias` values to reference linked-entity attributes (e.g., `a.pc.fullname`).
- Keep joins minimal for performance — prefer separate queries when you only need counts or small subsets.

### Schema adapter snippet

Use this template to quickly adapt examples to your schema; replace tokens with logical names and attributes.

```liquid
{%- comment -%} Replace: ENTITY_NAME, ATTR_ID, ATTR_TITLE, FILTER_ATTR, FILTER_VALUE {%- endcomment -%}
{% fetchxml results %}
<fetch top="10">
  <entity name="ENTITY_NAME">
    <attribute name="ATTR_ID" />
    <attribute name="ATTR_TITLE" />
    <filter>
      <condition attribute="FILTER_ATTR" operator="eq" value="FILTER_VALUE" />
    </filter>
  </entity>
</fetch>
{% endfetchxml %}

{% for r in results.entities %}
  <article>
    <h4>{{ r.ATTR_TITLE }}</h4>
  </article>
{% endfor %}
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

### Linked (joined) entity example

Sometimes you need attributes from related entities. Use `link-entity` in FetchXML to join related records (example: account with primary contact first/last name).

```liquid
{% fetchxml acct_contact %}
<fetch top="20">
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <link-entity name="contact" from="contactid" to="primarycontactid" alias="pc">
      <attribute name="fullname" />
      <attribute name="emailaddress1" />
    </link-entity>
  </entity>
</fetch>
{% endfetchxml %}

{% for a in acct_contact.entities %}
  <div class="account">
    <h3>{{ a.name }}</h3>
    {% if a.pc.fullname %}
      <p>Primary contact: {{ a.pc.fullname }}</p>
    {% endif %}
  </div>
{% endfor %}
```

Notes:
- Use `alias` values to reference linked-entity attributes (e.g., `a.pc.fullname`).
- Keep joins minimal for performance — prefer separate queries when you only need counts or small subsets.

### Schema adapter snippet

Use this template to quickly adapt examples to your schema; replace tokens with logical names and attributes.

```liquid
{%- comment -%} Replace: ENTITY_NAME, ATTR_ID, ATTR_TITLE, FILTER_ATTR, FILTER_VALUE {%- endcomment -%}
{% fetchxml results %}
<fetch top="10">
  <entity name="ENTITY_NAME">
    <attribute name="ATTR_ID" />
    <attribute name="ATTR_TITLE" />
    <filter>
      <condition attribute="FILTER_ATTR" operator="eq" value="FILTER_VALUE" />
    </filter>
  </entity>
</fetch>
{% endfetchxml %}

{% for r in results.entities %}
  <article>
    <h4>{{ r.ATTR_TITLE }}</h4>
  </article>
{% endfor %}
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

### Linked (joined) entity example

Sometimes you need attributes from related entities. Use `link-entity` in FetchXML to join related records (example: account with primary contact first/last name).

```liquid
{% fetchxml acct_contact %}
<fetch top="20">
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <link-entity name="contact" from="contactid" to="primarycontactid" alias="pc">
      <attribute name="fullname" />
      <attribute name="emailaddress1" />
    </link-entity>
  </entity>
</fetch>
{% endfetchxml %}

{% for a in acct_contact.entities %}
  <div class="account">
    <h3>{{ a.name }}</h3>
    {% if a.pc.fullname %}
      <p>Primary contact: {{ a.pc.fullname }}</p>
    {% endif %}
  </div>
{% endfor %}
```

Notes:
- Use `alias` values to reference linked-entity attributes (e.g., `a.pc.fullname`).
- Keep joins minimal for performance — prefer separate queries when you only need counts or small subsets.

### Schema adapter snippet

Use this template to quickly adapt examples to your schema; replace tokens with logical names and attributes.

```liquid
{%- comment -%} Replace: ENTITY_NAME, ATTR_ID, ATTR_TITLE, FILTER_ATTR, FILTER_VALUE {%- endcomment -%}
{% fetchxml results %}
<fetch top="10">
  <entity name="ENTITY_NAME">
    <attribute name="ATTR_ID" />
    <attribute name="ATTR_TITLE" />
    <filter>
      <condition attribute="FILTER_ATTR" operator="eq" value="FILTER_VALUE" />
    </filter>
  </entity>
</fetch>
{% endfetchxml %}

{% for r in results.entities %}
  <article>
    <h4>{{ r.ATTR_TITLE }}</h4>
  </article>
{% endfor %}
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

