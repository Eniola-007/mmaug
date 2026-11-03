# School Login Portal build decisions

Updated: 3 November 2026. This file tracks implementation decisions after the proposal was accepted. Pending items are not permissions to change the school tenant.

| ID | Decision | Status | Reason / next check |
| --- | --- | --- | --- |
| D-001 | Build the web app with Python FastAPI. | Confirmed | User preference; one modular app is manageable for a solo developer. |
| D-002 | Use the existing Microsoft Entra ID tenant for user authentication. | Confirmed | User confirmed the tenant exists. Validate tokens for one tenant and map users by `tid` + `oid`. |
| D-003 | Use the available Azure subscription for eventual deployment. | Confirmed in principle | Subscription access, region, budget, and resource permissions remain to be checked. |
| D-004 | User runs local commands; assistant provides and reviews each step. | Confirmed | User's preferred working mode. |
| D-005 | A portal administrator's deactivation action must disable the user's entire Entra account. | Confirmed requirement; design pending | User confirmed tenant-wide scope. This affects other Entra-connected services. Use a separately authorized Microsoft Graph delegated action, directory role, target safeguards, and session revocation; do not treat a local portal flag as completion. |
| D-006 | SSPR is disabled; recovery must use a school-approved administrator/helpdesk reset. | Confirmed constraint; workflow pending | Test accounts are cloud-only. A Password Administrator or other eligible role can reset a supported user's password in Entra. The portal must never collect Entra passwords. |
| D-007 | Enforce administrator MFA with an approved Entra policy. | Pending | Check tenant policy and licensing; do not assume a policy is already active. |
| D-008 | Use a separate test Entra tenant with cloud-only synthetic identities for development and the eight-user demo. | Confirmed | User confirmed the test tenant and account source. No live school accounts are needed for the demo. |
| D-009 | App-registration rights and tenant-admin support are unknown. | Pending | Check whether New registration is available and identify an administrator for Graph consent and a test password reset. Azure subscription access alone does not establish Entra permissions. |
| D-010 | Prefer delegated Graph permissions for account disable/enable. | Proposed | Microsoft Graph lists `User.EnableDisableAccount.All` plus `User.Read.All` for `accountEnabled`; the signed-in operator also needs an appropriate Entra role. Avoid tenant-wide application permissions for the web app. |
| D-011 | Disable workflow must revoke the portal's session immediately and revoke Entra sign-in sessions if authorized. | Proposed | Microsoft documents a delay before Entra token/session revocation takes effect. The portal must close its own session independently. |
