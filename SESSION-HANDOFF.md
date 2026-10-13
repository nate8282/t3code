# Claude authentication repair handoff

Objective: stop repeated personal Claude sign-outs in T3 without changing Codex or work accounts; include the personal account router if implicated.

Branch: fix/claude-profile-login-hint, upstream base ffb4af8d5d4fcfc5e9322295ff846a6d1e7ffbf8. Personal fork: nate8282/t3code.

Confirmed and repaired locally: Gmail T3 provider used the default profile, while the router used ~/.claude-personal. T3 now uses claude-gmail plus that named directory. Hotmail login succeeded and actual Opus calls worked. Only the two Gmail provider settings changed, with a private local backup.

Code: Opus authored profile-aware signed-out provider-status guidance in ClaudeProvider.ts and three regression cases in ProviderRegistry.test.ts. Contract and confirmed bug catalog are in .codex/. All 70 affected tests, targeted formatter/lint, server typecheck, and diff whitespace check pass. Baseline source fails both profile-specific tests for the claimed reason.

Router: installed child_env clears inherited Claude/Codex credential overrides. Synthetic boundary proof passed. No routing patch justified by current evidence.

Unresolved: original daily renewal failure, candidate probe/refresh interaction, and natural-refresh acceptance. Do not claim renewed-login reliability from a fresh-login smoke test. A supported long-lived setup token is being considered; its Remote Control and Claude.ai-connected-service limitations require preserving the user's needs.

Next: exact-head cross-family Review Jury, personal draft PR, isolated running-product check. Do not run a candidate against live ~/.t3/userdata or restart the user's active app. Do not copy, log, mutate or force-expire credentials. Preserve the independent daily-renewal investigation as a separate seam.
