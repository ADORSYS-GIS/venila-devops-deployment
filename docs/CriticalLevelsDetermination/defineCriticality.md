Let's delve into what constitutes "critical" functionality in any application. We'll break it down into steps and cover each aspect thoroughly.

### Step 1: Gather Input from Stakeholders

**1.1 Identify Stakeholders**
- **Internal Stakeholders:** Product managers, developers, quality assurance (QA) teams, IT operations, and customer support.
- **External Stakeholders:** Customers, end-users, and possibly partners or regulatory bodies.

**1.2 Conduct Interviews or Surveys**
- **Questions for Product Managers:** What features are essential for the application's primary function? What are the business goals related to these features?
- **Questions for Developers and QA Teams:** What components of the system are most critical from a technical standpoint? What are the potential failure points?
- **Questions for Customer Support:** What issues do users most frequently report? Which features are users most dependent on?
- **Questions for End-Users:** Which features do users consider indispensable for their daily tasks?

**1.3 Analyze Feedback**
- Aggregate responses to identify common themes and critical features.
- Look for features mentioned by multiple stakeholders or those crucial to business operations.

### Step 2: Define Criteria for "Critical" Functionality

**2.1 Uptime Requirements**
- **Mission-Critical Systems:** Require near 100% uptime. Any downtime directly impacts business operations and may lead to significant financial loss or damage to reputation.
- **High Availability Systems:** Require high uptime (e.g., 99.9%). Downtime may cause inconvenience but can be managed with some tolerance.
- **Moderate Availability Systems:** Downtime is less critical and often acceptable during off-peak hours or maintenance windows.

**2.2 Performance Metrics**
- **Response Time:** Critical features should meet performance benchmarks such as response time or transaction processing time.
- **Throughput:** The number of transactions or operations the system can handle in a given time frame should meet business requirements.
- **Scalability:** Ability to handle increasing loads or user activity without performance degradation.

**2.3 User Impact**
- **Business Continuity:** Features whose failure would halt essential business processes or operations.
- **Customer Satisfaction:** Features that significantly impact the user experience, and their failure leads to customer dissatisfaction or loss.
- **Legal and Compliance:** Features that are critical for meeting regulatory requirements or compliance standards.

### Step 3: Document Implications of Downtime or Performance Degradation

**3.1 Uptime Implications**
- **Business Impact:** Quantify potential financial losses, operational disruptions, and customer churn.
- **Reputation Impact:** Assess how downtime affects brand reputation and customer trust.

**3.2 Performance Degradation Implications**
- **User Experience:** Detail how performance issues impact user satisfaction and efficiency.
- **Operational Efficiency:** Identify any loss in productivity or increased operational costs due to performance issues.

**3.3 Feature-Specific Analysis**
- **Example 1: Payment Processing System**
    - **Downtime Implications:** Direct financial loss, potential legal issues, loss of customer trust.
    - **Performance Degradation Implications:** Slow transactions may result in abandoned purchases and customer frustration.
- **Example 2: User Authentication System**
    - **Downtime Implications:** Inability to log in may block user access, impacting operations and customer service.
    - **Performance Degradation Implications:** Slow authentication processes may lead to user dissatisfaction and increased support requests.

### Step 4: Acceptance Criteria

**4.1 Definition of Critical**
- **Approved Definition:** A "critical" functionality is defined as any feature or system component that, if disrupted, would lead to significant financial loss, operational disruption, user dissatisfaction, or compliance issues.

**4.2 Document Approval**
- Ensure the definition and criteria are reviewed and approved by all relevant stakeholders, including business leaders, technical teams, and end-users.

**4.3 Regular Review**
- Implement a process for periodic review and update of the criticality definition to reflect changes in business priorities, user needs, and technology.

**4.4 Communication Plan**
- Clearly communicate the defined criteria and implications to all relevant parties, including documentation in internal knowledge bases and training materials for teams involved in incident management and support.

By following these steps, you’ll have a comprehensive understanding of what "critical" means for your application, including how to measure and manage critical functionality effectively.