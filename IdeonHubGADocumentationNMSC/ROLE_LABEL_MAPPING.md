# Role Label Mapping

The concrete mapping between IdeonHub's display labels (shown in the admin panel, used to disambiguate roles across apps) and the wire values each app's own code actually expects in a token's `role` claim. See the ADR `IDEONHUB_SIDE_ROLE_LABEL_MAPPING.md` for why this mapping exists only inside IdeonHub rather than as a rename in each app.

## AssetSphere

| IdeonHub display label | Wire value sent to AssetSphere |
|---|---|
| `ASSETSPHERE_USER` | `USER` |
| `ASSETSPHERE_OPERATOR` | `OPERATOR` |
| `ASSETSPHERE_ADMIN` | `ADMIN` |
| `ASSETSPHERE_DEVELOPER` | `DEVELOPER` |

Source of truth for the wire values: `AssetsphereOrchestratorServiceLayerMSC/Models/Types/UserRoleType.cs`.

## SignForge

| IdeonHub display label | Wire value sent to SignForge |
|---|---|
| `SIGNFORGE_HR_MANAGER` | `HR_MANAGER` |
| `SIGNFORGE_ADMIN` | `ADMIN` |
| `SIGNFORGE_EXECUTIVE_DIRECTOR` | `EXECUTIVE_DIRECTOR` |
| `SIGNFORGE_DEVELOPER` | `DEVELOPER` |

Source of truth for the wire values: `SignForgeOrchestratorServiceLayerMSC/src/main/java/com/theweplm/signforge/Constants/UserRoleType.java`.

## InfraGrid

No roles exist yet — the app has no authentication or user/role model at all today. When InfraGrid gains roles, they get added to this table following the same `INFRAGRID_<ROLE>` display-label pattern; the exact role names are InfraGrid's own decision once it builds that feature.

## ObservaCore

Same situation as InfraGrid — no roles exist yet. Future entries follow `OBSERVACORE_<ROLE>`.
