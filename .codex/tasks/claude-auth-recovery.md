# Claude account recovery: profile-specific login instructions

Owner: Nathan Blevins. Date: 2026-10-12. Branch: fix/claude-profile-login-hint.

## Raw request and scope

Nathan reports daily Claude sign-outs in T3 over the past week, using personal Gmail and Hotmail accounts. Codex is unaffected. He asks to fix credential storage or the open-source T3 implementation, test it, and use a personal build if needed. He requests Opus 5.5 for architecture and code, and authorizes his existing Hotmail subscription when Gmail usage is exhausted. He also asks to patch the Model Matrix router if the issue affects routing.

This atomic patch fixes a confirmed recovery defect, not the unproved daily token-renewal cause: provider status currently instructs plain `claude auth login` even when the instance uses a separate config directory. Following that command can authenticate a different profile while leaving the selected one signed out. Turn-failure instructions already use claudeSignedOutMessage and provide the selected directory.

## Evidence and contract

- ClaudeProvider.ts unauthenticated status message omits the configured profile; ClaudeHome.ts already has the safe, literal-path recovery formatter.
- Opus 5.5 high investigated through personal-workers on claude-hotmail, native session be48d537-25ec-4d0d-b4f5-f16ac1af28bd. Its proposed probe lifecycle cause remains unconfirmed. Root verified the installed SDK already closes stdin and grants 2 seconds before SIGTERM; no lifecycle patch is authorized by this contract.
- Independent Sol high QA inspected the real Claude consumers and recovery tests. Root verified source, macOS launcher metadata, and the installed routing boundary separately.
- Local configuration repair: T3 Gmail was using the default Claude namespace rather than the named personal profile. Through T3's UI, it now uses claude-gmail and ~/.claude-personal, matching the router. Only those two settings changed. Hotmail's authorized login succeeded and Opus subsequently completed an actual request on it. Codex/work settings were preserved.
- Installed router child_env removes inherited CLAUDE_ credential/profile overrides. A synthetic boundary check passed. No router change is currently justified.

Acceptance:

1. When a configured Claude instance is unauthenticated, its provider-status message identifies the same literal config directory used by its child process and the applicable cwd. Reuse the established recovery formatter.
2. Explicit homePath takes precedence over inherited CLAUDE_CONFIG_DIR; an empty home with an inherited directory uses that directory; a default instance retains a generic login instruction.
3. The patch does not change successful provider status, token handling, process lifetime, account routing, schemas, Codex, or the work profiles.

Proof: extend the existing ProviderRegistry.test.ts Claude status tests for those behavioral cases, using synthetic paths and mocked capability data; run that focused family plus ClaudeHome.test.ts. Prove the regression fails with baseline production source and passes with the patch. Existing ready/error tests cover adjacent behavior.

Risk: Tier C, a bounded recovery-message correction with existing path quoting. Root reviews literal-path correctness and input precedence. No redesigned flow or new credential access is introduced.

Diagnostic logging disposition: preserve existing status/error diagnostics. No logs added; the fix changes the user recovery message and must never log environment values, credentials or account payloads.

Remaining original-task evidence: a freshly successful login cannot prove daily renewal reliability. Observe the next natural renewal and recurrence window; do not alter expiry, revoke tokens, provoke concurrent refreshes, or claim the upstream Claude refresh issue fixed. Supported long-lived setup-token is a possible later workaround with vendor-documented limitations, not implemented by this patch.

## Verification and implementation receipt

Opus 5.5 high authored the two executable files through claude-hotmail, native session aeaa971a-37bd-4e56-a2d2-1b1fb6325354. Root performed the independent source audit and commands.

- Baseline red proof: 2 new profile-specific tests fail for missing selected-directory guidance. Patched production bytes were restored exactly.
- Full affected ProviderRegistry.test.ts and ClaudeHome.test.ts selection: 70 passed.
- Targeted formatter and lint: passed.
- Server-only typecheck: passed; preexisting Effect suggestions in unrelated files remain.
- git diff --check: passed.
- Node 24.13.1, frozen-lockfile pnpm install; lockfile unchanged.

Independent exact-head Review Jury and isolated running-product validation remain pending. This patch does not yet justify replacing the installed desktop binary.
