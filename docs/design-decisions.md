# Design decisions and trade-offs

These are **original, generalized explanations** of decisions reflected in the investigated workflow. They are not reproductions of internal decision records. The default distribution and the observed adaptation are distinguished where needed.

## 1. Record approval state; do not create new approval authority

**Problem.** A workflow that reads an approval flag and then decides whether it may proceed can confuse a recorded flag with actual authorization.

**Alternatives.** Strengthen internal gates; manage exceptions; or record approval status while keeping true authority external.

**Decision.** Observe and record approval fields without treating them as a mandatory internal transition or as proof of human approval. Retain technical verification and external authorization checks as separate concerns. *(Adaptation only)* The adopting project additionally records whether each decision originated from a human or an AI; this is not part of the default distribution.

**Trade-off.** The workflow avoids conflating its own flags with human permission, but depends on accurate records, audits and actual human governance. This policy is expressed partly through agent instructions, not guaranteed by a deterministic enforcement mechanism. Some legacy gate wording remains under migration.

## 2. Combine independent reviewers by union, not majority vote

**Problem.** A single reviewer can overlook a category of issues; forcing consensus can discard a valid minority finding.

**Alternatives.** One reviewer, a mandatory external review service, or majority-vote aggregation.

**Decision.** Use two independently executed model-based reviews and deduplicate the **union** of findings. Retain a more severe assessment when they disagree, while leaving human reviewers responsible for adoption.

**Trade-off.** More review coverage is sought at the cost of additional compute and triage. No comparative defect-detection improvement has been quantified. The implemented toolchain is tied to two specific coding-agent ecosystems.

## 3. Check where verification actually runs

**Problem.** A validation capability can exist on paper or in a distribution without being active in a project's CI.

**Alternatives.** Manual checks or relying on each validation tool's self-description.

**Decision.** Declare intended verification locations and compare distribution dependencies, execution contracts, and actual integration points where possible.

**Trade-off.** The declarations need maintenance. More importantly, a check against an available CI template does not establish that an adopter enabled that template. In the investigated case, several expected verifiers were not connected to the adopting CI, including the check that would audit that wiring.

## 4. Stabilize a shared interface for independent implementation

**Problem.** Requiring the frontend to wait for a completed backend serializes development. Detecting contract divergence only at integration may delay discovery.

**Alternatives.** Backend-first integration, late integration-only checks, or AI text comparison alone.

**Decision — adaptation only.** Coordinate around a stable common interface while implementation proceeds independently. Use CI checks that read the authoritative contract and compare consumer representations, including when the provider changes.

**Trade-off.** The shared interface requires timely agreement and maintenance of consumer-side mappings. Comprehensive verification of generated types and mocks is not yet established.

## 5. Allow selective adoption, while acknowledging update drift

**Problem.** A distributable workflow needs optional components, but local customization can conflict with future upstream changes.

**Alternatives.** Fork permanently or require every adopter to track upstream identically.

**Decision.** Permit selective activation of optional workflow categories and track local components as upstream, custom, or patched.

**Trade-off.** Adaptation becomes practical, but update safety is not automatic. A locally owned file can still be overwritten by some update paths; a protected file may receive no upstream changes, leaving a version split. These are observed limits, not guarantees of effortless portability.
