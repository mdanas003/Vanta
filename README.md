# Vanta

A lab design for giving each campus Wi-Fi user an individual identity instead of sharing one network password. The project documents an 802.1X / WPA2-Enterprise authentication service built around FreeRADIUS and OpenLDAP, packaged with Docker Compose.

## What it demonstrates

- Per-user authentication through RADIUS, with user records and group membership held in LDAP.
- EAP-based authentication profiles, including PEAP with MS-CHAPv2 and EAP-TTLS with PAP.
- Group-based authorization, including accept or reject decisions, session limits, and optional VLAN attributes.
- RADIUS accounting for session start, interim updates, and stop events, with queryable records and readable detail logs.
- A repeatable test suite that exercises authentication and policy behavior without requiring campus Wi-Fi hardware.

## Authentication flow

1. A device connects to a compatible access point and starts an 802.1X exchange.
2. The access point forwards the authentication conversation to FreeRADIUS.
3. FreeRADIUS validates the user through OpenLDAP and applies the configured group policy.
4. The service returns an accept or reject decision and, when configured, session and VLAN attributes.
5. Accounting records capture the session lifecycle.

## Components

The lab uses separate services for FreeRADIUS, OpenLDAP, a local directory administration interface, and automated tests. The directory and administration services are kept off the network-facing surface; the RADIUS service handles access-point requests. Docker Compose coordinates service startup, configuration, persistent data, and test execution.

## Security considerations

This is a lab and reference implementation, not a drop-in production deployment. Use a trusted server certificate and verify it on clients. Keep credentials, generated keys, and certificates out of version control. Limit directory and administration access, and review firewall and shared-secret configuration for the target network.

The supported authentication profiles have different security properties and client compatibility. MS-CHAPv2 is retained for compatibility in this lab and has known limitations; assess current campus security requirements before deploying any profile in production. Access-point and VLAN behavior should be validated on the actual hardware.

## Status

This repository currently contains the README only. It describes the Campus Wi-Fi AAA Lab architecture; implementation files and configuration have not yet been added.
