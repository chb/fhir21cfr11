---
title: Example Source Code
permalink: /docs/example-source-code/
---

Below are links and documentation for a full example deployment of a demonstration 21 CFR 11-compliant FHIR client and server. The system is built from open source applications, frameworks and developed code. The server code is integrated with a Docker Compose configuration that starts 7 containers to run and provision the example server systems. The simple 21CFR11 PRO client runs as a standalone application from the command line.

## Source

The example FHIR 21CFR11 PRO IG implementation server is available here: 

 * <https://github.com/chb/hapi-fhir-jpaserver-oauth-21cfr11>. 

The Keycloak Auth server <https://github.com/chb/keycloak-auth-bch> is built into the local docker repository and run in the docker-compose instance by the docker-compose.yaml script in <https://github.com/chb/hapi-fhir-jpaserver-oauth-21cfr11>. The docker-compose file also executes a job that provisions a test realm and user for client testing. 

Client interactions with the 21CFR11-compliant server are demonstrated with a command-line client available from <https://github.com/chb/fhir-21cfr11pro-client-example>  

## Context

![System Context]({{ '/assets/img/example-source-code_context.png' | relative_url }})

## Client operation

The test client uses a pre-provisioned user and password to login to the AuthServer authentication service. A more sophisticated system might provide self-service enrolment for study subjects, but in our client an administrator has provisioned the user and their credentials into the IdProvider in advance.

When the client application has logged in, it can generate an identity certificate or use one that is prepared earlier. When the certificate is available it creates a standard certificate request and sends it to the AuthServer's signing (`/sign`) endpiont.

The AuthServer's signing endpoint examines the signing request to determine (at a minimum) that the principal in the certificate to be signed is the same as the principal that has already authenticated with the AuthServer. More sophisticated implementations may have other checks, or use a proxy for real pricipal names to verify the identity of the requestor.
More sophisticated signing services might operate a Managed PKI (MPKI) certifying authority backed by a security services vendor instead of a self-signed CA.

If the AuthServer is satisfied with the request then it signs the certificate using its private key and returns the identity certificate in the response.

Now the client has a certificate signed by a CA that it can provide to systems that need to verify the provenance of data, like auditors of a FHIR server storing PRO data.

The client `POSTS` a copy of its newly-signed certificate to the FHIR server as an embedded value in a `DocumentReference` resource. It then uses the returned resource JSON to broadly verify the accurate representation of the `DocumentReference` resource and generates a `Provenance` resource containing a digital signature for a canonical version of the `DocumentReference` resource.

Once the certificate is stored on the FHIR server and the Provenance for that certificate is stored, the client can POST PRO QuestionnareResponse resources in the same way. First sending the Questionnaire Response, examining the returned JSON with the FHIR server's fixed-up version of the resource, then generating a digital signature of the canonical JSON version of the resource to include in a POST-ed Provenance resource.

An auditor can use the client's ID certificate to validate any resource against its corresponding Provenance digital signature.

![Client Operation]({{ '/assets/img/example-source-code_client-operation.png' | relative_url }})

## Technology
- Java 11
- Maven for Java dependency management
- Jetty
- Spring Framework
- [Keycloak](https://www.keycloak.org) OpenID Connect and OAuth2
- [HAPI FHIR](https://hapifhir.io/) Java FHIR Client and Server templates and frameworks
- [ImmuDB](https://immudb.io) 
- Postgres

## Notes
- [ProvenDB](https://www.provendb.com) and [AWS QLDB](https://aws.amazon.com/qldb/) are alternatives for ImmuDB
