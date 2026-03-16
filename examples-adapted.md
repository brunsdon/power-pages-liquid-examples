# Adapted Examples

These examples are closer to a typical Dataverse schema so they can be copied into a Power Pages Web Template with minimal adaptation.

## Account list pattern

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
  {% for account in accounts.entities %}
    <li>
      <h3><a href="/account/{{ account.accountid }}">{{ account.name | escape }}</a></h3>
      <p>Account #: {{ account.accountnumber | default: "n/a" | escape }}</p>
      <time>{{ account.createdon | date: "%Y-%m-%d" }}</time>
    </li>
  {% endfor %}
</ul>
```

## Account detail with related contacts

```mermaid
flowchart LR
  A[request.params.accountid] --> B[Fetch related contacts]
  B --> C[contacts.entities]
  C --> D[Render contact list or empty state]
```

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
    {% for contact in contacts.entities %}
      <li>
        <strong>{{ contact.fullname | default: "Unnamed contact" | escape }}</strong>
        {% if contact.emailaddress1 %}
          <a href="mailto:{{ contact.emailaddress1 }}">{{ contact.emailaddress1 | escape }}</a>
        {% endif %}
      </li>
    {% endfor %}
  </ul>
{% else %}
  <p>No contacts found for this account.</p>
{% endif %}
```

## Recent cases pattern

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
  {% for case in cases.entities %}
    <li>
      <a href="/case/{{ case.incidentid }}">{{ case.title | default: "(no title)" | escape }}</a>
      <span class="ref">{{ case.ticketnumber | default: "-" | escape }}</span>
      <time>{{ case.createdon | date: "%b %d, %Y" }}</time>
    </li>
  {% endfor %}
</ul>
```

## Permission-aware invoices example

```liquid
{% fetchxml invoices %}
<fetch top="10">
  <entity name="invoice">
    <attribute name="invoiceid" />
    <attribute name="name" />
    <attribute name="invoicenumber" />
    <attribute name="totalamount" />
    <order attribute="createdon" descending="true" />
  </entity>
</fetch>
{% endfetchxml %}

{% if invoices.entities.size > 0 %}
  <ul class="invoice-list">
    {% for invoice in invoices.entities %}
      <li>
        <a href="/invoice/{{ invoice.invoiceid }}">{{ invoice.name | default: invoice.invoicenumber | escape }}</a>
        <span class="ref">{{ invoice.totalamount | default: "Amount unavailable" }}</span>
      </li>
    {% endfor %}
  </ul>
{% elsif user %}
  <p>No visible invoices were found for your account.</p>
{% else %}
  <p>Sign in to view invoices.</p>
{% endif %}
```

## Status badge helper

```liquid
{% for case in cases.entities %}
  {% assign status_label = case.statuscode.label | default: "Unknown" %}
  <span class="status-badge">{{ status_label | escape }}</span>
{% endfor %}
```

## Adaptation checklist

- Replace logical names if your environment uses custom tables or columns.
- Confirm every referenced attribute is included in the FetchXML.
- Add empty states before putting a snippet into production.
- Verify portal permissions with a real contact, not only an admin user.