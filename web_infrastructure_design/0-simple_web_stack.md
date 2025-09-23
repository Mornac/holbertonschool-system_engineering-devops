# Simple web stack  
## 1. Web server infrastructure design
A user wants to access the website www.foobar.com from the web browser :
- Use a domain name : foobar.com
- Configure with : www
- Server IP: 8.8.8.8
- 1 server containing : 
    - 1 web server (Nginx)
    - 1 application server
    - 1 application files (code base)
    - 1 database (MySQL)

![Design of the Web server Infrastructure](https://raw.githubusercontent.com/Mornac/holbertonschool-system_engineering-devops/main/web_infrastructure_design/images/Web%20server%20infrastructure.jpg)

## 2. Specifics about the infrastructure  
### What is a Server?  
A server is a computer system that provides services, resources, or data to other computers called 'Clients' over a network.  
Here, it hosts and serves our website to users's browser.  

### What is the role of the domain name?  
The domain name serves as a humain-readable address that maps the server's IP address (8.8.8.8).  
In our case, we use foobar.com instead of remembering numbers, it's easier.

### What type of DNS record www is in www.foobar.com?  
The www in www.foobar.com is an address record that directly points to the server's IP address 8.8.8.8.

### What is the role of the web server?
- Receives HTTP/HTTPS requests from users's browser
- Serves static content : HTML, CSS, JavaScript, images
- Actes as a reverse proxi forwarding dynamic requests to the application server.

### What is the role of the application server?
- Executes business logic and application code
- Processes dynamic requests : user login, data processing, etc
- Generates dynamic content based on user requests
- Communicates with the database to retrieve / store data
- Returns processed data to the web server

### What is the role of the database?
- Stores application data : user accounts, content, configurations
- Manages data persistence and integrity
- Handles data queries from the application server
- Provides data security and access control
- Ensures data consistency through transactions

### What is the server using to communicate with the computer of the user requesting the website?  
The server uses HTTP or HTTPS (HyperText Transfer Protocol Secure) to communicate with users'browsers. This is the standard protocol for web communication.  


## 3. The issues with the infrastructure
### SPOF Single Point Of Failure.
| Problem | Risk | Impact |
|---------|------|--------|
|if the single server fails, the entire website becomes unavailable|Hardware failure, Software crash, or network issues affect all users|100% downtime until the server is restored|

### Downtime when maintenance needed (like deploying new code web server needs to be restarted).
| Problem | Risk | Impact |
|---------|------|--------|
|when deploying new code or updating the system, the server needs to be restarted|Service interruption during updates, patches, or deployments|Temporary unavailability affecting user experience|

### Cannot scale if too much incoming traffic.
| Problem | Risk | Impact |
|---------|------|--------|
|a single server has limited CPU, memory, and bandwidth resources|Performance degradation or crashes under high load|Slow response times, timeouts or complete service failure during traffic spices.  
