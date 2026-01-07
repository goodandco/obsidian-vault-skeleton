---
Type: Section
Project:
Category:
Date: ""
tags:
  - section
  - work
  - type/section
---
## Reports

```dataviewjs


const currentFilePath = dv.current().file.path


const values = dv.pages('#report')  
  .where(b => dv.func.contains(b.Project, dv.current().Project))
  .sort(p => p.Date, "desc")
  .map((p) => ([  
     dv.fileLink(p.file.name), p.Status , p.Date
  ]))
    
dv.table(['File', 'Status', 'Date' ],  values);
 

```

---

## Questions 

```dataview

TABLE Status, Date FROM #faq AND # WHERE !Parent SORT file.name
```

---


## Proposals / Ideas

```dataview

TABLE Status, Date FROM #ideas AND # WHERE !contains(file.name, "Template")

```


---

## Sprints

```dataviewjs

const {  
  Project: CProject,  
  file: {  
    path,  
    link  
  }  
} = dv.current();  
  
const curProjCb = b => dv.func.contains(b.Project, CProject);  
const f = (d) => moment(new Date(d))  
  .format("YYYY-MM-DD");  
  
const sprints = dv.pages("#sprint")  
  .where(curProjCb)  
  .sort(p => p["start date"], "desc");  
  
const items = dv.pages("#task")  
  .where(curProjCb);  
  
for (const s of sprints) {  
  const status = dv.fileLink("Done");  
  const postfix = ` (${f(s["start date"])} - ${f(s["end date"])})`;

  const activeSuffix = dv.func.contains(s.Status, dv.fileLink("Active")) 
  ? " - Active - " 
  : "";
  dv.header(3, s.file.link + activeSuffix + postfix);  
  
  const filtered = items  
    .filter(({ Status, Sprint }) => (activeSuffix ? true : dv.func.contains(Status, status)) && dv.func.contains(Sprint, s.file.link))  
    .map(({  
      file: { link: flink },  
      Status,  
      TaskType,  
      StoryPoints,
      ["start date"]: start,  
      ["end date"]: end  
    }) => ([flink, Status, TaskType, StoryPoints, start, end]));  
  
  dv.table(["File", "Status", "Type", "SP", "Start Date", "End Date"], filtered);  
  dv.el("p", "---");  
}


```
---

## Meetings

```dataviewjs

const {  
  Project: CProject,  
  file: {  
    path,  
    link  
  }  
} = dv.current();  

let map = {
  'Refinement': [],
  'Planning': [],
  'Weekly': [],
  'DevGuild': [],
};

const meetings = dv.pages('#meeting AND #domestic_and_general')  
  .where(b => b.MeetingType !== 'Daily' && dv.func.contains(b.Project, CProject) && b.DateTime && !dv.func.contains(b.file.name, "Template"))
  .sort(p => p['DateTime'], 'desc')
  .slice(0, 50);


for (const meeting of meetings) {
  if (!map[meeting.MeetingType]) {
     map[meeting.MeetingType] = [];
  }
   map[meeting.MeetingType].push(meeting);
}


for (const type in map) {
  
  dv.header(3, type);

  const data = map[type].map((p) => ([ p.file.link, p.DateTime ]));
  dv.table(['File', 'Date' ],  data);
  dv.el('p', '---');
}
```