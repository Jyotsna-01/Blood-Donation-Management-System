# Blood Donation Management System 
## Problem Statement

Blood banks, hospitals, and patients frequently struggle with the timely availability and efficient distribution of safe blood. 
Traditional systems rely on manual donor records, paperwork, and phone-based coordination, leading to delayed responses during emergencies,
lack of real-time inventory visibility, and missed opportunities for repeat donations. Donors are often not engaged proactively,
resulting in limited retention and low participation during crises. Hospitals face challenges in identifying suitable blood types
quickly or connecting with donors and nearby blood banks, which can put patient lives at risk. There is a critical need for a centralized,
intelligent blood donation management solution that enables real-time tracking of inventory, automates donor engagement, manages urgent requests,
and streamlines collaboration between donors, blood banks, and hospitals—ensuring faster, safer, and more reliable blood availability for all.

## Phase 1: Problem Understanding & Industry Analysis
### 1. Requirement Gathering
○ Identified key needs for blood banks, hospitals, and donors: donor registration, donation history tracking, blood inventory management, urgent hospital requests, and automated donor engagement.

### 2. Stakeholder Analysis
○ Donors: Individuals willing to donate blood

○ Blood Bank Staff: Manage donor information, inventory, and coordinate donations

○ Hospital Staff: Request blood during emergencies and routine cases

○ Administrators: Oversee compliance, quality, and inventory levels

○ Volunteers/NGOs: Assist organizing donor drives and awareness

### 3. Business Process Mapping
○ Current State: Manual donor tracking, telephonic requests, and paper inventory cause delays and shortages

○ Proposed Flow:

      Donor registration → Eligibility verification → Scheduled donation
  
      Donation completion → Inventory update
  
      Hospital request → Automated matching → Donor/stock allocation

○ Automated donor reminders and feedback collection

### 4. Industry-Specific Use Case Analysis
○ Challenges in Healthcare: Ensuring safe and timely blood availability, rapid response to emergencies, maintaining compliant donor history, and minimizing wastage due to expiry

○ Relevant Use Case: Real-time inventory tracking and urgent donor mobilization help address seasonal shortages and disaster scenarios

### 5. AppExchange Exploration
○ Explored existing Salesforce healthcare and donor management apps; found most lack custom blood inventory, real-time emergency matching, and integrated donor engagement for repeat donations—validating need for a dedicated, extensible solution.

## Phase 2: Org Setup & Configuration
### 1. Salesforce Editions
○ Used Developer Edition Developer Org (free dev org).

○ Full Salesforce platform capabilities for healthcare system development.
### 2. Company Profile Setup
○ Go to Company Settings → add company info, Indian Standard Time (IST).
○ Set currency to INR for local healthcare operations.

### 3. Business Hours & Holidays
○ Define 24/7 working hours (12:00 AM – 11:59 PM all days).
○ Healthcare operations don't follow traditional business hours.

### 4. Fiscal Year Settings
○ Standard (Jan–Dec) → good for healthcare reporting and blood drive planning.

### 5. User Setup & Licenses
○ Admin: System Administrator with Salesforce license.

○ Blood Bank Staff, Hospital Staff,  Volunteer Coordinator: Salesforce Platform license.

### 6. Profiles
○ Admin: System Administrator profile.

○ All Staff: Standard User profiles initially.

○ Future: Minimum Access + Permission Sets for better security.

### 7. Roles
○ Hierarchy: Admin → Blood Bank Staff/Hospital Staff/Volunteer Coordinator.

○ Admin sees all records, lateral roles maintain privacy.

### 8. Permission Sets
○ Plan: Specific permissions for custom objects and system functions.

### 9. Organization-Wide Defaults (OWD)
○ Blood Donor: Private (sensitive personal info).

○ Blood Request: Private (confidential medical requests).

○ Blood Inventory: Public Read Only (all staff need visibility).

○ Hospital Request: Private (confidential external communications).

### 10. Sharing Rules
○ "Blood Bank Staff Full Access" → Read/Write access to all donor records.

○ Role hierarchy provides Admin access to all subordinate records.

### 11. Login Access Policies
○ Session timeout: 2 hours (development-friendly).

○ Password policies: No expiration, no complexity (testing ease).

○ IP restrictions: None (access from anywhere).

### 12. Dev Org Setup
○ Developer Edition confirmed with full platform access.

○ Custom objects, workflows, automation features available.

### 13. Sandbox Usage
○ Not available in Developer Edition.

○ All development done directly in developer environment.

### 14. Deployment Basics
○ All setup done directly in Developer Org.

○ Future: Change sets for production deployment.

## Phase 3: Data Modeling & Relationships
### 1. Standard & Custom Objects
Created 4 core custom objects:

■ Blood Donor → individual donor information.

■ Blood Request → internal departmental requests.

■ Blood Inventory → available blood units and stock.

■ Hospital Request → external hospital blood requests.

### 2. Fields
○ Blood Donor: Donor Name, Blood Type (A+/A-/B+/B-/AB+/AB-/O+/O-), Phone, Email, DOB, Last Donation Date, Status, Address.

○ Blood Request: Hospital Name, Blood Type Required, Units Required, Priority (Emergency/High/Medium/Low), Status, Request Date, Required By Date, Contact Person, Contact Phone.

○ Blood Inventory: Blood Type, Units Available, Location, Last Updated, Status (In Stock/Low Stock/Out of Stock/Reserved).

○ Hospital Request: Hospital Name, Address, Blood Type Needed, Units Requested, Urgency Level (Critical/High/Normal), Status, Doctor Name, Doctor Phone.

### 3. Record Types
○ Current: Single record types for simplicity.

○ Future: Multiple record types (Regular Donor, VIP Donor, Emergency Request, Routine Request).

### 4. Page Layouts
○ Current: Default layouts with all custom fields.

○ Future: Role-specific layouts for optimized workflows.

### 5. Compact Layouts
○ Current: System-generated layouts.

○ Future: Custom compact layouts for mobile optimization.

### 6. Schema Builder
○ Used for visual verification of object relationships.

○ Confirmed proper data flow between all 4 objects.

### 7. Lookup vs Master-Detail vs Hierarchical Relationships
○ Blood Request → Blood Donor (Lookup): Multiple requests per donor.

○ Blood Request → Blood Inventory (Lookup): Connect requests to stock.

○ Hospital Request → Blood Inventory (Lookup): Allocate inventory to hospitals.

○ Avoided Master-Detail: Preserves historical data integrity.

### 8. Junction Objects
○ Current: None implemented (direct one-to-many relationships).

○ Future: Blood Donors ↔ Blood Drives, Staff ↔ Inventory allocation.

### 9. External Objects
○ Current: None implemented.

○ Future: Hospital systems, government health databases, lab systems, regional blood bank networks.
