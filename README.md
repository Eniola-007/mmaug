# MMAUG School Login Portal

Planning documents for a secure school login portal using Python FastAPI, Microsoft Entra ID, PostgreSQL, and Azure.

**Status:** Proposal and build planning. The application has not been implemented or deployed.

## Documents

- [Original participant RFP](02_School_Login_Portal_Participant_RFP%202.pdf)
- [FastAPI proposal](MMAUG_Secure_Login_Portal_FastAPI_Proposal.md) ([PDF](MMAUG_Secure_Login_Portal_FastAPI_Proposal.pdf))
- [Step-by-step build guide](BUILD_GUIDE.md) ([PDF](BUILD_GUIDE.pdf))
- [Build decisions](BUILD_DECISIONS.md)
- [Entra administrator handover](ENTRA_ADMIN_HANDOVER.md)

The build guide records the confirmed direction: a separate test Entra tenant, cloud-only synthetic accounts, and administrator-assisted password recovery. It also records the requested whole-account deactivation workflow. Where the earlier proposal differs, follow the newer [build decisions](BUILD_DECISIONS.md) until the school approves a change.

The next step is the read-only Entra and Azure check in [Step 0 of the build guide](BUILD_GUIDE.md#step-0-decisions-to-record-before-identity-code). Keep passwords, client secrets, tokens, and recovery codes out of this repository.
