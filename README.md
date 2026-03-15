# Power Pages Liquid Examples

A practical collection of Liquid template examples for Microsoft Power Pages, Dataverse, and Dynamics 365 portals.

This repository is a concise working reference for developers building Power Pages portals who need common Liquid patterns: conditional rendering, user-aware content, lists, navigation, and Dataverse-backed display logic.

## Topics Covered

- current user examples
- conditional rendering
- navigation and links
- web links
- looping patterns
- Dataverse-backed display concepts (FetchXML + Liquid examples)
- layout and content snippets
- troubleshooting and testing notes

## Contents

- [Current User Examples](current-user.md)
- [Conditional Rendering](conditional-rendering.md)
- [Loops and Lists](loops-and-lists.md)
- [Navigation and Web Links](navigation-and-weblinks.md)
- [Entity and Data Patterns](entity-and-data-patterns.md)
- [Layout and Content Snippets](layout-and-content-snippets.md)
- [Troubleshooting](troubleshooting.md)
- [Examples Adapted](examples-adapted.md)
- [Paging Demo](paging-demo.md)
- [Examples Preview (rendered HTML)](examples-preview.html)
- [Review Checklist](review-checklist.md)

## Dataverse examples added

- `examples-adapted.md`: Account, Contact, and Case examples you can copy into a Web Template.
- `loops-and-lists.md`: looping patterns and a paging-cookie (cursor) example.
- `paging-demo.md`: a full server + client paging demo using FetchXML paging cookies.
- A static preview harness is available in `demo/` (open `demo/index.html`).

## Quick start

1. Copy a FetchXML + Liquid block from this repo into a Portal Web Template.
2. Replace logical names (entity and attribute names) to match your Dataverse schema.
3. Publish the template and verify output in a development portal.

## Notes

- Examples are snippets for Power Pages and must be tested in a dev portal.
- Check Entity Permissions and web roles when testing authenticated vs anonymous behavior.

## Author

Maintained by Matthew Brunsdon.