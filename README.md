# The Crucible

The Crucible is the CyberHub cyber range and CTF platform. It provides isolated, repeatable environments for hands-on offensive and defensive training, including solo lanes, team lanes, and competitive formats.

The Crucible is planned and under active design. This document describes the target capabilities and the intended integration points with CyberCore.

## Goals

1. Provide safe, isolated training environments for club members
2. Support repeatable lab and CTF experiences with reliable resets
3. Enable competitive formats such as King of the Hill and attack-defense
4. Provide operator visibility, abuse controls, and auditing
5. Scale to 10 to 50+ concurrent users depending on hardware capacity

## Core Concepts

- Lane: An isolated network slice provisioned for a user or team with defined targets and access rules.
- Scenario: A packaged environment that defines targets, services, flags, and validation logic.
- Session: A time-bounded allocation of one or more lanes to a user or team.
- Provider: External systems used for provisioning such as Proxmox, OPNsense, Ceph, and supporting services.

## Range Types

Planned lane types include:

1. Single player vs single target
2. Single player vs multiple targets and networks
3. Multiple players vs single target (King of the Hill)
4. Team vs single target
5. Team vs multiple targets and networks
6. Team vs team (attack and defend)
7. Live SOC incident response

## CyberCore Integration

The Crucible relies on CyberCore as the control plane. CyberCore records allocations and triggers provisioning workflows.

CyberCore n8n workflows are expected to:

1. Create and manage lane allocations and TTL
2. Provision lane networks and routing rules
3. Deploy scenario targets from templates
4. Configure user access artifacts and rules
5. Drive lane resets and rebuilds
6. Record audit events and state transitions
7. Synchronize scoring and badge events

## Provisioning and Isolation

The Crucible is expected to use:

- Proxmox for VM and container provisioning
- Proxmox SDN and VXLAN for lane isolation
- OPNsense for access enforcement, routing, and VPN termination where required
- Ceph for VM disk storage and rapid clone workflows where applicable

Isolation requirements:

- No direct east-west traffic between lanes unless explicitly configured
- Per-lane access rules that are enforced at the lane boundary
- Deterministic teardown and cleanup on lane expiration

## Flags and Scoring

The Crucible is expected to support:

- User-level and root-level flags for target machines
- Service flags for multi-host scenarios
- Central validation of submitted flags
- Per-scenario scoring rules
- Scoreboards for competitive formats

Flag validation and scoring should be backed by CyberCore state tracking, with an auditable record of submissions and outcomes.

## Operator Controls

The Crucible should include operator visibility and controls such as:

- Lane inventory and current allocation state
- Lane reset and rebuild controls
- Abuse detection and rate limiting on provisioning actions
- Logging and audit trails for user actions and system operations
- Health checks for scenarios and targets

## Shared Services

Planned shared services that may be exposed to players:

1. Web-based Kali access for users who cannot run a local VM
2. Defensive visibility stack for training (SIEM and SOAR patterns)
3. Password cracking resources for approved challenges and exercises

Shared services should be policy-gated and tied to user roles and course completion where applicable.

## Content Authoring

Scenario content should be:

- Versioned and reproducible
- Documented with operator notes and walkthroughs when appropriate
- Designed with safety controls to prevent lateral movement outside the lane
- Tested with expected solve paths and reset behavior

## Status

The Crucible is under active design. Scenario development, orchestration workflows, and scoring implementation will be tracked as implementation begins.