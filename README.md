🏥 Hospital Management Automation with n8n

An n8n-based hospital management automation prototype that automates appointment booking, medical-report OCR and analysis, doctor consultation reminders, and administrative analytics.

The project uses n8n as the workflow orchestration layer and integrates Google Drive, OCR.space, Google Gemini, Gmail, and n8n Data Tables.

Project type: Workflow Automation / AI Automation / Healthcare Operations PrototypeAutomation Platform: n8n

📌 Overview

This project contains four independent automation workflows:

Appointment Booking — collects patient details, checks appointment availability, requests administrator approval, stores approved appointments, and sends confirmation/rejection emails.

OCR / Medical Report Analysis — detects newly uploaded medical reports in Google Drive, extracts text using OCR.space, structures the extracted information using Google Gemini, and emails an HTML report.

Doctor Consultation Reminder — stores follow-up consultation requirements and periodically sends reminder emails.

Admin Management / Analytics — periodically analyzes stored appointment data, calculates basic statistics, generates an administrative report using Gemini, and sends the report through Gmail.

The complete workflow documentation describes the triggers, inputs, processing logic, storage, external services, and outputs of each workflow. fileciteturn0file0L10-L21

🏗️ System Architecture



The architecture consists of modular n8n workflows connected to external services and n8n Data Tables. The supplied architecture diagram shows users/staff interacting with the automation workflows, which communicate with OCR, email/notification, database, and other service layers.

The project documentation represents the current system as four modular n8n automations connected to the project's actual external services and n8n Data Tables. fileciteturn0file0L186-L188

🔄 Workflows

1. 📅 Appointment Booking

Purpose:Automates the appointment-request process from patient data collection to availability checking, administrator approval, database insertion, and email confirmation.

Flow:

Patient
   ↓
Patient Details Form
   ↓
Check Date + Time in Data Table
   ↓
Is Slot Already Occupied?
   ├── Yes → Rejection Email
   └── No
         ↓
   Admin Approval Request
         ↓
      Approved?
      ├── No → Rejection Email
      └── Yes
            ↓
      Insert Appointment
            ↓
      Confirmation Email

Inputs:

Patient name

Age

Sex

Phone number

Appointment date

Appointment time

Storage: n8n Data Table

Output: Booking confirmation or rejection email.

The actual workflow uses an n8n Form Trigger, checks the hospital mangement Data Table for the requested date/time, requests administrator approval through Gmail, stores approved bookings, and sends confirmation. fileciteturn0file0L33-L46 fileciteturn0file0L50-L78

2. 📄 OCR / Medical Report Analysis

Purpose:Automatically processes newly uploaded medical-report files and converts OCR output into a structured report using Google Gemini.

Flow:

Medical Report Uploaded to Google Drive
              ↓
       Google Drive Trigger
              ↓
          Download File
              ↓
         OCR.space API
              ↓
       Extracted OCR Text
              ↓
       Gemini / AI Agent
              ↓
      Structured JSON Report
              ↓
       JavaScript JSON Parse
              ↓
          HTML Email Report

The OCR workflow watches a Google Drive folder, downloads newly created files, sends them to OCR.space, uses Gemini to structure the extracted text, parses the JSON response, and sends an HTML email report. fileciteturn0file0L79-L93

Example structured output:

{
  "patientName": "",
  "tests": [
    {
      "testName": "",
      "value": "",
      "unit": "",
      "referenceRange": "",
      "status": ""
    }
  ],
  "summary": ""
}

fileciteturn0file0L113-L125

3. 🔔 Doctor Consultation Reminder

Purpose:Records patients who require follow-up consultations and periodically checks stored records to send reminder emails.

Flow:

Consultation Form
       ↓
Follow-up Required?
       ↓ YES
Insert Follow-up Record
       ↓
Scheduled Reminder Processing
       ↓
Read Follow-ups
       ↓
Date Check
       ↓
Reminder Email

Inputs:

Patient name

Gmail address

Follow-up required

Follow-up date

Storage: n8n Data Table named consultation

Output: Doctor consultation reminder email.

The consultation records contain Name, Gmail, consult_required, and Date. A scheduled process reads these records and checks the stored follow-up date before sending reminders. fileciteturn0file0L127-L152

4. 📊 Admin Management / Analytics

Purpose:Generates periodic hospital-management analytics reports from stored appointment records.

Flow:

Schedule Trigger
       ↓
Read Data Table
       ↓
JavaScript Statistics
       ↓
Gemini AI Agent
       ↓
Administrative Report
       ↓
Gmail Email

The workflow calculates statistics such as total patients, booked appointments, and appointments not booked. Gemini then generates a short management report containing a summary, booking percentage, patients without appointments, and an administrative observation. fileciteturn0file0L155-L175

🧰 Technology Stack

Layer

Technology

Purpose

Automation

n8n

Workflow orchestration

Workflow Logic

JavaScript / n8n Code

Validation, calculations, JSON parsing

Forms

n8n Form Trigger

Patient and consultation input

Database

n8n Data Tables

Appointment and consultation records

File Storage

Google Drive

Medical-report intake

OCR

OCR.space REST API

Extract text from documents/images

AI

Google Gemini

OCR structuring and analytics generation

Email

Gmail

Confirmations, approvals, reminders and reports

Scheduling

n8n Schedule Trigger

Periodic processing

This stack is based on the supplied workflow documentation. fileciteturn0file0L22-L32

🗃️ Data Storage

Appointment Data

The appointment workflow currently uses the n8n Data Table:

hospital mangement

Fields include:

Name
Age
Sex
phone
Date
Time

Consultation Data

The reminder workflow uses:

consultation

Fields include:

Name
Gmail
consult_required
Date

fileciteturn0file0L176-L179

🔌 External Services

Service

Role

Google Drive

Medical-report file intake and trigger

OCR.space

OCR processing

Google Gemini

Structured extraction and analytics generation

Gmail

Approval, confirmation, reminder and report emails

fileciteturn0file0L180-L185

📂 Suggested Repository Structure

hospital-management-n8n/
│
├── workflows/
│   ├── appointment-booking.json
│   ├── medical-report-ocr.json
│   ├── doctor-consultation-reminder.json
│   └── admin-analytics.json
│
├── docs/
│   └── n8n-documentation.pdf
│
├── n8n_workflow.jpeg
│
├── README.md
│
└── .gitignore

Rename your exported n8n workflow JSON files according to your actual filenames. Do not commit credentials or API keys.

⚙️ Setup

1. Install / Run n8n

You can run n8n locally or use a hosted n8n instance.

Example with npm:

npm install n8n -g

Then:

n8n

Open the n8n editor in your browser.

2. Configure Credentials

Configure the required credentials/services inside n8n:

Gmail

Google Drive

Google Gemini

OCR.space API

3. Import Workflows

Export your n8n workflows as JSON and import them into your n8n instance.

n8n Dashboard
   ↓
Workflows
   ↓
Import from File
   ↓
Select workflow JSON

4. Configure Data Tables

Create the required n8n Data Tables:

hospital mangement
consultation

Make sure their fields match the workflow mappings.

5. Configure Google Drive

Create/select the Google Drive folder used for medical-report uploads and configure the Google Drive Trigger.

6. Configure OCR

Add your OCR.space API credential to the OCR HTTP Request node.

7. Configure Gemini

Connect your Google Gemini credentials to the AI Agent nodes used for:

Medical report structuring

Administrative analytics

8. Configure Gmail

Configure the Gmail nodes used for:

Administrator approval

Appointment confirmation/rejection

Medical report emails

Consultation reminders

Analytics reports

🔐 Security

Do not publish API keys, credentials, passwords, or personal email addresses on GitHub.

The supplied workflow documentation specifically identifies security issues that should be addressed before a public GitHub release:

Rotate/revoke any OCR API key embedded directly in the workflow.

Move secrets into n8n credentials or a secret-management system.

Avoid hard-coded personal email addresses.

Use dynamic recipient fields where appropriate.

Check reminder recipient mapping so the stored patient's email is used.

Add reminder status tracking to prevent duplicate reminders.

fileciteturn0file0L220-L232

Recommended .gitignore

# Environment variables
.env
.env.*

# Credentials / secrets
credentials.json
secrets.json
*.pem
*.key

# n8n local data
.n8n/

# Python
__pycache__/
*.py[cod]

# OS files
.DS_Store
Thumbs.db

⚠️ Current Limitations

This is a prototype, not a production hospital-management system.

The documentation identifies several implementation points that should be improved:

Reminder logic should clearly define when reminders are sent and prevent duplicates.

Appointment availability should explicitly handle zero matching rows.

Admin analytics expects an appointment field that should be checked against the current booking insert mapping.

Sensitive credentials and personal email addresses should not be hard-coded.

The OCR workflow currently does not maintain a persistent medical-report table.

fileciteturn0file0L220-L232

🚀 Future Improvements

A stronger production architecture could include:

PostgreSQL for structured persistent data

Object storage for medical documents

Role-based administrator access

Secure secret management

Audit logging

Reminder status tracking

SMS/WhatsApp notifications

Better appointment conflict handling

Authentication and authorization

Monitoring and error handling

Separate development and production environments

The supplied documentation lists these as recommended production improvements rather than features already present in the prototype. fileciteturn0file0L233-L245

📈 Workflow Inventory

Workflow

Trigger

Processing

Storage

Output

Appointment Booking

Form submission

Availability + approval

n8n Data Table

Booking/rejection email

OCR

Google Drive file created

OCR + Gemini structuring

No persistent report table

Email report

Reminder

Form + schedule

Follow-up storage + date check

n8n Data Table

Reminder email

Admin Analytics

Weekly schedule

Statistics + Gemini report

n8n Data Table

Analytics email

fileciteturn0file0L246-L253

🎯 Project Goal

The goal of this project is to automate repetitive hospital operations using workflow automation and AI:

Appointment Management + Medical Document OCR + Follow-up Reminders + Administrative Analytics

All four processes are orchestrated through n8n rather than being implemented as a traditional monolithic application. fileciteturn0file0L254-L259

👨‍💻 Author

Swayam Sachdeva

B.Tech CSE StudentABES College

⭐ Acknowledgement

Built as an n8n workflow automation project demonstrating the integration of forms, databases, APIs, OCR, generative AI, email automation, and scheduled workflows.

If you find this project useful, consider giving the repository a ⭐.
