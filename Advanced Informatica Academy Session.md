 #Day 1#
* 'Repository Service' helps the repository to interact with the other Client Components (Storing to repository and send the information to Other Client Components). This helps in Saving the Program
* 'Integration Service' helps in execution of ETL. THis helps n Executing the Program.
* The Client tools will interact with Server Components (repository/Integration Services and Repository) Using TCP/IP Protocol. Check what TCP/IP Protocol means.
* One Installation on Server is called NODE. To Improve Availability, We can install Informatica on Multiple Nodes. All the Nodes will Access the SAME Repository. DOMAIN is a Collection of NODES.
* GATEWAY NODE is UNIQUE in a DOMAIN. When ever a request comes to Informatica DOMAIN, it will first go to GATEWAY NODE. This Node will share the LOAD with other WORKER NODES based on Load.
* Changes from 8x to 9x: 1)Components packaged together 2)LKP Now can take All Matches and Increase the Output Rows (Earlier it was like SSIS (picks first (Or Last Match if configured))) 3) Dynamic Lookup Cache 4) Session Log Rotation (so No need to handle Log Clean-up now).
* Partitioning : Executing the JOB in Parallel (Dont get confused with DB Partitioning, its totally Different) partitions. Whenever we run a Mapping, Three Threads will be created (Source, Transformation, Target) one Source-Transformation-Target threads combination is called a Partition. When load is more. Example, 10 Million rows, instead of using 1 partition we can configure Infromatica to use Five partitions of 2 million records each, to run in parallel and produce better performance. 'Partition' Option will be available in MAPPING Tab of Session Properties. We can also give Queries to Partitions so that those partitions will pick only data filtered through that query. Dis advantage of partitions in that each partition will create a new Thread on the NODE and new threads can cause a bottle neck since a NODE can run only N number of threads at one point in time. 
* Partition Types are : Pass through, Key Partition and Database Partitioning. 'Pass through' is default (write SQL Quwery to deternime what data goes to that partition). 'Key partition' will allow us to specify the range that goes into a partition using some key value. 'Database Partitioning' Option makes use of Partition available on DB Table and proceed accordingly. In This case, we need the create N number of partitions in Mapping Properties, if there are N number of partitions on DB table. If partitions on DB table are more, informatica will assign one of the existing partition to run the extra partition (sharing). If partitions on the Informatica Mapping are more, then it ignores the Extra partitions and does not use them. Database Partioning on TARGET  is not supported if Target is in ORACLE but it is supported if Target is in DB2. So, we should check compatibility before choosing Database Partitioning option. 
* 'Round Robin'  and 'Hash key' partitions. Check these in google/informatica HELP. Also, DYNAMIC partitioning options are available on 'Config Object' tab of Session Properties.
* Versioning should be configured while configuring the Repository. Versioning is Linked to Repository.
* Deployment Options (1. Import/Export 2. Folder Copy 3. Deployment Group). 1: Export from a Source Repository and Import to a Target Repository. This is useful if you are moving only one component of your project to QA 2: Copy folder from one place to other? 3: Create a Group in 'Deployment Group' Folder of Repository and just Drag and Drop your code to the Group. Deployment Group Creation can be done in 2 ways Static and Dynamic. Static will need us to Drag and Drop code to the Depoyment group folder. Dynamic means, we need to write a query and based on that query , the code will automatically mode to the Created Deployment Group.
* Things to consider during Migration (1: Shell Scripts, 2: Connections, 3: DML and DDL)
* In Lookup Transformation, we can choose the Option 'Enable Caching' so that the data will be cached to a file on the Informatic Server (Similar to CAW files in SSIS). This is useful when the Lookup is HUGE. 'Dynamic Lookup' option, if enabled, will generate a 'New Lookup' Port. Based on this New Lookup Port, Cache will be updated automaticaly. Also Check about Index Cache and Data Cache in google/Informatica Help. 'Persistent Cache' option is used if we want to use the cache somewhere else. 
* Push Down Optimization: Under Mapping Table (in Session Properties) and then Transormations Tab (Left Bottom). check what this does in google/Informatica Help. The concept tis to generate SQL query instead of using Transforamtions which are costly. If you properly design the Query in the SQ Analyzer, Push down optimization is not needed? To Enable Pushdown Optimization , we need to check the option under Properties tab (2nd one from left) of session properties. Pushdown optimization cannot be done when we use Lookup Cache, Parameter or Variables etc since those things cannot be converted to a SQL query. 

#Day2#
* To Installed Informatica, Create a New DB, SCHEMA for repository.
* Next we need to create a domain then Node  and Repository Service (We have an option to Link to the DB schema created in above step)
* Next we need to create Integration Service, While creating, we need to Associate it with a Repository Service. 
* After Installation, We need to go to Designer and add Repository. Then go to Repository and Add a Folder. then that folder will show up in the Designer. You can start working in that folder. 
* User/Role Creation in Informatica. We can Create a user and can assign him access to different componets in informatica. 
* Dynamic Aggregation: Agggregator Transform will have Dynamic Aggregation option where in, instead of aggregating All details of employee at one shot (THis will be a Blocking Transformation), it will pick the records as it comes, pass on the value to index (based on group By columns) and Data cache and will add the value to the target based on the index. If a new record comes with the same index again it will insert into data cache for that index and go to the target and add the value to the existing value. This was Dynamic Aggregation will help in 1) do incremental loads instead of selecting * from source and 2) Make Aggregation a non-Blocking transformation (check this i am not sure about the second point). (We need to Select incremental Aggregation option in session properties for this to work) Check what is 'Re-initialize aggregation cache' option will remove the values in the cache before the session runs. If you use re-initialize option, and also use 'Dynamic aggregation', it will cause duplication of data when the session runs twice. This is because, since cache will be wiped out, it will consider records as INSERT records. 
* Disadvantages of Dynamic Aggregation : 1) we cannot use Dynamic lookup 2) we cannot sort data it will throw errors  3) If server has bottle necks, it is preferable to use Look Up and Insert or Update accordingly instead of using dynamic aggregation . Doing this will increase the time taken to run the job but will relieve the server from excess cache that has to be maintained.
* Variables and Parameters : Variables will have MAX() MIN ()option and will be dynamically varied as dataflow progresses. But the Value that comes in the END will be pushed to the Repository. This Value will be picked from repository again when session starts and this will be varied dynamically modified again in the Session. We cannot pass a variable value between sessions. We need to pass from session to workflow and then from workflow to another session.
* Variables have 3 values (initial, Persisted Rep Value, Current Value) Persited Rep Value is nothing but value stored int he Repository as explained in the previous point. Initial is the value that you manually give when you create the variable. Current Value is the dynamic value that variable holds at runtime. 
* Parameter is Constant ?? My understanding is Variables is dynamic and chages for different rows in a data stream. this is not possible with parameter
* Error Handling: Ootions are available in 'Config Object' tab of session properties. We can push errors to a file or DB table. By default informatica sends bad records to bad file?
* Normalizer Transformation is like PIVOT UN-PIVOT in SSIS. Only Un-pivot? or Pivot as well? GK and GKID will be automatically populated in normalizer. We can use these in filter later if we want. GK is nothing but unique value for each key. GKID is like an Identity in SQL Server? Check for 'LEVEL' and 'Occurence' options . Especially LEVEL, it does more that what SSIS Unpivot/pivot can do. 
* Performance Tuning: Check WorkLog to See where the problem is (Source, Transformation, Target). if problem is in Sorce, We can see if we can do any indexing/Materialized views etc to improve performance. If Target is problem choose the appropriate load type: BULK load, Conventional Path Load (pushes data to buffer cache and then to target ), Direct Path Load (Pushes data directly to target).
* EXTERNAL LOADER: SQL Loader Utility. COntrol File and Data File. Control File has metadata of source file. Data File will have data. It is like a Direct Path Load which by passes the Caches. Using this Utility the loading will be fast. This Can be seen Designer-> Connections -> Loader. Check if it is present for SQL Server. Once we create a connection, we can link this to Mapping target.
* When we execute a Workflow, First the integration Service will Apply a LOCK on Work FLow so that the same work flow cannot be run twice at the same time. Then it reads the Parameter files. THen Creates WorkFlow logs. THen runs the tasks in workflow, Then START DTM (Data Transformation Manager), then update repository with historic info then send any email tasks etc if configured. 
* LOAD BALANCER: Will manage Workflow Loads. This will manage Loads across Nodes. 
* DTM (Data Transformation Manager) : Reading Session info, push down, Partitions, Expand variables and Parameters, Creating Session logs, Connection object validation , pre-session actions, post Session actions etc. This is related to Session. 

--End of Session