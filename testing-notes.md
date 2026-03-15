# Testing notes — how to validate examples in a dev portal

Quick steps to verify the Liquid + FetchXML examples in a Power Pages development portal.

1) Prepare a dev portal environment
   - Use a sandbox or development Power Pages environment.
   - Ensure you have a contact with a web role that allows access to sample data, or use an admin contact.

2) Create a Web Template (or Web Page template)
   - In the Portal Management app, create a new Web Template and paste one of the FetchXML + Liquid blocks from this repo.
   - Replace logical names as needed to match your Dataverse schema.

3) Expose required params
   - For detail pages that require an id (for example `request.params.accountid`), create a test page with a query param or set the context accordingly.

4) Test as anonymous and authenticated users
   - Check the output while signed out (anonymous) and as an authenticated portal user to verify Entity Permission effects.

5) Validate paging
   - For the paging examples, navigate forward and back. Inspect sessionStorage in DevTools to see the client-side paging stack.

6) Troubleshooting
   - If queries return no results, verify entity logical names and attributes in FetchXML, and verify Entity Permissions (portal admin > Entity Permissions).
   - If output looks stale, clear portal cache (Portal Admin) and your browser cache.
   - If the FetchXML returns unexpected shapes, add a temporary debug block in the template to dump the `results` object (for example `{{ results | json }}`) and view it in the rendered page.

7) Accessibility checks
   - Use a screen reader or accessibility tools (Lighthouse) to confirm semantic markup and aria attributes.

8) Performance
   - Ensure you limit returned attributes and use paging for large datasets.

Collect a minimal failing example (FetchXML snippet and template) if you need targeted help; include the portal behavior and any error messages.
