# Polymesh Association Grant Proposal

- **Project Name:** RFID Registry — Polymesh Asset Evidence Toolkit
- **Team Name:** Copper Seventh LLC
- **Payment Address:** 2HheRX5sZNxbg358XzhrrHsFbaf5jju7sPUxLQMtDiHdD7D8; currency: POLYX
- **Level:** 2

## Project Overview :page_facing_up:

This is an independent proposal, not a response to a specific RFP or a follow-up to an awarded Polymesh grant.

### Overview

RFID Registry proposes an open-source toolkit that connects a Polymesh asset to signed, versioned supplier evidence while keeping the underlying business documents off-chain and access-controlled. Asset issuers, application developers and authorized reviewers would be able to associate evidence with an existing asset, retrieve an authorized evidence package, and check its integrity against the current on-chain commitment.

The first reference workflow is a window-supplier shipment: the supplier prepares an evidence package, an authorized asset agent records its commitment, and a reviewer checks the package against the asset's recorded evidence state. RFID or QR identifiers are optional external references. They do not prove physical authenticity or convey legal title.

The Polymesh-specific work is a TypeScript adapter for asset metadata and document references, permission checks, a repeatable testnet demonstration and developer documentation. This builds on Polymesh's native asset functionality. We are interested in reducing the integration work needed to connect operational records with tokenized-asset applications.

RFID Registry is currently at idea/planning stage with a live landing page at https://rfidregistry.com. There is no working toolkit, deployed Polymesh integration or existing user base.

### Project Details

**Proposed architecture**

1. A supplier uses a reference web interface to create a versioned evidence manifest and attach synthetic documents. The application encrypts document content before off-chain storage.
2. The manifest records document digests, an opaque evidence identifier, revision, issuer identity, issuance time and any preceding manifest digest. It is signed by a configured issuer key. A random salt is included in the commitment process to reduce guessing of predictable records.
3. An authorized Polymesh asset agent uses the adapter to register or update a minimal evidence commitment and status through the supported SDK asset-metadata functionality. The adapter checks permissions before preparing a transaction and reports finalized transaction status.
4. An authorized reviewer retrieves the manifest and permitted documents. The verifier recomputes digests, checks the signature and revision linkage, and compares the evidence to the current asset commitment and status. It separately reports integrity, signer verification, access and chain-state results.

Full documents, personal information, confidential quantities and reusable access credentials stay off-chain. Public metadata contains only the minimum commitment, schema version, opaque reference and state needed for verification. The milestone-one specification will document precisely which fields are public and the residual correlation risks. Public-chain history cannot be erased by changing off-chain access.

**Proposed data model**

The off-chain `EvidenceManifestV1` contains: `schemaVersion`, `evidenceId`, `assetId`, `revision`, `issuerDid`, `issuedAt`, optional `previousManifestDigest`, `documents[]` with content digests and media types, `salt`, and `signature`. Canonical serialization, digest algorithm, signature payload, key-to-issuer association and maximum sizes will be fixed in milestone one with test vectors. Signer-key validation will not be represented as a claim that the contents are factually true.

The on-chain evidence record contains: `schemaVersion`, `evidenceId`, `manifestCommitment`, `revision` and `status` (`active` or `revoked`). Implementation will respect the chain's current metadata limits. The exact encoding, number of metadata entries and use of native document references will be specified and demonstrated in milestone one before the reference application depends on them.

**Proposed API surface**

- `createManifest(input)` validates and canonicalizes a manifest and returns the unsigned payload and commitment inputs.
- `signManifest(manifest, signer)` produces a signed evidence package through a documented signer interface.
- `prepareEvidenceUpdate(assetId, evidence, signingAccount)` returns authorization findings and a transaction for the caller to approve and submit.
- `readEvidenceState(assetId, evidenceId)` resolves the asset's recorded evidence commitment, revision and status.
- `verifyEvidence(package, chainState, trustedIssuerConfig)` returns explicit results for the signature, document digests, current revision and revocation state.

These are proposed toolkit APIs, not claims about existing Polymesh SDK method names. The adapter will use supported SDK procedures, including asset metadata registration/read/update and their authorization checks, rather than introducing a custom chain or runtime pallet.

**Reference interface**

The UI will have three views: an issuer form for evidence and document selection; an asset view showing the current evidence state and transaction result; and a reviewer view showing permitted files and separate verification checks. Error states will distinguish denied access, missing documents, invalid signatures, stale revisions, revoked evidence and unavailable chain data. No polished consumer marketplace is included.

**Technology stack**

TypeScript, the supported Polymesh SDK, Node.js, Next.js, PostgreSQL, an open-source S3-compatible object store and Docker Compose. Milestone one will pin compatible runtime, SDK and chain versions, document testnet identity/account prerequisites and publish a reproducible setup. The full reference workflow will be self-hostable using open-source dependencies; proprietary hosted services will not be required. Existing dependency licenses and attribution will be retained.

**Scope limits**

This is a testnet prototype and reusable developer toolkit. It excludes securities issuance, custody, payments, investment products, legal-title verification, identity-provider services, production hardware procurement, RFID anti-counterfeit guarantees, bespoke ERP integrations and a full production security audit. It will not change Polymesh compliance rules or make private data confidential merely by hashing it. Real customer data will not be necessary for acceptance.

### Ecosystem Fit

**Audience and need:** Polymesh asset-application developers, asset administrators and authorized business reviewers who need verifiable evidence without putting source documents on a public ledger. The deliverable is a reusable integration pattern rather than a standalone general-purpose registry chain.

**Relationship to existing functionality:** Polymesh already provides asset metadata and document functionality. The toolkit will connect these primitives to signed manifests, encrypted off-chain storage, version/revocation handling and a reference verifier. It will not claim to invent native asset metadata or duplicate the SDK.

**Alternatives and differentiation:** ERP attachments, shared document stores and generic provenance registries can store or anchor records. This proposal focuses on a documented Polymesh asset-permission workflow and a self-hostable end-to-end example. No claim of exclusive technology or proven competitive superiority is made. During milestone one we will check for overlapping Polymesh ecosystem components and reuse suitable open-source work with attribution.

**Acceptance and adoption:** A successful delivery will include one reproducible Polymesh testnet integration, 100 synthetic evidence cases, documented negative tests and a developer tutorial. Two window-supplier pilots are prospective, including Houston Window Fashions; neither is a signed partnership, deployed pilot or existing user. Feedback from up to two prospects is an adoption goal, not a dependency for grant acceptance. No mainnet transaction volume or commercial uptake is promised.

## Team :busts_in_silhouette:

### Team members

- Joshua Kassabian — founder and lead developer, Copper Seventh LLC; sole confirmed core team member.
- Independent technical reviewer — to be selected within the milestone-three budget; no appointment or additional employee is claimed.

### Contact

- **Contact Name:** Joshua Kassabian
- **Contact Email:** joshkassabian@gmail.com
- **Website:** https://rfidregistry.com; company: https://www.copperseventh.com

### Legal Structure

- **Registered Address:** Warren, Michigan, United States; full registered mailing address available privately to the grants team.
- **Registered Legal Entity:** Copper Seventh LLC

### Team's experience

Joshua's full-stack work uses TypeScript, Next.js, PostgreSQL and web3 integrations. Relevant live work includes https://reanimateddead.com and https://www.copperseventh.com. These are references to prior work, not evidence of a completed RFID Registry toolkit or prior production Polymesh deployment. Polymesh-specific implementation and compatibility validation remain to be completed.

Previous grant applications have been made for **RFID Registry**, under **Copper Seventh LLC**. No awarded grant or committed external funding is being represented for RFID Registry. Any awarded overlapping support would be disclosed and the funded work and costs separated before acceptance. This proposal requests support for future Polymesh-specific development.

### Team Code Repos

- Team member and proposed repository owner: https://github.com/inkedinlove
- Planned implementation repository: `inkedinlove/rfid-registry-polymesh`; it has not yet been created and contains no existing implementation.
- All grant-funded source, examples, tests, Docker files and documentation will be published in that repository under Apache-2.0.

### Team LinkedIn Profiles (if available)

- https://www.linkedin.com/in/joshuakassabian

## Development Status :open_book:

The project is at idea/planning stage. The public site is a landing page. There is no working toolkit, Polymesh codebase, testnet deployment, completed security review or production customer deployment to present.

Preparation for this proposal has reviewed the official grant template, requirements and Polymesh SDK documentation, particularly asset metadata, native document references, transaction authorization and SDK/chain compatibility. The architecture, API outline and measurable milestones above are the proposed starting specification, not completed research or experimental results. No conversation with Polymesh or endorsement from its team is claimed.

Primary implementation references:

- Polymesh SDK: https://github.com/PolymeshAssociation/polymesh-sdk
- Official documentation: https://developers.polymesh.network/
- Grant program: https://github.com/PolymeshAssociation/Grants-Program

## Development Roadmap :nut_and_bolt:

### Overview

- **Total Estimated Duration:** 12 weeks, organized as three four-week milestones, beginning on grant acceptance and confirmed testnet access.
- **Full-Time Equivalent (FTE):** 0.40 average across 12 weeks, based on 168 engineering hours plus 24 reviewer hours and a 40-hour working week. This is part-time founder delivery with a separate reviewer, not a claim of multiple full-time employees.
- **Total Costs:** USD 25,000, paid in POLYX at the program's applicable conversion rate.

Engineering is budgeted at 168 hours × USD 125 = USD 21,000. Independent technical review is budgeted at 24 hours × USD 125 = USD 3,000. Test infrastructure and storage are budgeted at USD 1,000. Total: USD 25,000. Reviewer selection and availability will be confirmed before the review milestone; substitution or material scope changes will be agreed with the grants team.

Milestone-based payment is acceptable; no upfront payment is assumed. We understand the published terms describe payment following milestone acceptance and invoice receipt. Any discrepancy between program documentation, payment timing or acceptance criteria will be resolved before beginning the funded schedule.

### Milestone 1 — Evidence specification and Polymesh compatibility

- **Estimated duration:** Four weeks
- **FTE:** 0.35; 56 engineering hours
- **Costs:** USD 8,000: USD 7,000 engineering and USD 1,000 test infrastructure/storage

| Number | Deliverable | Specification |
| --- | --- | --- |
| 0a. | License | Publish grant-funded code and documentation under Apache-2.0, with dependency attribution and license inventory. |
| 0b. | Documentation | Publish architecture, public/private field map, manifest schema, canonicalization and signature specification, threat model, pinned compatibility matrix and testnet setup tutorial. Document identity/account, asset and permission prerequisites. |
| 0c. | Testing Guide | Provide executable unit tests and fixtures for canonicalization, digest verification and input validation, plus an integration guide for the authorized testnet flow. Demonstrate rejection of a changed document and an unauthorized update attempt. |
| 0d. | Docker | Supply Dockerfiles and Compose configuration for the local development/test services, with a documented external testnet endpoint and separately supplied test credentials. |
| 1. | Evidence format | Implement `EvidenceManifestV1`, deterministic commitment generation, a documented signing/verifying interface and at least 10 reproducible positive/negative test vectors. Record supported algorithms and size limits. |
| 2. | Compatibility demonstration | Using a testnet asset and authorized test identity, register the required metadata, write a commitment, read it back and demonstrate the permission-denied path. Publish runnable commands and transaction evidence. Confirm SDK/chain versions and the exact metadata/document encoding. |
| 3. | Design gate | Publish explicit scope, acceptance fixtures, remaining risks and any required proposal amendment. Any incompatibility that would change the committed scope must be resolved with the grants team before milestone two. |

### Milestone 2 — Adapter and reference workflow

- **Estimated Duration:** Four weeks
- **FTE:** 0.45; 72 engineering hours
- **Costs:** USD 9,000 engineering

| Number | Deliverable | Specification |
| --- | --- | --- |
| 0a. | License | Continue Apache-2.0 publication and update the dependency attribution inventory. |
| 0b. | Documentation | Publish inline API documentation and a tutorial covering issuer setup, evidence creation, authorized transaction submission, document retrieval, verification and evidence updates/revocation. |
| 0c. | Testing Guide | Supply unit and integration tests with documented setup. Cover invalid signatures, altered content, stale revisions, replay attempts, revoked evidence, denied off-chain access, denied chain permissions and chain unavailability. |
| 0d. | Docker | Extend the reproducible containers to run the reference UI, API, database and encrypted document storage. Testnet credentials remain user-supplied and are not bundled in images. |
| 1. | TypeScript adapter | Implement the proposed manifest, transaction-preparation, state-reading and verification APIs using the milestone-one encoding and supported SDK procedures. Expose errors and authorization outcomes without handling production custody. |
| 2. | Reference application | Deliver the issuer, asset and reviewer views. Support encrypted document upload/retrieval, signed manifests, current-state verification, a new evidence revision and revocation. Publish a walkthrough using synthetic data. |
| 3. | Access and observability | Enforce documented off-chain reviewer access and separate it from chain account permissions. Ensure application logs and UI error messages do not intentionally include raw document content, private keys or reusable access credentials. Document key loss and already-downloaded-document limitations. |

### Milestone 3 — Validation, review and public release

- **Estimated Duration:** Four weeks
- **FTE:** 0.40; 40 engineering hours and 24 independent reviewer hours
- **Costs:** USD 8,000: USD 5,000 engineering and USD 3,000 review

| Number | Deliverable | Specification |
| --- | --- | --- |
| 0a. | License | Publish a tagged Apache-2.0 release with complete grant-funded source and third-party notices. |
| 0b. | Documentation | Publish the final API reference, installation tutorial, operational/key-handling guide, upgrade/version notes and explicit production-readiness limitations. |
| 0c. | Testing Guide | Provide a repeatable suite of 100 synthetic evidence cases, an expected-results manifest and a release validation report. Include successful flows and each negative-test class from milestone two; distinguish synthetic tests from actual adoption. |
| 0d. | Docker | Publish release-pinned container build instructions and a clean-environment reproduction guide for all delivered functionality. |
| 0e. | Article | Publish an English developer article explaining the Polymesh evidence integration, runnable example, security boundaries and reuse instructions. |
| 1. | Independent review | Obtain a documented specialist review of the defined prototype's authorization, signature/commitment and off-chain data-access paths. Publish findings and remediation evidence. No unresolved critical/high findings in those paths may remain at acceptance; other findings must have documented disposition. This is not a full production audit. |
| 2. | Release demonstration | Publish a tagged release and reproducible testnet demonstration of creation, authorized anchoring, retrieval, verification, revision and revocation. Include transaction references and test reports sufficient for evaluators to reproduce the result. |
| 3. | Feedback and handover | Publish an issue backlog and support/contribution instructions. Request feedback from up to two prospective supplier pilots and document any received with permission. If they do not participate, state that clearly; synthetic validation remains the committed acceptance route. |

**Delivery risks and handling:** Validate chain permissions and metadata limits first. Keep the evidence format small and the application scope fixed. Test integrity separately from the truth of supplier assertions. Use synthetic records by default, publish no customer documents, and document the limits of access revocation and immutable chain history. Escalate blocked testnet access or a required scope change promptly; do not silently substitute a different chain or reduce accepted deliverables.

## Future Plans

After delivery, we intend to keep the repository, issue tracker and installation documentation available; collect integration feedback; and pursue the prospective supplier pilots if they remain interested. Short-term improvements would be prioritized from observed use and separately resourced. No unfunded service-level agreement or indefinite security-maintenance commitment is promised.

Longer term, Copper Seventh LLC may offer optional hosting, implementation and support around the open-source toolkit. The grant-funded reference workflow will remain self-hostable without a paid service. Production deployment, regulatory review, hardware controls and broader audits would require separate evaluation and funding.

## Additional Information :heavy_plus_sign:

**How did you hear about the Grants Program?** The official Polymesh website and public Grants Program repository during research into ecosystem development funding.

No Polymesh award, completed implementation, signed pilot or external endorsement is being represented. The requested grant funds future development. Prospective supplier participation is explicitly separate from the reproducible deliverables on which milestone acceptance is based.
