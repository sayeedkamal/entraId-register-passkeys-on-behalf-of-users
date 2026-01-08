# Setting Up the Project Environment

Follow these steps to create a Python virtual environment (venv), install dependencies, and work inside the environment. Instructions are provided for both macOS and Windows.

1. Create a Virtual Environment
# On macOS or Linux
python3 -m venv venv

# On Windows
python -m venv venv

⚠️ Caution:
It is recommended to use Python 3.11 for this project to ensure compatibility with all dependencies.

This will create a folder named venv containing an isolated Python environment.

2. Activate the Virtual Environment
# macOS / Linux
source venv/bin/activate

# Windows (PowerShell)
.\venv\Scripts\Activate.ps1

# Windows (Command Prompt)
.\venv\Scripts\activate.bat

After activation, your terminal prompt should show (venv).

3. Install Dependencies
pip install -r requirements.txt

This installs all required Python libraries listed in requirements.txt.

4. Deactivate the Virtual Environment
deactivate

Use this command when you want to exit the virtual environment.

⚠️ Important Windows-Specific Requirements
1. Run Scripts in Administrator Mode (Windows Only)

When running this project on Windows, the scripts must be executed from an Administrator-elevated terminal.

✔ Required for:

Windows WebAuthn API access

FIDO2 security key operations

PIN and user verification workflows

To do this:

Open Command Prompt or PowerShell

Right-click → Run as Administrator

2. PIN_INVALID Error Even When PIN Is Correct

In some cases, the script may repeatedly fail with a PIN_INVALID error even though the correct PIN is being entered, for example:

fido2.ctap.CtapError: CTAP error: 0x31 - PIN_INVALID
...
fido2.client.ClientError: (<ERR.BAD_REQUEST: 2>, CtapError('CTAP error: 0x31 - PIN_INVALID'))


This issue is often caused by:

A corrupted virtual environment

Cached Python bytecode

Stale or mismatched library versions (e.g., after upgrades)

🔧 Recommended Fix: Reset the Virtual Environment (Safe)

If you encounter this issue, resetting the virtual environment usually resolves it.

deactivate
rmdir /s /q venv
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt

✔ This guarantees a clean interpreter state
✔ Clears cached bytecode and compiled artifacts
✔ Reinstalls dependencies consistently

✅ Additional Notes

1. Always activate the virtual environment before running any scripts.
2. If switching between operating systems (macOS ↔ Windows), do not copy the venv folder.
3. Recreate it on the target OS and reinstall dependencies using requirements.txt.

If issues persist after a venv reset, verify:

1. You are using Python 3.11
2. The terminal is running in Administrator mode (Windows)
3. The correct security key is connected

# entra-Id-register-passkeys-on-behalf-of-users
- This project will use Microsoft Graph APIs to provision FIDO2 credentials on a FIDO2 security key. 
- This project is an unsupported proof of concept.
- This project is a simplistic demonstration of how to use the Microsoft Graph APIs to register a FIDO2 security key with a CTAP client. 

## Overview
Microsoft Graph APIs allow for customers to develop solutions that allow for FIDO2 security keys to be registered using an admin driven on-behalf-of registration flow. Customers may use this type of flow for a variety of reasons including:

1. To reduce user friction and remove the burden of registering security keys from their users.  The admin performs all the registration tasks so that the user doesn’t need to take the time and learn how to do it.

2. As a passwordless bootstrapping method so that the user doesn’t have to be issued a password, or Temporary Access Pass (TAP).

3. As a high-assurance workflow where the organization only allows administrators to register authenticators into their environment, so they can identity proof the user, and provide them with an authenticator that the company has purchased.

4. Thick client apps can support additional capabilities that aren’t supported from WebAuthn or from a browser. Additional capabilities would be available to thick clients like enforcing PIN complexity with the minPinLength or forcePinChange [CTAP2.1](https://fidoalliance.org/specs/fido-v2.1-rd-20210309/fido-client-to-authenticator-protocol-v2.1-rd-20210309.html#sctn-feature-descriptions) extensions. The security key must support the extensions see: https://www.yubico.com/blog/now-available-for-purchase-yubikey-5-series-and-security-key-series-with-new-5-7-firmware/. A thick client could also record serial numbers of the registered FIDO2 security keys for tracking purposes.

5. As a recovery mechanism after a user loses all of their security keys and other high-assurance authenticators and is locked out of their account.


## Major components
- Microsoft Graph FIDO2 credential registration API
- A thick client that supports CTAP or leverages system interfaces to interact with security keys.
- A FIDO2 security key like a YubiKey

![Sequence Diagram](images/SolutionOverview-FIDO2-security-key-Admin-on-behalf-of-registration.png)

## Scenarios supported
- Administrator led registration of users' primary and backup FIDO2 security keys.
- Bulk registration of security keys during FIDO2 security key deployment to an organization
- Recovery after a user loses all of their authenticators and is locked out of the account.

## Sample code
- [Bulk registration sample code](bulkRegistration/BulkRegistration.md)