# Auto Ticket Classification using Flow Designer

Automated IT ticket classification and notification system built natively in ServiceNow using Flow Designer. This solution eliminates manual effort by automatically analyzing incident descriptions, mapping them to appropriate categories/subcategories, and notifying users upon ticket creation.

---

## 📌 Problem Statement & Objective

School IT helpdesks face daily operational delays manually triaging repetitive requests regarding network connectivity, hardware failures, password resets, and performance issues. 

* **Objective:** Automatically classify tickets at creation time, enforce dependent choice logic, reduce agent manual triage, and automate user confirmation emails using a maintainable, no-code architecture.

---

## 🏗️ Technical Architecture & Data Model

### Custom Table: `Incident WorkFlow` (`u_incident_workflow`)
* **Auto Numbering Enabled:** Prefix `INC`, initial seed `500`, 5 padding digits.

| Field Label | Field Name | Data Type | Reference / Choices | Details |
| :--- | :--- | :--- | :--- | :--- |
| **Number** | `number` | Auto Number | — | Unique auto-generated ticket ID |
| **Caller** | `u_caller` | Reference | `sys_user` | Requesting user |
| **Category** | `u_category` | Choice | Network, Hardware, Access, Performance | Main issue classification |
| **Subcategory** | `u_subcategory` | Choice | Wi-Fi, Projector, Forgot Password, Slow Computer | Dependent choice options |
| **Short Description** | `u_short_description` | String | — | Summary of the issue |
| **Description** | `u_description` | String | — | Detailed explanation of the issue |
| **State** | `u_state` | Choice | New, In progress, On hold, Resolved, Closed | Workflow state |
| **Assigned Group** | `u_assigned_group` | Reference | `sys_user_group` | Fulfillment team |
| **Assigned to** | `u_assigned_to` | Reference | `sys_user` | Assigned agent |

### Choice Field Dependencies
Configured via Dictionary Entry with `Use dependent field` enabled on `Category`:
* **Network** -> Wi-Fi
* **Hardware** -> Projector
* **Access** -> Forgot Password
* **Performance** -> Slow Computer

---

## ⚡ Workflow Logic (Flow Designer)

* **Flow Name:** `Auto Classify School IT Tickets`
* **Application Scope:** Global
* **Trigger Condition:** Record Created on `u_incident_workflow` where `Category` is empty.
