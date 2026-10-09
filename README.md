# Counterfactual

Assesses which textual conditions a subject meets, then computes a minimum-cardinality Boolean flip set for an ALL or k-of-n rule.

## Status

Published and verified in Studio on 2026-10-09.
[Deployed contract](https://explorer-studio.genlayer.com/address/0x1c7C673649E0f5AcC7c4fFF12dB53318102dDc34).
SDK semantic validation and ABI extraction pass; 10 local regression groups pass.
Three live evaluations finalized with SUCCESS and MAJORITY_AGREE in normal
consensus mode. Inputs, transactions and observed results are in [evidence.json](evidence.json).
[Portal submission text](SUBMISSION.md) is ready; nothing was submitted to the portal.

## Material nondeterminism and independent validation

Each node independently calls the LLM to assess every condition against the committed subject. Validators compare that met-condition vector and subject hash. The accepted vector controls both the current status and which unmet conditions are returned. ALL/k-of-n aggregation is deterministic, but it operates on this contract's own consensus judgment, not settled upstream state.

Uses `gl.nondet.exec_prompt` within `gl.eq_principle.prompt_comparative`.
The comparative wrapper repeats the task and applies an LLM equivalence check;
it is not a byte-for-byte equality check. State is written only after the
equivalence call returns. See the
[official equivalence documentation](https://docs.genlayer.com/developers/intelligent-contracts/equivalence-principle).

## Safety changes

- Creator-only, one-time submission. Evaluation is permissionless.
- Input hashes are computed on-chain, not accepted from the caller.
- Input and policy commitments are rechecked before judgment.
- Bounded identifiers, terms and inputs; duplicate rubric entries rejected.
- Strict output shape/types and range validation, before and after consensus.
- Every settled outcome, including UNDETERMINED, is terminal.
- Result hash binds input commitments, outcome, decision fields and reason.
- SDK storage containers are not user-constructed. Views use ordinary lists
  or a JSON string. Runtime dependency is pinned in contract.py.

Breaking API change from the archive: the submission method no longer accepts
a caller-provided hash. Use the returned commitment. get_result returns a JSON
string; parse it once in clients. See schema.json for the exact ABI.

## API

- `open_case(id, mode, k, conditions_text)` -> terms hash; mode `all` or `kofn`
- `submit_subject(id, subject)` -> computed SHA-256
- `evaluate(id)` -> settlement
- `get_outcome(id)` -> 0 pending, 1 already passes, 2 change found, 3 undetermined
- `minimal_change(id)` -> condition text selected by the Boolean rule
- `get_result(id)` -> JSON string with met/change masks, reason and commitments

## Verification and deployment

From this directory:

```sh
python3 sim/check.py
uvx --python 3.13 --from genvm-linter genvm-lint check contract.py --json
genlayer network set studionet
genlayer deploy --contract contract.py
```

The constructor has no arguments. Deployment was executed in normal validator
consensus mode, not leader-only. Restore your previous CLI network after testing.

Use the exact method arguments in smoke.json with a fresh case ID each time:
open_case, the listed submit method, evaluate, then get_result.
The recorded cases also include observed results, all matching expectations.
Wait for FINALIZED and inspect leader execution SUCCESS as well as stored
state; transaction acceptance alone can also describe a reverted execution.
Deployment/evaluation hashes and source SHA-256 are recorded in evidence.json.

Test an unauthorized submission, duplicate submission, second evaluation and
prompt-injection input in Studio as additional negative cases. Source and
rubric mutation are simulated locally; there is no public mutation API.

## Limitations

Conditions are creator-defined and immutable, NOT countersigned. A flip set is not an action plan, a causal counterfactual, the cheapest change, or a guarantee of real-world feasibility. Mutually incompatible conditions and dependencies are not solved. For k-of-n, equal-size alternatives use lowest-index selection. In ALL mode the effective k is the number of conditions, irrespective of the input k.

UNDETERMINED describes catchable judgment/format/integrity failures. A network
consensus failure can instead leave a transaction unaccepted without storing
UNDETERMINED. Models can agree on an incorrect answer. Data delimiters do not
guarantee prompt-injection resistance. No external security audit or claim of
novelty across all existing GenLayer contracts is made.

## Local test scope

The harness prohibits TreeMap/DynArray construction and simulates independent
leader/validator calls. It tests authorization, immutable inputs, commitments,
strict payloads, terminal settlement and outcome logic. It uses an exact mock
comparator, not the real LLM comparator. SDK semantic validation verifies the
storage types and ABI but does not execute the contract inside GenVM.
