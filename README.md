# Power Pages Liquid Examples

A practical collection of **Liquid template examples for Microsoft Power Pages**, **Dataverse**, and **Dynamics 365** portals.

This repository is intended as a quick working reference for developers building **Power Pages portals** who need common Liquid patterns for conditional rendering, user-aware content, lists, navigation, and Dataverse-backed display logic.

## Topics Covered

- current user examples
- conditional rendering
- navigation and links
- web links
- looping patterns
- Dataverse-backed display concepts (FetchXML + Liquid examples)
# Power Pages Liquid Examples

A practical collection of **Liquid template examples for Microsoft Power Pages**, **Dataverse**, and **Dynamics 365** portals.

This repository is intended as a quick working reference for developers building **Power Pages portals** who need common Liquid patterns for conditional rendering, user-aware content, lists, navigation, and Dataverse-backed display logic.

## Topics Covered

- current user examples
- conditional rendering
- navigation and links
- web links
- looping patterns
- Dataverse-backed display concepts (FetchXML + Liquid examples)
- portal content patterns
- practical troubleshooting notes

## Contents

- [Current User Examples](current-user.md)
- [Conditional Rendering](conditional-rendering.md)
- [Loops and Lists](loops-and-lists.md)
- [Navigation and Web Links](navigation-and-weblinks.md)
- [Entity and Data Patterns](entity-and-data-patterns.md)
- [Layout and Content Snippets](layout-and-content-snippets.md)
- [Troubleshooting](troubleshooting.md)
- [Examples Adapted](examples-adapted.md) — copyable examples adapted for a common Account/Contact/Case schema
- [Paging Demo](paging-demo.md) — a full paging-demo showing FetchXML paging-cookie and client-side prev/next management
- [Examples Preview (rendered HTML)](examples-preview.html) — static rendered preview of an accounts list for visual guidance
- [Review Checklist](review-checklist.md)

## Dataverse examples added

Advanced examples and samples:

- See `examples-adapted.md` for ready-to-copy Account, Contact, and Case examples.
- See `loops-and-lists.md` for an advanced paging-cookie (cursor) example and a short JS pattern to propagate the cookie.
- See `paging-demo.md` for a full paging demo (server + client pattern).
- A static preview harness is available at `demo/index.html` (mock data in `demo/mock-data.json`) to preview final HTML output.

## Highlights

- Copyable FetchXML + Liquid patterns for common portal scenarios (lists, related records, paging).
- Practical templates for user-aware content (personalization, role checks) and safe rendering practices.
- Troubleshooting and operational notes covering permissions, caching, and FetchXML validation.

## Skills and practices shown

- Liquid templating patterns: conditionals, loops, filters (`default`, `escape`, `date`), and semantic markup.
- Dataverse integration: FetchXML examples, paging patterns, related-record queries, and entity permission considerations.
- Accessibility and UX: semantic navigation, ARIA usage, and empty-state handling.
- Performance and operations: server-side paging, caching guidance, and query simplification.

## Quick start

1. Open an example file (for example `loops-and-lists.md`) and copy a FetchXML + Liquid block into a Portal Web Template.
2. Replace logical names (entity and attribute names) to match your Dataverse schema.
3. Publish the template and view the page in a development portal to verify output.

Notes:
- Examples are snippets for use inside Power Pages (they don't run locally). Use a dev portal for testing.
- If you need schema-adapted examples, provide entity logical names and attributes and examples can be tailored.

## Review checklist and next steps

- A short review checklist is included in `review-checklist.md` to help validate templates for correctness, security, accessibility, and performance.
- For schema-specific examples, paste the logical names and 3–5 attributes you want illustrated and examples will be adapted and added to the repo.

## Why This Repo Exists

Developers often search for:

- Power Pages Liquid examples
- Power Pages template examples
- Dataverse portal Liquid examples
- Dynamics 365 portal Liquid syntax

This repo provides small, practical examples that can be adapted to real projects.

## Author

Maintained by Matthew Brunsdon.
 - Dataverse-backed display concepts (FetchXML + Liquid examples)
