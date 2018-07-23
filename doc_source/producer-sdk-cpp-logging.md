# Using Logging with the C\+\+ Producer SDK<a name="producer-sdk-cpp-logging"></a>

You configure logging for C\+\+ Producer SDK applications in the `kvs_log_configuration` file in the `kinesis-video-native-build` folder\.

The following example shows the first line of the default configuration file, which configures the application to write `DEBUG`\-level log entries to the AWS Management Console:

```
log4cplus.rootLogger=DEBUG, KvsConsoleAppender
```

You can set the logging level to `INFO` for less verbose logging\.

To configure the application to also write log entries to a log file, update the first line in the file to the following:

```
log4cplus.rootLogger=DEBUG, KvsConsoleAppender, KvsFileAppender
```

This configures the application to write log entries to `kvs.log` in the `kinesis-video-native-build/log` folder\.

To change the log file location, update the following line with the new path:

```
log4cplus.appender.KvsFileAppender.File=./log/kvs.log
```

**Note**  
If `DEBUG`\-level logging is written to a file, the log file can use up the available storage space on the device quickly\.