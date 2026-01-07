---
Date: <% moment(tp.file.title.slice(-10), 'DD.MM.YYYY').format('YYYY-MM-DD') %>
CreatedAt: <% tp.date.now('YYYY-MM-DD HH:mm') %>
Type: DailyNote
Mood:
tags:
  - daily
---
---

# Another wonderful day

## Planner

### Business

- [ ] 10:00 Daily


---
## Birthdays

```dataview

TABLE Role, Birthday
FROM #person 
WHERE contains(dateformat(Birthday, "dd-MM"), dateformat(this.Date, "dd-MM"))
```

## Meetings | Events

```dataview

TABLE Type, MeetingType as Kind, Project, dateformat(DateTime, "HH:mm:ss") as Time
FROM #meeting OR #event
WHERE contains(dateformat(this.Date, "dd-MM-yyyy"), dateformat(DateTime, "dd-MM-yyyy"))
```

## Working on

```dataview

TABLE Type, Status, Date, StartedAt as Start, FinishedAt as End
FROM #task OR #lesson OR #training OR #ideas 
WHERE (StartedAt AND StartedAt <= this.Date AND FinishedAt >= this.Date)
```

---
### Related active projects

```dataview

TABLE Type, Status, StartedAt as Start, FinishedAt as End
FROM #type/project
WHERE  Status="Active" AND (StartedAt AND StartedAt <= this.Date AND FinishedAt >= this.Date)
```

---

## Day review

XXX


---



### This day in the past

```dataview


TABLE Type, MeetingType as Kind, Project, dateformat(DateTime, "HH:mm:ss") as Time
FROM #daily
WHERE Date < this.Date AND contains(dateformat(this.Date, "dd-MM"), dateformat(Date, "dd-MM"))
```
