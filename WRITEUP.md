
# AI Proposal Triage Design — Write-up

Proposals are referred to by letter (A–E).

## 1. AI vs. Rules

AI is used only where language is messy:
- extracting facts from free-text proposals (licence, price, timeline, experience, warranties, team);
- checking proposal text for attempts to influence the reviewer;
- optionally writing a one-line reason from numbers already calculated.

Plain code handles everything that must be exact: loading the admin's criteria, validating extracted data, mandatory pass/fail checks, score calculation, ranking and audit logging. I drew the line here because AI is good at reading text but unreliable at arithmetic and repeatability, while rules are consistent and easy to audit.

## 2. Consistency and Explainability

- The AI never scores or ranks. It returns structured facts, each with an exact quote and a status (confirmed, missing, unclear). Code checks that each quote really appears in the proposal.
- Scoring uses fixed formulas applied to the admin's weights:
  - Experience = min(installs of 100 kW+ ÷ 10, 1)
  - Price = lowest eligible price ÷ bid price (over the $260,000 ceiling is not eligible)
  - Timeline = shortest time ÷ bid time (over 16 weeks or unstated is flagged)
  - Warranty = average of (panel years ÷ 25) and (workmanship years ÷ 12)
  - Team = 25% each for named lead, crew, safety plan, monitoring plan
- Extraction runs at temperature 0, and the extracted JSON, prompt version, model version and criteria version are stored. Re-ranking reuses the stored facts, so the same proposals give the same ranking. A fresh AI extraction could differ slightly, which is why the stored version is the one used and reviewers can correct it.
- The reviewer sees a per-criterion breakdown with the evidence quote for every number.

## 3. Human in the Loop

- A reviewer confirms the category and checks extracted facts, especially unclear or missing mandatory items.
- Any flagged proposal (embedded instruction, missing data, failed validation) must be reviewed by a person before it can be ranked.
- A reviewer can correct a fact or move a rank, and a written reason is required for every override.
- The tool only recommends; a human makes the final award decision.
- Audit log: uploaded file references and hashes, who reviewed, timestamps, criteria/prompt/model versions, extracted values and evidence, scores, flags, corrections, overrides with reasons, and the final decision.

## 4. Changing Criteria

Every criteria configuration has a version number. When an admin changes criteria or weights, the system validates it (weights total 100%) and saves a new version. Old results keep their original version and are never silently changed. The reviewer can re-score under the new version, and the results are labelled with the version used. Because facts are already extracted, changed weights need only re-scoring in code. A new AI call is needed only if a new field is added that was never extracted.

## 5. Risks and Safeguards

- **Incorrect extraction:** every value needs an exact quote checked by code; unclear items go to the reviewer.
- **Made-up facts:** no evidence means the value is rejected; missing information is treated as unknown, never as confirmed.
- **Document manipulation:** proposal text is treated as untrusted data. A separate scan flags text aimed at reviewers. Proposal C contains an instruction to rank it first; it is ignored, flagged and sent to a human rather than ranked.
- **Mandatory requirements:** Proposal B relies on a subcontractor licence to be confirmed after award, so the bidder's own licence is not confirmed and it is ineligible. The bidder's licence is not replaced by a subcontractor's.
- **Missing information:** Proposal D states no timeline, so it is flagged for clarification rather than guessed or scored as zero.
- **Inconsistent scores:** fixed formulas in code, not AI.
- **Bias:** only the admin's criteria are scored; company names, locations and branding are not scoring inputs.
- **Cost and performance:** extract once, score in code, limit file size, and monitor failures and AI usage.

The tool gives recommendations and explanations; a human reviewer remains responsible for the award.
