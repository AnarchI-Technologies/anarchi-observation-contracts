# AnarchI Observation Contracts

Public, implementation-free contracts for evidence emitted by Watchtower, Spine Spire, and Therma-Stats and retained by Chronicle.

The current canonical envelope is `schemas/evidence-envelope-v2.json`; v1 remains published for lineage. Raw evidence is immutable. Correlation adds stable linkage through `incident_code`; it never rewrites an observation. Producers join a shared cadence by using the same UTC window and deriving `cycle_id` as `obs:v1:<window-start>:<window-seconds>`.

## Deterministic rules

- `evidence_hash` is SHA-256 over canonical JSON with the `evidence_hash` field omitted.
- `incident_code` uses `anarchi.incident-code.v1`: normalize domain, subject, and condition family to lowercase stable tokens; concatenate them with the UTC cycle-window start using `|`; SHA-256 the UTF-8 bytes; prefix the first 32 lowercase hexadecimal characters with `inc_`. Arrival order, duplicates, late arrival, supersession, and new contributing observers do not change identity. A different subject or cycle window does.
- `observer_cycle_participation` has `incident_code: null`; participation proves cadence presence and never invents an incident.
- v2 records observation and emission timestamps, non-negative skew, allowed skew, and late/stale/missed-cycle Booleans.
- timestamps are UTC RFC 3339 strings.
- expiry is explicit and does not delete history.
- source-specific evidence remains under `measured_evidence`.

Validate both schemas with the deterministic command in `anarchi.yaml`.

## Authority

This repository defines data shape only. It cannot observe, route, retain, correlate, decide, or act.
