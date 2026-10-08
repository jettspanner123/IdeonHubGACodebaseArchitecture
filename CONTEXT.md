# IdeonHub GA

Documents and indexes the architecture of IdeonHub's global authentication system (GA). The system's actual Spring Boot implementation lives as a microservice subfolder inside this same repository, rather than as a separate top-level repo.

## Language

**MSC**:
Folder-name suffix marking a project as a deployable service-layer component (an application, as opposed to scripts or documentation). In this codebase family, confirmed to mean "Microservice."
_Avoid_: Service (alone), backend

**GA** (Global Authenticator):
The Spring Boot microservice, at `IdeonHubGAServiceLayerMSC/`, that implements the authentication system whose architecture this repo's graph documents. Acts as a standalone, standards-based (OIDC-flavored) identity provider: it owns the login page, the client registry, and — eventually — the single source of truth for user identity and profile/roles across all Client Applications. This is the only form of the name used — it is never spelled back out as "Global Authenticator" or "GAuth" in prose, docs, or code.
_Avoid_: GAuth, Global Authenticator (spelled out), auth service, authenticator, this repo (ambiguous — the repo is the architecture index; the service is the code inside it)

**Client Application**:
Any consumer of GA — a web app, CLI tool, desktop app, or mobile app (e.g. SignForge, AssetSphere, and future internal tools). A Client Application never collects credentials itself: it redirects the user to GA's login page, and GA redirects back with a token once the user authenticates. Each Client Application is a row in GA's client registry (a `client_id` plus its allowed redirect URIs/schemes).
_Avoid_: The app, the website, the tool (all ambiguous about which side of the redirect they mean)
