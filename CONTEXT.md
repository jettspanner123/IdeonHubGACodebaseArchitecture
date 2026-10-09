# IdeonHub

Documents and indexes the architecture of IdeonHub, the centralized authentication system for WePLM's internal applications (AssetSphere, SignForge, InfraGrid, ObservaCore, and an open-ended list of future tools). The system's actual Spring Boot implementation lives as a microservice subfolder inside this same repository, rather than as a separate top-level repo.

## Language

**MSC**:
Folder-name suffix marking a project as a deployable service-layer component (an application, as opposed to scripts or documentation). In this codebase family, confirmed to mean "Microservice."
_Avoid_: Service (alone), backend

**IdeonHub**:
The standing name for this system — both the repo and the Spring Boot microservice at `IdeonHubGAServiceLayerMSC/` that implements it. A standalone, standards-based (OIDC-flavored) identity provider: it owns the login page, the client registry, user entitlements, and — eventually — the single source of truth for user identity and profile/roles across all Client Applications. "GA" and "GAuth" are retired as spoken/prose names (they persist only inside existing folder/package names like `IdeonHubGAServiceLayerMSC` and `com.weplm.ga`, which is a separate, not-yet-done cleanup).
_Avoid_: GA, GAuth, Global Authenticator (spelled out), auth service, authenticator, this repo (ambiguous — the repo is the architecture index; the service is the code inside it)

**Client Application**:
Any consumer of IdeonHub — a web app, CLI tool, desktop app, or mobile app. Today: AssetSphere, SignForge, InfraGrid, and ObservaCore (all web apps); the list is open-ended. A Client Application never collects credentials itself: it redirects the user to IdeonHub's login page, and IdeonHub redirects back with a token once the user authenticates. Each Client Application is a row in IdeonHub's client registry (a `client_id`, its allowed redirect URIs/schemes, and its own declared roles — see **Role Label**).
_Avoid_: The app, the website, the tool (all ambiguous about which side of the redirect they mean)

**Role Label** (display label) vs. **Wire Value**:
Every role IdeonHub declares for a Client Application has two forms: the **display label** IdeonHub's own admin panel shows, prefixed per app to disambiguate (`ASSETSPHERE_ADMIN`, `SIGNFORGE_ADMIN` — same word, different apps, different meaning), and the **wire value**, the literal string that app's own code already expects in a token's `role` claim (`ADMIN`, unprefixed, unmodified). IdeonHub translates between the two at the boundary; neither AssetSphere nor SignForge's own code or database is ever changed or even aware the prefix exists. See `ROLE_LABEL_MAPPING.md` for the concrete table.
_Avoid_: Role (ambiguous about which of the two forms is meant — always say "display label" or "wire value" when it matters)

**Entitlement**:
The record of one user's access to one Client Application — a `(user, app, role)` tuple. Having an IdeonHub account never implies access to any app; access is always this explicit, per-app, per-role grant. A user can hold entitlements to some apps and not others, and a different role in each app they do have access to.
_Avoid_: Permission (too generic — this specifically means "may log into this app, with this role"), access (ambiguous about whether it means the IdeonHub account or a specific app's entitlement)

**IDEON_HUB_SUPER_ADMIN**:
The one IdeonHub-level role, independent of any Client Application's own roles, that can use IdeonHub's central admin panel — granting or revoking entitlements, assigning roles, and registering new Client Applications. Distinct from being an "admin" of any individual app (e.g. AssetSphere's own `ADMIN` role grants no IdeonHub admin-panel access).
_Avoid_: Admin, super admin (without the exact role name — this is a specific, named role, not a loose description)

**Shared Identity Payload**:
The minimal identity data IdeonHub hands a Client Application on login: `id`, `name`, `email`, and that app's own entitlement role. Not hard-capped to those four fields — things like avatar or department can ride along when relevant — but deliberately not a full profile sync by default, since duplicated profile data drifts out of sync once it's copied across five systems.
_Avoid_: User data, profile (too broad — this specifically means what crosses the IdeonHub → Client Application boundary)
