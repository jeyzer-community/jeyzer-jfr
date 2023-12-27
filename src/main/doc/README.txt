---------------------------------------
>>                                   <<
>>>      Java Flight Recorder       <<<
>>                                   <<
---------------------------------------

Although the Jeyzer Recorder is the recommended way to go, you may use JFR as a replacement solution,
especially if the DevOps are not ready to deploy the Jeyzer Recorder agent in a production environment.

Java Flight Recorder (JFR) is a JVM feature that collects information about the events 
in a particular instant of time during the execution of an application.
Since Java 11, the JFR new implementation (known as JFR 1.0) is free.
JFR 1.0 has been back ported on the latest Java 8 OpenJDK.

A JFR recording is usually analyzed with Java Mission Control (JMC).
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

Jeyzer supports JFR recordings made with Java 11+ versions and recent Java 8 OpenJDKs.


--------------------------------
===     Immediate usage      ===
--------------------------------

Prerequisites : as indicated previously, your application must run under Java 11 or above.

- Deploy the jeyzer.jfc file (available in the <Jeyzer home>/jfr) in the target environment.
- Add the following VM parameter to your Java application command line (replace the <> as indicated) : 
   --XX:StartFlightRecording=maxage=6h,maxsize=100M,dumponexit=true,settings=<JFR configuration>,filename=<JFR recording>
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

Note that the provided JFC configuration is backward compatible.
For example, the VirtualThreadStart and VirtualThreadEnd events are ignored in Java 11.
Those events require Jeyzer 3.1+ to be analyzed.


--------------------------------
===       JFR events         ===
--------------------------------

The following JFR events are captured and analyzed by Jeyzer :
- jdk.CPUInformation
- jdk.GarbageCollection
- jdk.GCConfiguration
- jdk.GCHeapSummary
- jdk.G1HeapSummary
- jdk.InitialEnvironmentVariable
- jdk.InitialSystemProperty
- jdk.JVMInformation
- jdk.ModuleExport
- jdk.ModuleRequire
- jdk.OldGarbageCollection
- jdk.OSInformation
- jdk.PhysicalMemory
- jdk.PSHeapSummary
- jdk.ThreadAllocationStatistics
- jdk.ThreadCPULoad
- jdk.ThreadDump
- jdk.ThreadEnd
- jdk.YoungGarbageCollection 
- jdk.VirtualThreadStart
- jdk.VirtualThreadEnd
- jdk.VirtualThreadPinned


--------------------------------
===     JFR limitations      ===
--------------------------------

At this stage, Jeyzer provides the same level of analysis on both JFR recordings and JZR recordings.
In the near future, extra JFR data will be handled and analyzed.

Some monitoring data - captured with the Jeyzer Recorder - is not available in JFR :
- The system properties created by the application. It limits the process card usage in Jeyzer.
- The list of loaded jar files. It prevents the usage of the process jar versions in Jeyzer : alternative is to use the module versions.
- The loaded jar file manifest attributes
- The disk space checks
- The data capture durations
- The Jeyzer Publisher events (expected). Those could be replaced by the JFR events on JDK 15+ (not yet supported).

The Jeyzer Monitor cannot process yet the JFR recordings : you must still use the Jeyzer Recorder.


--------------------------------
===       JFR testing        ===
--------------------------------

The JFR recording generation and analysis has been performed with the following JDK 11 and 17 implementations :
- OpenJDK
- Oracle JDK
- Azul Zulu JDK
- Amazon Correto JDK


--------------------------------
===    JFR Documentation     ===
--------------------------------

For more details about JFR, please see :
https://docs.oracle.com/javacomponents/doc/JDMUG/using-jdk-flight-recorder.htm

Source code (Java 11) :
https://github.com/AdoptOpenJDK/openjdk-jdk11/tree/master/src/jdk.jfr/share/classes/jdk/jfr