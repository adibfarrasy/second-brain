sorted by difficulties

```dataview
table difficulty
where difficulty != null and status != "Done"
sort difficulty
```


## Done Projects

```dataview
table difficulty
where difficulty != null and status == "Done"
sort difficulty
```