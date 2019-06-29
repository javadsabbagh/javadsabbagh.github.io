---
title: Groovy in VSCode
date: 2019-06-23 15:54:58
tags: [groovy, vscode]
categories:
- [groovy]
---

### Introduction
You can use VSCode as an **Editor**, but not an **~~IDE~~**, for writing and running groovy scripts. In this regard, you need following extensions:
- code-groovy extension by Marlon Franca
- Code Runner extension by formulahendry.code-runner

Now, you can create groovy files of .groovy extension and run them by right clicking in file editor and selecting 'code runner' menu item.

### Config Code Runner
You should put groovy location in environment PATH, otherwise you need to config code runner and set the application path explicitly:
- press "Ctrl + ," to open settings window
- Search for "Code runner config"
- Find 'Executor Map' and click on 'Edit in settings.json'
![code-runner-config](code-runner-config.png)
- Add following section to settings.json:
```json
    "code-runner.executorMap": {
        "groovy": "/home/javad/Applications/groovy-2.5.6/bin/groovy",
        "javascript": "node",
        "php": "C:\\php\\php.exe",
        "python": "python",
        "perl": "perl",
        "ruby": "C:\\Ruby23-x64\\bin\\ruby.exe",
        "go": "go run",
        "html": "\"C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe\"",
        "java": "cd $dir && javac $fileName && java $fileNameWithoutExt",
        "c": "cd $dir && gcc $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt"
    }
```

Now, you can run groovy files from VSCode.
