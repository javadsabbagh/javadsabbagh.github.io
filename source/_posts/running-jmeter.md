---
title: Running Jmeter
date: 2019-10-02 15:03:06
tags: [jmeter, testing]
---

Running jmeter from command line is mostly needed for two ain reasons:
- running jmeter tests from a CI tool, e.g jenkins
- running jmeter locally without gui overhead

```bash
jmeter -n -t test_file.jmx -l test_result.csv -e -o result_dir -j test_output.log
```

Test result in web format will be generated in result_dir.
- -n means non-gui mode
- -t test file
- [-p property-file]
- [-l results-file] 
- [-j log-file] 
- -g [csv results file] 
- -e -o [Path to output folder] 
- -H [your proxy server] 
- -P [your proxy server port]

For viewing different styles of running jmeter, issue this command:
```bash
jmeter -help
```
