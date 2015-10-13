# MDC context-propagation
Spring-integration MDC context-propagation proof of concept

## manual testing
Using (httpie)[httpie.org] for examples.


### Using only MDC propagation.
2 calls `http :8383/async/message1 -a user1:pass` to fill thread pool MDC's, and than 1 call to 
`http :8383/async/message2 -a user2:pass` to show incorrect username in log.  

```
2015-10-13 22:06:04.068  INFO 11964 --- [asyncExecutor-2] org.golonzovsky.MessageLoggingService    : [user1] name from context = 'null', message = 'test1'
2015-10-13 22:06:04.070  INFO 11964 --- [asyncExecutor-3] org.golonzovsky.MessageLoggingService    : [user1] name from context = 'null', message = 'test2'
2015-10-13 22:06:04.074  INFO 11964 --- [asyncExecutor-1] org.golonzovsky.MessageLoggingService    : [user1] name from context = 'null', message = 'message1'
2015-10-13 22:06:05.618  INFO 11964 --- [asyncExecutor-4] org.golonzovsky.MessageLoggingService    : [user1] name from context = 'null', message = 'message1'
2015-10-13 22:06:05.620  INFO 11964 --- [asyncExecutor-5] org.golonzovsky.MessageLoggingService    : [user1] name from context = 'null', message = 'test1'
2015-10-13 22:06:05.620  INFO 11964 --- [asyncExecutor-2] org.golonzovsky.MessageLoggingService    : [user1] name from context = 'null', message = 'test2'
2015-10-13 22:06:36.969  INFO 11964 --- [asyncExecutor-1] org.golonzovsky.MessageLoggingService    : [user1] name from context = 'null', message = 'test1'
2015-10-13 22:06:36.969  INFO 11964 --- [asyncExecutor-3] org.golonzovsky.MessageLoggingService    : [user1] name from context = 'null', message = 'message2'
2015-10-13 22:06:36.969  INFO 11964 --- [asyncExecutor-4] org.golonzovsky.MessageLoggingService    : [user1] name from context = 'null', message = 'test2'
```

*Result:* all messages have `user1` in log entries
*Expected result:* third call (last 3 entries) should have `user2`