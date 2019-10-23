---
title: Running Jmeter
date: 2019-10-02 15:03:06
tags: [jmeter, testing]
---

Running jmeter from command line is mostly needed for two main reasons:
- running jmeter tests from a CI tool, e.g jenkins
- running jmeter remotely on a server. In this case gui overhead is not desired, though many servers run in only text mode.

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

### Jmeter command-line options
```bash
	--?
		print command line options and exit
	-h, --help
		print usage information and exit
	-v, --version
		print the version information and exit
	-p, --propfile <argument>
		the jmeter property file to use
	-q, --addprop <argument>
		additional JMeter property file(s)
	-t, --testfile <argument>
		the jmeter test(.jmx) file to run. "-t LAST" will load last 
		used file
	-l, --logfile <argument>
		the file to log samples to
	-i, --jmeterlogconf <argument>
		jmeter logging configuration file (log4j2.xml)
	-j, --jmeterlogfile <argument>
		jmeter run log file (jmeter.log)
	-n, --nongui
		run JMeter in nongui mode
	-s, --server
		run the JMeter server
	-E, --proxyScheme <argument>
		Set a proxy scheme to use for the proxy server
	-H, --proxyHost <argument>
		Set a proxy server for JMeter to use
	-P, --proxyPort <argument>
		Set proxy server port for JMeter to use
	-N, --nonProxyHosts <argument>
		Set nonproxy host list (e.g. *.apache.org|localhost)
	-u, --username <argument>
		Set username for proxy server that JMeter is to use
	-a, --password <argument>
		Set password for proxy server that JMeter is to use
	-J, --jmeterproperty <argument>=<value>
		Define additional JMeter properties
	-G, --globalproperty <argument>=<value>
		Define Global properties (sent to servers)
		e.g. -Gport=123
		 or -Gglobal.properties
	-D, --systemproperty <argument>=<value>
		Define additional system properties
	-S, --systemPropertyFile <argument>
		additional system property file(s)
	-f, --forceDeleteResultFile
		force delete existing results files and web report folder if
		 present before starting the test
	-L, --loglevel <argument>=<value>
		[category=]level e.g. jorphan=INFO, jmeter.util=DEBUG or com
		.example.foo=WARN
	-r, --runremote
		Start remote servers (as defined in remote_hosts)
	-R, --remotestart <argument>
		Start these remote servers (overrides remote_hosts)
	-d, --homedir <argument>
		the jmeter home directory to use
	-X, --remoteexit
		Exit the remote servers at end of test (non-GUI)
	-g, --reportonly <argument>
		generate report dashboard only, from a test results file
	-e, --reportatendofloadtests
		generate report dashboard after load test
	-o, --reportoutputfolder <argument>
		output folder for report dashboard
```
