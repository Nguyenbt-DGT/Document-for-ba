# MPA User Story Templates

> **Nguồn:** Confluence Space VBKB – Manulife Vietnam  
> **Tổng hợp ngày:** 2026-07-12

---

## Mục lục

1. [Template 1: New Feature (Standard US)](#template-1-new-feature-standard-us)
2. [Template 2: New Feature – with UI Involve](#template-2-new-feature--with-ui-involve)
3. [Template 3: Enhancement](#template-3-enhancement)
4. [Template 4: Change Request](#template-4-change-request)
5. [Template 5: System Integration / 3rd Party](#template-5-system-integration--3rd-party)
6. [Template 6: Report / Dashboard / Export](#template-6-report--dashboard--export)
7. [Template 7: Notification Matrix](#template-7-notification-matrix)

---

## Template 1: New Feature (Standard US)

> **Confluence:** [Template User Story - New Feature (Standard US)](https://manulife-asia.atlassian.net/wiki/spaces/VBKB/pages/1624834110)

### Story Title : [New Feature] \<short meaningful title\>

### User Story

| Field | Value |
|---|---|
| **User story** | **As** a \<role / persona\>, **I want** \<capability/function\> **So that** \<business value/benefit\>. |
| **Actor:** | \<primary actor(s)\> |
| **Trigger:** | \<user/system trigger\> |
| **Pre-condition:** | \<state/config/data prerequisites\> |
| **Post-condition:** | \<state change/outcome/audit\> |

1. **Flow:** \<Figma flow link\>
2. **Screen demo:** \<Figma prototype link\>

### 1. Scope

- **In scope:** \<…\>
- **Out of scope:** \<…\>

### 2. Requirement

> List out all requirement/change required

#### R.01 \<Title\>

\<Description\>  
\<Sample if any\>

#### R.02 \<Title\>

\<Description\>  
\<Sample if any\>

#### R.03 \<Title\>

\<Description\>  
\<Sample if any\>

### 3. Business Rules & Validations (Optional)

> List only confirmed rules/fields.

| Rule ID | Rule Name | Business Description | Impacted flow | Example Scenario |
|---|---|---|---|---|
| BR-001 | \<...\> | \<policy/calc/threshold/validation rule\> Refer to AC.01, AC.0n & BR-001, BR-n | \<create/update/view/api\> | \<input → outcome\> |
| BR-\<...\> | \<...\> | \<...\> | \<...\> | \<...\> |

### 4. Impact and Risk (Optional)

1. Backend: \<new APIs/changed contracts/db schema/config\>
2. \<security risk if any\>
3. \<...\>

### 5. Non-Functional Requirements (Optional)

1. Performance/Throughput: \<...\>
2. Security/Privacy: \<...\>
3. Audit/Logging: \<...\>
4. Availability/Resilience/i18n/a11y: \<...\>

### 6. Dependencies / References (Optional)

1. \<… or N/A\>
2. \<…\>

### 7. Technical Note (Optional)

1. \<… or N/A\>
2. \<...\>

### 8. Acceptance Criteria

#### AC1. \<short title\>

**Given** \<precondition\> **and** \<additional context\>  
**When** \<user/system action\>  
**Then** \<expected outcome (UI/API/state/message)\> **And** \<additional expected result\>

#### AC(n). \<...\>

**Given** \<...\>  
**When** \<...\>  
**Then** \<...\>

---

## Template 2: New Feature – with UI Involve

> **Confluence:** [User story template (New feature - with UI involve)](https://manulife-asia.atlassian.net/wiki/spaces/VBKB/pages/1713308738)

### User Story

| Field | Value |
|---|---|
| **User story** | **As** a \<role / persona\>, **I want** \<capability/function\> **So that** \<business value/benefit\>. |
| **Actor:** | \<primary actor(s)\> |
| **Trigger:** | \<user/system trigger\> |
| **Pre-condition:** | \<state/config/data prerequisites\> |
| **Post-condition:** | \<state change/outcome/audit\> |

### A. Workflow

1. \<Activity diagram\>

### B. Figma

\<Figma prototype link\>

### B. Application

- \<CWS Mobile app / CWS web / Salesforce / CAS module / VNS / Doc engine / Alfresco / …\>

### C. Impact

- N/A

### D. Dependencies

- **Predecessor:** \<...\>
- **Successor:** \<...\>

### E. Story Description

#### E1. Screen navigation

> (if have multiple screens in the user story)

#### E2. Screen 1 specification

| UI screen | Component | Component Type | Function description | Editable | Mandatory | Default Value | Source and rules of data | Content (EN) | Content (VIE) |
|---|---|---|---|---|---|---|---|---|---|
| | | Text Input / Select / Button / Table / Toggle / DatePicker / Modal / File Upload / Link / Label / Alert message / Data | | \<Yes/No\> | \<Yes/No/NA\> | \<...\> | | | |

#### E3. Screen 2 specification

> (repeat table as needed)

### F. Business Rules & Validations

#### F1. Data Source Reference

| Data Field | Source System | OCDM table / CAS form / CAS table / CAS procedure | Column / Field | Transformation |
|---|---|---|---|---|
| | CAS form / CAS / OCDM | | | |

#### F2. Calculation Rules

> Note: Only display calculation; if field is not calculated and only has info of source data, then it is excluded.

| Calc ID | Field on UI | Formula | Source Input | Rule | Notes |
|---|---|---|---|---|---|
| | | | | Rounding rule … | |

#### F3. Business Validations

| Rule ID | Rule Name | Business Description | Impacted flow | Example Scenario |
|---|---|---|---|---|
| BR-\<US001\>-01 | \<...\> | \<policy/calc/threshold/validation rule\> Refer to AC.01, AC.0n & BR-001, BR-n | \<create/update/view/api\> | \<input → outcome\> |

### G. Messages List

> **Refer** sheet "\<...\>" - \<...\>.xlsx

| Message Code | Message Type | Message Content (ENG) | Message Content (VNM) | Description |
|---|---|---|---|---|
| MSG\<...\> | \<Toast message/Message/Error/Warning/Info/Success/...\> | \<...\> | \<...\> | \<...\> |

### H. Acceptance Criteria

> Notes:
> 1. Acceptance criteria in Gherkin format (GIVEN-WHEN-THEN) which include: happy cases, unhappy cases, exception cases, edge cases
> 2. Each AC should be displayed in separated code snippet box
> 3. AC không cần đề cập lại content EN, VIE, format text (bold, italic, color, etc.)
> 4. Make every AC testable
> 5. Keep AC independent
> 6. Do not make ACs too granular

#### AC1. \<short title\>

**Given** \<preconditions\>  
**When** \<user/system action\>  
**Then** \<expected outcome (UI/API/state/message)\>

#### AC(n). \<...\>

**Given** \<...\>  
**When** \<...\>  
**Then** \<...\>

### I. Risk (Optional)

1. \<functional risk\>
2. \<operational risk\>
3. \<security/privacy risk\>
4. \<UX/adoption risk\>
5. \<...\>

### J. Impact (Optional)

1. UI/UX: \<new screens/controls/flows impacted\>
2. Backend: \<new APIs/changed contracts/db schema/config\>
3. \<...\>

### K. Non-Functional Requirements (Optional)

1. Performance/Throughput: \<...\>
2. Security/Privacy: \<...\>
3. Audit/Logging: \<...\>
4. Availability/Resilience/i18n/a11y: \<...\>

### L. Related Document (Optional)

1. \<Confluence page / Figma board / API spec / Data dictionary / Jira links\>
2. \<...\>

### M. Technical Note (Optional)

1. \<constraints/assumptions/performance/SLA/limits\>
2. \<timezones/pagination/sorting/filtering conventions\>
3. \<error handling & retry idempotency\>
4. \<...\>

### N. Master Data (Optional)

1. \<reference tables/enums/status lifecycle\>
2. \<role & permission matrix\>
3. \<message catalog mapping\>
4. \<configuration keys & defaults\>
5. \<...\>

---

## Template 3: Enhancement

> **Confluence:** [User Story template - Enhancement](https://manulife-asia.atlassian.net/wiki/spaces/VBKB/pages/1592885886)

### User Story

| Field | Value |
|---|---|
| **User story** | **As** a \<role / persona\>, **I want** \<capability/function\> **So that** \<business value/benefit\>. |
| **Actor:** | \<primary actor(s)\> |
| **Trigger:** | \<user/system trigger\> |
| **Pre-condition:** | \<state/config/data prerequisites\> |
| **Post-condition:** | \<state change/outcome/audit\> |

1. **Flow:** \<Figma flow link\>
2. **Screen demo:** \<Figma prototype link\>

### 1. Current vs Target Behaviour

| Field | Value |
|---|---|
| **Current Behaviour:** | \<Describe current behaviour (as-is): steps, rules, and current pain points/defects\> |
| **Purpose/Gaps/Issues** | \<Describe purpose/gaps/issues: why enhance, impact, and what needs to be improved\> |
| **Target Behaviour:** | \<Describe target behaviour (to-be): expected flow, validation rules, and outcomes after enhancement\> |

### 2. Comparison

| As-is | To-Be |
|---|---|
| \<As-is description (1)\> | \<To-be description (1)\> |
| Description \<...\> | Description \<...\> |

### 3. \<...\> Screen

| Title Screen 1 | Title Screen 2 | Title Screen (n) |
|---|---|---|
| N/A | N/A | N/A |

#### \<...\> Specifications

| Component (EN) | Component (VNM) | Component Type | Editable | Mandatory | Default Value | Description |
|---|---|---|---|---|---|---|
| \<...\> | \<...\> | \<Text Input / Select / Button / Table / Toggle / DatePicker / Modal / File Upload / Link / Label / Alert\> | \<Yes/No\> | \<Yes/No\> | \<...\> | \<purpose/validation/format/behavior\> Refer to AC.01, AC.0n & BR-001, BR-n |

### 4. Acceptance Criteria

#### AC1. \<short title\>

**Given** \<preconditions\>  
**When** \<user/system action\>  
**Then** \<expected outcome (UI/API/state/message)\>

#### AC(n). \<...\>

**Given** \<...\>  
**When** \<...\>  
**Then** \<...\>

### 5. Business Rules & Validations (Optional)

| Rule ID | Rule Name | Business Description | Impacted flow | Example Scenario |
|---|---|---|---|---|
| BR-001 | \<...\> | \<policy/calc/threshold/validation rule\> Refer to AC.01, AC.0n & BR-001, BR-n | \<create/update/view/api\> | \<input → outcome\> |

### 6. Messages List

> **Refer** sheet "\<...\>" - \<...\>.xlsx

| Message Code | Message Type | Message Content (ENG) | Message Content (VNM) | Description |
|---|---|---|---|---|
| MSG\<...\> | \<Toast message/Message/Error/Warning/Info/Success/...\> | \<...\> | \<...\> | \<...\> |

### 7. Risk (Optional)

1. \<functional risk\>
2. \<operational risk\>
3. \<security/privacy risk\>
4. \<UX/adoption risk\>
5. \<...\>

### 8. Impact (Optional)

1. UI/UX: \<new screens/controls/flows impacted\>
2. Backend: \<new APIs/changed contracts/db schema/config\>
3. \<...\>

### 9. Non-Functional Requirements (Optional)

1. Performance/Throughput: \<...\>
2. Security/Privacy: \<...\>
3. Audit/Logging: \<...\>
4. Availability/Resilience/i18n/a11y: \<...\>

### 10. Related Document (Optional)

1. \<Confluence page / Figma board / API spec / Data dictionary / Jira links\>
2. \<...\>

### 11. Technical Note (Optional)

1. \<constraints/assumptions/performance/SLA/limits\>
2. \<timezones/pagination/sorting/filtering conventions\>
3. \<error handling & retry idempotency\>
4. \<...\>

### 12. Master Data (Optional)

1. \<reference tables/enums/status lifecycle\>
2. \<role & permission matrix\>
3. \<message catalog mapping\>
4. \<configuration keys & defaults\>
5. \<...\>

---

## Template 4: Change Request

> **Confluence:** [User Story template - Change Request](https://manulife-asia.atlassian.net/wiki/spaces/VBKB/pages/1592885938)

### User Story

| Field | Value |
|---|---|
| **User story** | **As** a \<role / persona\>, **I want** \<capability/function\> **So that** \<business value/benefit\>. |
| **Actor:** | \<primary actor(s)\> |
| **Trigger:** | \<user/system trigger\> |
| **Pre-condition:** | \<state/config/data prerequisites\> |
| **Post-condition:** | \<state change/outcome/audit\> |

1. **Flow:** \<Figma flow link\>
2. **Screen demo:** \<Figma prototype link\>

### 1. Reason & Problem Statement

| Field | Value |
|---|---|
| **Reason:** | \<Describe the reason for the change request (business/compliance/regulatory driver)\> |
| **Problem to solve:** | \<Describe the problem to solve (current issues, user pain points, operational impact)\> |
| **Impact Assessment** | Business process: \<...\> / Users/roles & training: \<...\> / Business rules (add/update/deprecate): \<...\> / Data model & migration/backfill: \<...\> / Interfaces/APIs & versioning: \<...\> / UI/UX & content: \<...\> / NFR (performance/security/availability): \<...\> |

### 2. Comparison

| As-is | To-Be |
|---|---|
| \<As-is description (1)\> | \<To-be description (1)\> |
| Description \<...\> | Description \<...\> |

### 3. \<...\> Screen

| Title Screen 1 | Title Screen 2 | Title Screen (n) |
|---|---|---|
| N/A | N/A | N/A |

#### \<...\> Specifications

| Component (EN) | Component (VNM) | Component Type | Editable | Mandatory | Default Value | Description |
|---|---|---|---|---|---|---|
| \<...\> | \<...\> | \<Text Input / Select / Button / Table / Toggle / DatePicker / Modal / File Upload / Link / Label / Alert\> | \<Yes/No\> | \<Yes/No\> | \<...\> | \<purpose/validation/format/behavior\> Refer to AC.01, AC.0n & BR-001, BR-n |

### 4. Acceptance Criteria

#### AC1. \<short title\>

**Given** \<preconditions\>  
**When** \<user/system action\>  
**Then** \<expected outcome (UI/API/state/message)\>

#### AC(n). \<...\>

**Given** \<...\>  
**When** \<...\>  
**Then** \<...\>

### 5. Business Rules & Validations (Optional)

| Rule ID | Rule Name | Business Description | Impacted flow | Example Scenario |
|---|---|---|---|---|
| BR-001 | \<...\> | \<policy/calc/threshold/validation rule\> Refer to AC.01, AC.0n & BR-001, BR-n | \<create/update/view/api\> | \<input → outcome\> |

### 6. Messages List

> **Refer** sheet "\<...\>" - \<...\>.xlsx

| Message Code | Message Type | Message Content (ENG) | Message Content (VNM) | Description |
|---|---|---|---|---|
| MSG\<...\> | \<Toast message/Message/Error/Warning/Info/Success/...\> | \<...\> | \<...\> | \<...\> |

### 7. Risk (Optional)

1. \<functional risk\>
2. \<operational risk\>
3. \<security/privacy risk\>
4. \<UX/adoption risk\>
5. \<...\>

### 8. Impact (Optional)

1. UI/UX: \<new screens/controls/flows impacted\>
2. Backend: \<new APIs/changed contracts/db schema/config\>
3. \<...\>

### 9. Non-Functional Requirements (Optional)

1. Performance/Throughput: \<...\>
2. Security/Privacy: \<...\>
3. Audit/Logging: \<...\>
4. Availability/Resilience/i18n/a11y: \<...\>

### 10. Related Document (Optional)

1. \<Confluence page / Figma board / API spec / Data dictionary / Jira links\>
2. \<...\>

### 11. Technical Note (Optional)

1. \<constraints/assumptions/performance/SLA/limits\>
2. \<timezones/pagination/sorting/filtering conventions\>
3. \<error handling & retry idempotency\>
4. \<...\>

### 12. Master Data (Optional)

1. \<reference tables/enums/status lifecycle\>
2. \<role & permission matrix\>
3. \<message catalog mapping\>
4. \<configuration keys & defaults\>
5. \<...\>

---

## Template 5: System Integration / 3rd Party

> **Confluence:** [User Story template - System int_3rd party](https://manulife-asia.atlassian.net/wiki/spaces/VBKB/pages/1592885995)

### Header

| Field | Value |
|---|---|
| **Title** | \<Project\> – \<Interface Name\> – Integration Spec |
| **Version** | v\<...\> |
| **Date** | \<YYYY-MM-DD\> |

### 1. Reference

| Field | Value |
|---|---|
| **Jira** | \<ID\> |
| **Confluence** | \<URL\> |
| **Design** | \<URL\> |

### 2. Summary

| Field | Value |
|---|---|
| **Business Objective** | \<why integrate\> |
| **Scope** | \<...\> |
| **Integration Type** | \<REST \| Webhook \| MQ \| File(SFTP) \| DB Link\> |
| **Direction** | \<Inbound/Outbound/Bi-directional\> |
| **Frequency** | Real-time / Near real-time / Batch / Ad-hoc |

### 3. Actors & Systems

| Field | Value |
|---|---|
| **Producer (System A):** | \<...\> |
| **Consumer (System B):** | \<...\> |
| **Business Roles:** | \<roles involved\> |

### 4. High-level Flow

| Field | Value |
|---|---|
| **Steps:** | 1. \<...\> / 2. \<...\> / n. \<...\> Refer to \<...\> |
| **Trigger/Events:** | \<who/what triggers\> |
| **Diagram (link):** | \<...\> |

### 5. Interface Contract

| Field | Value |
|---|---|
| **Endpoint/Channel:** | \<URL \| topic \| path \| file folder\> |
| **Method** | \<GET \| POST \| PUT \| PATCH \| DELETE\> |
| **Authentication** | \<...\> |
| **Headers** | \<...\> |

### 6. Request/Response Schema

| Field | Value |
|---|---|
| **Request** | \<JSON example\> |
| **Response 200** | \<JSON example\> |
| **Response 4xx/5xx** | \<error example\> |
| **Pagination/Filtering/Sorting** | \<...\> |

### 7. Acceptance Criteria

#### AC1. \<short title\>

**Given** \<preconditions\>  
**When** \<user/system action\>  
**Then** \<expected outcome (UI/API/state/message)\>

#### AC(n). \<...\>

**Given** \<...\>  
**When** \<...\>  
**Then** \<...\>

### 8. Related Document (Optional)

1. \<Confluence page / Figma board / API spec / Data dictionary / Jira links\>
2. \<...\>

---

## Template 6: Report / Dashboard / Export

> **Confluence:** [User Story template - Report/Dashboard/Export](https://manulife-asia.atlassian.net/wiki/spaces/VBKB/pages/1592886848)

### User Story

| Field | Value |
|---|---|
| **User story** | **As** a \<role / persona\>, **I want** \<capability/function\> **So that** \<business value/benefit\>. |
| **Actor:** | \<primary actor(s)\> |
| **Trigger:** | \<user/system trigger\> |
| **Pre-condition:** | \<state/config/data prerequisites\> |
| **Post-condition:** | \<state change/outcome/audit\> |

1. **Flow:** \<Figma flow link\>
2. **Screen demo:** \<Figma prototype link\>

### 1. \<...\> Screen

| Title Screen 1 | Title Screen 2 | Title Screen (n) |
|---|---|---|
| N/A | N/A | N/A |

#### \<...\> Specifications

| Component (EN) | Component (VNM) | Component Type | Editable | Mandatory | Default Value | Description |
|---|---|---|---|---|---|---|
| \<...\> | \<...\> | \<Text Input / Select / Button / Table / Toggle / DatePicker / Modal / File Upload / Link / Label / Alert\> | \<Yes/No\> | \<Yes/No\> | \<...\> | \<purpose/validation/format/behavior\> Refer to AC.01, AC.0n & BR-001, BR-n |

### 2. Definition of Columns (Exported File)

| Column | Definition | Data Type | Source Field.Source Table/View | Sample data |
|---|---|---|---|---|
| \<TBD_Column_1\> | \<Meaning; source field/table/view; transform rule (if any); unit/format/timezone\> | \<string/number/date…\> | \<field_xxx\>.\<vw_xxx / tbl_xxx\> | \<TBD_Sample_1\> |
| \<TBD_Column_2\> | \<...\> | \<...\> | \<...\> | \<...\> |
| \<TBD_Column_n\> | \<...\> | \<...\> | \<...\> | \<...\> |

### 3. Acceptance Criteria

#### AC1. \<short title\>

**Given** \<preconditions\>  
**When** \<user/system action\>  
**Then** \<expected outcome (UI/API/state/message)\>

#### AC(n). \<...\>

**Given** \<...\>  
**When** \<...\>  
**Then** \<...\>

### 4. Business Rules & Validations (Optional)

| Rule ID | Rule Name | Business Description | Impacted flow | Example Scenario |
|---|---|---|---|---|
| BR-001 | \<...\> | \<policy/calc/threshold/validation rule\> Refer to AC.01, AC.0n & BR-001, BR-n | \<create/update/view/api\> | \<input → outcome\> |

### 5. Messages List

> **Refer** sheet "\<...\>" - \<...\>.xlsx

| Message Code | Message Type | Message Content (ENG) | Message Content (VNM) | Description |
|---|---|---|---|---|
| MSG\<...\> | \<Toast message/Message/Error/Warning/Info/Success/...\> | \<...\> | \<...\> | \<...\> |

### 6. Risk (Optional)

1. \<functional risk\>
2. \<operational risk\>
3. \<security/privacy risk\>
4. \<UX/adoption risk\>
5. \<...\>

### 7. Impact (Optional)

1. UI/UX: \<new screens/controls/flows impacted\>
2. Backend: \<new APIs/changed contracts/db schema/config\>
3. \<...\>

### 8. Non-Functional Requirements (Optional)

1. Performance/Throughput: \<...\>
2. Security/Privacy: \<...\>
3. Audit/Logging: \<...\>
4. Availability/Resilience/i18n/a11y: \<...\>

### 9. Related Document (Optional)

1. \<Confluence page / Figma board / API spec / Data dictionary / Jira links\>
2. \<...\>

### 10. Technical Note (Optional)

1. \<constraints/assumptions/performance/SLA/limits\>
2. \<timezones/pagination/sorting/filtering conventions\>
3. \<error handling & retry idempotency\>
4. \<...\>

### 11. Master Data (Optional)

1. \<reference tables/enums/status lifecycle\>
2. \<role & permission matrix\>
3. \<message catalog mapping\>
4. \<configuration keys & defaults\>
5. \<...\>

---

## Template 7: Notification Matrix

> **Confluence:** [User Story template - Notification_Matrix](https://manulife-asia.atlassian.net/wiki/spaces/VBKB/pages/1592953468)

### User Story

| Field | Value |
|---|---|
| **User story** | **As** a \<role / persona\>, **I want** \<capability/function\> **So that** \<business value/benefit\>. |
| **Actor:** | \<primary actor(s)\> |
| **Trigger:** | \<user/system trigger\> |
| **Pre-condition:** | \<state/config/data prerequisites\> |
| **Post-condition:** | \<state change/outcome/audit\> |

**Flow:** \<Figma flow link\>

### 1. Acceptance Criteria

#### AC1. \<short title\>

**Given** \<preconditions\>  
**When** \<user/system action\>  
**Then** \<expected outcome (UI/API/state/message)\>

#### AC(n). \<...\>

**Given** \<...\>  
**When** \<...\>  
**Then** \<...\>

### 2. Email Matrix (Inline Table)

| Templatecode | Event Code | Event type | Notification Type | Navigate to screen | Title | Short message/description | Content (EN) | Content (VN) | Note | System trigger |
|---|---|---|---|---|---|---|---|---|---|---|
| \<Templatecode (one or more codes; separate by new line)\> | \<Event Code (e.g., CLM_SUBMITTED)\> | \<Event type (group/stream)\> | \<Notification Type (e.g., Claim request / Policy / Payment)\> | \<Navigate screen (e.g., Claim details)\> | \<Title (VN/EN as used in app)\> | \<Short message/description\> | \<Content (EN) – use numbered list if listing\> | \<Content (VN) – use numbered list if listing\> | \<Note (recipients/rule/system/component)\> | \<System trigger (Y/N or job/topic name)\> |

### 3. Related Document (Optional)

1. \<Confluence page / Figma board / API spec / Data dictionary / Jira links\>
2. \<...\>

---

## Bảng so sánh nhanh các Template

| # | Template | Khi nào dùng | Section đặc trưng |
|---|---|---|---|
| 1 | **New Feature (Standard US)** | Tính năng mới, không nặng UI | Scope, Requirements (R.01–R.0n) |
| 2 | **New Feature – with UI** | Tính năng mới có UI chi tiết | Screen Spec (Component table), Data Source, Calculation Rules, Messages |
| 3 | **Enhancement** | Cải tiến tính năng hiện có | Current vs Target Behaviour, Comparison (As-is / To-be) |
| 4 | **Change Request** | Yêu cầu thay đổi (business/compliance) | Reason & Problem Statement, Impact Assessment |
| 5 | **System Integration** | Tích hợp hệ thống / bên thứ ba | Interface Contract, Request/Response Schema, Actors & Systems |
| 6 | **Report/Dashboard/Export** | Báo cáo, dashboard, xuất file | Definition of Columns (Exported File) |
| 7 | **Notification Matrix** | Email/notification/push | Email Matrix (Templatecode, Event Code, Content EN/VN) |
