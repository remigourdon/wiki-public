# Text processing

## Tabular

### Combine CSV Files

This takes the second column from `csv1.csv` and appends it at the end of `csv2.csv` to create `csv3.csv`, with `,` delimiters.

```sh
cut -d \, -f 2 csv2.csv | paste -d \, csv1.csv - > csv3.csv
```

## JSON

### Find Objects in an Array

```sh
# By the content of one of their values
cat telem.json | jq '.[] | select(.id | contains("ang_vel"))'
```
