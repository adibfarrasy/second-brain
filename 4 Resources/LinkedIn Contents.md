sorted by difficulties

```dataview
table difficulty
where difficulty != null and status != "Done"
sort choice(difficulty = "Easy", "1",
choice(difficulty = "Medium", "2",
choice(difficulty = "Hard", "3", "other")))
```


## Done Projects

```dataview
table difficulty
where difficulty != null and status = "Done"
sort choice(difficulty = "Easy", "1",
choice(difficulty = "Medium", "2",
choice(difficulty = "Hard", "3", "other")))
```