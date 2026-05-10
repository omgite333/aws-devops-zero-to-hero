# AWS Route 53 Notes

A detailed beginner-to-intermediate guide for understanding AWS Route 53, DNS concepts, routing policies, health checks, and interview preparation.

---

# Table of Contents

1. What is Route 53?
2. Why the Name Route 53?
3. What is DNS?
4. How DNS Works
5. Core Features of Route 53
6. DNS Record Types
7. Route 53 Routing Policies
8. Health Checks
9. Route 53 with AWS Services
10. Public vs Private Hosted Zones
11. Route 53 Architecture Flow
12. Real-World Example
13. Advantages of Route 53
14. Important Interview Questions
15. Interview One-Liners

---

# 1. What is Route 53?

Amazon Route 53 is AWS’s scalable Domain Name System (DNS) web service.

It is used for:

* Domain registration
* DNS management
* Traffic routing
* Health monitoring
* High availability applications

Route 53 converts human-readable domain names into IP addresses.

Example:

```text
google.com → 142.250.183.14
```

Because computers communicate using IP addresses, not domain names.

---

# 2. Why the Name Route 53?

* "Route" means routing internet traffic.
* "53" refers to Port 53, which is used by DNS services.

DNS typically works on:

```text
TCP/UDP Port 53
```

---

# 3. What is DNS?

DNS stands for:

# Domain Name System

DNS works like the internet’s phonebook.

Instead of remembering IP addresses:

```text
142.250.183.14
```

we use easy names:

```text
google.com
```

DNS translates domain names into IP addresses.

---

# 4. How DNS Works

When a user opens:

```text
www.example.com
```

the following process happens:

---

## Step 1 — Browser Cache Check

The browser first checks whether the IP is already cached.

If found:

* DNS lookup stops
* browser uses cached IP

---

## Step 2 — OS Cache Check

Operating system checks local DNS cache.

---

## Step 3 — DNS Resolver

If not cached, request goes to DNS Resolver (usually ISP DNS).

---

## Step 4 — Root DNS Server

Resolver asks root DNS server:

```text
Where is .com?
```

---

## Step 5 — TLD Server

TLD (.com) server responds:

```text
Ask example.com's name server
```

---

## Step 6 — Route 53 Name Server

Route 53 returns the actual IP address.

Example:

```text
www.example.com → 52.95.110.1
```

---

## Step 7 — Browser Connects to Server

Browser uses returned IP to connect to application server.

---

# Complete DNS Flow

```text
User Browser
     ↓
DNS Resolver
     ↓
Root DNS Server
     ↓
TLD Server (.com)
     ↓
Route 53 Name Server
     ↓
IP Address Returned
     ↓
Browser Connects to Website
```

---

# 5. Core Features of Route 53

---

## 1. Domain Registration

You can purchase domains like:

```text
myapp.com
```

using Route 53.

---

## 2. DNS Management

Manage DNS records for domains.

Example records:

* A Record
* AAAA Record
* CNAME
* MX
* TXT

---

## 3. Traffic Routing

Route traffic intelligently using:

* latency
* geography
* weighted distribution
* health checks

---

## 4. Health Monitoring

Continuously checks whether applications are healthy.

If unhealthy:

* traffic can automatically move to backup systems.

---

# 6. DNS Record Types

---

## A Record

Maps domain to IPv4 address.

Example:

```text
example.com → 192.168.1.10
```

---

## AAAA Record

Maps domain to IPv6 address.

---

## CNAME Record

Maps one domain to another domain.

Example:

```text
api.example.com → backend.example.com
```

---

## MX Record

Used for mail servers.

Example:

```text
gmail.com mail handling
```

---

## TXT Record

Stores text information.

Used for:

* domain verification
* SPF
* DKIM
* security validation

---

## NS Record

Specifies authoritative name servers.

---

# 7. Route 53 Routing Policies

Route 53 supports multiple routing strategies.

---

# 1. Simple Routing

Used when:

* one domain points to one resource

Example:

```text
example.com → EC2 instance
```

---

# 2. Weighted Routing

Distributes traffic based on percentages.

Example:

```text
80% → Server A
20% → Server B
```

### Use Cases

* Canary deployments
* A/B testing
* Gradual rollouts

---

# 3. Latency-Based Routing

Routes users to region with lowest latency.

Example:

```text
India users → Mumbai region
US users → Virginia region
```

Improves:

* performance
* user experience

---

# 4. Failover Routing

Routes traffic to backup server if primary fails.

Example:

```text
Primary Server DOWN
        ↓
Traffic automatically switches to Backup
```

Used for:

* disaster recovery
* high availability

---

# 5. Geolocation Routing

Routes users based on geographic location.

Example:

```text
India → Indian server
Europe → European server
```

---

# 6. Multi-Value Routing

Returns multiple healthy IP addresses.

Provides:

* simple load balancing
* high availability

---

# 7. Geoproximity Routing

Routes traffic based on:

* user location
* AWS resource location

Can shift traffic geographically.

---

# 8. Health Checks

Route 53 can continuously monitor:

* websites
* APIs
* servers
* endpoints

---

## Health Check Process

Route 53 sends requests periodically.

If endpoint responds correctly:

* marked healthy

If endpoint fails repeatedly:

* marked unhealthy

---

## Example

```text
Primary Server → Unhealthy
```

Route 53 automatically:

* stops sending traffic there
* redirects users to healthy resources

---

# 9. Route 53 with AWS Services

Route 53 commonly integrates with:

---

## EC2

Point domain directly to EC2 instances.

Example:

```text
example.com → EC2 Public IP
```

---

## Elastic Load Balancer (ELB)

Most common production setup.

Example:

```text
example.com → Load Balancer
```

Benefits:

* scalability
* fault tolerance
* auto scaling support

---

## CloudFront

Use Route 53 with CDN for fast global delivery.

---

## S3 Static Website Hosting

Host static websites.

Example:

* portfolio websites
* documentation websites

---

## API Gateway

Custom domains for APIs.

---

# 10. Public vs Private Hosted Zones

---

# Public Hosted Zone

Accessible from internet.

Used for:

* public websites
* external applications

Example:

```text
example.com
```

---

# Private Hosted Zone

Accessible only inside VPC.

Used for:

* internal applications
* private microservices

Example:

```text
internal.company.local
```

---

# 11. Route 53 Architecture Flow

```text
User
 ↓
Route 53 DNS Query
 ↓
Route 53 Routing Policy
 ↓
Target Resource
 ↓
EC2 / ALB / CloudFront / S3
```

---

# 12. Real-World Example

Suppose you have:

* frontend application in Mumbai
* backup application in Singapore
* Application Load Balancer
* Route 53 failover routing

Flow:

```text
User → Route 53
           ↓
Primary ALB Healthy?
      YES → Mumbai
      NO  → Singapore Backup
```

This provides:

* high availability
* disaster recovery
* automatic failover

---

# 13. Advantages of Route 53

---

## Highly Scalable

Handles millions of DNS queries.

---

## Highly Available

AWS global infrastructure ensures reliability.

---

## Fast DNS Resolution

Low latency DNS service.

---

## Flexible Routing

Supports multiple routing policies.

---

## AWS Integration

Works seamlessly with AWS services.

---

# 14. Important Interview Questions

---

## What is Route 53?

AWS DNS and traffic routing service.

---

## Why is it called Route 53?

DNS uses Port 53.

---

## Difference between Public and Private Hosted Zone?

| Public Hosted Zone  | Private Hosted Zone   |
| ------------------- | --------------------- |
| Internet accessible | VPC only              |
| Public applications | Internal applications |

---

## What is Latency-Based Routing?

Routes users to lowest latency AWS region.

---

## What is Failover Routing?

Automatically redirects traffic to backup resource if primary fails.

---

## Difference between Weighted and Latency Routing?

| Weighted Routing         | Latency Routing      |
| ------------------------ | -------------------- |
| Traffic percentage based | Lowest latency based |
| Used for A/B testing     | Used for performance |

---

## What is Health Check in Route 53?

Monitors endpoint availability and routes traffic only to healthy resources.

---

# 15. Interview One-Liners

---

## Route 53

AWS-managed scalable DNS and traffic routing service.

---

## DNS

System that converts domain names into IP addresses.

---

## Hosted Zone

Container for DNS records.

---

## Health Check

Monitors endpoint health and supports failover routing.

---

## Latency Routing

Routes traffic to nearest AWS region.

---

## Weighted Routing

Splits traffic based on configured percentages.

---

# Summary

Route 53 is one of the most important AWS networking services used for:

* DNS resolution
* domain management
* intelligent traffic routing
* failover handling
* global application availability

It plays a critical role in highly available cloud architectures and production-grade AWS environments.
