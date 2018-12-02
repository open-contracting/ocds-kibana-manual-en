# Extras

## Create original files samples

We have used [shuf](https://en.wikipedia.org/wiki/Shuf) to create smaller and handier files creation.

Such as:
```
shuf -n NUMERO_ENTRADAS_DESEADAS -o OUTPUTFILE INPUTFILE
```

Example:
```
shuf -n 1000 -o sample.csv full.csv
```
