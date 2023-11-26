\```dataviewjs
const calendarData = {
    entries: [],               
}

//DataviewJS loop
for (let page of dv.pages('"Daily Notes"').where(p => p.exercise)) {
    calendarData.entries.push({
        date: page.file.name,     // (required) Format YYYY-MM-DD
        intensity: page.exercise, // (required) the data you want to track, will map color intensities automatically
    })
}

renderHeatmapCalendar(this.container, calendarData)
```