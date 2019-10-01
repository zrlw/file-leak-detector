# file-leak-detector for WebSphere Application Server 8.5 (IBM JDK 1.6)
Java agent that detects file handle leak for WebSpere Application Server 8.5 (IBM JDK 1.6) <br>
by Lao Wei(zrlw@sina.com) on Oct 01 21:19:00 GMT+8 2019 <br>
# Build Steps at Eclipse with m2e plugin
1. after git clone the project, you should change java.home in pom.xml to your Oracle JDK's JRE directory. <br>
2. Maven - Update Project... - OK <br>
3. Run as - Maven build... <br>
* in Main tab<br>
Goals: install -e <br>
checked 'Skip Tests' option <br>
* in JRE tab<br>
runtime JRE: Oracle JRE (suggest Oracle JRE 1.8) <br>
* click run button, it will download needed jars from maven repositories. <br>  
4. after Build Success, configure project's build path, set JRE System library to IBM JDK 1.6 <br>
5. Project - Clean <br>
6. Project - Build Project <br>
7. Export - Jar file - only export src/main/java <br>
8. edit the exported jar, add such contents to META-INF/MANIFEST.MF <br>
```
Premain-Class: org.kohsuke.file_leak_detector.AgentMain
Agent-Class: org.kohsuke.file_leak_detector.AgentMain
Main-Class: org.kohsuke.file_leak_detector.Main
Can-Redefine-Classes: true
Can-Retransform-Classes: true
Built-By: your-name
```
9. uncompress asm6 & args4j jar to a temp directory from your local repository which can be found in project build path. <br>
10. merge org directory of asm6 & args4j from the temp directory into the exported jar. <br>
11. copy the exported jar to the machine which run the WebSphere Application Server. <br>  
12. logon WAS administrator console, set Java virtual machine of the WebSpere Application Server to debug mode. <br>
---------------------------------------------------------------------------------------------------------------------------------
Detector Method A: <br>
```
1. add -javaagent:<your-path>/<your-jar>=http=<your-port>,host=<ip-of-WAS-machine> in general JVM parameters of the WAS.
2. start WAS
check <websphere_profile_home>/<node>/logs/<server>/native_stderr.log for error.
```
--------------------------------------------------------------------------------------------------------------------------------
Detector Method B: <br>
```
logon the account which run WAS process, run command：
<WAS-java-bin-path>/java -jar <your-path>/<your-jar> <WAS-PID> http=<your-port>,host=<ip-of-WAS-machine>
```
-------------------------------------------------------------------------------------------------------------------------------
```
now open your browser, open http://<ip-of-WAS-machine>:<your-port> to get detector result.
```
