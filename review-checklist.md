# Review Checklist

This checklist helps quickly evaluate portal Liquid templates and Dataverse-backed fragments. Use it when reviewing a Web Template, Web Page template, or snippet.

## Correctness
- Does the template reference correct logical names for entities and attributes?
- Are required attributes included in the FetchXML query (avoid relying on implicit fields)?

## Security and Permissions
- Are Entity Permissions accounted for (anonymous vs authenticated)?
- Are any elevated operations performed server-side with the appropriate identity?

## Output Safety
- Are outputs escaped where needed (`escape`) to prevent injection or layout breakage?
- Are default fallbacks provided for optional fields (`default`) to avoid empty/undefined text?

## Accessibility & Markup
- Is navigation semantic (`<nav>`, `<ul>`, `<li>`), and is `aria-current` used for the active link?
- Do interactive elements have accessible names and roles?

## Performance
- Is the FetchXML scoped to return only required attributes and rows (use `count`/paging)?
- Are heavy computations or aggregations moved to views or pre-computed fields when possible?

## Maintainability
- Are role or capability checks centralized (helper snippet) rather than duplicated across templates?
- Are snippets documented (what inputs they expect)?

## Testing & Troubleshooting
- Can the snippet be tested in isolation (small Web Template with a dev portal)?
- Are common failure modes documented (permissions, empty results, cache issues)?

## Documentation
- Does the repository include a short usage note for the snippet (how to adapt logical names and common variants)?

Use this checklist as an entry point — a passing checklist helps make template examples reliable and re-usable in production scenarios.
