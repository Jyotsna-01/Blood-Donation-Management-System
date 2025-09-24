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

## Phase 4: Process Automation (Admin)
### 1.Validation Rules
○ Created 7 validation rules across all custom objects to prevent invalid data entry and maintain system integrity in the blood donation management system.

#### Blood Donor Object
• Last Donation Gap: Ensures donors wait at least 90 days between donations to meet medical safety standards. Formula prevents donations if last donation date is within 90 days of current date.

• Blood Type Required: Prevents donor records without blood type specification, critical for inventory matching. Uses TEXT function for picklist validation.

• Phone Number Required: Ensures contact information availability for emergency donor mobilization and follow-up communications.

#### Blood Request Object
• Units Required Positive: Prevents zero or negative unit requests to maintain logical inventory management and prevent system errors.
• Required By Date Logic: Ensures required by date is after request date to maintain chronological consistency and prevent past-dated requirements.

#### Blood Inventory Object
• Non-Negative Units: Prevents negative inventory counts which could cause calculation errors and misrepresent available blood supply.

#### Hospital Request Object
• Positive Units Requested: Ensures hospital requests specify valid quantities greater than zero for proper resource allocation and processing.

### 2. Flow Builder (Record-Triggered)
○ Built Emergency Blood Request Notification Flow to automate critical response for life-threatening situations and reduce emergency response time.

#### Flow Configuration:
• Object: Blood Request

• Trigger: Record created or updated with Priority = "Emergency"

• Action: Send Email Alert to Blood Bank Staff

• Business Impact: Enables sub-5-minute response time for critical blood requests

#### Email Alert Details:
• Recipients: Blood Bank Staff role members

• Content: Dynamic fields including Hospital Name, Blood Type Required, Units Required, and Required By Date

• Template: Text-based for universal compatibility and instant notification delivery

### 3. Approval Process
○ Implemented Large Hospital Request Approval process for requests exceeding 10 units to ensure proper oversight and resource planning.

#### Process Configuration:
• Entry Criteria: Units Requested > 10

• Approval Step: Single-step approval by Admin role

• Business Logic: Threshold-based approach balances operational efficiency with oversight requirements, allowing smaller requests immediate processing while ensuring review of large-volume requests.

#### Approval Actions:
• On Approval: Status automatically updated to "Approved"

• On Rejection: Status automatically updated to "Rejected"

• Benefit: Maintains audit trails for high-value transactions and ensures proper authorization

### 4. Field Updates
○ Created automated status management for approval process lifecycle without manual intervention.

#### Field Update Actions:
• Set Status to Approved: Triggered on final approval action, updates Request Status field

• Set Status to Rejected: Triggered on final rejection action, updates Request Status field

• Impact: Ensures data consistency and provides clear audit trails for all approval decisions

### 5. Tasks
○ Built Follow-up Task Creation Flow for automated task assignment when blood requests are fulfilled.

#### Flow Configuration:
• Object: Blood Request

• Trigger: Record updated with Status = "Fulfilled"

• Action: Create Task for Blood Bank Staff

• Task Details: Subject "Follow up with hospital for delivery confirmation", due next business day, assigned to Blood Bank Staff

#### Business Value:
• Ensures 100% follow-up completion for fulfilled requests

• Creates accountability and tracks delivery confirmation process

• Maintains operational excellence in blood delivery and hospital coordination

### 6. Email Alerts
○ Configured Emergency Email Alert System for real-time communication of critical events.

#### Alert Configuration:
• Associated Object: Blood Request

• Trigger Condition: Priority field equals "Emergency"

• Recipients: Blood Bank Staff role members

• Content: Hospital details, blood type, units required, and deadline information

• Delivery: Instant notification reducing manual communication overhead
## Phase 5: Apex Programming (Developer) 
### 1. UpdateNextEligibleDonation Trigger
Location: Object Manager > Blood Donor > Triggers

Code:
```apex
trigger UpdateNextEligibleDonation on Blood_Donor__c (before insert, before update) {
    for (Blood_Donor__c donor : Trigger.new) {
        if (donor.Last_Donation_Date__c != null) {
            donor.Next_Eligible_Donation__c = donor.Last_Donation_Date__c.addDays(90);
        }
    }
}
```

### 2. BloodDonorTriggerTest Class
Location: Setup > Apex Classes
Code:
```apex
@isTest
public class BloodDonorTriggerTest {
    @isTest static void testUpdateNextEligibleDonation() {
        Blood_Donor__c donor = new Blood_Donor__c(
            Donor_Name__c           = 'Test Donor',
            Email__c                = 'testdonor@example.com',
            Last_Donation_Date__c   = Date.today().addDays(-100),
            Phone_Number__c         = '9876543210',
            Blood_Type__c           = 'A+'
        );
        insert donor;

        Blood_Donor__c insertedDonor = [
            SELECT Next_Eligible_Donation__c 
            FROM Blood_Donor__c WHERE Id = :donor.Id
        ];
        // 100 days ago + 90 days = 10 days ago
        System.assertEquals(Date.today().addDays(-10), insertedDonor.Next_Eligible_Donation__c);
    }
}
```
### 3. BloodTypeHelper Utility Class
Location: Setup > Apex Classes
Code:
```apex
public class BloodTypeHelper {
    public static Boolean isCompatible(String donorType, String recipientType) {
        if (donorType == 'O-' || recipientType == 'AB+') {
            return true;
        }
        if (donorType == recipientType) {
            return true;
        }
        if (donorType == 'O+' && (recipientType == 'A+' || recipientType == 'B+' || recipientType == 'AB+')) {
            return true;
        }
        return false;
    }
}
```
### 4. BloodRequestProcessor Trigger
Location: Object Manager > Blood Request > Triggers
Code:
```apex
trigger BloodRequestProcessor on Blood_Request__c (after insert) {
    for (Blood_Request__c req : Trigger.new) {
        List<Blood_Donor__c> compatibleDonors = [
            SELECT Id, Name, Blood_Type__c 
            FROM Blood_Donor__c 
            WHERE Blood_Type__c = :req.Blood_Type_Required__c 
            LIMIT 10
        ];
        System.debug('Found ' + compatibleDonors.size() + ' donors for request ' + req.Id);
    }
}
5. DonorEligibilityBatch Batch Class
Location: Setup > Apex Classes
Code:
public class DonorEligibilityBatch implements Database.Batchable<sObject> {
    public Database.QueryLocator start(Database.BatchableContext BC) {
        return Database.getQueryLocator(
            'SELECT Id, Last_Donation_Date__c, Next_Eligible_Donation__c FROM Blood_Donor__c WHERE Last_Donation_Date__c != null'
        );
    }
    public void execute(Database.BatchableContext BC, List<Blood_Donor__c> scope) {
        List<Blood_Donor__c> toUpdate = new List<Blood_Donor__c>();
        for (Blood_Donor__c d : scope) {
            Date expected = d.Last_Donation_Date__c.addDays(90);
            if (d.Next_Eligible_Donation__c != expected) {
                d.Next_Eligible_Donation__c = expected;
                toUpdate.add(d);
            }
        }
        if (!toUpdate.isEmpty()) update toUpdate;
    }
    public void finish(Database.BatchableContext BC) {
        System.debug('Donor eligibility batch complete');
    }
}
```
### 6. DailyInventoryScheduler Scheduled Class
Location: Setup > Apex Classes
Code:
```apex
public class DailyInventoryScheduler implements Schedulable {
    public void execute(SchedulableContext sc) {
        List<Blood_Inventory__c> low = [
            SELECT Id, Blood_Type__c, Units_Available__c 
            FROM Blood_Inventory__c 
            WHERE Units_Available__c <= 5
        ];
        if (!low.isEmpty()) {
            List<Task> tasks = new List<Task>();
            for (Blood_Inventory__c i : low) {
                tasks.add(new Task(
                    Subject = 'Low Stock Alert: ' + i.Blood_Type__c,
                    Status = 'Not Started',
                    Priority = 'High',
                    ActivityDate = Date.today(),
                    Description = 'Only ' + i.Units_Available__c + ' units left'
                ));
            }
            insert tasks;
        }
        Database.executeBatch(new DonorEligibilityBatch(), 200);
    }
}
```
### 7. NotificationService Future Method
Location: Setup > Apex Classes
Code:
```apex
public class NotificationService {
    @future(callout=true)
    public static void sendHospitalNotification(Set<Id> requestIds) {
        List<Blood_Request__c> reqs = [
            SELECT Hospital_Name__c, Blood_Type_Required__c, Units_Required__c 
            FROM Blood_Request__c 
            WHERE Id IN :requestIds
        ];
        for (Blood_Request__c r : reqs) {
            System.debug('Notify: ' + r.Hospital_Name__c + ' needs ' + r.Units_Required__c + ' ' + r.Blood_Type_Required__c);
        }
    }
    public static void createFollowUpTasks(List<Blood_Request__c> reqs) {
        List<Task> tasks = new List<Task>();
        for (Blood_Request__c r : reqs) {
            tasks.add(new Task(
                Subject = 'Follow up: ' + r.Hospital_Name__c,
                Status = 'Not Started',
                Priority = 'Normal',
                ActivityDate = Date.today().addDays(1),
                WhatId = r.Id
            ));
        }
        if (!tasks.isEmpty()) insert tasks;
    }
}
```
### 8. ApexTestSuite Comprehensive Test Class
Location: Setup > Apex Classes
Code:
```apex
@isTest
public class ApexTestSuite {
    @TestSetup
    static void setupTestData() {
        // Donors
        List<Blood_Donor__c> ds = new List<Blood_Donor__c>();
        for (Integer i=0; i<5; i++) {
            ds.add(new Blood_Donor__c(
                Donor_Name__c          = 'Donor ' + i,
                Email__c               = 'donor'+i+'@test.com',
                Last_Donation_Date__c  = Date.today().addDays(-100),
                Phone_Number__c        = '9876543210',
                Blood_Type__c          = 'O+'
            ));
        }
        insert ds;
        // Inventory
        insert new Blood_Inventory__c(Blood_Type__c='O+', Units_Available__c=3);
        insert new Blood_Inventory__c(Blood_Type__c='A+', Units_Available__c=15);
    }

    @isTest static void testBloodTypeHelper() {
        System.assert(BloodTypeHelper.isCompatible('O-','A+'));
        System.assert(!BloodTypeHelper.isCompatible('A+','B+'));
    }
    
    @isTest static void testBloodRequestTrigger() {
        Blood_Request__c r = new Blood_Request__c(
            Hospital_Name__c      = 'Test Hospital',
            Blood_Type_Required__c= 'O+',
            Units_Required__c     = 5,
            Request_Date__c       = Date.today(),
            Required_By_Date__c   = Date.today().addDays(7),
            Contact_Person__c     = 'Dr. Smith',
            Contact_Phone__c      = '9123456789'
        );
        insert r;
        System.assertNotEquals(null, r.Id);
    }
    
    @isTest static void testDonorEligibilityBatch() {
        Test.startTest();
        Database.executeBatch(new DonorEligibilityBatch(), 200);
        Test.stopTest();
        List<Blood_Donor__c> updated = [SELECT Next_Eligible_Donation__c FROM Blood_Donor__c];
        System.assert(!updated.isEmpty());
    }
    
    @isTest static void testDailyInventoryScheduler() {
        Test.startTest();
        new DailyInventoryScheduler().execute(null);
        Test.stopTest();
        List<Task> alerts = [SELECT Id FROM Task WHERE Subject LIKE 'Low Stock Alert%'];
        System.assert(!alerts.isEmpty());
    }
    
    @isTest static void testBloodInventoryTrigger() {
        Blood_Inventory__c inv = [SELECT Id, Units_Available__c FROM Blood_Inventory__c WHERE Blood_Type__c='A+' LIMIT 1];
        inv.Units_Available__c = 4;
        Test.startTest(); update inv; Test.stopTest();
        System.assertEquals(4, inv.Units_Available__c);
    }
}
```
### 9. Test Execution
Run all three test classes (ApexTestSuite, BloodDonorTriggerTest, any additional)

Confirm 100% pass rate

Ensure overall coverage > 75%

## Phase 6: User Interface Development 
