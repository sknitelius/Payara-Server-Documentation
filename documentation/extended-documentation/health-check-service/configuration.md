_Payara Server and Micro 161 (4.1.1.162) onwards_

Payara provides services for monitoring the health of the server by collecting information on CPU Usage, Machine Memory and Heap Memory Usage and Garbage Collector Execution Times. The Health Check Services can be configured through the domain.xml as given follows:
```xml
<health-check-service-configuration enabled="true">
    <cpu-usage-checker unit="SECONDS" name="CPU" time="5" enabled="true"></cpu-usage-checker>
    <garbage-collector-checker unit="SECONDS" name="GC" time="5" enabled="true"></garbage-collector-checker>
    <machine-memory-usage-checker enabled="true" unit="SECONDS" name="MMEM" time="5">
        <property name="threshold-critical" value="90"></property>
        <property name="threshold-warning" value="70"></property>
        <property name="threshold-good" value="0"></property>
    </machine-memory-usage-checker>
    <heap-memory-usage-checker unit="SECONDS" name="HEAP" time="5" enabled="true"></heap-memory-usage-checker>
</health-check-service-configuration>
```
The main configuration tag is the `<health-check-service-configuration>` and it can be placed under `<config name="server-config">` tag. It has only one attribute named `enabled`, which can be set to either `true|false` and this enables or disables the whole health check monitoring.

## The List of Available Checkers

`<health-check-service-configuration>` can contain a variety of checker tags from the list given as follows. All of these checker configurations do specific monitoring on determined resources like CPU, GC and etc.
* `<cpu-usage-checker>`: Calculates the CPU usage and prints out the percentage along with the usage time;
* `<garbage-collector-checker>`: Calculates and prints out how many times GC is executed with its elapsed time.
* `<machine-memory-usage-checker>`: Calculates the machine memory usage and prints out the percentage along with the total and used physical memory size.
* `<heap-memory-usage-checker>`: Calculates the heap memory usage and prints out the percentage along with initial and committed heap sizes.
* `<hogging-threads-checker>`: Identifies the threads that stuck on CPU-wise.

They can all have the base attributes from the list given as follows.
* __enabled__:Enables/Disables the specified checker
* __name__: Name of the checker that will be printed in the log messages for tracing
* __unit__: The time unit value, which could either be: `NANOSECONDS`, `MICROSECONDS`, `MILLISECONDS`, `SECONDS`, `HOURS`, `DAYS`. 
* __time__: The time interval specified in given unit to execute the checker

## Threshold configurations

There are threshold configurations for some of the checkers and checker tags that support this configuration are listed below.
* `<cpu-usage-checker>`
* `<machine-memory-usage-checker>`
* `<heap-memory-usage-checker>`

The threshold configurations can be specified in 3 different levels: _critical_, _warning_ and _good_. By default their values are __80__, __50__ and __0__ respectively. A sample configuration for the cpu-usage-checker is given as follows:
```xml
<cpu-usage-checker enabled="true" unit="SECONDS" name="CPU" time="3">
    <property name="threshold-critical" value="90"></property>
    <property name="threshold-warning" value="70"></property>
    <property name="threshold-good" value="0"></property>
</cpu-usage-checker>
```
## Checkers with Customised Configuration

####`<hogging-threads-checker>`
Hogging Threads Checker offers 2 properties for configuration. They are as follows:
* `threshold-percentage`: Defines the minimum percentage needed to count the thread is hogged CPU-wise. The percentage is calculated with the ratio of elapsed CPU time to checker execution interval. It default value is 95.
* `retry-count`: Represents the count value that should be reached by the hogged thread in order to give health check messages to the user. Its default value is 3.