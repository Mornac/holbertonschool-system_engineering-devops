# Distributed web infrastructure
## 1. Three server web infrastructure
A user wants to access the website www.foobar.com from the web browser :
- Use a domain name : foobar.com
- Configure with : www
- Server IP: 8.8.8.8
- 1 load-balancer (HAproxy)
- 3 web servers containing: 
    - 1 web server (Nginx)
    - 1 application server
    - 1 set of application files (code base)
- Separate database cluster:
    - 1 MySQL Primary database
    - 1 MySQL Replica database

![Design of the three server web Infrastructure](https://raw.githubusercontent.com/Mornac/holbertonschool-system_engineering-devops/main/web_infrastructure_design/images/Task1.jpg)

## 2. Specifics about the infrastructure
### For every additional element, why added it?  
- Load Balancer (HAproxy):
    - Eliminates single point of failure for web servers  
    - Distributes traffic accross multiple servers  
    - Improves performance by balancing load  
    - Enables high availability: if a server fails, traffics goes to the other  
    - Provides health checks to monitor server status  

- 2 more servers:
    - Redundancy: if one of the three servers fails, the other two continue serving
    - Load distribution: traffic spread across 3 servers instead of 1
    - Better performance
    - Maintenance capability: can update one server while others serve traffic

- Separate database cluster:
    - Centralized data management: All web servers connect to the same database cluster
    - Better resource allocation: Database resources dedicated to data operations
    - Independent scaling: Can scale web tier and database tier separately
    - Simplified data consistency: Single source of truth

- Database replica:
    - Data redundancy: data exists on multiple servers
    - Read performance: ditribute read queries across databases
    - Backup availability: replica can be promoted if the primary fails
    - Data consistency across the infrastructure

### What distribution algorithm your load balancer is configured with and how it works?
- Distribution Algorithm: Round Robin
- The working:
    - 1st request->Server 1
    - 2nd request->Server 2
    - 3rd request->Server 3
    - 4th request->Server 1
    - and so on ...

### Is your load-balancer enabling an Active-Active or Active-Passive setup? Explain the difference between both.
- Our setup: Active-Active
    - All 3 servers actively serve traffic simulaneously
    - Load balancer distrubutes requests between all servers
    - Better resource utilization: every server works
    - Higher performance: combined capacity of all servers

- Active-Passive setup:
    - Only one server serves traffic(active)
    - Other servers stays passive/standby
    - Passive servers only activate if active server fails
    - Wastes resources: passive servers sits idle (don't product any usefull work)
    - Lower performance: only one server handles load

### How a database Primary-Replica (Master-Slave) cluster works?
- Write operations (INSERT, UPDATE, DELETE) go to Primary database
- Primary database replicates changes to Replica database
- READ operations can be distributed between Primary and Replica
- Replica stays synchronized with Primary throuh replication log
- All 3 web servers connect to this database cluster

### What is the difference between the Primary node and the Replica node in regard to the application?
|Primary Node|Replica Node|
|------------|------------|
|Handles ALL write operations|Read-only: no direct writes|
|Source of truth for data|Copy of Primary's data|
|Can handle read operations|Optimized for read operations|
|If fails, Replica can be promoted|Can be promoted to Primary if needed|
|Connected to all 3 web servers|Connected to all 3 web servers|

## 3. The issues with the infrastructure
### Where are SPOF?
1. Load Balancer(HAProxy)  

|Condition|Solution|
|---------|--------|
|HAProxy fails, entire website becomes unavailable|Add second load balancer with virtual IP failover|

 2. Primary Database  

|Condition|Solution|
|---------|--------|
|Primary database fails, no write operations possible|Automatic failover to promote Replica to Primary|

3. Database Cluster Connection  

|Condition|Solution|
|---------|--------|
|Connection to database cluster fails, all web servers affected|Multiple network paths to database cluster|

4. Network Connection  

|Condition|Solution|
|---------|--------|
|Single internet connection point|Multiple internet service providers|

### Security issues (no firewall, no HTTPS)
- No firewall
    - Servers exposed to direct attacks
    - All ports potentially accessible
    - Database cluster directly accessible from internet  

  Solution: Implement firewall rules, allow onlu necessary ports

- No HTTPS
    - Data transmitted in plain text
    - Vulnerable to man-in-the-middle-attacks
    - User credientials and sensitive data exposed
    
  Solution: Implement SSL/TLS certificates, force HTTPS

### No monitoring
|No visibility|
|-------------|
|Cannot detect performance issues|
|No alerting when servers fail|
|No capacity planning data|
|Cannot monitor database cluster health|

|Problems|
|--------|
|Server failures go unnoticed until users complain|
|Performance degradation not detected early|
|Resource exhaustion not predicted|
|Security breaches not detected|
|Database replication lag not monitored|

|Solutions|
|---------|
|Implement monitoring tools (Nagios, Zabbix, Prometheus)|
|Set up alerting for critical metrics|
|Log aggregation and analysis|
|Performance metrics collection|
|Database cluster monitoring and replication status tracking|  
