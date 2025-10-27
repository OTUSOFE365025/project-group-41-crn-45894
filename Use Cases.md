## **Use Case Model**

| **Use Case** | **Description** | **Associated Requirement ID** |
|---------------|----------------|-------------------------------|
| **UC-1: Query institutional data** | A student or lecturer interacts with the digital assistant through a text or voice interface to gather information such as class schedules, deadlines, and announcements. The assistant also interprets the natural-language input and gathers data from the connected LMS or registration systems and delivers a response all in real time. | R1, R3, R5, RS1, RS9, RL6 |
| **UC-2: Post announcements and notifications** |A lecturer or administrator uses the assistant to publish announcements or schedule automated reminders. The system verifies permissions, broadcasts the message to relevant users, and notifies students through their connected devices or dashboard. | R1, RL2, RA3, RS2, RL4 |
| **UC-3: Manage course materials** | A lecturer uploads or updates course materials that students can view through the assistant. The system makes sure only authorized users can make changes. | RL1, RL5, RL8 |
| **UC-4: View analytics and performance summaries** | A lecturer or administrator asks the assistant to show class or system analytics. The assistant gathers and shows summaries like attendance or engagement. | RL3, RL6, RA4 |
| **UC-5: Manage institutional integrations and policies** | An administrator sets up or manages system connections (like LMS or calendars) and controls data rules such as retention and response logging. | RA1, RA2 |
| **UC-6: Maintain and monitor system operations** | A system maintainer updates, monitors, and manages the system to make sure it runs smoothly. This includes checking health, latency, and AI model settings. | RM1, RM2, RM3, RM4, RM6 |
| **UC-7: Synchronize external data** | External systems (like LMS or registration) share and sync data with AIDAP using standard APIs. The system retries if connections fail and keeps data accurate. | RD1, RD2, RD3, RD4 |
