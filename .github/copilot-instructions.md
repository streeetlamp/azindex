# AZIndex Workspace Instructions

## Project Scope
- This repository is a legacy WordPress plugin that builds alphabetical indexes and renders them with the `[az-index id="N"]` shortcode.
- Prefer backward-compatible changes. The code targets older WordPress and PHP patterns.
- See `readme.txt` for user-facing behavior, plugin capabilities, and installation notes.

## Architecture Boundaries
- `az-index-admin.php`: plugin bootstrap, activation/deactivation, admin menu/page, request processing, option and table management.
- `az-index-content.php`: shortcode rendering and HTML output formatting.
- `az-index-cache.php`: cache invalidation and rebuild logic tied to post/page lifecycle hooks.
- Keep responsibilities in the same file area when adding or changing behavior. Do not move logic across modules unless the task requires a refactor.

## WordPress Conventions
- Keep the `az_` prefix for new functions, classes, constants, options, and hooks.
- Preserve existing shortcode and filter behavior for compatibility:
  - Shortcode: `az-index`
  - Output filters include `azindex_display_index`, `azindex_display_item`, `azindex_page_links`, `azindex_alpha_links`
- Plugin state depends on the custom table `AZ_TABLE` and options such as `az_cache_dirty`, `az_max_index`, and `az_plugin_version`. Treat schema/option changes as high risk.

## Editing Guidelines
- Prefer minimal, surgical changes over style rewrites.
- Keep legacy coding style consistent in touched regions (brace style, spacing, control flow).
- When changing SQL, prioritize safe escaping and prepared statements where practical, while preserving current behavior.
- Be careful with globals and hook timing (`init`, `admin_menu`, post save/delete hooks) because ordering affects cache and admin workflows.

## Validation
- No automated test suite is present in this repository.
- After PHP changes, run syntax checks:
  - `php -l az-index-admin.php`
  - `php -l az-index-content.php`
  - `php -l az-index-cache.php`
- If behavior changes, manually verify in a WordPress instance:
  - plugin activation path
  - index create/update flows in admin
  - shortcode rendering and pagination on a page/post

## Agent Workflow Notes
- Start by reading the relevant function and nearby hooks before editing.
- For risky changes, describe likely regressions (cache invalidation, pagination, shortcode output) and include a quick manual test plan.