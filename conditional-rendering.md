# Conditional Rendering

Liquid is commonly used to control what appears on a page.

## Basic If Else

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

## Use Case

Typical scenarios include:

- showing content only for signed-in users
- handling optional fields
- rendering different navigation or page content
