1.1   Purpose of this document and of the system
 
This document is intended to be an overview of the proposal and requirements for the development of a password / identity and security management system. The document is intended for the project's team as guidance, and for the relevant stakeholders (Dr. Seker). It provides a common understanding of the proposed system's objectives, design features, deliverables, and a planned outline. It will function as a reference point throughout the software engineering process.
 
The system aims to solve the issues users encounter when creating, organizing, and managing passwords, and other online account information across many different services. Remembering unique passwords with varying requirements may be difficult, leading to users choosing weak ones, or storing sensitive information in insecure areas. This proposed system will serve as a centralized application for users to enhance their password security, while keeping the process simple and convenient.
 
 
2.1   Product perspective and context
 
[See 2.1 Context Diagram] 
 
The system boundary points out the functionality developed by the project team for the password and security management system, which includes password management, credential encryption, and localized vault handling. The diagram includes optional external integrations: external backup storage.
 

2.6 Feasibility Summary
 
[See 2.6 Feasibility Summary]
 
 
3.2 Constraint Requirements Table
 
[See 3.2 Constraint Requirements]


7.1 Architecture Decision Records

ADR-01: Layered Architecture

Status - Accepted

Date and Owner: 11/7/2026 - Project Team

Context (Drivers) 
This system requires a maintainable, secure architecture that reliably allows for separate responsibilities and simplified development. Using a layered architecture will support the project's quality attribute scenarios (QAS-01 to QAS-04) and constraints (CON-01 to CON-03). The application is intended for a single user on one device, making multi-device synchronization and/or service-oriented architecture unnecessary.  

Options Considered
Layered Architecture  - Separate presentation, application logic, and data accessing into multiple layers.
Cloud-Hosted Architecture - Provides centralized storage, and allows for multi-device capabilities, but would require hosting services.
Microservices Architecture - Separates functions into deployable services, introducing complications and several new requirements.

Decision
Maintain a layered architecture with separate user interface, application logic, and data management. Each layer communicates through developed interfaces.

Consequences
Favors - Maintainability, modularity, separate concerns.
Move - Organize application functions into distinct layers with different responsibilities. 
Cost - Additional structure decisions and communication between design layers.

Evidence
Implementation and prototype evidence is not available. This decision is based on the current project scope, architectural constraints, and comparisons made in section 6.2.

Related
QAS-01, QAS02, QAS-03, and QAS-04. CON-01, CON-02, and CON-03.


ADR-02: Localized Vault Storage

Status - Proposed

Data and Owner: 11/7/2026 - Project Team

Context (Drivers)
The password management system is intended for a single user operating a single device. Storing vault information locally avoids dependency on external services, and reduces the operational complexity of using cloud-based storage. This decision aligns with the project's constraints on hosting.

Options Considered
Local Storage - Store the password vault on the user's device using a local database/file.
Cloud-Based Storage - Store vault information on a remote server, allowing centralized access and potential synchronization across multiple different devices for users.
Mixed Storage - Maintain an encrypted local vault that also periodically updates/uploads the information with a remote service.

Decision
Use local-only storage for the password vault. Sensitive vault data will remain only on the user's device without requiring a remote server or cloud database. A simple, application-embedded database system created with SQLite may be used.

Consequences
Favors - Security, simplicity, independence from external services.
Move - Implement localized, persistent storage and ensure that vault data is protected from unauthorized access.
Cost - Users will not have automatic multi-device synchronization, and device failure or data corruption results in permanent loss of vault information, unless a backup mechanism is added.

Evidence
Implementation and Prototype evidence is not available yet. The decision is supported by the project's scope regarding single-device systems, and section 6.2. 
Open question: What local storage format best supports encrypted vault data, and allows for reliable persistence and recovery?

Related
ADR-01. CON-02. CH-01, CH-02, CH-03.


ADR-03: Encrypt Vault Data From Password

Status - Proposed

Date and Owner: 11/7/2026 - Project Team

Context (Drivers)
The application stores information like the user's account credentials and passwords. This information must remain protected, even if unauthorized access occurs with the device's stored vault. Since the application is designed for local use, a remote service cannot perform encryption and decryption.

Options Considered
Password-Based Encryption - Derive an encryption key from the user's passkey and use it to encrypt and decrypt the vault content.
Unencrypted Local Storage - Store credentials directly in the local database or files.

Decision
Encrypt sensitive vault contents using a key derived from the user's main password. A password-based key function should be used to obtain an encryption key, and use an algorithm to protect the stored vault data against unauthorized reading and writing.

Consequences
Favors - Confidentiality, data integrity, security.
Move - Implement password-based key derivation and encryption for vault data.
Cost - Encryption and decryption introduces computational requirements, and implementation complexity. Losing the main password for the vault may also lead to the whole vault becoming unrecoverable if no other recovery mechanisms exist.

Evidence
Implementation and cryptographic testing has not been performed. This decision is based on the project's established principles involving security.
Open question: What authenticated encryption algorithm, and key derivation strategy should be chosen, and how will vault recovery be handled?

Related
ADR-01, ADR-02. QAS-01, QAS-02. CH-01.

