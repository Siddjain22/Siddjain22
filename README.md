# CSE 3015 – AWS Cloud Practitioner | MOD 1 Exam Answers
### (10-Mark Format) — Based on your Question Bank

---

## Q1. Explain the different Cloud Deployment Models. (10 Marks)

**Definition:** A cloud deployment model describes *where* the infrastructure is located, *who* owns/manages it, and *who* is allowed to access it. AWS supports three main deployment models.

### 1. Cloud (Public Cloud)
- The application is **fully deployed in the cloud** — every part of it runs on cloud infrastructure.
- Applications are either **born in the cloud** or **migrated** to it to gain cloud benefits (elasticity, pay-as-you-go, scalability).
- Can be built on low-level infrastructure (IaaS, e.g., raw EC2 + storage) or on higher-level abstracted services (PaaS/SaaS) that hide management, architecting, and scaling complexity.
- **Example:** A startup running its entire web app on AWS EC2, S3, and RDS.

### 2. Hybrid Cloud
- Connects **cloud-based resources** with **existing on-premises infrastructure**.
- The most common hybrid setup links a private data center to the public cloud, letting an organization **extend and grow** its infrastructure into the cloud while keeping some systems internal.
- Useful when: legacy systems can't be moved yet, data residency laws require some data to stay local, or a gradual cloud migration is preferred.
- **Example:** A bank keeps core transaction servers on-premises (for compliance) but runs analytics and customer apps on AWS.

### 3. On-Premises (Private Cloud)
- Resources are deployed **within the organization's own data center**, using virtualization and resource-management tools to mimic cloud-like flexibility.
- Does **not** provide most benefits of true cloud computing (no elastic scaling from a global provider, high CapEx) but gives **full control and dedicated resources**.
- Functionally, this is usually just legacy IT infrastructure enhanced with virtualization to raise utilization.
- **Example:** A defense/government agency running a VMware-virtualized private data center for classified workloads.

### Comparison Table

| Factor | Public (Cloud) | Hybrid | Private (On-Premises) |
|---|---|---|---|
| Control | Low | Medium | High |
| Cost model | OpEx, pay-as-you-go | Mixed | CapEx, high upfront |
| Scalability | Very high | High (cloud part) | Limited |
| Security/Compliance | Shared responsibility | Configurable | Fully owned |
| Best for | Startups, variable workloads | Regulated orgs migrating gradually | Highly sensitive/legacy workloads |

**Conclusion:** The choice of deployment model depends on data sensitivity, compliance needs, budget, and how much control an organization wants over its infrastructure. Most modern enterprises trend toward **hybrid** as a transition step from private to full public cloud.

---

## Q2. Explain the Cloud Delivery (Service) Models — IaaS, PaaS, SaaS. (10 Marks)

**Definition:** The cloud service/delivery model defines **how much of the IT stack** is managed by the customer versus the cloud provider. As you move from IaaS → PaaS → SaaS, you get **less control but less management burden**.

### The IT Stack (9 layers)
Applications → Data → Runtime → Middleware → O/S → Virtualization → Servers → Storage → Networking

### 1. Infrastructure as a Service (IaaS)
- Provides the **building blocks** for cloud IT: networking, virtual computers, and data storage space.
- Customer manages: **Applications, Data, Runtime, Middleware, OS**.
- Provider manages: **Virtualization, Servers, Storage, Networking**.
- Gives the **highest level of flexibility** and maps most closely to traditional on-premises IT — easiest to migrate existing workloads to.
- **AWS Examples:** Amazon EC2, Amazon EBS, Amazon VPC.

### 2. Platform as a Service (PaaS)
- **Removes the need to manage underlying infrastructure** (OS, servers, virtualization).
- Customer focuses only on **deploying and managing their application and data**.
- Provider manages: Runtime, Middleware, OS, Virtualization, Servers, Storage, Networking.
- **AWS Examples:** AWS Elastic Beanstalk, AWS Lambda.

### 3. Software as a Service (SaaS)
- A **completed product** that is run and managed entirely by the service provider.
- Customer only uses the application through a browser/client — usually just manages their own **data and user settings**.
- Provider manages everything else in the stack.
- **Examples:** Gmail, Dropbox, Salesforce.

### Diagram (Responsibility Split)

```
              On-Prem   IaaS      PaaS      SaaS
Applications   [You]    [You]     [You]     [Provider]
Data           [You]    [You]     [You]     [Provider]
Runtime        [You]    [You]     [Provider][Provider]
Middleware     [You]    [You]     [Provider][Provider]
O/S            [You]    [You]     [Provider][Provider]
Virtualization [You]    [Provider][Provider][Provider]
Servers        [You]    [Provider][Provider][Provider]
Storage        [You]    [Provider][Provider][Provider]
Networking     [You]    [Provider][Provider][Provider]
```

**Conclusion:** IaaS gives maximum control (best for custom/legacy workloads), PaaS speeds up development by removing infra management (best for developers who just want to deploy code), and SaaS removes almost all technical burden (best for end-users who just need working software).

---

## Q3. Explain AWS Regions in the AWS Global Infrastructure. (10 Marks)

**Definition:** An **AWS Region** is a geographical area containing a cluster of **Availability Zones (AZs)** that are physically separate but close enough together to form a low-latency network.

### Key Points
- Each Region is a **separate geographic area** (e.g., US East (N. Virginia), EU (Ireland), Asia Pacific (Mumbai)).
- AWS **logically groups Regions** into larger geographic areas for ease of management — e.g., N. Virginia and Ohio both fall under "US East."
- A Region **typically consists of 2 or more Availability Zones**, each providing full redundancy and network connectivity.
- **Data replication across Regions is fully controlled by the customer** — AWS does not automatically copy your data between Regions.
- **Communication between Regions** happens over the AWS private backbone network infrastructure, not the public internet.
- **Naming convention:** Availability Zones are named using the Region code + a letter. E.g., in `eu-west-1` (EU Ireland): `eu-west-1a`, `eu-west-1b`, `eu-west-1c`.

### Factors for Selecting a Region
1. **Data governance & legal/compliance requirements** (e.g., GDPR may require EU data to stay in an EU Region).
2. **Proximity to customers** — reduces latency.
3. **Services available within that Region** — not all AWS services are available in every Region.
4. **Cost** — pricing varies by Region.

### Why Regions Matter
- Enable **fault isolation** — a failure in one Region does not affect another.
- Allow businesses to **"go global in minutes"** by launching resources in the Region nearest their customers.
- Support **disaster recovery** strategies through multi-Region architecture.

**Conclusion:** Regions are the foundational geographic building block of AWS's global infrastructure, letting customers balance latency, compliance, cost, and service availability when deciding where to run workloads.

---

## Q4. Case Study: A company wants to use AWS services to achieve fault tolerance, disaster recovery, and improved application performance. Explain how AWS Global Infrastructure helps achieve this. (10 Marks)

**Approach:** This is solved by intelligently using AWS's layered global infrastructure — **Availability Zones, Regions, and Edge Locations** — instead of relying on a single server/data center.

### 1. Achieving Fault Tolerance — Multi-AZ Deployment
- Each **Availability Zone (AZ)** is a fully isolated partition of AWS infrastructure, made up of one or more discrete data centers with **redundant power, networking, and connectivity**.
- AZs within a Region are interconnected via **high-speed, low-latency private fiber links**.
- By deploying the application (e.g., EC2 instances, RDS databases) across **2 or more AZs**, if one AZ fails (power outage, hardware fault), the application keeps running from the other AZ — this is **built-in redundancy**, one of AWS's core infrastructure features.
- **Recommendation:** Use an Auto Scaling Group + Elastic Load Balancer spanning multiple AZs, and a Multi-AZ RDS database.

### 2. Achieving Disaster Recovery — Multi-Region Strategy
- For protection against a Region-wide disaster (natural disaster, major outage), the company should **replicate critical data and infrastructure across multiple Regions**.
- Since **data replication across Regions is controlled by the customer**, the company can set up cross-Region replication (e.g., S3 Cross-Region Replication, RDS Read Replicas in another Region).
- Communication between Regions travels over AWS's own backbone network, ensuring reliable, fast replication.

### 3. Improving Application Performance — Edge Locations & Regional Edge Caches
- Deploy **Amazon CloudFront** with **Edge Locations** (400+ globally) to cache frequently accessed content close to end users, drastically reducing latency.
- **Regional Edge Caches** sit between the Edge Locations and the origin server, holding a larger, less-frequently-accessed cache — reducing the number of requests that hit the origin servers directly.
- Choosing a **Region close to the majority of customers** further reduces latency for dynamic content.

### Summary of Mapping

| Requirement | AWS Infrastructure Feature Used |
|---|---|
| Fault Tolerance | Multiple Availability Zones within a Region |
| Disaster Recovery | Multiple Regions + cross-Region data replication |
| Improved Performance | Edge Locations + Regional Edge Caches (CloudFront CDN) |

**Conclusion:** By architecting across AZs (fault tolerance), Regions (disaster recovery), and Edge Locations (performance), the company achieves a highly available, resilient, and low-latency application — exactly the elasticity and reliability that traditional single-data-center IT cannot offer.

---

## Q5. Case Study: A university wants to deploy on a cloud platform but is unsure which deployment model to choose. Justify the best deployment model considering cost-effectiveness and sensitive data (e.g., student health records). (10 Marks)

### Step 1 — Identify the Requirements
- **Cost-effectiveness:** Universities typically have limited IT budgets and want to avoid heavy CapEx (data center hardware, cooling, staff).
- **Sensitive data:** Student health records, financial aid data, and personal information are sensitive and often subject to legal/compliance regulations (data privacy laws).
- **Mixed workloads:** Universities run both non-sensitive public-facing systems (course websites, LMS, event pages) and sensitive systems (health records, HR, finance).

### Step 2 — Evaluate Each Deployment Model

| Model | Fit for University? |
|---|---|
| **Public Cloud** | Excellent for cost and scalability (e.g., LMS, course portals, research computing bursts) but raises concerns for storing sensitive health data directly. |
| **Private (On-Premises)** | Best for security/compliance of health data, but expensive, hard to scale for growing student services, and requires the university to hire/manage its own infra team. |
| **Hybrid Cloud** | Combines both — cost savings for general workloads, security control for sensitive data. |

### Step 3 — Recommendation: **Hybrid Cloud Deployment Model**

**Justification:**
1. **Cost-effectiveness:** General-purpose, high-traffic systems (learning management system, public website, email, research applications) are moved to the **public cloud**, taking advantage of pay-as-you-go pricing, elasticity, and no upfront hardware investment — trading capital expense for variable expense.
2. **Data governance for sensitive data:** Health records and other regulated data remain **on-premises** (private) or in a compliance-certified isolated environment, satisfying legal/data-governance requirements without giving up control.
3. **Gradual migration path:** A hybrid model lets the university **extend and grow into the cloud over time** rather than a risky "big-bang" migration — legacy systems can stay in place while new systems are cloud-native.
4. **High availability & performance:** Public-facing content (course catalogs, admissions pages) can additionally use **Edge Locations/CloudFront** for fast global access, useful for prospective international students.
5. **Scalability for peak loads:** Course registration periods create huge, short-term traffic spikes — the elastic public cloud portion easily scales up and back down, something a fixed private data center cannot do cost-effectively.

**Conclusion:** A **Hybrid Cloud model** is the most justified choice — it balances the university's need for **low-cost, elastic infrastructure** for general services with the **strict control and compliance** required for sensitive health/student data, while allowing a gradual, low-risk migration path.

---

## Quick Revision Checklist Before the Exam
- [ ] Deployment models: **Cloud / Hybrid / On-Premises** — know definitions + 1 example each
- [ ] Delivery models: **IaaS / PaaS / SaaS** — know the 9-layer stack and who manages what
- [ ] AWS Global Infra hierarchy: **Region → Availability Zone → Data Center**; also **Edge Location** and **Regional Edge Cache**
- [ ] Region selection factors: **compliance, latency, service availability, cost**
- [ ] AZ naming convention: `region-code` + letter (e.g., `eu-west-1a`)
- [ ] Be ready to **apply** these concepts to a scenario (fault tolerance → multi-AZ; DR → multi-Region; performance → CloudFront/Edge)
