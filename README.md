Using Sauce Labs with Geb-Spock and Junit
=========================================

## Setup:

* Install [maven](https://maven.apache.org/)
* Install a recent version of the [JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* Set environment variables:
    * SAUCE_USERNAME=[your user name]
    * SAUCE_ACCESS_KEY=[your access/api key]
    
## Runing Tests:

* Edit the **gen.saucelabs.capabilities.json** file with the configurations you'd like to run. Each line needs to be a valid json object and the file needs to end with a newline. As shown below. 
```json
{"browserName": "MicrosoftEdge", "platform": "Windows 10", "version": "20.10240"}
{"browserName": "Firefox", "platform": "Windows 10", "version": "42"}
```
Please refer to the [Sauce Platform Configurator](https://wiki.saucelabs.com/display/DOCS/Platform+Configurator/) for more information.
* Use the command below from the root of the project.
```bash
./run.sh
```
It will run the tests in your project with each of the configurations listed concurrently by configuration. i.e. all configurations will run at the same time. 
Individual test outputs will be routed to a log file named after the configuration in the project root folder. 
```bash
==> {"browserName": "Firefox", "platform": "Windows 10", "version": "42"}.log
```
* For debug/local runs the test suite will default to Firefox. Use the command below form project root folder to run tests locally.
```bash 
mvn clean test
```

## Parallel Testing Notes:

 The maven surefire fork options enable parallel testing by the test class (spec). You can change the JVM settings listed below to adjust memory allocation and limits and number of processes to be launched. For best results the number of processes should not exceed the number of cores available and the total memory needed for all forks should be less then available memory on test host.

```xml
                    <forkCount>6</forkCount>
                    <reuseForks>false</reuseForks>
                    <argLine>-Duser.language=en</argLine>
                    <argLine>-Xmx1024m</argLine>
                    <argLine>-Xms256m</argLine> 
                    <argLine>-XX:MaxPermSize=256m</argLine>
                    <argLine>-Dfile.encoding=UTF-8</argLine>
                    <useFile>false</useFile>
```

###Important flags to note:

* forkCount: Number of processes to be forked.
* Xmx: Max memory to be allocated
* Xms: Initial memory allocation

## Known Issues:
* Concurrency by test method is not available.
* **gen.saucelabs.capabilities.json** file is not a true json file. It is meant to contain one json object per line and to be terminated with an empty line or line break.
 
