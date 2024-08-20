# Building-Your-Own-Database-Agent
Microsoft
[Complete](https://learn.deeplearning.ai/accomplishments/26690cc8-d8fd-40e6-b664-a4950cd35a37?usp=sharing)

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQsHK6PqKMFbGDD25jqps5OI-lnx1YAWtn7bds2iLRKYGcWf0qOcpz5kynR&s=10)

When an application accesses a database, several processes or threads begin to perform the various application tasks. These tasks include logging, communication, and prefetching. Database agents are threads within the database manager that are used to service application requests. In Version 9.5, agents are run as threads on all platforms.

The maximum number of application connections is controlled by the max_connections database manager configuration parameter. The work of each application connection is coordinated by a single worker agent. A worker agent carries out application requests but has no permanent attachment to any particular application. Coordinator agents exhibit the longest association with an application, because they remain attached to it until the application disconnects. The only exception to this rule occurs when the engine concentrator is enabled, in which case a coordinator agent can terminate that association at transaction boundaries (COMMIT or ROLLBACK).

There are three types of worker agents:

- Idle agents
  
This is the simplest form of worker agent. It does not have an outbound connection, and it does not have a local database connection or an instance attachment.

- Active coordinator agents
  
Each database connection from a client application has a single active agent that coordinates its work on the database. After the coordinator agent is created, it performs all database requests on behalf of its application, and communicates to other agents using interprocess communication (IPC) or remote communication protocols. Each agent operates with its own private memory and shares database manager and database global resources, such as the buffer pool, with other agents. When a transaction completes, the active coordinator agent might become an inactive agent. When a client disconnects from a database or detaches from an instance, its coordinator agent will be:

-  An active coordinator agent if other connections are waiting
  
-  Freed and marked as idle if no connections are waiting, and the maximum number of pool agents is being automatically managed or has not been reached
  
-  Terminated and its storage freed if no connections are waiting, and the maximum number of pool agents has been reached
  
- Subagents
  
The coordinator agent distributes database requests to subagents, and these subagents perform the requests for the application. After the coordinator agent is created, it handles all database requests on behalf of its application by coordinating the subagents that perform requests against the database. In Db2® Version 9.5, subagents can also exist in nonpartitioned environments and in environments where intraquery parallelism is not enabled.

Agents that are not performing work for any application and that are waiting to be assigned are considered to be idle agents and reside in an agent pool. These agents are available for requests from coordinator agents operating on behalf of client programs, or for subagents operating on behalf of existing coordinator agents. The number of available agents depends on the value of the num_poolagents database manager configuration parameter.

If no idle agents exist when an agent is required, a new agent is created dynamically. Because creating a new agent requires a certain amount of overhead, CONNECT and ATTACH performance is better if an idle agent can be activated for a client.

When a subagent is performing work for an application, it is associated with that application. After it completes the assigned work, it can be placed in the agent pool, but it remains associated with the original application. When the application requests additional work, the database manager first checks the idle pool for associated agents before it creates a new agent.

- [**Database agent management**](https://www.ibm.com/docs/en/SSEPGG_11.5.0/com.ibm.db2.luw.admin.perf.doc/doc/c0005410.html)
  
Most applications establish a one-to-one relationship between the number of connected applications and the number of application requests that can be processed by the database server. Your environment, however, might require a many-to-one relationship between the number of connected applications and the number of application requests that can be processed.

- [**Client-server processing model**](https://www.ibm.com/docs/en/SSEPGG_11.5.0/com.ibm.db2.luw.admin.perf.doc/doc/c0005427.html)
  
Both local and remote application processes can work with the same database. A remote application is one that initiates a database action from a machine that is remote from the machine on which the database server resides. Local applications are directly attached to the database at the server machine.

- [**Connection-concentrator improvements for client connections**](https://www.ibm.com/docs/en/SSEPGG_11.5.0/com.ibm.db2.luw.admin.perf.doc/doc/c0007869.html)
  
The connection concentrator improves the performance of applications that have frequent but relatively transient connections by enabling many concurrent client connections to be processed efficiently. It also reduces memory use during each connection and decreases the number of context switches.

- [**Agents in a partitioned database**](https://www.ibm.com/docs/en/SSEPGG_11.5.0/com.ibm.db2.luw.admin.perf.doc/doc/c0005411.html)
  
In a partitioned database environment, or an environment in which intrapartition parallelism has been enabled, each database partition has its own pool of agents from which subagents are drawn.
Parent topic:
