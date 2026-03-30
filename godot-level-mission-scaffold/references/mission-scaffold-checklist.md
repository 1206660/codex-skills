# Mission Scaffold Checklist

Before stopping, verify these seams exist:

## Data seams

- level id exists
- mission id exists
- objective or trigger definitions have a clear home
- reusable mission resources are separated from one-off data

## Scene seams

- level scene location is obvious
- mission helper scenes or overlays have a stable location
- scenes do not embed mission data that should live in resources or data files

## Script seams

- mission runtime controller has a clear home
- shared gameplay logic stays outside mission-specific folders
- class names match mission or level ids where that improves traceability

## Validation

- scene paths resolve
- resource paths resolve
- naming is consistent
- no second parallel mission folder was created accidentally
