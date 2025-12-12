# Pascal ALS Optimizer – SaaS Terms & Data Requirements

These general terms and data requirements apply to the ALS Optimizer software-as-a-service provided by Pascal Technologies AS (“Pascal”) to each customer (“Customer”).

This page is incorporated by reference into the ALS Optimizer Order Form / Front Page executed between Pascal and Customer (the “Order Form”). Capitalised terms used but not defined here have the meaning given in the Order Form.

## 1. Scope and Structure of the Agreement

(a) These Terms apply to Customer’s use of the ALS Optimizer SaaS service, any related professional services and integrations, and any associated documentation and materials that Pascal makes available (together, the “Service”).

(b) The contractual documents consist of:

- The signed Order Form / front page (commercial details and signature page);
- This ALS Optimizer SaaS Terms & Data Requirements page; and
- Any additional schedules or statements of work that the parties expressly reference in the Order Form.

(c) In case of conflict, the Order Form prevails over this page, and this page prevails over any other documentation, unless the parties expressly agree otherwise in writing.

## 2. Service Description and Access

(a) Pascal provides ALS Optimizer as a hosted service. Customer accesses the Service remotely over the internet through browser-based interfaces, APIs, and/or other access mechanisms specified by Pascal.

(b) No software is delivered or installed on Customer systems, other than any lightweight connectors or agents that may be agreed for telemetry integration under Section 5.

(c) Subject to timely payment of applicable fees and compliance with these Terms, Pascal grants Customer a non-exclusive, non-transferable, time-limited right to access and use the Service for Customer’s internal business purposes and for the number of vessels, users, and environments specified in the Order Form.

## 3. Intellectual Property and Licence

(a) Pascal and its licensors own all right, title and interest in and to the Service, including all software, algorithms, models, interfaces, documentation, design, and any improvements, updates or derivative works.

(b) Except for the limited rights expressly granted in Section 2, no rights or licences are granted to Customer, whether by implication or otherwise. Customer must not (and must not permit any third party to):

- copy, modify, translate or create derivative works of the Service;
- reverse engineer, decompile, disassemble or attempt to extract source code, except where permitted by applicable law; or
- remove or alter any proprietary notices or branding.

(c) Customer retains ownership of Customer Data (including vessel telemetry, operational data and other materials supplied by Customer). Customer grants Pascal a non-exclusive, worldwide licence to host, process, analyse and use the Customer Data as necessary to provide the Service, improve the Service (on an aggregated and anonymised basis) and otherwise perform its obligations under the Agreement.

## 4. Fees, Free Trials and Payment Terms

(a) Fees and commercial details (including any free trial periods, pilot terms, vessel counts and subscription charges) are set out in the Order Form.

(b) Unless otherwise stated in the Order Form:

- subscription fees are invoiced in advance for the applicable billing period; and
- integration or professional services are invoiced on completion of milestones or on a time-and-materials basis, as agreed in writing.

(c) Customer is responsible for all applicable taxes (other than taxes on Pascal’s income), and for reasonable expenses incurred by Pascal in connection with agreed services.

(d) Late payment may result in suspension of access to the Service until the outstanding amounts are paid, after reasonable notice.

## 5. Telemetry Integration and Data Upload Requirements

ALS Optimizer relies on a predictable data pipeline from each vessel and/or fleet system. The purpose of this section is to describe how telemetry data is supplied to Pascal, in what form, and with which responsibilities on each side.

### 5.1 General Principles

- Customer ensures that data sources required for ALS Optimizer are available in one of the integration patterns described below.
- Pascal configures and maintains the ingestion pipeline from the agreed interfaces into the ALS Optimizer platform.
- Unless otherwise agreed, data is provided in UTC or clearly documented local time including offset.
- Where schemas or formats change, Customer notifies Pascal in advance so ingestion can be adjusted without loss of data.

### 5.2 Preferred Option – API-Based Integration

The preferred integration method is a set of REST or similar APIs exposed by Customer or by a third-party telemetry provider.

Customer will provide:

- API documentation describing endpoints, parameters, authentication, and rate limits;
- production credentials (for example API keys, OAuth client credentials, or service accounts); and
- network access (for example allow-listing the Pascal IP ranges or enabling secure VPN/privatelink connectivity, if required).

Pascal will:

- configure queries and schedules (polling or event-based) to retrieve telemetry as soon as it is available from the API;
- manage retries, backoff and fault handling on the ingestion side; and
- minimise operational load on Customer by treating this as a one-time setup with low ongoing maintenance.

### 5.3 Alternative – Periodic Tabular File Exports

If APIs are not available, the parties may agree periodic exports of telemetry and operational data as tabular files (for example CSV, Parquet, or delimited text files) with a stable, documented schema.

Customer will typically:

- produce files on an agreed schedule (for example daily or weekly);
- ensure the column structure, units and encodings are consistent across files; and
- deliver the files via a mutually agreed mechanism (for example secure upload, SFTP, or object storage such as Azure Blob Storage or S3-compatible storage).

Pascal will:

- configure ingestion pipelines to read and validate each new file;
- log and report any schema or quality issues to Customer; and
- update ALS Optimizer analyses in line with the agreed file frequency.

### 5.4 Custom Integration Options

Where neither APIs nor regular file exports are feasible, the parties may agree on more custom options, such as:

- A read-only database connection (for example PostgreSQL, SQL Server or MySQL) using a dedicated reporting schema or view that Customer maintains.
- Periodic ingestion from a data lake or object store (for example Azure Data Lake or S3-compatible storage), where Customer writes telemetry snapshots into a defined folder structure and format.
- Subscription to a message or event stream (for example Azure Event Hubs, Kafka or MQTT) where Customer publishes telemetry events and Pascal consumes them as a downstream subscriber.
- A lightweight “bridge” or connector service in Customer’s environment that reads from internal systems (for example historians, on-prem databases or internal APIs) and forwards the required telemetry securely to a Pascal endpoint over HTTPS.

*Custom integrations may involve additional one-off setup effort and will be scoped and priced separately in the Order Form or a statement of work.*

### 5.5 Data and Documentation Sheet

In addition to the transport mechanisms above, Pascal and Customer will agree a detailed “Data & Documentation Sheet” for each project. This sheet typically specifies, per vessel and data source:


| Parent System               | Parameter Name                                                  | Description                                                                   | Units    | Comment                                                                         | Tier            |
| --------------------------- | --------------------------------------------------------------- | ----------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------- | --------------- |
| Navigation                  | STW                                                             | Speed Through Water                                                          | kts      |                                                                                 | Tier 1 + Tier 2 |
| Navigation                  | Heading                                                         | Magnetic Heading                                                              | deg      |                                                                                 | Tier 1 + Tier 2 |
| Navigation                  | COG                                                             | Course over Ground                                                            | deg      |                                                                                 | Tier 1 + Tier 2 |
| Navigation                  | SOG                                                             | Speed Over Ground                                                             | kts      |                                                                                 | Tier 1 + Tier 2 |
| Navigation                  | Lat                                                             | Latitude position                                                             | standard |                                                                                 | Tier 1 + Tier 2 |
| Navigation                  | Lon                                                             | Longitude position                                                            | standard |                                                                                 | Tier 1 + Tier 2 |
| Navigation                  | Draft Fwd, mid, aft                                             | Vessel draft                                                                  | m        |                                                                                 | Tier 1 + Tier 2 |
| Navigation                  | Water Depth                                                     | Distance to sea bed                                                           | m        |                                                                                 | Future          |
| Navigation                  | Apparent Wind Speed (AWS)                                       | Measured wind speed onboard moving vessel                                     | kts      |                                                                                 | Future          |
| Navigation                  | Apparent Wind Direction (AWD)                                   | Measured wind direction onboard moving vessel                                 | deg      | important to know frame of reference. In relation to bow? Or compass direction? | Future          |
| Navigation                  | Rudder angle 1 and 2                                            | Rudder angle in degrees                                                       | deg      | 0 is neutral. Indicate +ve and -ve rotation                                     | Future          |
| Navigation                  | Heel / roll                                                     | Vessel heel / roll angle                                                      | deg      | 0 is neutral. Indicate +ve and -ve rotation. Nice to know how its measured.     | Future          |
| Navigation                  | Trim / pitch                                                    | Vessel Trim                                                                   | deg or m |                                                                                 | Future          |
| Machinery                   | Main Engine Power                                               |                                                                               | kW       |                                                                                 | Tier 1 + Tier 2 |
| Machinery                   | Main Engine RPM                                                 |                                                                               | RPM      |                                                                                 | Tier 1 + Tier 2 |
| Machinery                   | Main Engine Torque                                              |                                                                               | Nm       |                                                                                 | Tier 1 + Tier 2 |
| Machinery                   | Shaft Power                                                     |                                                                               | kW       |                                                                                 | Tier 1 + Tier 2 |
| Machinery                   | Shaft RPM                                                       |                                                                               | Nm       |                                                                                 | Tier 1 + Tier 2 |
| Machinery                   | Shaft Torque                                                    |                                                                               | Nm       |                                                                                 | Tier 1 + Tier 2 |
| Machinery                   | Shaft Generator (PTI) Power                                     | Power Take-In. If applicable.                                                 | kW       |                                                                                 | Tier 1 + Tier 2 |
| Machinery                   | Shaft Generator (PTO) Power                                     | Power Take-Off. If applicable.                                                | kW       |                                                                                 | Tier 1 + Tier 2 |
| Machinery                   | Stabilizers                                                     | If vessel has stabilizer fins, we need data on when they are active           | bool     |                                                                                 | Tier 1 + Tier 2 |
| Machinery                   | Aux Generators Power                                            | Auxillary generators as applicable.                                           | kW       |                                                                                 | Future          |
| ALS                         | Compressor Power                                                | Combined and individual Compressor Load                                        | kW       |                                                                                 | Tier 1 + Tier 2 |
| ALS                         | Compressor torque, RPM                                          |                                                                               |          |                                                                                 | Future          |
| ALS                         | All pressure sensors in system                                  | All Sensors tagged according to P&ID                                          | barg     |                                                                                 | Future          |
| ALS                         | All flowmeters in system                                        | All Sensors tagged according to P&ID                                          | m3/h     |                                                                                 | Future          |
| Documentation / Information | General arrangement                                             | Main Particulars + Overview of every onboard space and system                 | n/a      |                                                                                 | Tier 1 + Tier 2 |
| Documentation / Information | Stability Booklet                                               | All loading conditions and hydrostatics                                       | n/a      |                                                                                 | Tier 1 + Tier 2 |
| Documentation / Information | EEDI report                                                     | Speed power curves and ALS particulars                                        | kW/kts   |                                                                                 | Tier 1 + Tier 2 |
| Documentation / Information | Engine room arrangement or Shaft arrangement                    | Incl. Shaft power sensor placement                                            | Kq/J     |                                                                                 | Tier 1 + Tier 2 |
| Documentation / Information | ALS Nozzle Arrangement                                          | Incl. nozzle positions and vessel speed logger placement                      |          | Is the speed logger placed before, or after the ALS nozzles?                    | Future          |
| Documentation / Information | ALS Piping and Instrumentation Diagram                          |                                                                               |          |                                                                                 | Future          |
| Documentation / Information | Shell Expansion Plan                                            |                                                                               |          |                                                                                 | Future          |
| Documentation / Information | Midship section                                                 |                                                                               |          |                                                                                 | Future          |
| Documentation / Information | Propeller document (with diameter, pitch ratio, area ratio etc  |                                                                               | n/a      |                                                                                 | Future          |
| Documentation / Information | Propeller Curve                                                 | propeller demand versus advance coefficient                                   | n/a      |                                                                                 | Future          |
The Data & Documentation Sheet, once agreed, forms part of the Agreement and may be updated by mutual written agreement (including email confirmation).

## 6. Customer Responsibilities

Customer is responsible for:

- providing and maintaining internet connectivity and internal infrastructure required to access the Service;
- ensuring that users are trained and use the Service in accordance with the documentation and these Terms;
- ensuring that all data supplied is, to the best of Customer’s knowledge, accurate, lawful and suitable for the intended use; and
- notifying Pascal promptly of any security incidents or suspected misuse related to the Service.

## 7. Support and Service Levels

Pascal provides remote support for the Service during its normal business hours (unless otherwise specified in the Order Form). Additional or out-of-hours support, if required, may be agreed separately.

Pascal may perform scheduled maintenance windows; where possible these will be communicated in advance and planned to minimise operational impact.

## 8. Warranties and Disclaimers

(a) Pascal will provide the Service with reasonable skill and care and in accordance with these Terms.

(b) Except as expressly stated in the Agreement, the Service is provided “as is” and Pascal disclaims all other warranties (express or implied), including any implied warranties of merchantability, fitness for a particular purpose, and non-infringement, to the maximum extent permitted by law.

(c) Customer is solely responsible for its own compliance with laws and regulations and for decisions taken based on insights from the Service. The Service is a decision-support tool and does not replace Customer’s operational judgement.

## 9. Limitation of Liability

(a) Neither party excludes liability for death or personal injury caused by its negligence, for fraud, or for any other liability that cannot be excluded under applicable law.

(b) Subject to (a), neither party is liable to the other for any indirect, consequential, incidental or special losses, including loss of profit, revenue, or business interruption.

(c) Subject to (a) and (b), each party’s aggregate liability arising out of or in connection with the Agreement (whether in contract, tort or otherwise) is capped at an amount equal to the total subscription fees actually paid by Customer to Pascal for the Service in the twelve (12) months immediately preceding the event giving rise to the claim.

## 10. Confidentiality and Data Protection

(a) Each party will treat as confidential all information received from the other party that is marked or reasonably understood to be confidential, and will use such information only for the purposes of performing the Agreement.

(b) The parties will comply with applicable data protection laws in relation to any personal data processed through the Service. Typically, Customer acts as data controller and Pascal acts as data processor in respect of Customer’s personal data, and the parties may enter into a separate data processing agreement where required.

## 11. Term, Suspension and Termination

(a) The term of the Agreement is specified in the Order Form.

(b) Either party may terminate the Agreement with immediate effect if the other party commits a material breach that is not remedied within a reasonable cure period after written notice, or becomes insolvent.

(c) Pascal may suspend access to the Service on notice if Customer fails to pay undisputed fees when due, or if suspension is reasonably necessary to address a security risk or misuse. Access will be restored promptly once the underlying issue is resolved.

(d) Upon termination, Customer’s right to access the Service ends. Pascal will make a final export of Customer Data available on request within a reasonable period, in a standard industry format (for example CSV or Parquet), and will then delete or anonymise Customer Data in accordance with its retention policies, unless otherwise required by law.

## 12. Governing Law and Dispute Resolution

The governing law and dispute resolution mechanism (for example court jurisdiction or arbitration seat) are stated in the Order Form. If not specified, they default to the laws of Norway, with disputes subject to the jurisdiction of the Norwegian courts.

---

Pascal ALS Optimizer – SaaS Terms & Data Requirements · This page may be updated from time to time. The version applicable to a given Order Form is the version in force on the date the Order Form is signed, unless the parties agree otherwise in writing.
