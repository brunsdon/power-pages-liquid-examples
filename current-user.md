# Current User Examples

These patterns use the current signed-in user context exposed by Power Pages. The main rule is simple: always assume the page may also render for an anonymous visitor.

## User context flow

```mermaid
flowchart LR
  A[Request arrives] --> B{user available?}
  B -->|Yes| C[Render personalized content]
  B -->|No| D[Render guest experience]
  C --> E[Optional role or profile checks]
```

## Welcome message

```liquid
{% if user %}
  <p>Welcome, {{ user.fullname | default: "there" | escape }}</p>
{% else %}
  <p>Welcome, guest</p>
{% endif %}
```

## Current user email

```liquid
{% if user and user.emailaddress1 != blank %}
  <p>{{ user.emailaddress1 | escape }}</p>
{% endif %}
```

## Signed-in only link

```liquid
{% if user %}
  <a href="/profile">My Profile</a>
{% endif %}
```

## Anonymous-only link

```liquid
{% unless user %}
  <a href="/sign-in">Sign In</a>
{% endunless %}
```

## Show a profile summary card

```liquid
{% if user %}
  <section class="profile-card">
    <h2>{{ user.fullname | default: "Portal user" | escape }}</h2>
    <p>{{ user.emailaddress1 | default: "No email on file" | escape }}</p>
    <a href="/profile">Manage profile</a>
  </section>
{% endif %}
```

## Role-based UI branch

Portal implementations differ in how role information is exposed. If your project maps web roles into a helper variable, keep the actual check small and readable.

```liquid
{% assign can_view_premium = user and user.webroles contains "PremiumUser" %}

{% if can_view_premium %}
  <a class="btn" href="/premium">Premium dashboard</a>
{% endif %}
```

## Defensive display pattern

```liquid
{% assign display_name = user.fullname | default: user.emailaddress1 | default: "Portal user" %}

{% if user %}
  <p>Signed in as {{ display_name | escape }}</p>
{% endif %}
```

## Practical rules

- Never assume every user property is populated.
- Escape user-derived output even when it looks harmless.
- Keep authenticated and anonymous experiences both intentional.
- Centralize role and capability checks if more than one template depends on them.