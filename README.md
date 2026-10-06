# VMware VCF 9 Zero Trust Lab

**Designing and validating a Zero Trust architecture on VMware Cloud Foundation 9** — from hypervisor to orchestrated SDDC, with measured security validation rather than a theoretical design.

Built during my PFA internship at INEOS Solutions (Cloud & Data Center Infrastructure team), as a Proof of Concept for migrating a traditional perimeter-secured virtualization environment toward a Zero Trust model.

![Zero Trust Architecture](architecture/zero-trust-architecture.png)

## Why this project

Traditional perimeter security assumes that anything inside the network boundary can be trusted. Once an attacker breaches that boundary, they can move laterally with little resistance. This lab answers a concrete question: **how do you apply Zero Trust principles — explicit verification, least privilege, micro-segmentation — natively inside a VMware virtualization stack, without making the platform unmanageable for operations teams?**

## What's in this repo

| Folder | Content |
|---|---|
| [`architecture/`](architecture/) | The target architecture diagram (Management Domain / Workload Domain split, NSX DFW enforcement points) |
| [`documentation/`](documentation/) | Component-by-component write-ups: ESXi, vSAN, NSX, VCF/SDDC Manager, and the Zero Trust model applied |
| [`policies/`](policies/) | The actual NSX Distributed Firewall rule set and network addressing plan used in the lab |
| [`testing/`](testing/) | The 22-scenario test plan and full validation results |
| [`screenshots/`](screenshots/) | Captures from the live environment referenced throughout the docs |

## Technology stack

`VMware ESXi 8.0 U3` · `vCenter Server` · `vSAN` (hyperconverged storage, data-at-rest encryption) · `NSX` (Distributed Firewall, micro-segmentation, Tier-0/Tier-1 gateways) · `VMware Cloud Foundation 9` · `SDDC Manager`

## Architecture at a glance

- **Management Domain** (isolated): vCenter Server, NSX Manager, SDDC Manager — Lockdown mode enabled on every host, encrypted vSAN datastore
- **Workload Domain**: web and database segments, each tagged by role (`role:web`, `role:db`) rather than static IP
- **NSX Distributed Firewall**: deny-all by default; every allowed flow is an explicit, documented exception
- **Encryption at rest**: vSAN Data-at-Rest Encryption via a KMS, KMIP-based

Full design rationale and trade-offs are in [`documentation/06-zero-trust-model.md`](documentation/06-zero-trust-model.md) and [`documentation/01-context-and-requirements.md`](documentation/01-context-and-requirements.md).

## Validation results

| Test family | Executed | Passed | Compliance |
|---|---|---|---|
| Connectivity | 6 | 6 | 100% |
| Security & legitimate flows | 4 | 4 | 100% |
| Micro-segmentation (DFW) | 5 | 5 | 100% |
| Access control | 3 | 3 | 100% |
| Logging | 2 | 2 | 100% |
| Resilience | 2 | 2 | 100% |
| **Total** | **22** | **22** | **100%** |

Full methodology, individual test cases, and raw results: [`testing/`](testing/).

## Honest scope and limitations

This is a lab-scale Proof of Concept, not a production deployment or an independent security audit. Specifically **not** covered:
- No independent penetration test
- No load testing under production-representative traffic
- No enterprise IAM / MFA integration (Identity Firewall validated at the RBAC level only)
- No automated SIEM correlation of DFW logs

These are documented explicitly rather than glossed over — see [`documentation/06-zero-trust-model.md`](documentation/06-zero-trust-model.md) for the full discussion and the roadmap toward closing them.

## About me

Ilyas Benkhadra — Cybersecurity Engineering student (Cloud Security Engineering specialization), ENSA Oujda.
[LinkedIn](#) · [Other portfolio repos](#)
