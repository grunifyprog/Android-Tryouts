# Broadcast Receivers

A Broadcast Receiver is a component that can respond to a broadcast message from a client.

> Broadcast Receiver

A Broadcast Receiver inherits from BroadcastReceiver of the Android SDK and registers itself in the 
manifest file through a *receiver* tag. A *receiver* tag is way that a component (a Broadcast Receiver) advertises to the Android system that it can handle messages of a certain type.

A message can be responded by more than one Broadcast Receiver if they register to respond to messages of that type.

> Client

A client component such as an Activity or a Service uses *sendBroadcast(intent)* method of the Context class 
to send a broadcast message.

### Execution

The Broadcast Receivers that belong to a process are executed in the main thread. 

Unlike an Activity that gets 5 seconds to run on the main thread, a Broadcast Receiver gets 10 seconds before getting an ANR.

A client calls sendBroadcast() to broadcast a message which is enqueued in a queue. The messages in the queue are processed by one or more registered Broadcast Receivers that run on the main thread.

If more than one Broadcast Receiver responds to a message, the order of execution of the Broadcast Receivers (i.e. their onReceive() methods) is not certain. 

If the process ONLY runs a Broadcast receiver and no other component, then the process will start and terminate along with the Broadcast Receiver. Unlike a service process, a process that runs a Broadcast Receiver will not be restarted. If the Broadcast Receiver were to spawn threads, they would be abruptly terminated after the receiver returns back to the main thread. 

Android acquires a partial wake lock (a way in the SDK to keep the device from going to sleep or wake up if sleeping) when invoking a Broadcast Receiver and releases it when it returns to the main thread.

### Execution of an "outside" Broadcast Receiver

Consider the following scenario:

Application A has a Broadcast Receiver that can respond to a message of Type MSG.

Application B has a Broadcast Receiver that can respond to the same message type.

A Client in Application B broadcasts a message of type MSG. 

Application B's Broadcast Receiver runs in Application B's main thread, where as Application A's Broadcast Receiver runs in its own main thread. 


### Flag to start an Application in "stopped" state

After API 12 (Android 3.1), the SDK introduced a launch model for security concerns. In this model, an application when installed will be in a "stopped" state. 

The system adds a flag to an intent by default to exclude applications that are in stopped state. However, a client can explicitly set a flag on the message i.e. the broadcast intent to include these "stopped" applications. 
