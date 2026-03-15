# Troubleshooting

A few common Power Pages Liquid issues appear again and again.

## Blank Output

Check:

- variable exists
- user is signed in
- field has data
- correct template is being used

## Navigation Not Rendering

Check:

- correct web link set
- page visibility rules
- signed-in versus anonymous differences

## Output Looks Stale

Check:

- portal cache
- browser cache
- unpublished changes
- wrong environment

## Liquid Becomes Too Complex

That is usually a sign to move logic elsewhere:

- Dataverse model
- Power Automate
- plugin logic
- Azure integration service

## Practical Advice

When debugging, reduce the template to the smallest possible working example and add pieces back gradually.

## Common portal troubleshooting checklist

Below are concise checks and fixes for problems you might encounter when rendering Dataverse-backed content in Power Pages.

- Permission / security trimmed results
	- Symptom: FetchXML returns fewer records or empty results for some users.
	- Check: Ensure the portal user (contact/portal identity) has proper table permissions (table permissions and web role configuration) for the underlying Dataverse table.
	- Fix: Configure Entity Permissions in the portal admin (grant Read access for the required scope) or use a service account pattern for server-side components.

- FetchXML errors or no output
	- Symptom: FetchXML block doesn't return entities or errors out.
	- Check: Validate the FetchXML syntax (matching attribute and entity logical names), ensure fields exist, and test the query in a Web Template with a simple top/short fetch.
	- Fix: Start with a minimal fetch (one attribute, one entity) and expand. Watch portal logs for detailed errors.

- Stale or cached output
	- Symptom: Portal shows old data after updates in Dataverse.
	- Check: Portal caching is enabled; also browser caching may show old pages.
	- Fix: Publish/unpublish strategy, clear portal cache from Power Apps Portals admin, and test in a private browser session. Use short cache windows for frequently changing data.

- Missing fields or null values
	- Symptom: Template shows empty values or errors when accessing nested attributes.
	- Check: Confirm the attributes are included in the FetchXML query and that optional fields are handled with `default` or conditional checks.
	- Fix: Add attributes to FetchXML, or use `{{ field | default: "—" }}` to provide fallbacks.

- Performance: slow queries or timeouts
	- Symptom: Slow page loads when rendering large datasets.
	- Check: Are you requesting many attributes or performing many joins? Are you using server-side paging?
	- Fix: Limit attributes returned, use indexed fields in filters, page results, or prepare pre-aggregated views in Dataverse.

If problems persist, collect a minimal failing example (FetchXML, template snippet, and the exact portal behavior) and share it for targeted debugging.
