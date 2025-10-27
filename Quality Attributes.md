## Quality Attributes 

| **ID** | **Quality Attribute** | **Scenario** | **Associated Use Case** |
|---------|----------------------|---------------|--------------------------|
| **QA-1** | **Performance** | When a student provides a natural-language question during normal use, the assistant should reply within 2 seconds on average under normal load. | UC-1 |
| **QA-2** | **Availability** | The system should stay available 99.5 % of the time each month. If a server goes down, the assistant should automatically recover with little or no downtime. | UC-1, UC-6 |
| **QA-3** | **Scalability** | The assistant should be able to handle up to 5,000 concurrent users without slowing down or losing responses. | UC-1, UC-6 |
| **QA-4** | **Security** | Only authorized users should access their data. The system must use SSO and follow university privacy and security policies. | UC-1 – UC-6 |
| **QA-5** | **Modifiability** | Developers should be able to add new AI models or connect extra data sources with only small code changes. | UC-6, UC-7 |
| **QA-6** | **Usability** | The assistant should be easy to use and support both text and voice input. It should also allow users to change the language of interaction. | UC-1, UC-2 |
| **QA-7** | **Reliability** | If a data source fails, the system should retry automatically and continue working normally once the source is back online. | UC-7 |
Add the Quality Attributes to this file
