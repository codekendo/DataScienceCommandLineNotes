

## Example of Parallels


```bash

cd / book/ch01/data
parallel -j1 --progress --delay 0.1 --results results "curl -SL"\
"'http://api.nytimes.com/svc/search/v2/articlesearch.json?q=New+York+'"\
"'Fashion+Week&begin_date={1}0101&end_date={1}123&page={2}&api-key='"\
"'<your-api-key>'" ::: {2009..2013} ::: {0.99} > /dev/null

```

---
## Example of jq

```bash
cat results/1/*/2/*/stdout |
jq -c '.response.docs[] | {date: .pub_date, type: .document_type, '\
'title: .headline.main '
}' | json2csv -p -k date,type,title > fashion.csv
```

Line 1 combine the output of each of the 500 parallel jobs.

line 2 Use jq to extract the publication date, the document type and the headline of each article.
Line 3 convert the json data to csv using json2csv and store it.

---

```bash
wc -l
```

word count per line



---
redirection 

https://www.tutorialspoint.com/unix/unix-io-redirections.htm


print a table from a csv file

```bash
$ < fashion.csv cols -c date cut -dT -f1 | head | csvlook

```


print a R graph from csv file

```bash
$ < fashion.csv Rio -ge 'g + geom_freqpoly(aes(as.Date(date), color=type), '\
> 'binwidth=7) + scale_x_date() + labs(x="date", title="Coverage of New York'\
> ' Fashion Week in New York Times")' | display
```




