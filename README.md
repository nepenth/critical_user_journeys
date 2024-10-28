# Critical User Journeys (CUJ) Guide for SRE Teams

## Table of Contents
1. [Introduction](#introduction)
2. [What is a Critical User Journey?](#what-is-a-critical-user-journey)
3. [Why CUJ Matters to SRE](#why-cuj-matters-to-sre)
4. [Process for Defining a CUJ](#process-for-defining-a-cuj)
5. [Tools and Practices for Monitoring CUJs](#tools-and-practices-for-monitoring-cujs)
6. [CUJ Example: E-commerce Checkout Journey](#cuj-example-e-commerce-checkout-journey)
7. [Service Paths and API-Level CUJ Details](#service-paths-and-api-level-cuj-details)
8. [Tracking Progress Using Rally](#tracking-progress-using-rally)
9. [Incident Response for CUJ Degradations](#incident-response-for-cuj-degradations)
10. [Continuous Improvement & Blameless Postmortems](#continuous-improvement--blameless-postmortems)
11. [Conclusion](#conclusion)

---

## Introduction

A **Critical User Journey (CUJ)** is a structured sequence of actions or workflows that represent the essential paths users take to accomplish key goals with a product. SRE teams need to **define**, **monitor**, and **optimize CUJs** to align their reliability efforts with business objectives. This guide integrates practices from leading SRE organizations and details how tools like **Dynatrace**, **Splunk ITSI**, **Splunk**, and **Rally** can be leveraged to monitor and improve these journeys.

---

## What is a Critical User Journey?

A **CUJ** is typically made up of multiple **service paths**, **microservice calls**, and **API endpoints** that together ensure smooth user interaction. Think of a CUJ as both a **functional flow** and a **technical path**—each action taken by a user depends on backend services working in sync. **Identifying these dependencies and interactions is critical** to monitoring reliability.

**Characteristics of CUJs:**
- Essential to user satisfaction and business outcomes.
- Involves multiple services and components.
- Sensitive to latency, failures, or degradations.

---

## Why CUJ Matters to SRE

### Key Outcomes of CUJ-based Reliability:
- **Aligned reliability priorities:** SRE teams focus efforts on key user interactions.
- **Actionable SLOs and SLIs:** Based on real user impact.
- **Effective alerting:** Alerts are structured around critical points in the CUJ.
- **Proactive reliability:** CUJs are continuously improved through error budget policies and postmortems.

---

## Process for Defining a CUJ

### 1. Identify Key User Flows
- Collaborate with **product managers** and **engineering teams** to identify essential user actions.
- Example Flows: Login, File upload, Checkout, Funds transfer, etc.

### 2. Break Down the User Journey into Steps
- Example for E-commerce Checkout: 
  - **Step 1:** Search for product  
  - **Step 2:** Add product to cart  
  - **Step 3:** Proceed to checkout  
  - **Step 4:** Submit payment  
  - **Step 5:** Confirmation page  

### 3. Map System Dependencies and Service Paths
- Create a **service dependency map** using **Dynatrace**.
- Identify each **API call, microservice, and external dependency** involved.

### 4. Define SLIs and SLOs
- Example SLI: "90th percentile response time for checkout API < 2 seconds"
- Example SLO: "99.9% of checkout requests should succeed within the SLI threshold."

### 5. Document the CUJ in Rally
- Create an **epic** with linked stories tracking dependencies, risks, SLO breaches, and monitoring needs.

---

## Tools and Practices for Monitoring CUJs

### 1. Dynatrace
- **Service flow mapping:** Visualize inter-service dependencies across a CUJ.
- **API monitoring:** Track key endpoints used in the CUJ.
- **Synthetic tests:** Continuously test CUJ flows (e.g., checkout simulation).

### 2. Splunk & Splunk ITSI
- **Custom KPIs:** Monitor response times and success rates for key CUJ steps.
- **Log aggregation:** Collect and analyze logs from services involved in CUJs.
- **Glass tables:** Create **ITSI Glass Tables** for a visual overview of CUJ health.

### 3. Rally
- Maintain a **CUJ-focused backlog**.
- Track CUJ-related incidents, SLO breaches, and improvements.

---

## CUJ Example: E-commerce Checkout Journey

### Overview of the Journey
1. **Step 1:** User searches for a product (via search API).
2. **Step 2:** Adds product to cart (via cart API).
3. **Step 3:** Proceeds to checkout (via checkout microservice).
4. **Step 4:** Submits payment (via payment gateway API).
5. **Step 5:** Receives order confirmation (via confirmation microservice).

---

## Service Paths and API-Level CUJ Details

In this section, we dive deeper into the **service paths** and **API calls** involved in the **checkout journey example**.

### 1. Service Flow
User → Frontend UI → API Gateway → Search Service → Product DB ↓ Cart Service → Redis Cache → Payment Gateway ↓ Confirmation Service → Order DB

This flow highlights **key services** and their dependencies:
- **Search Service:** Fetches product details.
- **Cart Service:** Stores the user’s selected products.
- **Redis Cache:** Used for session management and cart persistence.
- **Payment Gateway:** Handles payment authorization.
- **Order DB:** Stores completed order details.

---

### 2. API Paths for Checkout CUJ

| **Step**               | **Service/Endpoint**           | **HTTP Method** | **Response Time SLI** |
|------------------------|--------------------------------|----------------|----------------------|
| Product Search         | `/api/v1/search?query=...`    | `GET`          | < 500ms              |
| Add to Cart            | `/api/v1/cart/add`            | `POST`         | < 300ms              |
| Checkout               | `/api/v1/checkout`            | `POST`         | < 2s                 |
| Payment Authorization  | `/api/v1/payment`             | `POST`         | < 1.5s               |
| Order Confirmation     | `/api/v1/confirmation`        | `GET`          | < 500ms              |

---

### 3. Error Budget and Alerting Example for Checkout API

- **SLO:** 99.9% of requests to `/api/v1/checkout` must succeed in <2s.
- **Error Budget Policy:** If the error budget is exhausted:
  1. **Pause feature rollouts** affecting checkout services.
  2. Create **incident tasks in Rally** linked to the SLO breach.
  3. Monitor recovery and SLA alignment via **Splunk ITSI KPIs**.

- **Alerts:**
  - **Dynatrace:** Create synthetic tests to monitor `/api/v1/checkout`.
  - **Splunk ITSI:** Alert when success rate <99.9% or response time >2s.

---

## Tracking Progress Using Rally

1. **Create Rally Epics and Stories for CUJs**
   - Define stories for each service component (e.g., Search Service, Payment Gateway).
   - Link **SLOs and error budgets** to specific Rally tasks.

2. **Monitor Progress on CUJ Improvements**
   - Create **backlog items** for long-term improvements or optimizations.
   - Use **dependency tracking** to align multiple services under a single CUJ.

3. **Connect Incidents to CUJs**
   - When incidents occur, link them to the relevant CUJ in Rally for visibility and tracking.

---

## Incident Response for CUJ Degradations

1. **Detection**
   - Use **Splunk ITSI KPIs** and **Dynatrace synthetic tests** to detect CUJ failures or slowdowns.

2. **Triage**
   - Prioritize incidents affecting **critical paths or endpoints** (e.g., `/api/v1/checkout`).
   - Use **service flow analysis** in Dynatrace to trace root causes.

3. **Resolution**
   - Use **feature flags** to disable or throttle problematic components.
   - Track fixes in Rally and ensure error budget recovery.

---

## Continuous Improvement & Blameless Postmortems

1. **Blameless Postmortems for CUJ Incidents**
   - Document incidents in Rally and analyze contributing factors.
   - Use insights to refine SLOs or enhance monitoring.

2. **Chaos Testing and Automation**
   - Use **synthetic monitoring** to simulate CUJ failures.
   - Perform **chaos experiments** to ensure resilience under failure scenarios.

3. **Error Budget Policies**
   - Adjust reliability planning if CUJ-related error budgets are consumed too quickly.

---

## Conclusion

Defining, monitoring, and optimizing **Critical User Journeys (CUJs)** ensures that your SRE efforts align with business goals and customer satisfaction. By leveraging tools like **Dynatrace**, **Splunk ITSI**, and **Rally**, your team can effectively monitor technical paths, resolve incidents, and continuously improve reliability.

---

## Further Reading
- [Google SRE Book](https://sre.google/books/)
- [Principles of Chaos Engineering](https://principlesofchaos.org/)
- [Dynatrace Documentation](https://www.dynatrace.com/support/)
- [Splunk ITSI Documentation](https://docs.splunk.com/ITSI)
