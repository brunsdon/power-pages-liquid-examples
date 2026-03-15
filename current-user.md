# Current User Examples

These examples use the current signed-in user context in Power Pages.

## Show Welcome Message

```liquid
{% if user %}
  <p>Welcome, {{ user.fullname }}</p>
{% else %}
  <p>Welcome, guest</p>
{% endif %}
```

## Show Current User Email

```liquid
{% if user %}
  <p>{{ user.emailaddress1 }}</p>
{% endif %}
```

## Show Link Only To Signed-In Users

```liquid
{% if user %}
  <a href="/profile">My Profile</a>
{% endif %}
```

## Show Link Only To Anonymous Users

```liquid
{% unless user %}
  <a href="/sign-in">Sign In</a>
{% endunless %}
```

## Practical Notes

- always handle anonymous users safely
- avoid assuming every user field contains a value
- keep user-facing output simple and maintainable
