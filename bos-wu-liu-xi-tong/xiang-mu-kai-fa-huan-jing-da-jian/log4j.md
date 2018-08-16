# Log4j

log4j is used to store and control the console information like error information.

We can output these console information to a log file and store in our computer.

And we can also control the error information not appear on console screen.

## log4j configuration file:

{% code-tabs %}
{% code-tabs-item title="log4j.properties" %}
```text
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=d:\\mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### set log levels - for more verbose logging change 'info' to 'debug' ###
### fatal error warn info debug trace
log4j.rootLogger=debug, stdout


```
{% endcode-tabs-item %}
{% endcode-tabs %}



