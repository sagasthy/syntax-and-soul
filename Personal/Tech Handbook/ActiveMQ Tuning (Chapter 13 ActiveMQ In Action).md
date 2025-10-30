Use connection parameter trace=true to print messages on the broker instance, helpful for debugging.

# Other References

[https://www.javacodegeeks.com/2015/03/speeding-up-activemq-persistent-messaging-performance-by-25x.html](https://www.javacodegeeks.com/2015/03/speeding-up-activemq-persistent-messaging-performance-by-25x.html)

# Producer side optimization

1. **tcpNoDelayEnabled**=true (default is false)
Provides a hint to the peer transport to enable/disable tcpNoDelay. If this is set, it may improve performance where you’re sending lots of small messages across a relatively slow network.
2. cacheEnabled=false (default is true)
Commonly repeated values (like producerId and destination) are cached, enabling short keys to be passed instead. This decreases message size, which can have a positive impact on performance where network performance is relatively poor. The cache lookup involved does add overhead to CPU load on both the clients and the broker machines, so take this into account.
3. tightEncodingEnabled=false (default is true)
A CPU-intensive way to compact messages. We recommend disabling this if the broker starts to consume all the available CPU.
4. Consider persistent vs non-persistent messages
5. send messages async and in batches if persistent messaging is required

**Note:** We may not need option 3 in our case

# Consumer side optimization

1. **Setting the optimizeAcknowledge property**
When using optimizeAcknowledge or the DUPS_OK_ACKNOWLEDGE acknowledgment mode on a session, the message consumer can send one message acknowledgment to the ActiveMQ message broker containing a range of all the messages consumed. This reduces the amount of work the message consumer has to do, enabling it to consume messages at a much faster rate.

```java
ActiveMQConnectionFactory cf = new ActiveMQConnectionFactory();
cf.setOptimizeAcknowledge(true);
```

2. **Disabling the alwaysSessionAsync property
Disabling asynchronous dispatch allows messages to be pass the internal queueing and dispatching done by the session**

```java
ActiveMQConnectionFactory cf = new ActiveMQConnectionFactory();
cf.setAlwaysSessionAsync(false);
```
 
# Other considerations

producer flow control, set using sendFailIfNoSpace (throws exception immediately) or sendFailIfNoSpaceAfterTimeout (throws exception after timeout), client needs to catch exception and wait before sending more messages

```xml
<systemUsage>
	<systemUsage sendFailIfNoSpace="true">
		<memoryUsage>
			<memoryUsage limit="128 mb"/>
		</memoryUsage>
	</systemUsage>
</systemUsage>
```

```xml
<systemUsage>
	<systemUsage sendFailIfNoSpaceAfterTimeout="5000">
		<memoryUsage>
			<memoryUsage limit="128 mb"/>
		</memoryUsage>
	</systemUsage>
</systemUsage>
```