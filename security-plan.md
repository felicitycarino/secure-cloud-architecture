# Secure Cloud Architecture Plan

## CDN
By holding localized, cached copies of static assets (such as HTML documents and CSS stylesheets), the Content Delivery Network minimizes network latency and accelerates page load times while offloading routine traffic from primary infrastructure servers.

## Load Balancer
Serving as the single entry point for external web requests, the load balancer disperses incoming client traffic across the application fleet. This prevents individual node exhaustion and maintains service availability during peak loads.

## Application Servers
These instances process business logic and execute dynamic user transactions. Placing application servers inside a private subnet guarantees that they process only internal requests forwarded by the load balancer, isolating them from outside threats.

## Database
The database functions as the centralized repository for confidential student information. Isolating it within a private subnet without public internet access protects sensitive datasets against unauthorized access and external threat vectors.

# Public and Private Resources

| Resource | Public or Private? | Explanation |
| :--- | :--- | :--- |
| CDN | Public | Must be accessible to users over the internet to deliver static content globally. |
| Load Balancer | Public | Acts as the entry point for internet traffic, directing it to the private application servers. |
| Application Server | Private | Should only accept internal traffic routed through the load balancer, protecting it from direct internet exposure. |
| Database | Private | Contains sensitive student data and must only be accessible by the internal application servers. |


# Security Controls

## IAM
Identity and Access Management (IAM) restricts cloud environment access to authorized personnel only. Only designated administrators and developers should have specific permissions to manage or modify these cloud resources.

## MFA
Multi-Factor Authentication (MFA) must be enabled for all administrator and developer accounts. This provides a critical second layer of security in case account passwords are stolen or compromised.

## Firewall / Security Group
Network traffic must be strictly controlled to prevent unauthorized access to backend systems.
* Internet -> Load Balancer = Allowed
* Load Balancer -> Application Server = Allowed
* Application Server -> Database = Allowed
* Internet -> Database = Blocked

## Encryption
Student data must be encrypted **in transit** (TLS/HTTPS between client and servers) and **at rest** (AES-256 for database storage and backups) to protect personal identifiable information (PII) from eavesdropping or exposure during a physical breach.

## Logging
A comprehensive record of system events—such as authentication attempts, infrastructure modifications, and database transactions—must be systematically captured. This audit log provides critical forensic data necessary for tracing unauthorized actions and investigating security breaches.

## Monitoring
Continuous surveillance of system behavior is required to detect abnormal patterns, such as sudden bandwidth anomalies or brute-force login attempts. Proactive tracking enables quick intervention against emerging threats like DDoS attacks to prevent service disruption.
## Backup
Automated, encrypted backups of the database must run on a regular schedule. Securing offsite or isolated data copies ensures complete information recovery following hardware crashes, unintended deletions, or malicious extortion software.


# Principle of Least Privilege

| User | Allowed Access |
| :--- | :--- |
| Administrator | Complete administrative permissions to modify network topologies, cloud infrastructure, and security policies. |
| Instructor | View-only access within the web portal to review academic and student records. |
| Student | Restricted view-only privileges strictly limited to their own individual student profile.|
| Developer | Operational access to application repositories and deployment pipelines, with zero access to live student databases. |


# Shared Responsibility Model

| Responsibility | Cloud Provider or Customer? |
| :--- | :--- |
| Physical data center | Cloud Provider |
| Physical servers | Cloud Provider |
| User accounts | Customer |
| Student data | Customer |
| IAM permissions | Customer |
| Application security | Customer |
| Database access rules | Customer |
| Backups | Customer |

1. What does Security OF the Cloud mean?

It refers to the cloud provider's obligation to safeguard the foundational infrastructure—including physical data centers, host hardware, underlying storage, and core networking facilities—that powers the platform.

2. What does Security IN the Cloud mean?
It covers the tenant's responsibility to manage and protect everything deployed inside the cloud environment, such as user identities, source code, operating systems, network firewalls, and sensitive datasets.

3. Which resource should be directly accessible from the Internet?
Public-facing components like the Content Delivery Network (CDN) and the Load Balancer are the only resources configured to receive direct incoming web traffic.

4. Why should the database remain private?
Because it stores confidential records, isolating it within a private subnet shields it from public exposure, direct web attacks, and unauthorized external breaches.

5. Why should users not connect directly to the database?
Direct connections bypass application-level authorization and input validation, leaving the database vulnerable to malicious exploits like SQL injection or unintended data manipulation.

6. What is the purpose of a load balancer?
It serves as a traffic distribution layer that routes incoming client requests across multiple backend instances to optimize throughput and prevent server overload.

7. What happens if one application server fails?
Automated health monitoring alerts the load balancer to bypass the compromised server and route all dynamic requests exclusively to operational nodes, preventing downtime.

8. What is the purpose of a CDN?
A CDN replicates static media across edge locations globally to shorten latency for users while simultaneously reducing the traffic burden on primary application servers.

9. Why should administrator accounts use MFA?
Administrative credentials grant broad system control; multi-factor authentication ensures that stolen or leaked credentials alone cannot grant access without a secondary identity check.

10. Why should administrator access not be given to every employee?
Restricting administrative privileges adheres to the principle of least privilege, reducing internal security risks, system misconfigurations, and accidental operational disruptions.

11. Why are logging and monitoring important?
Logging builds an immutable record of system events for auditing and forensics, while real-time monitoring detects anomalies early to enable prompt incident mitigation.

12. Why are backups important?
Routine backups guarantee data resilience, allowing full system restoration in cases of hardware degradation, unintended deletion, or ransomware threats.
