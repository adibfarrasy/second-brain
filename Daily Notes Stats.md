```dataviewjs
const calendarData = {
    entries: [],
}

for (let page of dv.pages('"Daily Notes"').where(p => p.word_count)) {
    calendarData.entries.push({
        date: page.date,
        intensity: page.word_count,
    })
}

renderHeatmapCalendar(this.container, calendarData)
```


