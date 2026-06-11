###### **Redis:**
redis is open source in-memory data structure store used as database,cache,message broker and streaming engine.

It is written in ANSI C and works on system like Linux, Mac-OS-X without external dependencies.

###### **Redis-Server:**
It is a process or daemon which runs in background and listens for client connections. It is responsible for storing and managing data in memory.

###### **Redis-CLI:**
It is interface used to interact with redis by sending prompts.

###### **Redis-Insight:**
It is graphical User interface used to work with redis deployment.
You can visually browse and interact with data.

###### **Redis cmd Syntax:**
cmd key value
if key includes more than one word then it should be separated by dot or colon "user:100:followers"

###### **Redis Docs:**
read docs for redis data types.
config commands

###### **Redis setup:**
vagrant init
vagrant up
vagrant ssh
sudo apt-get update
sudo apt-get install -y redis-server
sudo systemctl start redis-server
redis-cli

###### **1. What is Redis Pub/Sub?**
Redis Pub/Sub is a messaging technology that facilitates communication between different components in a distributed system \[00:09]. It follows a model where "Publishers" send messages without knowing who the "Subscribers" are, and "Subscribers" receive messages without knowing who the "Publishers" are \[01:24].

Asynchronous \& Scalable: It is an asynchronous and scalable messaging service that separates the producer of a message from the consumer who processes it \[01:36].

Decoupling: The system is decoupled; the sender (publisher) does not need to know the receiver (subscriber) and vice versa \[01:45].


###### **2. Key Components**

**Publisher**: An application or service that sends messages to a specific channel \[03:23].

**Subscriber**: An application or service that receives messages by subscribing to one or more channels \[03:31].

**Channel**: A named "topic" or medium where publishers send messages and subscribers listen for updates \[03:59].

**Message**: The actual data (payload) being transmitted from the publisher to the subscriber \[04:13].

**Broker**: Acts as a middleman that receives messages from publishers and distributes them to all active subscribers of a specific channel \[04:22].


###### **3. Communication Models**

The video explains that Redis Pub/Sub can be configured in several ways \[02:49]:

**One-to-One**: One publisher sending to one subscriber.

**One-to-Many**: One publisher sending to multiple subscribers (Broadcasting).

**Many-to-Many**: Multiple publishers sending messages to multiple subscribers.

**Many-to-One**: Multiple publishers sending messages to a single subscriber.



###### **4. Redis Pub/Sub Commands**

The video demonstrates several essential CLI commands for managing messaging:

**SUBSCRIBE \[channel\_name]**: Subscribes the client to the specified channel to start receiving messages \[04:46].

**PUBLISH \[channel\_name] \[message]**: Sends a message to the specified channel \[04:58].

**UNSUBSCRIBE \[channel\_name]**: Unsubscribes the client from the channel \[05:07].

**PSUBSCRIBE \[pattern]:** Subscribes to multiple channels matching a glob-style pattern (e.g., PSUBSCRIBE geek\* will subscribe to all channels starting with "geek") \[05:19].

**PUNSUBSCRIBE \[pattern]:** Unsubscribes from channels matching a specific pattern \[06:04].

**PUBSUB NUMSUB \[channel\_name]:** Returns the number of active subscribers for a specific channel \[15:48].

**PUBSUB CHANNELS**: Lists all currently active channels in the system \[16:03].



###### **5. Real-World Use Cases**

Redis Pub/Sub is ideal for scenarios requiring instant updates \[01:55]:

**Real-time Messaging**: Chat applications where users need instant message delivery.

**Alerts \& Notifications:** Sending system alerts or push notifications to users.

**IoT Devices:** Communication between different smart devices.

**Live Streaming \& Gaming**: Distributing live scores, status updates, or game state changes to multiple players simultaneously.



###### **6. What is Redis persistence?**
Redis Persistence is the mechanism of writing in-memory data to a durable storage disk (like an SSD) so that your data is not lost if the Redis server crashes, restarts, or experiences a power outage.

Important Points \& Options

The video highlights four primary persistence strategies:

No Persistence: Data is not saved to the disk. This is ideal if you are using Redis purely as a temporary cache, for short-lived data, or session storage \[00:54].

RDB (Redis Database): Takes point-in-time snapshots of your dataset at specific intervals and saves them as a binary .rdb file. It's great for backups and fast recovery but risks losing data written between the last snapshot and a server crash \[09:59].

AOF (Append Only File): Logs every single write operation (like SET) to an append-only file. It is highly durable and reconstructs the data by replaying the log upon startup \[16:57].

RDB + AOF: You can enable both simultaneously for a balance of robust backups and high data durability \[22:06].

Syntax \& Commands:-
Persistence is managed by modifying the redis.conf file.

Open Configuration File: sudo nano /etc/redis/redis.conf

Disable Persistence (No Persistence):
save "" (Pass an empty string to disable)

Configure RDB Snapshotting:
save <seconds> <minimum\_changed\_keys>
(Example: save 3600 1 means save a snapshot every 3600 seconds if at least 1 key is changed) \[11:06].

Configure AOF:
appendonly yes (Change the default no to yes) \[21:20].

Restart Redis Service: sudo systemctl restart redis-server

Step-by-Step Configuration Process
Access the Config File: Open your terminal and edit the Redis configuration file using sudo nano /etc/redis/redis.conf \[06:12].

Locate the Section: Use Ctrl + W to search the file for the SNAPSHOTTING section (for RDB rules) or APPEND ONLY MODE (for AOF rules) \[06:44].

Modify the Settings:
To use No Persistence: Comment out existing save intervals and add save "" \[06:55].

To use RDB: Set your desired save intervals based on your acceptable data loss window \[12:37].

To use AOF: Find the appendonly directive and change its value to yes \[22:21].

Save the File: Press Ctrl + X, then Y, and Enter to save and exit the editor.

Restart the Server: Apply the newly configured persistence rules by restarting the server with sudo systemctl restart redis-server \[07:06].


###### **7. What is Redis Replication(Master-Slave) ?**
Redis Replication (Master-Slave) is a setup where a "Master" Redis instance automatically copies its data to one or more "Slave" (or Replica) instances [00:12]. If the Master server fails, the replicas can still serve read requests to keep the application running, acting as exact copies of the master [00:59].

Important Points
Roles: The Master handles both read and write operations, whereas the Slaves are generally "Read-Only" [02:14].

High Availability: If the master crashes, applications can still read data from the slaves, preventing downtime and maintaining a good user experience [02:39].

Persistence is Crucial: It is highly recommended to enable persistence (RDB or AOF) on both the Master and Slaves [03:33]. If a Master has no persistence and restarts after crashing, it will wake up empty. It will then sync this "empty" state to the Slaves, deleting all their backup data [06:08].

Promoting a Slave: If a Master dies permanently, a Slave can be promoted to become the new Master using the REPLICAOF NO ONE command [36:25].

Syntax & Commands
REPLICAOF <master-ip> <master-port>: Executed on a slave instance to connect it to a master and start syncing data [26:04].

REPLICAOF NO ONE: Executed on a slave to disconnect it from the master and promote it to act as its own independent master [36:25].

INFO REPLICATION: A diagnostic command that shows the replication status, whether the instance is a master or a slave, and lists connected replicas [32:17].

Step-by-Step Configuration Process
The video demonstrates setting up one Master and two Slaves on a local machine using Windows Subsystem for Linux (WSL).

Step 1: Set Up Instances

Run three separate Redis instances (one for the Master, two for the Slaves) [18:20].

Assign different ports to them in their respective redis.conf files (e.g., 6379 for Master, 6380 for Slave 1, 6381 for Slave 2) [21:04].

Step 2: Configure the Master

Open the Master's redis.conf file.

Find the line bind 127.0.0.1 and comment it out (# bind 127.0.0.1) so it can accept external connections [24:21].

Set protected-mode no [24:26].

Restart the Master server [25:23].

Step 3: Configure the Slaves

Open the redis.conf file for each Slave.

Comment out the bind line and set protected-mode no, just like the Master [25:46].

Find the replicaof directive. Uncomment it and add the Master's IP and Port.

Example: replicaof 10.0.2.15 6379 [26:31].

Restart the Slave servers [26:57].

Step 4: Verify Replication

Connect to the Master using redis-cli -p 6379 and set a key (e.g., SET name Sonam) [28:59].

Connect to a Slave using redis-cli -p 6380 and fetch the key (GET name). The data should be there automatically [29:31].

Run INFO REPLICATION on the Master to see the connected slaves [32:17].


###### **8. What is Redis sentinel ?**
sentinel monitor mymaster 10.0.2.15 6379 2
    sentinel down-after-milliseconds mymaster 5000
    sentinel failover-timeout mymaster 10000
    sentinel parallel-syncs mymaster 1
    ```

### **Short Definition**
**Redis Sentinel** is a system designed to monitor your Redis Master and Slave instances and automatically handle "failovers." If your Master instance crashes or goes down, Sentinel will detect this, elect one of the connected Slaves to become the new Master, and automatically reconfigure the remaining Slaves to connect to the new Master [[00:11](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=11)]. This prevents your application from crashing by ensuring data is always accessible.

### **Important Points**
*   **Automation:** Sentinel replaces the need to manually promote a Slave to a Master when the original Master fails, which is not practical in a real-world production environment [[00:59](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=59)].
*   **Key Features:** Sentinel provides monitoring, notifications, automatic failover, and acts as a configuration provider [[02:15](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=135)].
*   **Quorum:** This is the number of Sentinel instances that must agree that a Master is "down" before a failover process begins. A typical minimum setup requires 3 Sentinels with a quorum of 2 [[04:42](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=282)].
*   **Subjectively Down (sdown) vs. Objectively Down (odown):** 
    *   *sdown:* A single Sentinel believes the Master is down because it's not responding.
    *   *odown:* Multiple Sentinels (meeting the quorum) agree the Master is down, triggering the failover.

### **Syntax & Commands**
To configure Sentinel, you need a `sentinel.conf` file. Here are the key directives:
*   **`sentinel monitor <master-name> <ip> <port> <quorum>`**
    *(Example: `sentinel monitor mymaster 127.0.0.1 6379 2`)* - Tells Sentinel to monitor the master at the given IP and port, requiring 2 Sentinels to agree on a failure [[03:54](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=234)].
*   **`sentinel down-after-milliseconds <master-name> <milliseconds>`** - The time in milliseconds an instance must be unreachable before it's considered down [[04:22](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=262)].
*   **`sentinel failover-timeout <master-name> <milliseconds>`** - How long to wait before trying another failover if the first one fails [[04:31](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=271)].
*   **`sentinel parallel-syncs <master-name> <number>`** - How many replicas can be reconfigured to use the new master simultaneously [[04:34](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=274)].

**Running Sentinel:**
*   `redis-server /etc/redis/sentinel.conf --sentinel` - Command to start a Sentinel instance using its config file [[07:08](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=428)].

### **Step-by-Step Configuration Process**
The video demonstrates setting up Sentinel on a system with 1 Master and 2 Slaves.

1.  **Create the Config File:** On each server (Master and both Slaves), create a new file named `sentinel.conf` (e.g., `nano /etc/redis/sentinel.conf`) [[09:56](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=596)].
2.  **Add Configuration:** Add the basic monitoring directives to the file. Ensure the `<ip>` and `<port>` match your initial Master instance.
    ```
    sentinel monitor mymaster 10.0.2.15 6379 2
    sentinel down-after-milliseconds mymaster 5000
    sentinel failover-timeout mymaster 10000
    sentinel parallel-syncs mymaster 1
    ```
    *(The video uses a 5-second down-after threshold for demonstration purposes)* [[10:06](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=606)].
3.  **Start Redis Instances:** Ensure your Master and Slave Redis instances are running normally using `service redis-server start` [[11:41](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=701)].
4.  **Start Sentinel:** On all three servers, start the Sentinel process using the command: `redis-server /etc/redis/sentinel.conf --sentinel` [[12:14](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=734)].
5.  **Test the Failover:** Stop the Master server manually (e.g., `service redis-server stop` or `Ctrl+C`). The running Sentinel terminals will log the failure (`+sdown`, then `+odown`) and begin the election process to promote one of the Slaves to the new Master [[16:03](https://www.youtube.com/watch?v=WHH1XMxoFQY&t=963)].
6.  **Verify the New Setup:** Log into the newly promoted Master and check `INFO REPLICATION`. It will show its role as `master` and list the remaining slave as connected 

