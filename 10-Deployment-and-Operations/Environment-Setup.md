# Environment Setup

Document status: Draft v0.1 (Stage 3 part 2). Proposed. Vendor-neutral. Hosting is undecided (OQ-016), so sizes and providers are not given.
Related: [Deployment Architecture](../04-System-Architecture/Deployment-Architecture.md), [Infrastructure Architecture](../04-System-Architecture/Infrastructure-Architecture.md), [Test Data Management](../08-QA-and-Testing/Test-Data-Management.md), [CI/CD Strategy](../09-Development-and-Delivery/CICD-Strategy.md).

## 1. Environments

| Environment | Purpose | Data | Topology | Access | Refresh |
|---|---|---|---|---|---|
| Local | Developer work | Synthetic | Containers on a workstation | Developer | On demand |
| Dev | Integration of merged changes | Synthetic | Single instance each, shared database | Developers, QA | Rebuilt often |
| Test | QA execution | Controlled synthetic baselines | Same components as production at small size | QA, developers read | Nightly rebuild |
| Staging | Release rehearsal, performance, UAT, migration rehearsal, penetration test | Masked or synthetic at sized volume | Same topology as production (HA, replica, workers) | Named testers, operations | Before each release cycle |
| Production | Live | Real | HA across zones, backups, monitoring | Restricted, MFA, logged | None |
| DR | Recovery | Replica and backups of production | Per DR level (cold, warm or hot, OQ-028) | Operations | Continuous replication |

## 2. Rules for all environments

1. Built from code (infrastructure as code) and the same container images (TR-017, NFR-038).
2. Differences are configuration: sizes, endpoints, feature flags, secrets, stub or real partner connections.
3. Production data is never copied to lower environments unless masked and the copy is approved (Data Protection DP-005).
4. Secrets come from the secret store, unique per environment (SEC-038).
5. Time synchronisation on all hosts (TR-016).
6. Each environment has monitoring and a named owner.

## 3. Configuration items per environment

| Item | Notes |
|---|---|
| Application and worker instance counts and sizes | Proposed starting point in Deployment Architecture section 5, TBD |
| Database primary, standby, replica, size, parameters | Staging and Production only for standby and replica |
| Object storage buckets, encryption keys | Per environment, per tenant prefix |
| Cache | Optional |
| Identity provider settings (SSO) | Test tenant for lower environments (OQ-023) |
| Mail provider | Sandbox mode in lower environments, real in Production |
| Bank and gateway adapters | Stubs in Dev and Test. Provider sandbox in Staging. Real in Production |
| Feature flags | Environment defaults documented |
| Rate limits | Lower in lower environments, real values in Staging and Production |
| Log level and retention | Debug allowed only in Dev |
| Domain names and certificates | One per environment, managed and rotated |

## 4. Provisioning procedure (outline)

1. Create network zones and security groups from code.
2. Provision database, object storage, key store, secret store, cache.
3. Deploy application and workers from the approved image.
4. Apply migrations. Seed system roles, permissions, status lists and code lists.
5. Create the first tenant and company by script. Create the break-glass administrator with MFA (Authentication and Authorization section 2).
6. Load configuration: numbering, settings, chains, templates.
7. Connect partner adapters or stubs.
8. Run the smoke suite. Record the result.
9. Enable monitoring and alerts. Confirm a test alert reaches the on-call channel.
10. Record the build, configuration and approvals in the environment log.

## 5. Access control

| Role | Dev | Test | Staging | Production |
|---|---|---|---|---|
| Developers | Full | Deploy through pipeline, read | Read logs, no data access | None, break-glass by approval |
| QA | Read | Full | Execute tests | None |
| Operations | Support | Support | Full | Full, with MFA and session logging |
| Security tester | Scan | Scan | Scan and test with approval | None |
| Business testers | None | None | UAT access | None |

Access to Production is by named accounts only, reviewed every quarter (proposed), and logged (AUD-092).

## 6. Environment readiness checklist

| Item | Done when |
|---|---|
| Infrastructure from code | Plan applied with no drift |
| Smoke suite | 13 cases pass |
| Baseline data (Test and Staging) | BASE sets loaded and checked |
| Stubs or partner sandboxes | Failure modes configurable |
| Monitoring | Dashboards live, test alert received |
| Backup (Staging and Production) | First backup and a restore test done |
| Access list | Reviewed by the owner |

## 7. Open points

Hosting and sizes (OQ-016, OQ-015), SSO test tenant (OQ-023), partner sandboxes (OQ-017), DR level (OQ-028).
