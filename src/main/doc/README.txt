---------------------------------------
>>                                   <<
>>>      Java Flight Recorder       <<<
>>                                   <<
---------------------------------------

Java Flight Recorder (JFR) is a monitoring tool that collects information about the events 
in a particular instant of time in a Java Virtual Machine during the execution of an application.
Since Java 11, JFR is free and open source.

A JFR recording is usually analyzed with Java Mission Control. 
Jeyzer can also achieve the same to generate a JZR report.
Jeyzer analysis advantages are :
- Active focus on the threads of interest
- Advanced issue detection rules
- Unique abstraction features
- Portable JZR report
- Centralized analysis with the Jeyzer Web Analyzer 
- Applicative detection rules (*)
- De-obfuscation support (*)

(*) Requires to setup Jeyzer applicative profiles

Jeyzer supports JFR recordings made with Java 11+ versions.


--------------------------------
===     Immediate usage      ===
--------------------------------

Prerequisites : as indicated previously, your application must run under Java 11 or above.

- Deploy the jeyzer.jfc file (available in the <Jeyzer home>/jfr) in the target environment.
- Add the following VM parameter to your Java application command line (replace the <> as indicated) : 
   --XX:StartFlightRecording=maxage=6h,maxsize=100M,settings=<JFR configuration>,filename=<JFR recording>"
     <JFR configuration> is the path to the jeyzer.jfc file
     <JFR recording> is the path to the resulting JFR recording
- Start your application. Let it run for some time 
- Stop it properly (service shutdown, kill -10, CTRL+C..) to let the JVM dumping the latest events.

The maxage and maxsize parameters indicate the maximum retention period of the collected VM data.

Analysis :
- Start the Jeyzer Web Analyzer and connect to it
- Drop the JFR recording in the upload section of the analyzer
- Generate the report


--------------------------------
===    JFR configuration     ===
--------------------------------

It is recommended to use the jeyzer.jfc configuration file shipped 
within any Jeyzer Recorder or Jeyzer Ecosystem installation 2.4+. 

This configuration defines the optimal JFR settings to :
 - limit the performance impact of JFR
 - record only the information required for a Jeyzer analysis

The thread dump period is set by default to 30 seconds.
To update it, edit the jeyzer.jfc file and change this parameter value :
  <setting name="period" label="Period" description="Record event at interval" contentType="jdk.jfr.Period">30 s</setting>


--------------------------------
===    JFR Documentation     ===
--------------------------------

For more details about JFR, please see :
https://docs.oracle.com/javacomponents/doc/JDMUG/using-jdk-flight-recorder.htm

Source code (Java 11) :
https://github.com/AdoptOpenJDK/openjdk-jdk11/tree/master/src/jdk.jfr/share/classes/jdk/jfr