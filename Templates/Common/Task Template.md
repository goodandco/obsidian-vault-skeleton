---
ID:
Type: Task
Status: New
Project:
Category:
Dimension:
StartedAt: ""
FinishedAt: ""
tags:
  - task
  - type/task
  - status/new
Parent:
---
## Notes

**Link**: 


## Docs

- 


## Subtasks


```dataviewjs

const { link } = dv.current().file

const values = dv.pages('#task')  
  .where(b => dv.func.contains(b.Parent, link))   
  .map(p => ([ p.file.link, p.Status, p["start date"], p["end date"]]))
    
dv.table(['File', 'Status', 'Start', 'End' ],  values);
 

```

---


## Questions


```dataviewjs
const { link } = dv.current().file

const values = dv.pages('#faq')  
  .where(b => dv.func.contains(b.Parent, link))
  .sort(k => k.file.name, 'asc')
  .map(p => ([ p.file.link, p.Status ]))
 
dv.table(['File', 'Status'],  values);
```








