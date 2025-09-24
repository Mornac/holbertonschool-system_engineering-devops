# Secured and monitored web infrastructure
## 1. Three server web infrastructure, secured, serve encrypted traffic and be monitored
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
- 3 firewalls
-1 SSL certificate to serve www.foobar.com over HTTPS
-3 monitoring clients: 
    - 1 Monitoring client installed on Server1
    - 1 Monitoring client installed on Server2
    - 1 Monitoring client installed on Server3


![Design of the Secured and monitored web infrastructure](https://raw.githubusercontent.com/Mornac/holbertonschool-system_engineering-devops/main/web_infrastructure_design/images/Task2.jpg)

## 2. Specifics about the infrastruture
### For every additional element, why added it?  
- 3 firewalls:
    - Perimeter Firewall: 1st line of defense against internet attacks
    - Web Tier Firewall: protects web servers from unauthorized access
    - Database Firewall: isolates database from direct external access
    - Defense in depth: multiple security layers reduce risk
    - Network segmentation: each tier has appropriate security controls

- 1 SSL certificate to serve www.foobar.com over HTTPS:
    - Data encryption: protect sensitive user data during transmission
    - Authentication: verify server indentity to users
    - Trust building: display security indicators in browser
    - SEO benefits: search engines favor HTTPS websites
    - Regulatory compliance: many standards require encrypted connections

- 3 monitoring clients:
    - Comprehensive visibility: monitor all web servers sumultaneously
    - Proactive issue detection: identify problems before users are affected
    - Performance tracking: collect metrics from each server individually
    - Load balancing insights: understand how traffic is distrubuted
    - Alerting capability: notify administrators of critical issues

### What are firewalls for?
- Traffic filtering
- Access control
- Attack prevention
- Network isolation
- Logging and auditing


### Why is the traffic served over HTTPS?
|Security Benefits|Business Benefits|
|-----------------|-----------------|
|Encryption in transit|User trust|
|Data integrity|SEO ranking|
|Server authentication|Comliance requirements|
|Protection against eavesdropping|Modern web standards|


### What monitoring is used for?
|Proactive Management|Reactive Management|
|--------------------|-------------------|
|Performance optimization|incident response|
|Capacity planning|Troubleshooting|
|Early problem detection|Security monitoring|
|SLA monitoring|Audit trail|


### How the monitoring tool is collecting data?
1. Agent-based collection  
- Lightweight agents installed on each web server
- Real-time metrics: CPU usage, memory, disk I/O, network traffic
- Application metrics: response times, error rates, request counts
- System logs: error logs, access logs, security events  

2. Push or Pull Models
- Push Model: agents actively send data to monitoring service
- Pull Model: monitoring service requests data from agents
- Buffering: agents store data locally if connection to service fails  

3. Data types collected
- Infrastructure metrics: server hardware performance
- Application metrics: web server statistics, database queries
- Business metrics: user sessions, transactions, revenue impact
- Security events: failed login attempts, unusual access paterns

### what to do if you want to monitor your web server QPS (Queries Per Second)?
We can use:
- Web Server Logs
- System tools
- Web Server Status Modules
- Monitoring tools

## 3. The issues with the infrastructure
### Why terminating SSL at the load balancer level is an issue?
- Internal network vulnerabilities: if someone gains access to internal network, they can intercept data
- Insider threats: users can capture sensitive data
- Network sniffing: internal traffic can be monitored and analyzed
- Regulatory non-compliance: industries like healthcare and finance may require full encryption

### Why having only one MySQL server capable of accepting writes is an issue?
- Performance ceilling: write performance cannot scale horizontally
- Availability risk: total write capability depends on one server
- Maintenance downtime: updates to Primary database affect all write operations
- Disaster recovery complexity: failover procedures are complex and error-prone

### Why having servers with all the same components (database, web server and application server) might be a problem?
- Resource waste: web server needs CPU while database needs memory, leading to suboptimal resource allocation
- Scaling inflexibility: adding web capacity also adds unnecessary application server capacity
- Technology constraints: all components must use compatible technologies and versions
- Maintenance complexity: updates affect multiple services, increasing downtime risk
- Blast radius: problem in on component can affect all others on the same server
