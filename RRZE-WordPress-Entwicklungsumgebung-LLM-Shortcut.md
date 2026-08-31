# RRZE WordPress Development Environment - LLM Installation Specification

( Version: 1.0,  Date: 25.08.2026 )

Audience: LLMs and automation agents that create, configure, or verify a local WordPress development environment.

## 1. Authority and objective

Read and apply these sources before making changes:

1. `RRZE-WordPress-Plugin.md` or a newer approved RRZE WordPress Plugin Engineering Standard.
2. https://www.wp.rrze.fau.de/entwicklung/eigene-testinstanz/

Create a reproducible, non-production, domain-based WordPress Multisite environment suitable for developing and testing RRZE-compatible plugins and themes.

This environment MUST NOT use production credentials, production API keys, personal production data, or live external service accounts.

## 2. Required input

Obtain or derive these values before installation. Do not silently invent values that affect network identity, domains, credentials, or paths.

```text
ENVIRONMENT_ROOT=
WEB_SERVER=
DATABASE_ENGINE=
PHP_VERSION=
WORDPRESS_VERSION=
NODE_VERSION=
NPM_VERSION=
MULTISITE_BASE_DOMAIN=
MULTISITE_NETWORK_TITLE=
NETWORK_ADMIN_EMAIL=
DEFAULT_ADMIN_USER=
DEFAULT_ADMIN_EMAIL=
DEFAULT_LANGUAGE=de_DE
SECONDARY_LANGUAGE=en_US
```

Recommended local defaults where no project-specific value exists:

```text
MULTISITE_BASE_DOMAIN=my-testsite.test
MULTISITE_NETWORK_TITLE=WordPress Development
DEFAULT_LANGUAGE=de_DE
SECONDARY_LANGUAGE=en_US
```

Use a locally resolvable development domain. Never configure a real public domain merely for local testing.

## 3. Required software

Install and verify:

```text
Git
PHP compatible with the current RRZE target and the plugin under test
MySQL or MariaDB
Web server with local virtual-host support
WordPress
Node.js and npm when the project has Node/npm dependencies
```

Install when needed by the project or test workflow:

```text
Composer and PHP extensions required by the plugin
@wordpress/scripts for projects with blocks, Block Editor extensions, JSX/TSX,
or other WordPress-specific JavaScript build requirements
WP-CLI for repeatable setup, Multisite administration, and test automation
Browser automation and accessibility-test tooling
```

XAMPP, LAMPP, Local, a virtual machine, or a container-based local stack MAY provide the PHP, database, and web-server layer. Select the solution already established by the project or team. The resulting behavior, not a particular local-stack product, is the requirement.

## 4. WordPress installation requirements

Install WordPress as a domain-based Multisite network. Subdirectory Multisite is not an acceptable substitute for the standard reference environment.

The installation MUST provide:

- wildcard or explicit local host resolution for network sites;
- a working Network Admin area;
- site activation and network activation tests;
- a German default site and an English test site;
- HTTPS where the local stack can provide it without blocking development;
- permalinks enabled;
- a writable uploads directory resolved through WordPress APIs;
- no public internet exposure.

Create at least these sites:

```text
my-testsite.test       German main site for editorial and frontend tests
en.my-testsite.test    English editorial and frontend test site
```

The environment SHOULD support additional sites for regression tests, plugin-specific scenarios, and, where required, individual faculty-theme variants.

## 5. Required development configuration

Configure WordPress so development faults are visible without being displayed to normal frontend visitors.

Required settings:

```php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
define('SCRIPT_DEBUG', true);
```

The implementation MAY use an environment-specific configuration file. It MUST NOT commit credentials or local paths into the plugin repository.

Errors, PHP warnings, database warnings, browser-console errors, and failed background tasks MUST be investigated before a production candidate is approved.

## 6. Required themes

Install the following themes from their canonical repositories:

| Theme | Required use |
| --- | --- |
| FAU Elemental | Required current reference theme for RRZE/FAU-compatible frontend and Block Editor testing. |
| FAU Einrichtungen | Optional legacy/regression theme for compatibility tests where the plugin supports existing installations. |
| Current default WordPress Twenty theme | Required neutral comparison theme. Use the newest Twenty theme shipped with the WordPress version under test. |
| Previous default WordPress Twenty theme | Recommended additional comparison theme for backwards-compatibility testing. |

Activate FAU Elemental on at least the German and English test sites. Activate the current Twenty theme on an additional test site or temporarily switch a test site to it for comparison. Do not make the plugin under test depend on a theme unless such a dependency is explicit and documented.

## 7. Required plugins

Install and keep current the following network-level reference plugins where they apply to the test scope:

| Plugin | Purpose | Activation |
| --- | --- | --- |
| RRZE Settings | Network settings, API-key management, block/editor controls, and Multisite behavior. | Network activate. |
| RRZE Updater | Git-based installation and update testing for plugins and themes. | Network activate where its configuration requires it. |
| RRZE Elements Blocks | Reference Block Editor content and styling integration. | Activate on test sites or network activate according to the test scenario. |
| RRZE Legal | Consent and external-resource behavior. | Activate when external resources, cookies, or consent behavior are in scope. |
| RRZE Video | Video and external-media integration testing. | Activate when relevant. |
| FAU oEmbed | oEmbed integration testing. | Activate when relevant. |
| RRZE Multisite Manager | Reference behavior for Multisite administration and site management. | Network activate when Multisite-management behavior is in scope. |
| RRZE Log | RRZE logging actions and safe diagnostic behavior. | Network activate for plugin development and operational logging tests. |
| Plugin Check (PCP) | Required production-readiness check for plugins. | Activate for audit and release checks; do not treat as production runtime dependency. |
| Wordfence | Security-plugin compatibility and security-administration tests. | The free version is sufficient; activate when security-plugin interaction is in scope. |
| Loco Translate | Translation-file inspection and development support. | Optional; activate only when translation maintenance is in scope. |

Optional functional reference plugins from the RRZE test-instance recommendation include Shariff Wrapper, Statify, and Redirection. Install them only where an interaction or compatibility scenario requires them.

## 8. Plugin-under-test installation

Use a Git checkout as the primary development source. The working copy MUST remain identifiable and reviewable through Git.

For ordinary development, install the plugin under test from its local Git checkout. For updater testing, use RRZE Updater against the repository `main` branch and verify that a fresh checkout is directly executable.

Do not use manual ZIP replacement as the normal development or deployment workflow.

If the plugin provides blocks or WordPress-specific JavaScript:

1. Run `npm ci` when `package-lock.json` is present.
2. Use the project's `wp-scripts` build commands for block and WordPress-specific source.
3. Run any project-local asset or metadata scripts in addition, where documented.

If the plugin has no blocks and no WordPress-specific JavaScript build requirement, a project-local Node.js script using `esbuild` and `sass` MAY build ordinary assets and synchronize metadata.

## 9. Roles and test data

Create non-production test accounts for at least these roles:

```text
Super Admin / Network Admin
Site Administrator
Editor
Author or Contributor where the plugin exposes editorial features to them
```

Test data MUST include:

- posts, pages, media, and at least one custom post type when relevant;
- German and English content;
- valid, invalid, incomplete, and boundary-value form data;
- at least one site-specific and one network-owned configuration scenario;
- a centrally stored dummy API key or license key when the plugin uses one;
- fixtures or test doubles for external APIs whenever possible.

Never expose a network-owned secret in the Site Admin UI. Validate that Site Admins cannot read, edit, reveal, or override it.

## 10. Mandatory verification

Perform and record the applicable checks:

```text
PHP syntax and project PHP checks
Node/npm dependency installation and build
JavaScript/CSS linting
Plugin Check (zero errors for a production candidate)
Single-site behavior where supported
Multisite site activation
Multisite network activation where supported
Network Admin, Site Admin, Editor, and other relevant role checks
German and English UI checks; de_DE_formal where provided
Frontend and wp-admin output checks
Block Editor insertion, editing, update, and deprecation checks
Keyboard-only, visible-focus, labels/errors, and contrast checks
Browser console check
External-service failure behavior
Update and migration behavior from a supported prior version
```

Report every check as exactly one of:

```text
passed
failed
not applicable
not executed, with reason
```

Do not claim runtime, accessibility, Multisite, PCP, or build verification when only static source inspection was performed.

## 11. Completion state

The environment is ready only when all of the following are true:

- domain-based Multisite works with Network Admin access;
- German and English sites are reachable and editable;
- the required reference themes and plugins are installed for the scope;
- `WP_DEBUG` is enabled and frontend debug display is disabled;
- the plugin under test can be activated and deactivated without warnings or fatal errors;
- network and site configuration are demonstrably separated;
- build, PCP, and relevant manual checks have either passed or are explicitly reported as unavailable.
