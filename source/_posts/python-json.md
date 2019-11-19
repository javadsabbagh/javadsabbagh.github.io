---
title: Working with JSON in Python
date: 2019-11-19 15:04:22
tags: [python]
categories:
- Python
---

### Provided methods
```python
import json
dir(json)

obj = json.load(file)
obj = json.loads(string)

json.dump(obj, file)  ### writes JSON object to file
str = json.dumps(obj)
```

### Complete sample

Read json file
```python
import json

json_file = open("file_path", "r", encoding = "utf-8")
students = json.load(json_file)
json_file.close()
```