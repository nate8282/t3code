# Claude authentication repair handoff

Objective: stop repeated personal Claude sign-outs in T3 without changing Codex or work accounts; include the personal account router if implicated.

Branch: fix/claude-profile-login-hint, upstream base ffb4af8d5d4fcfc5e9322295ff846a6d1e7ffbf8. Personal fork: nate8282/t3code.

Confirmed and repaired locally: Gmail T3 provider used the default profile, while the router used ~/.claude-personal. T3 now uses claude-gmail plus that named directory. Hotmail login succeeded and actual Opus calls worked. Only the two Gmail provider settings changed, with a private local backup.

Code: Opus authored profile-aware signed-out provider-status guidance in ClaudeProvider.ts and three regression cases in ProviderRegistry.test.ts. Contract and confirmed bug catalog are in .codex/. All 70 affected tests, targeted formatter/lint, server typecheck, and diff whitespace check pass. Baseline source fails both profile-specific tests for the claimed reason.

Router: installed child_env clears inherited Claude/Codex credential overrides. Synthetic boundary proof passed. No routing patch justified by current evidence.

Unresolved: original daily renewal failure, candidate probe/refresh interaction, and natural-refresh acceptance. Do not claim renewed-login reliability from a fresh-login smoke test. A supported long-lived setup token is being considered; its Remote Control and Claude.ai-connected-service limitations require preserving the user's needs.

Next: exact-head cross-family Review Jury, personal draft PR, isolated running-product check. Do not run a candidate against live ~/.t3/userdata or restart the user's active app. Do not copy, log, mutate or force-expire credentials. Preserve the independent daily-renewal investigation as a separate seam.

Review checkpoint: personal PR https://github.com/nate8282/t3code/pull/1 is linked to the T3 thread. Both exact-head strong-family reviews and deterministic gates pass at 51d91839. Structured verdict is .codex/qa/claude-recovery-verdict.json. Isolated real web client confirms profile-specific onboarding guidance; daily renewal remains open.

Personal macOS arm64 installer built successfully: 0.0.45-personal.claude-profile.20261012. Private artifact directory: /Users/nathanblevins/.local/state/t3-auth-repair/20261012/desktop-artifacts. Disk image checksum passed; app metadata matched the 65dc5b3961c5 source prefix and packaged server chunk matched build bytes with the actual corrected recovery seam. No installer was applied or live app restarted. Original build prerequisite failure was missing Rust; installed Rust1.99 through mise and the changed-environment build passed.

Root published the reusable confirmed recovery-profile pattern to the existing global seen-bug store, preserving prior entries. No further Jury candidate. Model Matrix has the scoped diagnosis at decisions/20261012-t3-claude-auth/diagnosis.md; installed routing executable remains unchanged after its inheritance boundary passed. The next decision is whether the user's setup needs Claude Remote Control or Claude.ai-connected services before trying supported long-lived tokens. Daily natural renewal remains unverified. No task-owned dev/build/model process remains running.
