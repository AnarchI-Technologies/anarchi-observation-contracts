# AnarchI Observation Contracts

Public, implementation-free contracts for evidence emitted by Watchtower, Spine Spire, and Therma-Stats and retained by Chronicle.

The canonical envelope is `schemas/evidence-envelope-v1.json`. Raw evidence is immutable. Correlation adds stable linkage through `incident_code`; it never rewrites an observation. Producers join a shared cadence by using the same UTC window and deriving `cycle_id` as `obs:v1:<window-start>:<window-seconds>`.

## Deterministic rules

- `evidence_hash` is SHA-256 over canonical JSON with the `evidence_hash` field omitted.
- `incident_code` is SHA-256 over normalized domain, subject, condition family, and cycle window.
- timestamps are UTC RFC 3339 strings.
- expiry is explicit and does not delete history.
- source-specific evidence remains under `measured_evidence`.

Validate with `python -m json.tool schemas/evidence-envelope-v1.json`.

## Authority

This repository defines data shape only. It cannot observe, route, retain, correlate, decide, or act.
