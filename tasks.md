# Project Tasks

This document tracks work for the Edge TTS extension in a hierarchical structure with actionable items, metadata, and file mappings.

Legend for metadata fields:
- Status: Not Started | In Progress | Completed | Blocked
- Priority: Critical | High | Medium | Low
- Effort: approximate person-hours (e.g., 1h, 2h, 4h)
- Dependencies: upstream task IDs (e.g., 1.1, 2.3.1)

---

## 1.0 Code Quality & Technical Debt

### 1.1 ESLint configuration fixes
- 1.1.1 Browser global definitions
  - Status: Not Started | Priority: Medium | Effort: 2h | Dependencies: –
  - Files: `eslint.config.js`, `global.d.ts`, `tsconfig.json`
  - Acceptance Criteria:
    - No `no-undef` for `MediaSource`, `SourceBuffer`, `WebSocket`, `TextEncoder`, `fetch`, `Audio` in source.
    - Browser env enabled for extension context without masking legitimate issues.
- 1.1.2 Lint rule configuration
  - Status: Not Started | Priority: Medium | Effort: 2h | Dependencies: 1.1.1
  - Files: `eslint.config.js`
  - Acceptance Criteria:
    - Reasonable `no-unused-vars` and `no-undef` for extension scripts.
    - `prefer-const` enabled where safe; build is lint-clean.
- 1.1.3 Ignore generated bundles
  - Status: Not Started | Priority: Low | Effort: 0.5h | Dependencies: –
  - Files: `.eslintignore`
  - Acceptance Criteria: `dist/**` excluded from linting.

### 1.2 TypeScript type safety improvements
- 1.2.1 Type definitions for Edge TTS API responses
  - Status: Not Started | Priority: High | Effort: 3h | Dependencies: –
  - Files: `src/lib/EdgeTTSClient.ts`
  - Acceptance Criteria:
    - Interfaces for voices and synth responses defined; `any` removed.
- 1.2.2 State and messaging types
  - Status: Not Started | Priority: Medium | Effort: 2h | Dependencies: 1.2.1
  - Files: `src/contentScript.ts`, `src/popup/index.tsx`
  - Acceptance Criteria: Strongly typed `ExtensionMessage`, event handlers, and storage objects.
- 1.2.3 Strict TS settings (selective)
  - Status: Not Started | Priority: Low | Effort: 2h | Dependencies: 1.2.1
  - Files: `tsconfig.json`
  - Acceptance Criteria: Safer compiler options without breaking build.

### 1.3 Code cleanup & refactoring
- 1.3.1 Extract constants and enums
  - Status: Not Started | Priority: Low | Effort: 1h | Dependencies: –
  - Files: `src/lib/EdgeTTSClient.ts`, `src/utils/index.ts`
  - Acceptance Criteria: No magic strings for URLs, headers, or message actions.
- 1.3.2 Logging controls
  - Status: Not Started | Priority: Low | Effort: 1h | Dependencies: –
  - Files: `src/lib/EdgeTTSClient.ts`
  - Acceptance Criteria: Logging toggled via flag; no noisy console in production.

### 1.4 Build system optimization
- 1.4.1 Webpack build checks
  - Status: Not Started | Priority: Low | Effort: 1h | Dependencies: –
  - Files: `webpack.config.js`
  - Acceptance Criteria: Vendor splitting where helpful, source maps appropriate per env.

---

## 2.0 Feature Enhancements & Improvements

### 2.1 Voice management system
- 2.1.1 Fetch and cache voices
  - Status: Not Started | Priority: High | Effort: 3h | Dependencies: –
  - Files: `src/lib/EdgeTTSClient.ts`, `src/popup/index.tsx`
  - Acceptance Criteria: Voices loaded once per session; errors handled; UI shows list.
- 2.1.2 Favorites and custom voices
  - Status: Not Started | Priority: Medium | Effort: 2h | Dependencies: 2.1.1
  - Files: `src/popup/index.tsx`
  - Acceptance Criteria: Favorite toggle persists; custom voice overrides selection.

### 2.2 User interface enhancements
- 2.2.1 Dark mode persistence
  - Status: Not Started | Priority: Low | Effort: 1h | Dependencies: –
  - Files: `src/popup/index.tsx`, `src/popup/styles.css`
  - Acceptance Criteria: Theme persisted in `storage.sync`; UI toggles immediately.
- 2.2.2 Controls accessibility
  - Status: Not Started | Priority: Medium | Effort: 2h | Dependencies: –
  - Files: `src/components/controlPanel.ts`, `src/content-styles.css`
  - Acceptance Criteria: Keyboard operable controls and ARIA labels present.

### 2.3 Audio playback improvements
- 2.3.1 Seamless pause/resume
  - Status: Not Started | Priority: High | Effort: 2h | Dependencies: –
  - Files: `src/contentScript.ts`
  - Acceptance Criteria: `togglePause` reliably pauses and resumes without desync.
- 2.3.2 Robust stop behavior
  - Status: Not Started | Priority: High | Effort: 2h | Dependencies: –
  - Files: `src/contentScript.ts`
  - Acceptance Criteria: `stopPlayback` tears down media graph, no leaks, idempotent.

### 2.4 Settings & configuration
- 2.4.1 Speed and output format persistence
  - Status: Not Started | Priority: Medium | Effort: 2h | Dependencies: –
  - Files: `src/popup/index.tsx`, `src/contentScript.ts`
  - Acceptance Criteria: Speed and chosen format persist; applied to new sessions.

---

## 3.0 Browser Compatibility & Cross-Platform

### 3.1 Firefox support hardening
- 3.1.1 Message fallback
  - Status: Not Started | Priority: High | Effort: 2h | Dependencies: –
  - Files: `src/popup/index.tsx`, `src/contentScript.ts`
  - Acceptance Criteria: For Firefox, uses `window.postMessage` fallback (already present) and verified end-to-end.
- 3.1.2 Scripting API guard
  - Status: Not Started | Priority: Medium | Effort: 1h | Dependencies: –
  - Files: `src/popup/index.tsx`
  - Acceptance Criteria: `browser.scripting.executeScript` guarded; failures handled.

### 3.2 Manifest parity
- 3.2.1 Verify host permissions and content scripts
  - Status: Not Started | Priority: Medium | Effort: 1h | Dependencies: –
  - Files: `manifests/manifest.chrome.json`, `manifests/manifest.firefox.json`
  - Acceptance Criteria: Required hosts present; content scripts consistent; Firefox nuances respected.

---

## 4.0 Performance & Optimization

### 4.1 Streaming pipeline
- 4.1.1 MediaSource type selection
  - Status: Not Started | Priority: Medium | Effort: 1h | Dependencies: –
  - Files: `src/contentScript.ts`
  - Acceptance Criteria: Chooses WebM Opus when supported else MP3; no playback stutter.
- 4.1.2 WebSocket reuse and backoff
  - Status: Not Started | Priority: Low | Effort: 2h | Dependencies: –
  - Files: `src/lib/EdgeTTSClient.ts`
  - Acceptance Criteria: Reasonable reconnect/backoff; avoids frequent reconnects per request.

---

## 5.0 Testing & Quality Assurance

### 5.1 Manual test plan
- 5.1.1 Popup play/pause/stop
  - Status: Not Started | Priority: Medium | Effort: 1h | Dependencies: –
  - Files: N/A (docs)
  - Acceptance Criteria: Verified on sample pages; logs captured for failures.

### 5.2 Automated sanity checks (where feasible)
- 5.2.1 Type-level tests
  - Status: Not Started | Priority: Low | Effort: 1h | Dependencies: 1.2
  - Files: `tsconfig.json`, CI config (future)
  - Acceptance Criteria: `tsc --noEmit` clean in CI.

---

## 6.0 Documentation & Developer Experience

### 6.1 README and usage docs
- 6.1.1 Clarify permissions and features
  - Status: Not Started | Priority: Low | Effort: 1h | Dependencies: –
  - Files: `edge-tts-extension-main/README.md`
  - Acceptance Criteria: Up-to-date setup, build, install steps for Chrome and Firefox.

### 6.2 Contributor guidance
- 6.2.1 Development scripts and workflows
  - Status: Not Started | Priority: Low | Effort: 1h | Dependencies: –
  - Files: `README.md`
  - Acceptance Criteria: Document common commands and debug tips.

---

## 7.0 Security & Privacy

### 7.1 Token and DRM handling
- 7.1.1 Sensitive values review
  - Status: Not Started | Priority: High | Effort: 2h | Dependencies: –
  - Files: `src/lib/EdgeTTSClient.ts`, `src/lib/drm.ts`
  - Acceptance Criteria: No accidental leakage; rationale documented; scope minimized.

### 7.2 Permissions minimization
- 7.2.1 Validate `host_permissions` and `permissions`
  - Status: Not Started | Priority: Medium | Effort: 1h | Dependencies: –
  - Files: `manifests/manifest.chrome.json`, `manifests/manifest.firefox.json`
  - Acceptance Criteria: Only necessary permissions requested.

---

## 8.0 Deployment & Release Management

### 8.1 Build and packaging
- 8.1.1 Multi-target builds (Chrome/Firefox)
  - Status: Not Started | Priority: Medium | Effort: 2h | Dependencies: –
  - Files: `webpack.config.js`, `web-ext-config.cjs`
  - Acceptance Criteria: Produces `dist/chrome` and `dist/firefox` reliably.

### 8.2 Store distribution (future)
- 8.2.1 Submission checklists
  - Status: Not Started | Priority: Low | Effort: 2h | Dependencies: 8.1
  - Files: `README.md`
  - Acceptance Criteria: Checklist exists for Chrome Web Store and AMO submissions.

---

## Cross-Reference: Current Code Touchpoints

These are key files referenced by tasks above:
- Edge TTS core: `src/lib/EdgeTTSClient.ts`
- DRM utilities: `src/lib/drm.ts`
- Content script (streaming and controls): `src/contentScript.ts`
- Control panel UI: `src/components/controlPanel.ts`
- Popup UI: `src/popup/index.tsx`, `src/popup/index.html`, `src/popup/styles.css`
- Background (context menus): `src/background/index.ts`
- Browser utils: `src/utils/browserDetection.ts`
- Styles: `src/content-styles.css`
- Manifests: `manifests/manifest.chrome.json`, `manifests/manifest.firefox.json`
- Build: `webpack.config.js`, `web-ext-config.cjs`
- Lint & Types: `eslint.config.js`, `tsconfig.json`, `global.d.ts`

---

Notes:
- Verify messaging flow: popup → active tab (content script) via `browser.tabs.sendMessage` or fallback to `window.postMessage` for Firefox; content script → TTS playback via `initTTS` with `EdgeTTSClient`.
- According to current implementation, output format is chosen via `MediaSource.isTypeSupported`; ensure acceptance tests cover both WebM Opus and MP3 paths.


