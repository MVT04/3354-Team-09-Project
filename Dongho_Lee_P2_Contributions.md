# 5.1 Domain Model

## Domain Model Classes

### AccountHolder
- Represents a user who owns and accesses a password vault.
- Key attribute: accountId
- Each AccountHolder owns one Vault.
- Each AccountHolder specifies 0..* PasswordCriteria.

### Vault
- Represents the collection of credentials owned by an account holder.
- Key attribute: lockStatus
- Each Vault belongs to one AccountHolder.
- Each Vault contains 0..* Credentials.

### Credential
- Represents login information stored for a specific service or website.
- Key attributes: service, username, password
- Each Credential belongs to one Vault.

### PasswordCriteria
- Represents requirements selected when generating a password.
- Key attributes: length, allowedCharacters, requiredStrength
- An AccountHolder can define 0..* PasswordCriteria.
- Each PasswordCriteria can produce 0..* GeneratedPassword.

### GeneratedPassword
- Represents a password created according to selected criteria.
- Key attributes: value, strengthRating
- Each GeneratedPassword is produced from one PasswordCriteria.


# 6.3 Component View

## User Interface
- Responsibility: Receives account-holder requests and presents results from the password manager.
- Provides: UserInteraction
- Requires: Authentication, CredentialManagement, PasswordGeneration
- Answers to: UC-01, UC-02, QAS-04

## Authentication / Vault Access
- Responsibility: Verifies access to the vault and manages locked/unlocked state.
- Provides: VaultAccess
- Requires: VaultStorage
- Answers to: FR-01, QAS-01, QAS-03

## Credential Management
- Responsibility: Creates, retrieves, edits, and deletes stored credentials.
- Provides: CredentialManagement
- Requires: VaultAccess, VaultStorage
- Answers to: UC-02, FR-02, FR-03, FR-04, FR-07

## Password Generation
- Responsibility: Generates passwords according to user-selected restrictions and evaluates their strength.
- Provides: PasswordGeneration
- Requires: UserInteraction
- Answers to: UC-01, FR-05, FR-06

## Vault Storage / Encryption
- Responsibility: Stores credentials in encrypted local storage and retrieves them for authorized components.
- Provides: SecureVaultStorage
- Requires: Local File System
- Answers to: QAS-02, FR-02, FR-03


# 9.1 Extended Traceability Matrix

Section 9.1 was updated to trace the project requirements through use cases, analysis classes, architectural components, quality scenarios, architectural decisions, and supporting evidence.

The matrix includes:
- FR-01 through FR-10
- QR-01 through QR-04
- CON-01 through CON-03
- FR-08 retained as Withdrawn under CH-03
- Links from analyzed use cases to Sections 4.2 and 5.2
- Links from quality requirements to QAS-01 through QAS-04
- Links from constraints to architectural decisions and Sections 6.2 and 6.4

# 11 Open Questions

- Updated the P1 open questions based on the current P2 decisions.
- Closed Q1 after autofill was removed under CH-03.
- Closed Q2 and Q3 based on the current backup and restore design in UC-03.
- Added Q4 from ADR-02 about the local storage format.
- Added Q5 from ADR-03 about master password recovery.
- Updated the owners and due dates for the questions.