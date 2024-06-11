default usage with jq-filter "." (copy input to output unmodified):
```bash
jq '.' d.json
```

get specific key or object and print it's value:
```bash
jq '.apiName' d.json -r
```

 `-r` output raw strings, not JSON format

pipe long one-line JSON to jq for a human-readable format:
```bash
cat some.json | jq '.'
```

unpack all objects individually out of the array (not comma-separated):
```bash
cat some.json | jq '.[]'
```

`[0]`,`[2:4]` get some index or a range of objects

get keys from object with index 5:
```bash
cat some.json | jq '.[5].title, .[5].id'
```

pipe inside filters:
```bash
cat some.json | jq '.[] | .title, .id'
```

add another key 'points' to every object and assign 2 to each of them:
```bash
cat some.json | jq '.[] | .points = 2'
```

add string to beginning of each key:
```bash
cat some.json | jq '.[] | "Some text next to \(.title)"'
```

show only keys:
```bash
cat some.json | jq '.[] | keys'
```

get the length of the array:
```bash
cat some.json | jq '. | length'
```

add +1 to each id in every object:
```bash
cat some.json | jq '. | map(.id +=1)'
```

override some key to other name:
```bash
cat some.json | jq '. | map(.title = "overriden title")'
```

get keys with condition:
```bash
cat some.json | jq '.[] | select(.id >= 197)'
cat some.json | jq '.[] | select(.completed = "true")'
```

fizzbuzz analog in jq:
```bash
cat some.json | jq '[ .[] | if (.id % 3 == 0) and (.id % 5 == 0) then .points = 8 elif .id % 5 == 0 then .points = 5 elif .id % 3 == 0 then .points = 3 else .points =1 end]'
```