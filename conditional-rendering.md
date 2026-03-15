# Conditional Rendering

Liquid is commonly used to control what appears on a page.

## Basic If / Else

```liquid
{% if user %}
  <p>Signed in</p>
{% else %}
  <p>Not signed in</p>
{% endif %}
```

## Check For Blank Value

```liquid
{% if page.title != blank %}
  <h1>{{ page.title }}</h1>
{% endif %}
```

## Combine Conditions

```liquid
{% if user and user.fullname != blank %}
  <p>Hello {{ user.fullname }}</p>
{% endif %}
```

## Use Cases

Typical scenarios include:

- showing content only for signed-in users
- handling optional fields
- rendering different navigation or page content

## Personalization pattern

Use small, data-driven flags to personalize content. This keeps templates simple and makes feature targeting predictable. Below is an example using a boolean flag (e.g., stored on the Contact record) to enable a "beta" banner for selected users.

```liquid
{% if user %}
  {% comment %}Assume `contact` is available in the page context or fetched server-side{% endcomment %}
  {% if contact and contact.new_is_beta_user == true %}
    <div class="banner banner-beta">Try new features — feedback welcome</div>
  {% endif %}
  <h1>Welcome back, {{ user.fullname }}</h1>
{% else %}
  <h1>Welcome</h1>
{% endif %}
```

Notes:
- Prefer boolean or small-enumeration fields for feature flags. Compute or sync them in Dataverse rather than overloading Liquid with logic.
- Keep personalized content minimal and accessible — ensure headings and banners are readable by screen readers.
