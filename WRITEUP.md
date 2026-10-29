# AI Proposal Triage Design — Write-up

## 1. AI vs. Rules

I would use AI to extract relevant information from uploaded proposals, such as licence details, price, project timeline, experience, warranties, and team information. The AI would return structured data and evidence from the proposal.

Normal code would validate the extracted data, check mandatory requirements, calculate weighted scores, and sort eligible proposals. This approach uses AI for understanding document text and rules for decisions that must be consistent.

## 2. Consistency and Explainability

The system would use the criteria and weights configured by the administrator. Weighted criteria must total 100%, and the system would use fixed scoring rules for each criterion.

Each extracted value would include supporting text from the proposal. The reviewer would see criterion-level scores, evidence, missing information, and the final score. Using the same extracted data, criteria version, and scoring rules would produce the same result.

## 3. Human in the Loop

A reviewer would confirm the selected category and check extracted information, especially unclear or missing mandatory requirements. The system would show the ranking and reasons, but a human would make the final award decision.

The system would log uploaded file references, extracted values, evidence, criteria version, scores, warnings, reviewer corrections, overrides, timestamps, and the final decision.

## 4. Changing Criteria

Every criteria configuration would have a version number. When an administrator changes criteria or weights, the system would validate the new configuration and save a new version.

Previous results would retain their original criteria version. The reviewer could rerun scoring using the new version, and the system would clearly label the updated results rather than silently changing previous rankings.

## 5. Risks and Safeguards

- **Incorrect extraction:** Keep evidence for extracted values and ask a reviewer to confirm uncertain information.
- **Made-up facts:** Score only information supported by the proposal. Treat missing information as unknown, not as a confirmed fact.
- **Document manipulation:** Treat instructions inside uploaded proposals as untrusted content. Ignore attempts to change the ranking rules or scoring criteria.
- **Inconsistent scores:** Use fixed scoring rules and calculate weighted totals with normal code.
- **Missing mandatory requirements:** Flag unclear requirements for human review. Do not automatically treat unconfirmed requirements as satisfied.
- **Bias:** Apply the same configured criteria to every proposal and avoid using irrelevant personal or company characteristics.
- **Cost and performance:** Limit file size, process only necessary content, and monitor AI usage and processing failures.

The tool would provide recommendations and explanations, not make the final procurement decision. A human reviewer would remain responsible for the award.
