---
title: "Reality Execution Assurance (REA) Conceptual Framework"
abbrev: "REA Framework"
category: info
docname: draft-todo-yourname-protocol-latest
submissiontype: IETF
consensus: true
v: 3
area: Security
workgroup: Network Management Research Group
keyword:
 - next generation
 - reality execution
 - digital trust

author:
 -
    fullname: "Your Name Here"
    organization: "Your Organization Here"
    email: "your.email@example.com"

--- abstract

This document defines the problem domain of Reality Execution Assurance (REA). 
As digital requests increasingly trigger physical execution in the real world 
through autonomous agents and physical AI, traditional virtual protocols reveal 
critical verification gaps. This framework proposes a methodology to appraise 
whether digitally declared conditions conform to real-world observed states.

--- middle

# Introduction

The Internet was built from its inception on the foundation of trust between communicating entities within the digital domain. To ensure that packets, sessions, or communicating actors maintain their integrity within this digital space, mechanisms such as IP reachability, TLS end-to-end confidentiality, and credential- and token-based authorization frameworks have been devised and continuously advanced.

The Remote ATtestation ProcedureS (RATS) architecture {{RFC9334}}, which verifies the trustworthiness of remote devices, introduced standardized formats for Evidence and Attestation Results. This has been further extended by the EAT specification {{RFC9711}} to granularly express device characteristics, and the Endorsements structure {{I-D.ietf-rats-endorsements}} designed to supplement the appraisal process. Alongside these developments, the Supply Chain Integrity, Transparency, and Trust (SCITT) architecture {{I-D.ietf-scitt-architecture}}, which addresses supply chain trustworthiness, connects the integrity and transparency of the entire supply chain in an auditable manner. It achieves this based on a structure of Statements made by supply chain components (such as software or hardware) and tamper-evident cryptographic Receipts. Recently, subsequent standard specifications have materialized—including the HTTP-based Reference APIs {{I-D.ietf-scitt-scrapi}} and the CCF (Confidential Consortium Framework) ledger-based Profile for COSE Receipts {{I-D.ietf-scitt-receipts-ccf-profile}}—deepening the layer of trust within the digital domain. Although their focus areas differ, these technologies share a common structural paradigm: evaluating "what can be trusted" based on evidence.

However, recent shifts are fundamentally transforming the object of trust itself. The recipients of network requests are no longer confined to servers, software, or user terminals. Autonomous vehicles, robots, drones, smart locks, industrial automation equipment, and physical AI—systems that physically move to execute tasks—are taking their place. Consequently, a single digital request translates directly into a physical action: opening a door, moving a vehicle, handing over an object, or occupying a physical space. While RATS asks, "How do we prove the current internal state of this system?" the question we face today represents the next evolution: "Does the digitally proven state actually align with the physical reality right in front of us at this very moment?"

This is a novel boundary problem naturally encountered by the Internet and digital systems as they attempt to extend their established trust boundaries to physical execution in the real world. This challenge can no longer be deferred, particularly due to the rapid proliferation of agent-based AI. The delegation of execution to autonomous agents is becoming commonplace through Agent-to-Agent (A2A) interaction, Agent-to-Infrastructure (A2I) interaction, and the Model Context Protocol (MCP). As we approach a reality where autonomous agents make reservations, approve transactions, make calls, and enter into contracts on behalf of humans, and physical AI and automated systems translate these into real-world actions {{EXEC-GOV}}, we urgently need a methodology to verify whether the conditions declared in the digital domain match the states observed in reality. Furthermore, we need a way to formulate these answers into a reusable structure that other systems can directly ingest.

This document defines this problem domain as Reality Execution Assurance (REA). Through this document, we propose a framework to apply the familiar structure of evidence-appraisal-attestation results, established by RATS and SCITT, directly to the problem of verifying physical execution rather than purely digital systems.

# Problem Statement

The traditional Internet security and protocol stack has evolved strictly to guarantee logical consistency and data integrity within the digital domain, spanning from the network interface layer to the transport and application layers. End-to-end encryption and credential authentication mechanisms are near-perfect for proving that data has not been tampered with and has been delivered to the correct destination within a virtual infrastructure. However, now that the ultimate destination of communication has shifted to the physical execution environment, this traditional protocol framework reveals a critical limitation. No matter how robust the security and digital trust of the upper logical layers may be, they cannot guarantee factual conformity in the physical world where actual commands are executed.

Digital services that trigger physical execution are already emerging across various domains, including access control, remote smart lock approval, shipping and asset handover, mobility vehicle control, and blockchain smart contract integration. Existing digital frameworks can effectively address questions internal to the digital domain—such as to whom an identifier was issued, whether a session or request was delivered through a valid path, and whether a given account possesses the required permissions {{CISCO-DUO-POLICY}}. Yet, the moment physical execution is introduced, distinct questions arise. It is difficult to assure through existing frameworks whether an authenticated user has actually arrived at the physical site, whether the subject holding digital authority matches the subject performing the physical execution, whether execution conditions are maintained until completion, and whether inputted sensor values and location data align with the actual state of reality. In particular, the verification of real-world data consistency acts as a significant limitation in contexts like the blockchain oracle problem, where a decentralized ledger must reliably reference data from the off-chain world {{CALDARELLI-2025}}.

Existing Internet systems process digital representations—such as identifiers, tokens, sessions, logs, sensor values, and device events—but these representations themselves do not directly assure the conformity of physical execution. Even if a command is received using a valid cryptographic key, it could lead directly to system failure or safety accidents if the executing device is experiencing a voltage drop or outputting distorted data due to sensor contamination. Furthermore, even if a secure session is established in full compliance with credentials, the possibility cannot be ruled out that the opposing party is a software emulator mimicking a real device rather than the physical hardware itself. In future physical AI and autonomous agent environments, this problem becomes increasingly vital because systems must verify whether digital commands generated by autonomous agents are safely executed within permitted physical spatial boundaries. The core of the issue lies in how to evaluate the relationship between the execution conditions declared in the digital domain and the states observed in the physical execution environment, and how to express the results in a format that subsequent systems can ingest.

# Terminology

This section defines the core terms used to describe the problem domain.

Reality Execution Assurance (REA):
: A problem domain focused on evaluating whether requests, rights, policies, contracts, or conditions declared in the digital domain conform to the execution conditions observed in the physical execution environment, and generating assurance results that subsequent systems can ingest. REA does not replace existing authentication, authorization, access control, payment, contract, or device control logic; rather, it supplements the grounding points required when these elements interface with physical execution.

Digital Execution Condition:
: An execution condition represented within the digital domain. Examples include a condition requiring a specific user to be in front of a specific door, or a condition requiring a particular delivery item to be handed over to a designated recipient. Such declarations of credentials or authority can be structured into digital expressions through frameworks like the W3C Verifiable Credentials Data Model {{W3C-VC-2.0}}.

Reality Observed Evidence:
: Information received, referenced, or derived to represent the state of the physical execution environment. This includes location, proximity, device events, boundary crossings, presence/duration of stay, object movement, sensor events, and audit logs, all of which are evaluated within the context of their relationship to the declared conditions.

Physical Execution State:
: The actual, real-world state of a specific person, device, object, space, or process in relation to the execution conditions. This includes states such as on-site presence, approach to a door, asset handover, vehicle entry, space occupancy, and task completion status.

Assurance Result:
: The output of the REA evaluation and the input data ingested by subsequent systems. It is expressed in states such as condition fulfilled, suspended, condition unfulfilled, insufficient evidence, or conflicting evidence.

Subsequent System (e.g., Relying Party):
: A system that performs domain-specific processing by referencing the REA assurance results. Examples include access control systems, payment systems, blockchain smart contract integration systems, and robot control systems.

# Key Challenges

## Gaps Between Digital Representation and Physical Execution

* **Temporal Gap**: While digital authentication or authorization checks succeed at a specific point in time to issue a command, physical execution in the real world progresses over a duration of time. Due to real-time dynamic environmental shifts, the data at the time of command generation and the physical state at the moment the hardware operates may mismatch, potentially causing control errors or security voids during this brief interval.

* **Subject Gap**: The account or session possessing digital credentials does not always align with the actor performing the actual physical execution. Even if session continuity is securely maintained at the upper logical layer, traditional protocol architectures cannot easily detect instances where physical hardware detaches without the network's knowledge, masquerades using sophisticated clones, shares credentials, acts via unauthorized proxy, or falls victim to relay attacks mid-communication.

* **Continuity Gap**: Real-world execution is not a single, isolated event; rather, it consists of a continuous workflow involving entry, approach, execution, and termination. Existing systems typically verify a one-time digital authentication only at the initial phase and fail to monitor whether the execution conditions are continuously maintained across the physical space throughout the entire required duration, leaving them vulnerable to man-in-the-middle attacks that intercept legitimate signals.

* **Factual Conformity Gap**: Data such as location points, sensor readings, and device statuses represent reality; however, in environments where multiple pieces of evidence are incomplete, conflicting, or susceptible to manipulation, it is difficult to determine actual physical alignment solely through a single signal or the internal consistency of a digital signaling system. If the initial real-world data entering the system is distorted, the protective safeguards of the virtual world fail to defend against real-world physical anomalies or destruction.

## Limitations of Existing Digital Components

Network protocols, session management, authentication, authorization, decentralized identity (DID), and access control frameworks have advanced to a high degree of maturity in expressing and processing subjects and rights within the digital domain. However, these frameworks cannot sufficiently express or evaluate whether an execution target actually exists within the required boundaries of physical space, whether the executing subject is the legitimate owner or authorized delegate of the digital credentials, or whether the physical state conforms to the execution conditions. REA does not replace existing security components; instead, it plays a complementary role by comparing observed evidence regarding physical execution with declared conditions to generate conformity assurance results that subsequent systems can reference.

# Design Requirements

Discussions regarding REA standardization must take the following requirements into consideration:

* **Inclusivity of Subjects and Targets**: The framework must be capable of referencing actors, devices, objects, spaces, environments, and temporal conditions within the physical execution environment, and it must not be restricted to a single domain.

* **Condition Expressiveness**: The framework must be able to express not only location or proximity, but also the continuity of conditions, the freshness of evidence, and states of partial fulfillment or insufficient evidence.

* **Single-Flow Integrity**: The collection of observed evidence, appraisal against declared conditions, generation of assurance results, and delivery to subsequent systems must be consistently linked and interpreted within the same execution context.

* **Observation Technology Independence**: The framework must not depend on specific sensors, signals, devices, or measurement methods, ensuring that assurance results can be interpreted with a common meaning across varying deployment environments.

* **Privacy Protection**: Beyond the scope required to support the assurance results, the system must not excessively expose identifiable information—such as the location or behavior of physical subjects—and must strictly adhere to the principle of data minimization.

* **Resistance to Mismatch and Misuse**: The framework must be resilient against real-world execution anomalies and adversarial misuse, including evidence reuse, subject substitution, credential sharing, relay attacks, and observation delays.

# REA Conceptual Framework

## Basic Concepts

REA is a logical functional framework that receives observed evidence from the physical execution environment, appraises it against the declared conditions of the digital domain, and generates assurance results that subsequent systems can ingest. A single system may execute this entire workflow, or multiple systems may distributively perform the distinct functions of observation, normalization, appraisal, and delivery.

Rather than simply replicating the physical world within the digital domain, REA functions as a grounding point that evaluates and contextualizes the relationship between real-world observed expressions and digital declared conditions to provide definitive assurance results.

## Relationship Between the Physical Execution Environment and the Digital Domain

The physical execution environment encompasses actors, vehicles, robots, delivery assets, entry doors, parking spaces, sensors, physical boundaries, and real-world actions. The digital domain includes network connections, sessions, authentication, authorization, application policies, contracts, payments, and access control logic. REA acts as a bridge, building upon the rich expressive capabilities developed within the traditional digital domain by evaluating a distinct assurance problem—the conformity of physical execution conditions—and conveying those findings to subsequent digital systems.

## Assurance Results

The final output of REA is not an execution command, but rather assurance result data, which serves as input data referenced by subsequent systems when they perform actions such as granting access, controlling devices, executing contracts, confirming payments, or dispatching robotic commands. Assurance results can be expressed in states such as condition fulfilled, suspended/additional verification required, condition unfulfilled, insufficient or conflicting evidence, post-verification required, or recorded state for audit and dispute resolution.

# Reality Execution Assurance Procedures

## Digital Declaration or Inception of Rights

An execution request, contract, reservation, ticket, or authorization is generated within the digital domain by authentication, authorization, or application logic. REA utilizes the declared conditions established during this phase as its initial baseline input.

## Execution Context Binding

The digital declaration is bound to a specific, real-world execution context, which may include details regarding the user, space, device, physical boundary, temporal window, or delegation relationship. Moving beyond a simple query regarding credential validity, this step structures the necessary criteria so that the system can realistically appraise whether the physical execution conditions conform to the digital declaration.

## Pre-Execution REA Appraisal

Immediately prior to or at the onset of a subsequent physical execution—such as opening an entry door, boarding a vehicle, handing over goods, entering a parking facility, unlocking a smart lock, or deploying a robot—REA appraises the received observed evidence against the structured declared conditions. The assurance results generated from this appraisal are delivered to subsequent systems, which then determine whether to proceed with execution, suspend the operation, reject the request, or demand additional verification.

## Subsequent Physical Execution

Referencing the REA assurance results, domain-specific execution systems (e.g., access control systems, payment platforms, smart contract integration infrastructures, or robot control units) perform actual physical operations, such as unlocking a door or finalizing a financial transaction. REA does not directly substitute or intervene in this execution control mechanism.

## Post-Execution Verification and Audit

The definitive outcome of the physical execution is verified post-execution —confirming events such as whether a door actually opened and allowed only a single individual to pass, whether an asset handover was completed, whether a vehicle successfully entered the designated zone, or whether a robot strictly adhered to its operational boundaries. These outcomes are committed to an audit log to be used for dispute resolution, post-settlement processing, violation detection, policy refinement, and risk assessment for future execution cycles.

# Use cases of REA

The use cases and sequence diagrams presented in this section are non-normative examples and do not mandate specific protocol message formats or particular product interfaces. In these diagrams, Biz Application refers to existing systems responsible for reservations, payments, permissions, and settlements. Field Proxy represents components that observe and represent real-world onsite conditions, such as doors, gates, and unmanned equipment.

## Shipping, Receipt, and Asset Handover

In a delivery session where a business context (transaction/shipping contract) is established beforehand, the system verifies that the transferor and transferee at the physical site actually encounter each other and that their respective intentions to transfer and receive are properly combined and fulfilled within the same execution context.

~~~ text
Transferor         Transferee             Biz Application          REA Function
    |                     |                           |                         |
    |                     |-- 1. Order Contract ------|                         |
    |                     |                           |-- 2. Send Policy ------>|
    |                     |                           |                         |-- 3. Initialize Session
    |-- 4. Encounter -----|                           |                         |
    |                     |                           |                         |
    |-- 5. Submit Transferor Evidence ---------------------------------------->|
    |                     |-- 6. Submit Transferee Evidence ------------------->|
    |                     |                           |                         |
    |                     |                           |                         |-- 7. Match Evidence
    |                     |                           |<-- 8. Assurance ----|
    |<-- 9. Confirm -----|                           |                         |
    |                     |-- 10. Complete --------->|                         |
    |                     |                           |                         |
~~~

## Parking Entry, Settlement, and Exit

Instead of the business session initiating first, a vehicle entry event occurs at the ingress gate beforehand. This shows the characteristics of a reality-first procedure, where a temporary entry context is later bound to a payment/user context during the settlement phase and then re-verified at the exit stage.

~~~ text
Vehicle              Gate Proxy          Biz Application           REA Function
|                      |                          |                          |
|-- 1. Approach ------>|                          |                          |
|                      |-- 2. Report Entry -------------------------------->|
|                      |                          |                          |-- 3. Create Temporary Context
|                      |                          |                          |
|-- 4. Settle Parking Fee ----------------------->|                          |
|                      |                          |-- 5. Send Session------>|
|                      |                          |                          |-- 6. Bind Contexts
|                      |                          |                          |
|-- 7. Exit ---------->|                          |                          |
|                      |-- 8. Request Verification ------------------------->|
|                      |                          |                          |-- 9. Evaluate Conformance
|                      |                          |<-- 10. Assurance --------|
|                      |<-- 11. Open Barrier -----|                          |
|                      |                          |                          |
~~~

## Task Initiation for Robots, Drones, and Equipment

In physical AI and autonomous driving environments, digital commands generated by autonomous agents are converted into physical operations performed by robots, drones, and unmanned facilities. In this scenario, the Biz Application establishes task directives and permissible boundary policies, while the equipment/object Field Proxy transmits the physical constraints of the real-world environment. REA pre-emptively verifies not only the validity of the digital command but also whether the actual physical space where the robot operates, the location of the target asset, and the surrounding environmental conditions align with the declared boundaries before delivering the task initiation assurance result.

~~~ text
Robot             Target Proxy        Biz Application        REA Function
|                         |                    |                         |
|                         |                    |-- 1. Task Policy ------>|
|                         |                    |                         |-- 2. Set Boundary
|-- 3. Move & Dock ------>|                    |                         |
|                         |                    |-- 4. Check Status ----------->|
|                         |                    |                         |
|-- 5. Submit Equipment Evidence ---------------------------------------->|
|                         |- 6. Submit Target Physical Evidence --------->|
|                         |                    |                         |
|                         |                    |                         |-- 7. Verify Safety & Align
|                         |                    |<-- 8. Assurance ---------|
|<-- 9. Permit Task ---------------------------|                         |
|                         |                    |                         |
~~~

# Security, Privacy, Safety, and Accountability Considerations

Because Reality Execution Assurance systems handle sensitive real-world attributes—including physical locations, subject identities, operational states, and environmental profiles—maintaining robust security and privacy protection is paramount.

* **First**, collected evidence must be restricted to the minimum scope necessary to fulfill the specific service objective. For instance, if the objective is simple access verification, recording passing events, occupancy counts, or proximity telemetry should be prioritized over storing comprehensive video surveillance feeds. Furthermore, the use of persistent identifiers that track a device's long-term trajectory must be avoided in favor of temporary tokens or ephemeral, single-use identifiers.

* **Second**, the integrity and confidentiality of reality assurance metadata must be strictly safeguarded. Data assets such as evaluation results, evidence identifiers, policy IDs, execution logs, and confidence scores must be heavily protected against forgery, tampering, replay attacks, and unauthorized disclosure. Clear architectural independence among distinct observation functionalities must also be explicitly guaranteed.

* **Third**, the multi-evidence convergence process must account for false positives and over-blocking. Operational buffers and formal appeal mechanisms are required to ensure that physical sensor degradation, location inaccuracies, network latency, or legitimate user anomalies are not erroneously classified as malicious behavior. In particular, self-defending mechanisms capable of filtering out sensor contamination or external injection attacks at the ingestion stage are essential.

* **Fourth**, Reality Execution Assurance must not be repurposed or misused as a surveillance mechanism. Systems must incorporate explicitly stated purposes, explicit user notifications, rigorous access control matrices, defined data retention ceilings, and thorough audit trails. Responsible governance frameworks must be established to prevent automated decisions from causing unfair or disproportionate disadvantages to users.

* **Fifth**, feedback and remediation mechanisms must be applied differentially based on the operational risk level of the service. In high-risk deployment environments—such as critical facility access control, financial settlements, delegated authorities, or autonomous equipment operations—a fail-closed posture is typically appropriate. Conversely, for low-risk services, adaptive approaches such as supplementary verification steps or conditional approvals may be preferred.

# IANA Considerations

This document requires no actions from IANA.

# Future Standardization Items

Since this is a problem statement document, it does not define specific data protocols or implementation specifications. However, subsequent discussions within future standardization working groups should address the following items in depth:

* Definition of REA-specific terminology and standard operational scope.
* Data model structures to standardize and represent digital declared conditions and real-world observed evidence.
* A state model to manage the operational state transitions at each stage of the execution lifecycle.
* Standard interface specifications for interoperability with legacy identity frameworks (DID/VC), Web architectures, the Internet of Things (IoT), and smart contract infrastructures.
* Development of domain-specific profiles and application guidelines.

# Conclusion

The Internet and digital systems have reached a high level of maturity in representing information, identifying subjects, transmitting rights and requests, and coordinating remote execution within the digital domain. However, in environments where digital requests trigger physical execution in the real world, a critical verification gap and a disconnect in trust still remain. Determining whether an authenticated user is actually on-site, whether the executing actor matches the authorized subject, whether execution conditions persist for the required duration, or whether the real-world conditions referenced by a smart contract are truly fulfilled remains difficult to adequately express and evaluate using legacy virtual protocols alone.

REA is a novel problem domain established to address this structural void. It consistently links observed evidence, declared conditions, execution contexts, and assurance results at the intersection of the physical execution environment and the digital domain. This document proposes this domain as a formal subject for Internet standardization, aiming to develop it into a foundational standard that extends digital trust into physical spaces across diverse autonomous execution systems and physical AI service environments of the future.

--- back

# References

## Normative References

* [RFC9334]: https://datatracker.ietf.org/doc/html/rfc9334
* [RFC9711]: https://datatracker.ietf.org/doc/html/rfc9711

## Informative References

* [I-D.ietf-rats-endorsements]: https://datatracker.ietf.org/doc/html/draft-ietf-rats-endorsements
* [I-D.ietf-scitt-architecture]: https://datatracker.ietf.org/doc/html/draft-ietf-scitt-architecture
* [I-D.ietf-scitt-scrapi]: https://datatracker.ietf.org/doc/html/draft-ietf-scitt-scrapi
* [I-D.ietf-scitt-receipts-ccf-profile]: https://datatracker.ietf.org/doc/html/draft-ietf-scitt-receipts-ccf-profile

* [EXEC-GOV]:
    title: "Execution Governance: A Structural Control Layer for Autonomous Systems"
    date: "2026"
    author:
      - fullname: "Anonymous"

* [CISCO-DUO-POLICY]:
    title: "Duo Administration - Policy & Control"
    date: "2026"
    author:
      - org: "Cisco Duo"

* [CALDARELLI-2025]:
    title: "Can Artificial Intelligence solve the blockchain oracle problem? Unpacking the Challenges and Possibilities"
    date: "2025"
    author:
      - fullname: "G. Caldarelli"

* [W3C-VC-2.0]:
    title: "Verifiable Credentials Data Model v2.0"
    date: "2025"
    author:
      - org: "W3C"
