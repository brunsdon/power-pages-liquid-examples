# Current User Examples

These examples use the current signed-in user context in Power Pages.

## Show Welcome Message

```liquid
{% if user %}
  <p>Welcome, {{ user.fullname }}</p>
{% else %}
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

  - Always handle anonymous users safely.
  - Avoid assuming every user field contains a value.
  - Keep user-facing output simple and maintainable.

  ## Role-based UI patterns

  Where UI elements should be visible only to users with specific web roles, centralize checks to make maintenance easier. If role data isn't available in the `user` object by default, fetch it server-side or expose it via a helper snippet.

  ```liquid
  {% comment %}Example assumes `user.webroles` is a string or array containing role names{% endcomment %}
  {% if user and user.webroles contains "PremiumUser" %}
    <a class="btn" href="/premium">Premium dashboard</a>
  {% endif %}
  ```

  Notes:
  - Prefer a small helper snippet to evaluate capabilities rather than spreading raw role checks across templates.
  - Test role-based UI as both anonymous and authenticated users to ensure correct visibility.
