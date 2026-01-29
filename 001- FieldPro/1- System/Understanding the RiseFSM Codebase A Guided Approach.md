To effectively understand this complex field service management system for oil & gas, follow this structured approach. Check off items as you go through them to build a comprehensive understanding of both the technical implementation and business domain.

---

## ✅ Core Business Workflows

- [ ] **Job Lifecycle Management**
  - [ ] Review `/jobs/edit` to understand job creation
  - [ ] Examine `JobStatusEnum` to understand the status progression
  - [ ] Study `JobUser` relationships to understand resource assignment
  - [ ] Review ticket handling in `/tickets/index`

- [ ] **Workflow Specializations**
  - [ ] Study the Wireline workflow (`Const.Workflow.Wireline`)
  - [ ] Analyze the Well Intervention workflow (`Const.Workflow.WellIntervention`)
  - [ ] Explore the Completion workflow (`Const.Workflow.Completion`)
  - [ ] Compare the different call sheet implementations for each workflow

- [ ] **Asset & Equipment Management**
  - [ ] Review asset classes (`Asset`, `AssetThirdParty`)
  - [ ] Understand equipment categorization through `AssetType` and `AssetSubType`
  - [ ] Study the load out process in `/jobs/loadout`
  - [ ] Examine BHA (Bottom Hole Assembly) configuration in `/bhas/index`

---

## 🏛 Architecture Components

- [ ] **Multi-tenancy Implementation**
  - [ ] Analyze how tenant isolation works with `TenantKey`
  - [ ] Study the claims-based approach in `GetLoginDetails` method
  - [ ] Examine how tenant-specific configurations are applied

- [ ] **Security Model**
  - [ ] Review role-based security in `Security.Roles` class
  - [ ] Understand permission checking with `IsInAnyRoles`
  - [ ] Study the device registration process in `CreateDeviceAndNotify`

- [ ] **Offline Capabilities**
  - [ ] Compare online/offline controllers (`IndexOnline.cshtml.cs` vs `IndexOffline.cshtml.cs`)
  - [ ] Study synchronization logic in `Changes<T>` models
  - [ ] Analyze device enablement via `device.OfflineEnabled`

- [ ] **Data Access Patterns**
  - [ ] Review base domain classes (`DomainBase`, `ChangesDomainBase`)
  - [ ] Study SQL parameter handling in `DataModelBase`
  - [ ] Understand the entity relationship model through DB context configuration

---

## 🎨 UI Structure & Navigation

- [ ] **Tab-Based Navigation**
  - [ ] Study the tab generation methods (`GetJobTabs`, `GetAssetTabs`, etc.)
  - [ ] Understand workflow-specific tab variations
  - [ ] Examine role-based tab visibility

- [ ] **Field Data Collection**
  - [ ] Explore specialized data entry for pipe tally in `/jobs/pipetally`
  - [ ] Understand tool placement tracking in `/jobs/toolplacement`
  - [ ] Review job logging interfaces in `/jobs/joblog`

---

## 💰 Financial & Reporting Systems

- [ ] **Pricing & Invoicing**
  - [ ] Study the cost item model hierarchy
  - [ ] Understand quote generation via `/ticketquotes/index`
  - [ ] Review invoice generation logic and approvals

- [ ] **Financial Integration**
  - [ ] Look at export formats for accounting systems
  - [ ] Study currency conversion in revenue reports
  - [ ] Review tax calculation logic

- [ ] **Reporting Framework**
  - [ ] Examine reporting controllers in `API/Reporting`
  - [ ] Review revenue reporting views like `vrJobsRevenue.sql`
  - [ ] Study scheduled report functionality

---

## 🛢 Domain-Specific Features

- [ ] **Oil & Gas Well Data**
  - [ ] Look at LSD/NTS coordinate formats
  - [ ] Study well name and location tracking
  - [ ] Understand downhole vs. surface location distinction

- [ ] **Safety & Compliance**
  - [ ] Review incident reporting functionality
  - [ ] Understand non-conformance processes
  - [ ] Study certification tracking for personnel and equipment

---

## 🛠 Development Tools & Extensions

- [ ] **Cross-Platform Support**
  - [ ] Study hybrid TypeScript/C# implementation
  - [ ] Review type generation with `[GenerateTypeScript]` attributes
  - [ ] Understand the offline app architecture

- [ ] **Code Organization**
  - [ ] Map the project structure and responsibilities
  - [ ] Identify key extension points for customization
  - [ ] Review multi-workflow configurability

---

## 🚀 Getting Started Steps

1. [ ] Set up a test tenant in a development environment  
2. [ ] Create a sample job with all key information  
3. [ ] Follow the job lifecycle from creation through invoicing  
4. [ ] Test offline capabilities to understand synchronization  
5. [ ] Generate reports to see how data flows through the system  

---

By systematically exploring these areas, you'll gain both technical understanding and business domain knowledge of the RiseFSM system, enabling you to effectively maintain, extend, or troubleshoot the application.
