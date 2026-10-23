# PR #12 merge-readiness follow-up

## Scope and acceptance

Brad authorized fixes on his fork branch and full retesting on 2026-10-23.
Base reviewed head: `08dcc5caf45062e73d29949716abffe1dabdf222`.
Target: `bradvin/ai-provider-for-openrouter`, branch
`feat/consolidated-provider-capabilities`. No upstream push, merge, release,
draft-state change, or comment publication is authorized.

- Correct the reproduced PHPStan 2.1.32 max-level return-shape diagnostic in
  `getModalities()` without changing modality selection behavior.
- Preserve SDK 0.4.3 and the existing Composer lock. Unrelated development
  dependency/configuration repairs are outside this focused follow-up.
- Rerun all 203 repository contracts on PHP 8.4.6 and actual PHP 7.4.30,
  source syntax/PSR-12/PHPStan, and the independent WordPress integration probes.
- Rerun the eight authenticated WordPress cases on Core SDK 1.3.1, within
  the renewed eight-request / US$1 allowance. Retain generated raster artifacts
  for full decoding and semantic image inspection this time.
- Obtain independent QA of the actual diff before committing and pushing.
- Keep unavailable OpenStation and image-only-model checks explicitly qualified.

## Execution

Current supervisor model/effort cannot be independently verified from the tools.
Developer: GPT-6 Luna High (bounded type-inference fix with existing coverage).
Independent QA: GPT-6 Sol Medium. Supervisor owns this plan and Git operations.

| Task | Scope | Developer | QA | Status | Repairs | Evidence | Commit |
|---|---|---|---|---|---|---|---|
| T1 | Explicit modality return shape; preserve behavior | Luna High (`modality_fix`) | Sol Medium (`independent_qa`) | validating | 0 | Independent QA PASS; 203/203 on PHP 8.4.6 and 7.4.30; PHPStan max clean | Awaiting validated commit |
| T2 | Full baseline, static and WordPress/live validation | Supervisor | Sol Medium reviews T1 and coverage | validating | 0 | 203/203 per PHP runtime; 28 WP cases; 8/8 live; syntax/PSR-12/PHPStan pass | Awaiting validated commit |

T2 depends on the frozen T1 implementation. No parallel implementation edits.
Workers may not edit this tracker, commit, push, or delegate further.

## Validation commands

`DEPS` is the isolated dependency directory built per `tests/README.md`.
`PHP74` is an actual PHP 7.4.30 binary; `PHPSTAN` is the 2.1.32 PHAR.

```sh
php tests/run.php "$DEPS/vendor/autoload.php"
"$PHP74" tests/run.php "$DEPS/vendor/autoload.php"
php "$PHPSTAN" analyze src --level=max --autoload-file="$DEPS/vendor/autoload.php" --no-progress --memory-limit=256M
php "$PHPCS" --standard=PSR12 src
find src -name '*.php' -exec php -l {} \;
php -l ai-provider-for-openrouter.php
php wp-probes.php "$DISPOSABLE_WP" "$EVIDENCE/models.json"
```

External evidence scripts and logs stay outside the candidate checkout. Live
calls use the normal plugin registration, WordPress builder, model selection,
HTTP transport and result parsing; no protected-parser shortcuts. Persisted
request counts prevent retries and calls beyond the approved limit.

## Budget and limitations

Brad explicitly waived a Codex usage budget: “Don't worry about budget.approved.
And also run integration tests.” No percentage allowance or measured task cost
is invented. The earlier OpenRouter allowance is exhausted; the renewed live
test allowance is separate and capped at eight requests / US$1.

Known pre-existing debt: root development Composer lock is inconsistent;
default PHPCS references `plugin.php`. Use documented isolated dependencies and
source-scoped analysis without changing SDK/lock. Repository workflows deploy
or mutate GitHub comments/labels; these are not run as a local test suite.
Actual OpenStation is not installed, and no image-only model was found in the
previous `/models` snapshot. No live completeness claim covers those paths.

## Checkpoint

- Preparation: clean isolated checkout switched to the existing fork PR branch.
  Fork is `origin`; upstream push URL is disabled locally.
- Next: T1 implementation, independent QA, full validation, scoped commits,
  then push only the authorized fork branch.

## Validation results — 2026-10-23

- Production diff: initialize `getModalities()` with both directional keys and
  document its exact array shape. Existing normalization overwrites both keys;
  no behavior change, ignored errors, baseline growth, or dependency update.
- Independent QA: PASS. SDK 0.4.3 suites: 14 supported-option, 43 text, 146 image
  cases on each of PHP 8.4.6 and actual PHP 7.4.30; all exit 0.
- PHPStan 2.1.32 level max: no errors with 256 MB, exit 0. Developer's initial
  run omitted the memory flag and exhausted 128 MB; explicit-limit reruns pass.
- Source PSR-12 (PHPCS 3.13.5), syntax on six PHP runtime files, and diff checks pass.
- Disposable WordPress 7.1.3 / Core SDK 1.3.1: 28 independent offline cases pass,
  including zero generation requests for unsupported options/media.
- Eight renewed authenticated calls all pass, no retries. Five use
  `openai/gpt-4.1-mini` (text, tools, JSON, schema, combined); three use
  `google/gemini-2.5-flash-image` (ordinary text, image generation, reference edit).
  Actual reported cost totals US$0.0776865, below US$1.
- Both image results fully decode using GD. Visual inspection confirms red
  square generation and a blue-square edit on white. Response IDs, byte/order
  preservation, usage mapping, system/history/reference preservation pass.
- OpenStation-specific execution and live image-only-model variant remain
  unverified; this follow-up does not invent evidence for unavailable paths.
- No remote result URLs or absent usage were observed live; corresponding
  rejection/sentinel contracts remain covered by offline tests.

Full repository deployment/Props Bot workflows were not run; root development
Composer/default PHPCS debt remains unchanged. The complete available contract
suite and the explicit integration matrix above were rerun after the source fix.
