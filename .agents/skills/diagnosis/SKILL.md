---
name: diagnosis
description: Diagnose failing generation or validation, cross-version regressions, schema/payload mismatches, unexpected warnings, or optional-integration failures. Reproduce the smallest failing Ash resource, route, or consumer path and fix its cause; skip feature planning without an observed failure.
user-invocable: false
---

# Diagnosis

Input: the failing command or behavior, resource/domain and route inputs,
selected OpenAPI version, loaded optional modules, and actual error or document
fragment. Gather the missing evidence from the failing test or caller.

1. Minimize the reproduction while preserving the failure. Use existing
   fixtures where possible and keep the expected behavior explicit. Check
   the other OpenAPI version when it can distinguish a version-specific cause.
2. Follow the failing value through version selection, the owning builder,
   and normalization. Probe a causal hypothesis that predicts a different
   result from alternatives; record actual probe results in the task conversation.
3. Fix the owner of the mismatch. If a probe rejects the hypothesis, remove
   only the exploratory edits it motivated, preserving unrelated work and
   independently useful diagnostics. Retain the rejected evidence in task notes
   so a resumed investigation does not repeat it. Reopen schema ownership,
   route identity, state/cache or version assumptions when successive patches
   fail to explain the observation; do not accumulate rescue/retry branches.
4. Retain a focused regression that fails at the intended assertion on the
   original defect and passes with the fix. A dependency, fixture or optional
   module startup failure is not this red path. Assert independently expected
   document bytes/shape or actual payload acceptance and rejection; rerun the
   minimized reproduction and affected boundary tests. Preserve exact source,
   version, route/resource, cache mode and fixture identity with results.

Load sources according to the symptom:

- Type or null mismatch: [TypeMapper](../../../lib/ash_oaskit/core/type_mapper.ex)
  and [Nullable](../../../lib/ash_oaskit/schemas/nullable.ex).
- Missing or colliding components:
  [SchemaBuilder](../../../lib/ash_oaskit/schemas/schema_builder.ex) and
  [reference validation](../../../lib/ash_oaskit/schemas/references.ex).
- Ambiguous output keys or normalization failures:
  [JsonKeys](../../../lib/ash_oaskit/core/json_keys.ex) and
  [OpenApi](../../../lib/ash_oaskit/open_api.ex).
- Route collisions or unexpected operation IDs:
  [PathRegistry](../../../lib/ash_oaskit/core/path_registry.ex).
- Actual response data differing from generated schemas:
  [serialized payload tests](../../../test/ash_oaskit/serialized_payload_test.exs).

Output: the reproduction, supported cause, owning fix, retained regression,
and fresh verification results. If an optional package or consumer environment
is unavailable, explain which hypothesis or behavior remains unverified.
