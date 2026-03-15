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
