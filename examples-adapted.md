# Adapted examples (Account / Contact / Case)

These examples are adapted to a common Dataverse schema so you can copy them directly into a Power Pages Web Template and swap out logical names if needed.

## 1) Accounts list (top 10 recent active)

```liquid
{% fetchxml accounts %}
<fetch top="10">
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="accountnumber" />
    <attribute name="createdon" />
    <filter>
      <condition attribute="statecode" operator="eq" value="0" />
    </filter>
    <order attribute="createdon" descending="true" />
  </entity>
</fetch>
{% endfetchxml %}

<ul class="accounts-list">
{% for a in accounts.entities %}
  <li>
    <h3><a href="/account/{{ a.accountid }}">{{ a.name }}</a></h3>
    <p>Account #: {{ a.accountnumber | default: "n/a" }}</p>
    <time>{{ a.createdon | date: "%Y-%m-%d" }}</time>
  </li>
{% endfor %}
</ul>
```

## 2) Account detail with related Contacts

This example fetches contacts related to the current account id (assumes `request.params.accountid` is provided).

```liquid
{% fetchxml contacts %}
<fetch>
  <entity name="contact">
    <attribute name="contactid" />
    <attribute name="fullname" />
    <attribute name="emailaddress1" />
    <filter>
      <condition attribute="_parentcustomerid_value" operator="eq" value="{{ request.params.accountid }}" />
    </filter>
    <order attribute="fullname" />
  </entity>
</fetch>
{% endfetchxml %}

{% if contacts.entities.size > 0 %}
  <ul class="contact-list">
    {% for c in contacts.entities %}
      <li>
        <strong>{{ c.fullname }}</strong>
        {% if c.emailaddress1 %}
          — <a href="mailto:{{ c.emailaddress1 }}">{{ c.emailaddress1 }}</a>
        {% endif %}
      </li>
    {% endfor %}
  </ul>
{% else %}
  <p>No contacts found for this account.</p>
{% endif %}
```

## 3) Recent Cases (incidents)

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

<ul class="cases-list">
{% for c in cases.entities %}
  <li>
    <a href="/case/{{ c.incidentid }}">{{ c.title | default: "(no title)" }}</a>
    <span class="ref">{{ c.ticketnumber | default: "—" }}</span>
    <time>{{ c.createdon | date: "%b %d, %Y" }}</time>
  </li>
{% endfor %}
</ul>
```

## How to adapt

- Replace logical names if your tenant uses custom entity names.
- Ensure required attributes are present in the FetchXML so the template doesn't reference missing fields.
