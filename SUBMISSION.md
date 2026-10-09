# Counterfactual portal submission

## Category

Builder > Intelligent Contracts

## Title

Counterfactual

## Contribution date

October 9, 2026 (publication and live verification).

## Description and notes

Counterfactual evaluates textual conditions against a committed subject, then derives a minimum-cardinality Boolean change set for ALL or k-of-n. gl.nondet.exec_prompt runs inside gl.eq_principle.prompt_comparative; validators independently assess the conditions and compare the met vector and subject hash. The contract's own validated non-deterministic output controls settlement and the returned changes; it does not aggregate settled upstream state. Inputs and policy are immutable, commitments are rechecked, output types are validated, and settlement is terminal. Three Studio cases finalized with successful execution and majority agreement: ALL with a missing condition, 2-of-3 requiring one change, and already passing with no changes. Ten local regression groups and SDK validation pass. Minimality is only in the Boolean model: this is not a causal action plan or a feasibility guarantee. Ties use lowest-index selection.

## Primary evidence links

- Source repository: https://github.com/Zhekinmaksim/genlayer-counterfactual
- Deployed Studio contract: https://explorer-studio.genlayer.com/address/0x1c7C673649E0f5AcC7c4fFF12dB53318102dDc34

## Additional evidence

- Exact deployed source: https://github.com/Zhekinmaksim/genlayer-counterfactual/blob/03498022edd840c418acc9f2d3ad650b16b07aaa/contract.py
- Deployment: https://explorer-studio.genlayer.com/tx/0xabd96aaf17ca2ab44d259c23df2e551e23c8fff2146192b19a0587d843451689
- counterfactual-all-001: https://explorer-studio.genlayer.com/tx/0x16747e94ac0931bd582607ae92665bfe21fb395d13408330be838e434248bc41
- counterfactual-k-001: https://explorer-studio.genlayer.com/tx/0x43f2d26e2a03f3b9bf5708b3bd9fa71aa38b157ab31d9db20eeecd70f8c9ea84
- counterfactual-pass-001: https://explorer-studio.genlayer.com/tx/0x97340c379c16ee50f880e940a00f8e3430f7f60327627d39a6c01cf5147cf989
- Inputs, expected and observed results: https://github.com/Zhekinmaksim/genlayer-counterfactual/blob/main/evidence.json

## Verification

10/10 local regression groups; SDK semantic validation and ABI extraction passed.
All three live evaluations and the deployment are FINALIZED, execution SUCCESS,
consensus MAJORITY_AGREE, with leader_only=false. Stored results match expectations.
Local tests mock the SDK and models; they are separate from the live Studio checks.

## Submission status

Ready for manual entry. No portal submission has been created or sent.
Studio is a test environment, not mainnet. Portal acceptance is not guaranteed.
