# Entra administrator handover for the School Login Portal

**Prepared:** 3 November 2026  
**Status:** Review request. No tenant changes or permission grants have been made by this guide.

## Purpose

The portal will use a **separate test Microsoft Entra tenant with cloud-only synthetic accounts** for its first build and demonstration. The project owner also requires a portal administrator to **disable and re-enable a user's entire Entra account**, not merely block access to the portal. That action affects the user's other Entra-connected services and needs a tenant-approved operator, Microsoft Graph consent, and test accounts. Self-service password reset (SSPR) is disabled, so the recovery demonstration needs an administrator/helpdesk reset route.

## Information needed from the tenant administrator

1. Confirm the owner of the **separate test tenant** and whether eight **synthetic cloud-only** users can be created or are already available for the demo.
2. Confirm that the intended Azure subscription is linked to the test tenant and identify a resource group or budget for later deployment.
3. Who may register a single-tenant **Web** application and approve its redirect URIs? An Azure subscription does not itself grant Entra app-registration rights.
4. Who can grant Microsoft Graph **delegated** permissions and assign the operator an appropriate Entra role, preferably scoped to an administrative unit if the school uses one?
5. Who will perform the SSPR-disabled password reset demonstration? Confirm a Password Administrator or another eligible role and a secure method for handing the temporary password to the synthetic user.
6. Which Entra policy will enforce MFA for the portal administrators? Confirm the policy and licensing before the demonstration.

Do not send passwords, client secrets, access tokens, MFA codes, or temporary passwords in a ticket or chat reply.

## Planned access model

| Function | Design | Approval needed |
| --- | --- | --- |
| Sign in | Single-tenant OpenID Connect authorization-code flow; FastAPI maps validated `tid` + `oid` to a pre-provisioned portal account | App registration and exact Web redirect URI |
| Portal role assignment | Stored in portal PostgreSQL; audited and checked server-side | School-approved role matrix |
| Disable/enable Entra account | Microsoft Graph `PATCH /users/{id}` with `accountEnabled`; delegated `User.EnableDisableAccount.All` plus `User.Read.All` | Admin consent, an eligible signed-in Entra administrator role, approved target scope |
| Revoke Entra sessions | Microsoft Graph `POST /users/{id}/revokeSignInSessions`; delegated `User.RevokeSessions.All` | Admin consent and test of expected timing |
| Reset password | Authorized administrator uses the Entra admin center/helpdesk route; portal never handles the password | Approved reset operator and verified delivery channel |

The initial login integration will be built and tested before requesting the Graph write permissions. The portal will reject self-targeting, privileged and break-glass accounts, and any target outside its approved roster. A Graph failure must be shown as a failed directory action, with an audit event; the portal will not claim that the account was disabled. The portal revokes its own sessions immediately after a successful disable. Entra revocation can take several minutes, and other applications must handle their own sessions.

For ordinary non-admin users, Microsoft documents **User Administrator** as an eligible role to disable accounts; more privileged targets require stronger roles. If the school uses administrative units, the administrator's role may be scoped to the appropriate users. Administrative-unit role scoping has licensing requirements and should be checked before selection. The current demo uses cloud-only test users; a later move to synchronized live users would require a separate source-of-authority review.

## References

- [Register an Entra application](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app)
- [Microsoft Graph: update a user](https://learn.microsoft.com/en-us/graph/api/user-update?view=graph-rest-1.0)
- [Microsoft Graph: revoke sign-in sessions](https://learn.microsoft.com/en-us/graph/api/user-revokesigninsessions?view=graph-rest-1.0)
- [Entra: reset a user's password](https://learn.microsoft.com/en-us/entra/fundamentals/users-reset-password-azure-portal)
- [Entra: revoke access and account disable steps](https://learn.microsoft.com/en-us/entra/identity/users/users-revoke-access)
- [Entra administrative units](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/administrative-units)
