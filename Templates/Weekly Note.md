---
Type: WeeklyNote
Date: <% tp.date.now('YYYY-MM-DD HH:mm') %>
tags:
  - weekly
---
---

## Tracking

```dataviewjs 

// habits
const business =['Good_Teammate'];
const family = ['Good_Parent', 'Good_Husband', 'Sex', 'Travel'];
const friends = ['Good_Friend'];
const selfDev = ['Reading', 'Writing'];
const health = ['Ewake', 'Esleep', 'Morning_exercise', 'MornEx', 'Walk_10k', 'Swimming', 'Meditation'];
const props = { business, family, friends,  selfDev,  health };  
const lists = [];

let busSum = 0;
let famSum = 0;
let friSum = 0;
let perSum = 0;
let helSum = 0;
let sumSum = 0;
let moodSum = 0;
let moodCount = 0;

const pages = dv.pages('#daily')  
  .where(b => dv.func.contains(b.file.folder, dv.current().file.folder) && !!b.Date)
  .sort(b => b.Date, 'asc')  
  .map(page => {  
    const [bus, fam, fri, per, hel] = Object.keys(props).map(  
      dim => props[dim].reduce(  
        (r, key) => r + (lists.includes(key) ? page[key] && page[key].length : !!page[key]),  
        0  
      )  
    );
    const summary = bus + fam + fri + per + hel;
    const mood = page.Emotional_Level || page.Mood;
    
    busSum+=bus;
    famSum+=fam;
    friSum+=fri;
    perSum+=per;
    helSum+=hel;
    sumSum+=summary;
    moodSum+=mood;
    moodCount++;
  
    return [  
      dv.fileLink(page.file.name), mood, summary, bus, fam, fri, per, hel 
    ];  
  });
const wrap = (t) => `<span style="font-weight: bold;">${t}</span>`;

const rows = [
   ...pages,
   ['_','_','_','_','_','_','_'],
   [wrap("Summary"), wrap(parseFloat(moodSum / moodCount).toFixed(2)), wrap(sumSum), wrap(busSum), wrap(famSum), wrap(friSum), wrap(perSum), wrap(helSum)]
];
  
dv.table(['File','Mood', 'Sum', 'Business', 'Family', 'Friends', 'Personal', 'Health'], rows);
```

---

## Accomplished tasks

```dataview
TASK FROM #daily WHERE contains(file.folder, this.file.folder) SORT Date GROUP BY meta(header).subpath
```

---
