## Checkpoint 0
Blog link: https://github.com/jweiss0/oss-repo-template/wiki/Term-Project-Progress-Updates#part-2-lab-discussion

## Checkpoint 1
Browser at `http://localhost:5984`:

## Checkpoint 2
#### Getting Started Part 1

#### Getting Started Part 2

## Checkpoint 3

## Checkpoint 4

Finding all movies whose titles come after "L" in alphabetic order:
```
curl -X POST admin:admin@localhost:5984/hello-world/_find -d '{
  "selector": {
    "title": {
      "$gt": "L"
    }
  }
}' -H 'Content-Type: application/json'
```

Creating index based on title:
```
curl -X POST admin:admin@localhost:5984/hello-world/_index -d '{
   "index": {
      "fields": [
         "title"
      ]
   },
   "name": "title-json-index",
   "type": "json"
}' -H 'Content-Type: application/json'
```

Re-running query with index:
