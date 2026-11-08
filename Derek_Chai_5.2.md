# 5.2 Object Interaction Models

Author: Derek Chai (Derek-Chai)

## 5.2.1 UC-01 Generate New Password

**(a) Non-trivial steps.** Step (4) is non-trivial: the system records the restrictions, checks that they do not conflict (A1), produces a password and rates its strength. Steps (1), (3) and (5) are actor actions. Steps (0) and (2) only display options or ask for input, and step (6) only confirms the accepted password.

**(b) Scenario table.**

| Step | Subject (who acts) | Subject action (what it does) | Other data / objects (what is used) | Object acted upon (who receives) |
|---|---|---|---|---|
| 1 | AccountHolder | Submits the password restrictions | length, allowedCharacters, requiredStrength | GeneratorController |
| 2 | GeneratorController | Records the chosen restrictions | length, allowedCharacters, requiredStrength | PasswordCriteria |
| 3 | GeneratorController | Asks whether the restrictions conflict | | PasswordCriteria |
| 4 | PasswordCriteria | Reports the restrictions are consistent | | GeneratorController |
| 4a | PasswordCriteria | Reports which restrictions conflict (A1) | length, requiredStrength | GeneratorController |
| 4b | GeneratorController | Tells the account holder what conflicts and asks to change it | conflicting restrictions | AccountHolder |
| 5 | GeneratorController | Asks for a password that fits the restrictions | | PasswordCriteria |
| 6 | PasswordCriteria | Produces a candidate password | value | GeneratedPassword |
| 7 | GeneratorController | Asks for its strength rating | value | GeneratedPassword |
| 8 | GeneratedPassword | Reports its strength rating | strengthRating | GeneratorController |
| 9 | GeneratorController | Shows the password and its strength rating | value, strengthRating | AccountHolder |
| 10 | AccountHolder | Accepts the password | value | GeneratorController |
| 10a | AccountHolder | Cancels (A3) | | GeneratorController |
| 10b | GeneratorController | Confirms no password was kept | | AccountHolder |
| 11 | GeneratorController | Confirms the password is ready to use | value | AccountHolder |

GeneratorController is an interaction object that coordinates password generation and is in control of this interaction. Messages 5 to 9 repeat each time the account holder rejects the offered password (A2).

**(c) Analysis sequence diagram.** See `diagrams/uc01-sequence.png` (source: `diagrams/uc01-sequence.puml`).

## 5.2.2 UC-02 Store New Credential

**(a) Non-trivial steps.** Step (4) is non-trivial: the system checks that the details are complete, checks that the vault is unlocked, checks for a duplicate, creates the credential and adds it to the vault. Steps (1) and (3) are actor actions, step (2) only asks for input, and step (0) only displays the existing list.

**(b) Scenario table.**

| Step | Subject (who acts) | Subject action (what it does) | Other data / objects (what is used) | Object acted upon (who receives) |
|---|---|---|---|---|
| 1 | AccountHolder | Submits the new credential's details | service, username, password | CredentialController |
| 2 | CredentialController | Checks that every required detail is present | service, username, password | CredentialController |
| 2a | CredentialController | Names the missing detail and keeps what was entered (A1) | missing detail | AccountHolder |
| 3 | CredentialController | Asks whether the vault is unlocked | lockStatus | Vault |
| 4 | Vault | Reports the vault is unlocked | lockStatus | CredentialController |
| 4a | Vault | Reports the vault is locked (A5) | lockStatus | CredentialController |
| 4b | CredentialController | Asks the account holder to unlock before saving | | AccountHolder |
| 5 | CredentialController | Asks whether a credential exists for this service and username | service, username | Vault |
| 6 | Vault | Reports no matching credential | | CredentialController |
| 6a | Vault | Reports a matching credential (A4) | existing Credential | CredentialController |
| 6b | CredentialController | Shows the existing credential and asks to replace or cancel | existing Credential | AccountHolder |
| 7 | CredentialController | Creates the credential from the entered details | service, username, password | Credential |
| 8 | CredentialController | Asks the vault to add the credential | Credential | Vault |
| 9 | Vault | Confirms the credential is stored | | CredentialController |
| 9a | Vault | Reports the credential could not be stored (A2) | | CredentialController |
| 9b | CredentialController | Tells the account holder nothing was saved and to re-enter the details | | AccountHolder |
| 10 | CredentialController | Confirms the save and shows the credential in the list | service, username | AccountHolder |

CredentialController is an interaction object that coordinates storing a credential and is in control of this interaction.

**(c) Analysis sequence diagram.** See `diagrams/uc02-sequence.png` (source: `diagrams/uc02-sequence.puml`).
