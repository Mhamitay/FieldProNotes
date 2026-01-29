---
tags:
  - System-Overview
  - Core-Domain
  - Business-Intelligence
  - Technical-Architecture
aliases: []
---

# RiseFSM: Field Service Management System

## 1. Business Context & Industry Challenges

### 1.1. What is RiseFSM?
RiseFSM (Rise Field Service Management) is a SaaS (Software-as-a-Service) platform designed to optimize and digitize the operations of businesses that deploy technicians, engineers, or specialists to customer locations. This includes industries like HVAC, telecommunications, plumbing, electrical, medical equipment servicing, and renewable energy installation.

### 1.2. Core Industry Challenges Solved
Before RiseFSM, companies in this space typically relied on a chaotic mix of:
*   **Phone calls & paper dispatches:** Inefficient, prone to errors, and lacking visibility.
*   **Manual scheduling:** Dispatchers using whiteboards or simple calendars, leading to poor technician utilization, long customer wait times, and high fuel costs.
*   **Paper-based reporting:** Job sheets, invoices, and signatures collected on clipboards, leading to data entry delays, lost information, and slow billing cycles.
*   **Lack of real-time data:** Management was blind to what was happening in the field until the end of the day, making proactive decision-making impossible.

RiseFSM solves these by providing a single, integrated digital platform that connects the office, the field, and the customer, ensuring **right first-time service, improved efficiency, and faster revenue capture.**

## 2. Detailed Real-World Workflows

### 2.1. Workflow 1: Scheduling & Dispatch (The "Sunrise" Process)
**Business Goal:** Efficiently assign the right technician to the right job at the right time.

1.  **Trigger:** A new `WorkOrder` is created from a `Customer` call, a scheduled `PreventativeMaintenance` plan, or an automated `Contract` trigger.
2.  **Scheduling:** The Dispatcher views the **Schedule Board**. The system intelligently suggests available Technicians based on:
    *   **Skill Matching:** Does the `Technician` have the `Skill` and `Certification` required for the `WorkOrder` type?
    *   **Location Proximity:** Who is closest to the `CustomerSite`, minimizing travel time and cost?
    *   **Parts Availability:** Does the technician's van stock (`Inventory`) contain the likely required parts?
    *   **Time Availability:** Who has capacity within the promised time window?
3.  **Dispatch:** The Dispatcher drags and drops the `WorkOrder` onto a `Technician`'s timeline. The system automatically:
    *   Sends a push notification to the technician's mobile app with job details.
    *   Updates the `WorkOrder.status` from `Created` to `Dispatched`.
    *   Sends an SMS/email to the `Customer` with an ETA and technician details.

### 2.2. Workflow 2: Job Execution & Mobile Workflow (The "In-Field" Process)
**Business Goal:** Execute the job professionally, capture all necessary data on the first visit, and get paid faster.

1.  **Travel:** The `Technician` reviews the `WorkOrder` on their mobile app and uses integrated GPS for navigation.
2.  **On-Site:** Upon arrival, the technician taps **"Start Job"**, updating the `WorkOrder.status` to `InProgress`. This logs the precise arrival time for SLA tracking.
3.  **Execution:**
    *   The technician follows the `Workflow` checklist on their device.
    *   They scan parts used from their van stock (`Inventory`), which automatically deducts inventory levels and adds them to the `Invoice`.
    *   They record time spent on different tasks (`Labor`).
    *   They attach `Evidence`:
        *   **Photos/Videos** of the issue, repair, and installed serial numbers.
        *   **Digital Signatures** from the customer for approval.
        *   **Notes** on what was found and done.
4.  **Completion:** The technician taps **"Complete Job"**. The system:
    *   Updates the `WorkOrder.status` to `Completed`.
    *   Generates a PDF `Invoice` or `Service Report` on the spot for the customer.
    *   Syncs all data, including the updated `Inventory` list, back to the office in real-time.

### 2.3. Workflow 3: Invoicing & Knowledge Capture (The "Sunset" Process)
**Business Goal:** Close the financial loop and learn from the job to improve future service.

1.  **Automated Invoicing:** For simple jobs, the `Invoice` generated in the field is automatically sent to the accounting system (e.g., QuickBooks, Xero) via an integration. No office staff data entry is required.
2.  **Review & Approval:** For more complex jobs, a `Supervisor` reviews the `Evidence` and `Labor` entries from the mobile app before the `Invoice` is finalized and sent.
3.  **Knowledge Capture:** The completed `WorkOrder` becomes a permanent part of the `Asset`'s and `Customer`'s history. This creates a valuable knowledge base for diagnosing recurring issues.

## 3. Entity Explanations (Business Data Model)

| Entity | Business Definition & Purpose | Key Attributes (Business Context) |
| :--- | :--- | :--- |
| **Customer** | A company or person who contracts for services. The source of revenue. | `Name`, `BillingAddress`, `PaymentTerms`, `AccountManager` |
| **CustomerSite** | A physical location where work is performed. A Customer can have multiple Sites. | `Address`, `ContactPerson`, `AccessInstructions` (e.g., "Gate code: 1234") |
| **Asset** | A piece of equipment located at a `CustomerSite` that is serviced. The "what" of a job. | `SerialNumber`, `InstallationDate`, `Model`, `ServiceHistory` (links to past WorkOrders) |
| **Contract** | A service-level agreement (SLA) defining response times, pricing, and covered services. | `Type` (e.g., Platinum, Gold), `CoveredParts`, `ResponseTimeSLA`, `RenewalDate` |
| **WorkOrder (WO)** | The central entity. A request for work to be done. A single job. | `Status` (e.g., Created, Dispatched, InProgress, Completed), `Priority`, `ScheduledDate`, `IssueDescription` |
| **Technician** | The field employee who performs the work. A primary resource to be optimized. | `Name`, `SkillSet`, `Certifications`, `CurrentLocation` (GPS), `Status` (e.g., Available, EnRoute, OnJob) |
| **Skill** | A definable capability required to complete certain `WorkOrders`. | `Name` (e.g., "Fiber Optic Splicing", "Boiler Repair Level 3"), `IsCertificationRequired` |
| **Inventory** | Trackable parts and materials, both in the warehouse and in technician vans. | `PartNumber`, `Description`, `QuantityOnHand`, `Cost`, `Price`, `VanStockLevel` |
| **Invoice** | The bill sent to the customer for labor, parts, and other charges from a `WorkOrder`. | `TotalAmount`, `Status` (e.g., Draft, Sent, Paid), `LaborLines`, `PartLines` |
| **Workflow** | A predefined checklist or process for a specific type of job. Ensures consistency and compliance. | `Name` (e.g., "Annual HVAC Maintenance"), `Steps` (e.g., "Check refrigerant pressure", "Clean condenser coils") |

## 4. Real-World Examples

### 4.1. Example: Emergency Repair for a Restaurant
*   **Scenario:** A `Customer` ("Downtown Diner") calls with a broken walk-in freezer.
*   **RiseFSM in Action:**
    1.  The office admin creates a high-`Priority` `WorkOrder` linked to the "Downtown Diner" `CustomerSite` and the "Freezer" `Asset`.
    2.  The **Schedule Board** automatically highlights `Technician` Maria, who is certified in commercial refrigeration and is currently 5 minutes away.
    3.  Maria receives the job on her app, sees the priority, and navigates directly to the site.
    4.  On-site, she uses a `Workflow` for "Emergency Refrigeration Diagnosis." She finds a faulty compressor.
    5.  She scans the barcode on the new compressor from her van stock (`Inventory`). The system automatically adds it to the `Invoice`.
    6.  She takes a `Photo` of the installed part and gets a digital `Signature` from the manager on her tablet.
    7.  She marks the job `Completed`. The `Invoice` is generated instantly and emailed to the restaurant manager before she even leaves the parking lot.
*   **Business Value:** Minimized downtime for the customer, correct part was available, instant invoicing improves cash flow.

### 4.2. Example: Preventative Maintenance for a Manufacturing Plant
*   **Scenario:** A `Contract` with "ACME Manufacturing" stipulates quarterly servicing of their industrial air compressors.
*   **RiseFSM in Action:**
    1.  The system **automatically generates** a `WorkOrder` 30 days before the scheduled service date, linked to the specific `Asset` ("Compressor C-12").
    2.  The `WorkOrder` is pre-populated with the detailed `Workflow` checklist for that asset model.
    3.  The dispatcher assigns it to John, a specialist technician.
    4.  John performs each step on the checklist, recording measurements and photos directly into the app.
    5.  He notes a minor wear-and-tear issue, recommends a parts replacement, and creates a follow-up `WorkOrder` for the repair, which is immediately sent to the customer for approval.
*   **Business Value:** Guaranteed contract compliance, upsell opportunities identified, and a complete digital history of the asset is maintained.

## 5. Technical Architecture (Supporting Business Needs)

RiseFSM is built on a modern, cloud-native microservices architecture to ensure **scalability, reliability, and security**.

*   **Frontend:**
    *   **Web Portal (React.js):** For dispatchers, managers, and administrators. Used for complex data views like the Schedule Board and reporting dashboards.
    *   **Mobile App (React Native):** For technicians. Designed for offline-first operation, ensuring functionality in areas with poor cell service. Data syncs automatically when a connection is restored.

*   **Backend (Microservices - Node.js/Python/.NET):**
    *   **Scheduling Service:** Handles the complex logic of matching Technicians to WorkOrders.
    *   **Workflow Engine:** Orchestrates the state (`Status`) of a `WorkOrder` and manages `Workflow` checklists.
    *   **Inventory Service:** Tracks `Inventory` levels across warehouses and vans in real-time.
    *   **Notification Service:** Manages all outbound communications (SMS, Email, Push) to technicians and customers.
    *   **Reporting Service:** Aggregates data for the BI dashboards.

*   **Data Storage:**
    *   **PostgreSQL:** Primary relational database for structured data (Customers, WorkOrders, Assets). Ensures data integrity and complex queries.
    *   **MongoDB:** Used for unstructured data like uploaded `Evidence` (photos, videos) and audit logs.
    *   **Redis:** Used for caching (e.g., making the Schedule Board load quickly) and managing real-time technician location data.

*   **Cloud Infrastructure (AWS):**
    *   Hosted on AWS for high availability and global scalability.
    *   Uses **S3** for storing documents and images.
    *   Uses **Lambda** for serverless functions (e.g., generating PDF invoices on-demand).

## 6. Business Intelligence & Metrics

The system transforms operational data into actionable business insights.

### 6.1. Key Performance Indicators (KPIs)
*   **First-Time Fix Rate (FTFR):** % of jobs completed on the first visit. *(Directly impacts customer satisfaction and reduces costs)*.
*   **Technician Utilization:** % of a technician's paid time spent on billable work vs. travel or downtime. *(Measures productivity and scheduling efficiency)*.
*   **Mean Time to Repair (MTTR):** Average time from `WorkOrder` creation to completion. *(Measures team efficiency)*.
*   **Schedule Adherence:** % of jobs started within the promised time window. *(Key for customer satisfaction and SLA compliance)*.
*   **Revenue per Technician:** Total invoiced amount per technician over a period. *(Measures profitability and performance)*.

### 6.2. Dashboards & Reports
*   **Operational Dashboard (For Dispatchers):** Real-time view of all technicians, active jobs, and SLA statuses.
*   **Financial Dashboard (For Managers):** Shows KPIs like FTFR, MTTR, revenue, and profitability trends over time.
*   **Inventory Report:** Identifies slow-moving stock, predicts part demand based on upcoming PMs, and tracks inventory shrinkage.
*   **Asset Health Report:** For customers, provides a overview of all their assets, service history, and predicted maintenance needs—a valuable value-add report.