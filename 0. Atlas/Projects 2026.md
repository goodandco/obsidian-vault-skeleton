---
tags:
  - dashboard
  - projects
---

```dataviewjs

const to = '2026-01-01';
const from = '2025-01-01';

const projects = dv.pages('#project')  
  .where(
	  p => p.Dimension 
	  && (p.StartedAt && p.StartedAt.ts < dv.date(to).ts) 
	  && (!p.FinishedAt || p.FinishedAt.ts <= dv.date(to).ts && p.FinishedAt.ts >= dv.date(from).ts)
);  
  
const dimensions = projects.groupBy(p => p.Dimension)  
  .map(p => p.key);  
  
for (const dimension of dimensions) {  
  const dimProjects = projects.where(p => dv.func.contains(p.Dimension, dimension));  
  const categories = dimProjects.groupBy(p => p.Category)  
    .map(p => p.key);  
  
  dv.header(3, `Dimension: ${dimension} (${dimProjects.length})`);  
  for (const category of categories) {  
    const categoryProjects = dimProjects  
      .where(p => dv.func.contains(p.Category, category))  
      .sort(p => p.Status, 'ASC')
      .map(p => [p.file.link, p.Status, p.StartedAt, p.FinishedAt, p.Deadline]);  
    dv.header(4, `Category: ${category} (${categoryProjects.length})`);  
    dv.table(["File", "Status", "Started", "Finished", "Deadline"], categoryProjects);  

  }  
  dv.el("p", "---");  
  
}
```
