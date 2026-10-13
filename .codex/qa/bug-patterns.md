# Confirmed bug patterns

### BUG-0001 — Login recovery targets the default account instead of the selected profile

- Status: fixed on branch, release pending
- First confirmed: 2026-10-12
- Surfaces: claude, provider-status, authentication, profile-isolation
- Suspect when: a named provider instance reports signed out but its recovery message gives a default CLI login instruction without the selected configuration directory.
- Root cause: checkClaudeProviderStatus used a generic literal rather than the established profile-aware claudeSignedOutMessage formatter.
- Confirmation: both new profile-specific status tests fail on baseline ffb4af8d5d4fcfc5e9322295ff846a6d1e7ffbf8 specifically because the message omits the launched config directory. They pass with the correction; the complete two-file provider/home test selection passes 70 tests.
- Guard: ProviderRegistry.test.ts tests `points the signed-out hint at the configured home over an inherited one`, `points the signed-out hint at an inherited CLAUDE_CONFIG_DIR as written`, and `keeps the signed-out hint generic without a Claude config dir`.
- Prevention: derive recovery guidance from the same resolved instance environment as its real child process. A successful login in another profile does not establish recovery.
- Scope: this confirms the incorrect recovery instruction. It does not confirm the cause of recurring OAuth expiry or vendor credential corruption.
