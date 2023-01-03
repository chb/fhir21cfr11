---
title: Overview
permalink: /docs/home/
redirect_from: /docs/index.html
---

U.S. requirements for FDA regulated clinical studies are provided in U.S. 21 CFR Part 11, which describes a set of requirements for closed systems intended to ensure that clinical data is collected from authentic sources and stored in a system that protects the integrity of the data in a verifiable way.

The FHIR standard will be increasingly important for FDA studies reliant on real-world data (e.g. disease registry data, patient-reported outcomes, and electronic health data records), and without guidance implementers may resort to a variety of bespoke or proprietary approaches to meeting 21 CFR Part 11 requirements.

21 CFR Part 11 requires that a system implement measures that protect it from external and internal attacks[1] and provide "the ability to discern invalid or altered records"

If a FHIR-based warehouse is to protect against tampering and make tampering evident to system operations and auditors then it must maintain a log of transactions with digital signatures, and a way of proving that the log itself has not been altered or fabricated.

A process that derives benefit from a 21 CFR Part 11-compliant system is safety and adverse event reporting and analyses. A 21 CFR Part 11 system provides assurances about the integrity of data provided to report and act on safety events.

This project describes two complementary approaches to meeting 21 CFR Part 11 requirements for authenticity, integrity and non-repudiability of captured PRO data:

- FHIR client-generated signatures of verifiably stored client-originated resources
- Server-side implementation of a digitally verified change ledger

These requirements are not yet strictly met with current FHIR guidance and documents a recommended guidance for meeting 21 CFR 11 requirements when using a FHIR server as the repository for real-world data.
