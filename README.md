# Critical User Journeys (CUJ) Guide for SRE Teams

## Table of Contents
1. [Introduction](#introduction)
2. [What is a Critical User Journey?](#what-is-a-critical-user-journey)
3. [Why CUJ Matters to SRE](#why-cuj-matters-to-sre)
4. [Process for Defining a CUJ](#process-for-defining-a-cuj)
5. [Tools and Practices for Monitoring CUJs](#tools-and-practices-for-monitoring-cujs)
6. [CUJ Example: E-commerce Checkout Journey](#cuj-example-e-commerce-checkout-journey)
7. [Tracking Progress Using Rally](#tracking-progress-using-rally)
8. [Incident Response for CUJ Degradations](#incident-response-for-cuj-degradations)
9. [Continuous Improvement & Blameless Postmortems](#continuous-improvement--blameless-postmortems)
10. [Conclusion](#conclusion)

---

## Introduction

A **Critical User Journey (CUJ)** is a set of steps or workflows that represent the essential actions a user takes to achieve their primary goal with your service or product. In SRE practices, defining, monitoring, and improving CUJs ensures that reliability work aligns with customer experience and business value. This guide helps SREs define CUJs, measure their performance using **Dynatrace**, **Splunk**, **Splunk ITSI**, and **Rally**, and track continuous improvement.

---

## What is a Critical User Journey?

A **Critical User Journey (CUJ)** focuses on the most valuable, time-sensitive, or frequently used interactions in your system. For example, an e-commerce website’s checkout process or a banking application’s funds transfer flow could be critical journeys. Identifying these journeys helps SRE teams ensure the performance and reliability of what matters most to customers and stakeholders.

**Characteristics of CUJs:**
- Essential to user satisfaction and business outcomes.
- Involves multiple services and components.
- Sensitive to latency, failures, or degradations.

---

## Why CUJ Matters to SRE

Top SRE organizations like **Google, Netflix, and Amazon** emphasize **service level objectives (SLOs)** and error budgets centered around critical user flows. Defining CUJs helps you:
1. **Align reliability work with business priorities.**
2. **Create meaningful SLOs** that reflect user expectations.
3. **Monitor distributed systems** effectively by focusing on key paths.
4. **Optimize alerting** to focus on what matters most.
5. **Prioritize incidents and changes** based on customer impact.

---

## Process for Defining a CUJ

Follow these steps to define, document, and measure your CUJs:

### 1. Identify Key User Flows
- Collaborate with product and business teams to identify **top user actions**.
- Examples: Login, Checkout, Payment processing, File upload.

### 2. Break Down the User Journey into Steps
- Outline each **step, interaction, and dependency** involved.
- Example: "Search for product" → "Add to cart" → "Proceed to checkout" → "Payment."

### 3. Map System Dependencies
- Identify the **infrastructure, microservices, APIs, and databases** used for each step.
- Use **Dynatrace** to generate service dependency maps and flows.

### 4. Define SLIs and SLOs for the CUJ
- **Example SLI:** Checkout time < 3 seconds.
- **Example SLO:** 99.9% of checkout requests should succeed within the SLI threshold.

### 5. Document the CUJ in Rally
- Create a **story or epic in Rally** that documents:
  - Name and description of the CUJ.
  - SLI/SLO definitions.
  - Dependencies and monitoring tools.
  - Current known risks and mitigations.

### 6. Set Up Error Budgets and Alerts
- Use **Splunk ITSI** to create **dashboards** and alerts linked to CUJs.
- Establish **error budgets** for tracking deviations from SLOs.

---

## Tools and Practices for Monitoring CUJs

Leverage the following tools to monitor CUJs effectively:

### 1. Dynatrace
- **Real User Monitoring (RUM):** Track real-time interactions and performance.
- **Service flow mapping:** Visualize service dependencies in user journeys.
- **Synthetic Monitoring:** Test critical paths proactively.

### 2. Splunk & Splunk ITSI
- **Log aggregation** and correlation for CUJ-related events.
- Create **ITSI Glass Tables** for visualizing CUJ health across systems.
- Build **KPIs and alerts** based on SLIs (e.g., "90th percentile latency").

### 3. Rally
- Use **Rally epics and stories** to align CUJs with reliability work.
- Link **SLO breaches** to Rally tasks for action tracking.
- Maintain a **backlog of known CUJ-related issues**.

---

## CUJ Example: E-commerce Checkout Journey

Here’s an example of a well-defined CUJ for an **e-commerce checkout flow**.

1. **CUJ Name:** Checkout Journey  
2. **Description:** The sequence a user follows to purchase items from the store.  
3. **Steps:**
   1. User searches for a product.
   2. Adds product to the cart.
   3. Proceeds to checkout.
   4. Selects payment method and submits payment.
   5. Receives confirmation page.

4. **Dependencies:**
   - Search service  
   - Cart API  
   - Payment gateway  
   - Database (for product and order details)  

5. **SLI:** 95th percentile response time < 2 seconds for each step.  
6. **SLO:** 99.9% of users should complete the checkout process successfully.  
7. **Alerting:**  
   - **Splunk ITSI** alert when SLI threshold exceeds 2 seconds.  
   - **Dynatrace** synthetic test alerts when checkout fails 3 times in a row.  

---

## Tracking Progress Using Rally

### 1. Create Rally Stories for CUJs  
- Use **CUJ documentation as an epic** and break it into smaller Rally stories:
  - Define SLOs for checkout.
  - Create Dynatrace monitors.
  - Set up ITSI dashboards and alerts.
  - Monitor error budget consumption.

### 2. Connect SLO Breaches to Rally Tasks  
- When an **SLO breach occurs**, create Rally stories or defects to track the issue.
- Use **Rally’s dependency tracking** to link incidents to CUJs for visibility.

### 3. Build a CUJ-Focused Backlog  
- Maintain a **backlog in Rally** with CUJ-related tasks, reliability improvements, and automation opportunities.

---

## Incident Response for CUJ Degradations

### 1. Detection  
- Use **Splunk ITSI alerts** to detect CUJ failures or slowdowns.  
- **Dynatrace dashboards** can provide real-time impact visibility.

### 2. Triage  
- Prioritize incidents affecting **critical CUJs** over non-critical paths.  
- Use service dependency maps in **Dynatrace** to identify root causes.

### 3. Resolution  
- Coordinate across teams using Rally stories to track fixes.  
- Implement **feature flags** to disable problematic features quickly.

---

## Continuous Improvement & Blameless Postmortems

1. **Postmortems for CUJ Incidents:**  
   - Conduct **blameless postmortems** for incidents involving CUJs.  
   - Use findings to **refine SLOs** and update CUJ documentation.

2. **Error Budget Policy:**  
   - If error budgets are exhausted, **pause feature rollouts** and prioritize reliability work.  
   - Track error budget consumption in Rally and adjust capacity planning accordingly.

3. **Automation & Chaos Testing:**  
   - Use **Dynatrace Synthetic Monitoring** to create automated tests for CUJs.  
   - Implement **chaos experiments** to test CUJ resilience under failure conditions.

---

## Conclusion

Defining and monitoring **Critical User Journeys** ensures that your SRE team focuses on the most important aspects of reliability. With **Dynatrace, Splunk, ITSI, and Rally**, your team can measure, monitor, and improve CUJs effectively. Use this guide to align your reliability practices with customer experience, ensuring a proactive, business-aligned SRE culture.

---

## Further Reading
- [Google SRE Book](https://sre.google/books/)
- [Principles of Chaos Engineering](https://principlesofchaos.org/)
- [Dynatrace Documentation](https://www.dynatrace.com/support/)
- [Splunk ITSI Documentation](https://docs.splunk.com/ITSI)
