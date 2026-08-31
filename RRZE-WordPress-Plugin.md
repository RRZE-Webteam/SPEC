# RRZE WordPress Plugin Engineering Standard and AI Development Prompt

( Version: 1.18,  Date: 31.08.2026,  Source: https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Plugin.md )

> **Purpose:** This document defines the mandatory baseline for creating, extending, reviewing, and maintaining WordPress plugins intended for operation in RRZE/FAU environments.
>
> It is designed both as an engineering standard for human developers and as a system/project prompt for AI-assisted development tools such as Codex, Claude Code, GitHub Copilot, or similar systems.
>
> **Normative language:** The terms **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are to be interpreted as binding priority levels. Where this document conflicts with generic AI-generated conventions, this document wins.

---

# 1. Core principle

A WordPress plugin is not considered complete merely because it works once or because generated code passes a superficial test.

A production-ready plugin MUST be:

- technically correct;
- maintainable over several years;
- secure;
- accessible;
- usable by real editors and administrators;
- compatible with WordPress Single Site and, where technically applicable, WordPress Multisite;
- suitable for operation in centrally managed university infrastructure;
- versioned and reproducibly buildable;
- documented;
- testable;
- translatable;
- updateable without manual intervention in generated source files;
- designed so that another developer can take over maintenance later.

AI MAY be used to generate, refactor, review, document, or test code. AI MUST NOT be treated as a substitute for requirements analysis, architecture, UX decisions, security review, accessibility review, testing, or long-term maintenance responsibility.

Before implementing a feature, the developer or AI MUST first verify whether WordPress Core, an existing RRZE plugin, an existing block, an existing API, or an established workflow already provides the required functionality.

Do not duplicate existing functionality without an explicit architectural reason.

---

## 1.1 Binding rules for admission to the RRZE CMS

For plugins intended to be operated on or admitted to the central FAU/RRZE CMS, the current rules published by RRZE are binding:

```text
https://www.wp.rrze.fau.de/entwicklung/einsatz-fremdplugins/vorgaben-an-plugins/
```

These operational rules apply in addition to this engineering standard.

For WordPress blocks, the current additional rules published by RRZE are also binding:

```text
https://www.wp.rrze.fau.de/entwicklung/einsatz-fremdplugins/vorgaben-an-bloecke/
```

If the published FAU/RRZE CMS admission rules are updated, the current published version MUST be considered during development, review, and release.

In particular:

- every plugin MUST have a named, competent and reachable responsible contact person;
- this responsible person or team MUST be able to provide user support;
- responsibility does not end after initial delivery;
- the maintainer MUST react to defects and compatibility problems in a timely manner;
- the plugin MUST remain compatible with the WordPress and PHP versions operated by RRZE;
- the maintainer MUST monitor upcoming WordPress changes and adapt the plugin proactively;
- an abandoned plugin or a plugin without a reachable maintainer is not suitable for continued production operation;
- a one-time AI-generated implementation without a durable maintainer and support responsibility is not acceptable.

Technical compliance alone does not create an entitlement to installation or continued operation on the RRZE CMS.

## 1.2 Priority requirements for RRZE CMS plugins

The following are high-priority acceptance criteria and MUST be considered from the beginning of development.

### 1.2.1 Multisite is mandatory

Every plugin intended for the RRZE CMS MUST be fully WordPress Multisite-capable. This requirement cannot be waived by a project-local exception. A plugin that only works correctly in Single Site MUST NOT be admitted to the RRZE CMS.

The plugin MUST correctly separate network-wide and site-specific configuration, permissions, data, uploads, activation/update behavior and migrations, and MUST be designed for large Multisite installations.

### 1.2.2 Usability for non-technical users

Usability by editors and administrators without technical training is a primary functional requirement. Settings and workflows MUST be self-explanatory, unambiguous, task-oriented and consistent with WordPress conventions. Technical implementation details MUST NOT be exposed merely because this is convenient for developers.

Normal settings MUST be separated from advanced or rarely used settings. Advanced functionality SHOULD be placed in a clearly identified `Advanced` / `Erweitert` section. Safe defaults SHOULD be used wherever possible.

### 1.2.3 WordPress-native GUI and Block Editor

Plugin administration MUST follow the established WordPress GUI and interaction model. WordPress-native controls, notices, settings patterns and editor concepts MUST be used wherever suitable equivalents exist. Editorial controls associated with posts, pages or block-based content MUST integrate naturally with the Block Editor.

### 1.2.4 End-user documentation is mandatory

Every production plugin MUST have both a named, competent and reachable maintainer/support contact and maintained user documentation available on a web page.

The documentation MUST be understandable without computer-science or software-development knowledge and explain purpose, normal workflows, relevant settings, permissions, limitations and common problems where applicable. Repository-only developer documentation is not sufficient. The canonical documentation URL MUST be referenced from `README.md` and SHOULD be linked from the plugin administration interface where appropriate.

### 1.2.5 Block-first content integration

New plugins that insert or manage content in posts, pages or custom post types MUST use WordPress blocks. New shortcodes for content-authoring functionality are prohibited. Existing legacy shortcodes MAY only be retained for backwards compatibility.

New plugins SHOULD avoid classic metaboxes on block-enabled post types. Where document-level configuration is needed, current Block Editor-compatible mechanisms MUST be preferred. A classic metabox MAY only be introduced when no suitable current mechanism exists and the reason is documented.

### 1.2.6 Capability-based separation

Advanced functionality intended only for experienced or privileged users MUST use the WordPress capability system. Hiding controls with CSS or JavaScript is not authorized; permissions MUST be enforced server-side.

On Multisite, network-global configuration MUST only be editable by appropriately authorized Network Admin/Super Admin users. Site-specific configuration with substantial effects on site appearance, behavior, integrations or data MUST only be editable by appropriately authorized Site Admin users. Ordinary editors MUST only receive controls required for their editorial tasks.

### 1.2.7 Network-wide license keys and secrets

If a license key, API credential, token or comparable secret is valid for the complete Multisite network, it MUST be stored and managed exclusively as a network option using `get_site_option()`, `update_site_option()`. The architecture for API-Key storage of `rrze-faudir` SHOULD be considered as a reference pattern where applicable.

Such credentials MUST be managed in Network Admin by authorized Network Admin/Super Admin users. They MUST NOT be copied to per-site options. Site-level settings MUST NOT display the credential or its input field, and Site Admins MUST NOT be able to reveal, modify, replace or override it. Site-level UI MAY indicate that the service is centrally configured without revealing the credential.

*On FAU-CMS Site-wide API Keys MUST be added to the `rrze-settings`-Plugin. RRZE Webteam can assist if Multisite-Wide API Options need to be stored.*

# 2. Reference standards

Development MUST take the following standards and documentation into account.

## 2.1 WordPress

Primary references:

- WordPress Plugin Developer Handbook  
  https://developer.wordpress.org/plugins/
- WordPress Plugin Header Requirements  
  https://developer.wordpress.org/plugins/plugin-basics/header-requirements/
- WordPress Coding Standards  
  https://developer.wordpress.org/coding-standards/wordpress-coding-standards/
- WordPress PHP Coding Standards  
  https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/
- WordPress JavaScript Coding Standards  
  https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/
- WordPress CSS Coding Standards  
  https://developer.wordpress.org/coding-standards/wordpress-coding-standards/css/
- WordPress Accessibility Coding Standards  
  https://developer.wordpress.org/coding-standards/wordpress-coding-standards/accessibility/
- WordPress Plugin Security guidance  
  https://developer.wordpress.org/plugins/security/
- WordPress REST API Handbook  
  https://developer.wordpress.org/rest-api/
- WordPress Multisite documentation  
  https://developer.wordpress.org/advanced-administration/multisite/

WordPress best practices concerning security, interoperability, escaping, sanitization, internationalization, hooks, APIs, database access, capabilities, nonces, and compatibility MUST be followed.

Project-specific formatting rules in this document override WordPress formatting rules where they differ.

---

## 2.2 Accessibility

Frontend and backend user interfaces MUST be designed for accessibility.

Target:

- WCAG 2.2 Level AA as the general minimum requirement;
- WCAG 2.2 Level AAA for form input workflows, form controls, validation, instructions, error handling, and user input interactions is desirable wherever the applicable WCAG success criteria can be satisfied by the plugin itself, but it is not mandatory;
- semantic HTML;
- keyboard operability;
- visible focus;
- sufficient contrast;
- proper accessible names;
- labels for form controls;
- meaningful error messages;
- no information communicated by color alone;
- correct heading hierarchy;
- accessible status and validation messages;
- correct ARIA only where native HTML is insufficient.

Reference:

- WCAG 2.2  
  https://www.w3.org/TR/WCAG22/

Accessibility is a mandatory functional requirement, not a cosmetic enhancement.

For forms and other direct data-entry workflows, **WCAG 2.2 AAA** SHOULD be considered and applied where reasonable and technically practical. It is a desired quality target, not a mandatory acceptance requirement.

Corporate design requirements MUST NOT override accessibility requirements.

---

# 3. Repository requirements

Every plugin MUST be maintained in Git.

GitHub or GitLab MUST be used as the canonical repository.

The repository MUST contain at least:

```text
plugin-slug/
├── .gitignore
├── LICENSE
├── README.md
├── readme.txt
├── package.json
├── plugin-slug.php
├── includes/
├── src/
├── build/
└── languages/
```

If the plugin uses Node/npm dependencies, `package-lock.json` MUST be committed. It is required for reproducible development, CI, and release builds, but is not a universal runtime requirement for every plugin.

Additional directories MAY be added when justified, for example:

```text
includes/
src/
build/
tests/
docs/
.github/
vendor/
templates/
assets/
```

Generated artifacts MUST NOT be edited manually.

Changes MUST be made in source files and then rebuilt.

---

# 4. Naming conventions

The institutional prefix indicates responsibility and MUST NOT be chosen merely for convenience.

## 4.1 Examples and placeholders

Examples in this standard that use names, namespaces, or slugs such as RRZE, FAU, or UTN are placeholders only. The applicable naming convention MUST be determined before development and used consistently for the actual plugin.

## 4.2 RRZE-owned or RRZE-commissioned plugins

The prefix `RRZE` / `rrze-` is reserved for plugins whose development has been commissioned, accepted, or is maintained under responsibility of RRZE.

Example:

```text
Plugin Name: RRZE Example
Plugin slug: rrze-example
PHP namespace: RRZE\Example
PHP prefix where required: rrze_example_
Text domain: rrze-example
Main file: rrze-example.php
```

A developer, chair, institute, project group, professor, contractor, or other FAU institution MUST NOT use the `RRZE` prefix merely because the plugin is intended for use on RRZE infrastructure.

## 4.3 Plugins created by other FAU institutions

Plugins originating from other FAU institutions SHOULD use the `FAU` / `fau-` prefix unless another approved institutional naming convention exists.

Because the central RRZE CMS operating rules reserve the prefixes `rrze-`, `fau-`, and `cms-` for coordinated use, the `FAU` / `fau-` prefix MUST be agreed with RRZE before a plugin is admitted to the central RRZE CMS.

Example:

```text
Plugin Name: FAU Example
Plugin slug: fau-example
PHP namespace: FAU\Example
PHP prefix where required: fau_example_
Text domain: fau-example
Main file: fau-example.php
```

The repository and plugin documentation MUST identify the actual responsible institution and maintainer.

These identifiers MUST remain internally consistent.

The AI MUST NOT invent multiple spellings of the slug, namespace, text domain, package name, or constant prefix.

Names MUST be chosen once at project initialization and reused consistently.

## 4.4 Plugins in use at other higher education institutions

Plugins in use for the Technical University of Nuremberg SHOULD use the `UTN` / `utn-` prefix unless another approved institutional naming convention exists.

---

# 5. Plugin header

The main plugin file MUST contain the WordPress plugin header.

Recommended baseline:

```php
<?php

/*
Plugin Name:        RRZE Example
Plugin URI:         https://github.com/RRZE-Webteam/rrze-example
Version:            1.0.0
Description:        Short description of the plugin.
Author:             RRZE Webteam
Author URI:         https://www.wp.rrze.fau.de/
License:            GNU General Public License Version 3
License URI:        https://www.gnu.org/licenses/gpl-3.0.html
Text Domain:        rrze-example
Domain Path:        /languages
Requires at least:  6.8
Requires PHP:       8.2
*/
```

Adjust minimum WordPress and PHP versions according to the deployment target.

Only the main plugin file MUST contain the WordPress plugin header.

The header SHOULD additionally use `Update URI` if required by the deployment/update architecture.

`Network: true` MUST only be used if the plugin is intentionally restricted to network activation.

A normal plugin that also supports Multisite MUST NOT use `Network: true` merely because Multisite is supported.

---

# 6. Versioning

Semantic Versioning SHOULD be used:

```text
MAJOR.MINOR.PATCH_BUILD
```

Examples:

```text
1.0.0
1.1.0
1.1.1
2.0.0
2.5.3_17
```

Interpretation:

- PATCH: backwards-compatible bug fix;
- MINOR: backwards-compatible feature;
- MAJOR: incompatible API, storage, configuration, or behavioral change;
- BUILD: used for continous development within the dev branch

The version number MUST have a single canonical source.

At release/build time the same version MUST be synchronized to all relevant locations, including where applicable:

- plugin header;
- `package.json`;
- `readme.txt`;
- build metadata;
- release ZIP filename;
- constants used by the plugin;
- asset cache-busting logic.

A build or release MUST fail if inconsistent versions are detected.

Do not manually maintain unrelated version strings in several files without validation.

---

# 7. README files

Two different README files SHOULD exist because they serve different purposes.



## 7.1 readme.txt

`readme.txt` SHOULD follow WordPress plugin readme conventions where applicable.

It SHOULD contain:

```text
=== Plugin Name ===
Contributors:
Tags:
Requires at least:
Tested up to:
Requires PHP:
Stable tag:
License:
License URI:

Short description.

```

`Stable tag` MUST match the released plugin version if used.

---

## 7.2 README.md

`README.md` is the developer and repository documentation.

It SHOULD contain:

```text
# Plugin name

Short description.

## Contributors

 List of: Autor/Team, URL of Autor/Team

## Copyright

GNU General Public License (GPL) Version 3

## Documentation

Canonical public documentation URL and end-user guidance.

## Feedback

* Canonical public documentation URL for issues and feedback
* Email contact


## Requirements (optional)

If needed for devs or for users

## Installation (optional)

If needed for devs or for users

## Configuration (optional)

If needed for devs or for users

## Multisite behavior  (optional)

If needed for devs or for users

## Accessibility (optional)

If accessibility errors or problems exists, document them here. Otherwise list 
accessibility problems in a accessibility.json file like defined in 
https://github.com/xwolfde/Accessibility-Metadata-Format

## External services

## Cookies and browser storage (optional)

If cookies or local store is used, document them here

## Data storage (optional)

If used.

## Hooks and APIs (optional)

If avaible.

```

The README MUST state all external dependencies and services.

If external APIs are used, document:

- provider;
- purpose;
- transmitted data;
- authentication method;
- timeout behavior;
- failure behavior;
- privacy implications;
- whether data leaves FAU infrastructure;
- required API scopes.



### Badge in GitHub-Repo

By default, RRZE plugins use a badge in the first line containing the version number, release, and other information.

Example: 

```
[![Aktuelle Version](https://img.shields.io/github/package-json/v/rrze-webteam/rrze-legal/main?label=Version)](https://github.com/RRZE-Webteam/rrze-legal) [![Release Version](https://img.shields.io/github/v/release/rrze-webteam/rrze-legal?label=Release+Version)](https://github.com/rrze-webteam/rrze-legal/releases/) [![GitHub License](https://img.shields.io/github/license/rrze-webteam/rrze-legal)](https://github.com/RRZE-Webteam/rrze-legal) [![GitHub issues](https://img.shields.io/github/issues/RRZE-Webteam/rrze-legal)](https://github.com/RRZE-Webteam/rrze-legal/issues)
```



---

# 8. Recommended PHP architecture

For non-trivial plugins, procedural code SHOULD be limited to the plugin bootstrap.

Business logic MUST be organized in classes.

Recommended structure:

```text
includes/
├── API/
│   └── Client.php
├── Blocks/
│   └── Blocks.php
├── Main.php
├── Config.php
├── Plugin.php
├── REST.php
├── Scheduler.php
├── Settings.php
├── Template.php
└── Utils.php

```

The exact structure MUST reflect the actual responsibilities of the plugin.

Do not create classes merely to imitate architecture.

Do not put the entire plugin into one giant `Main` class.

Prefer small classes with a clear responsibility.

---

# 9. Namespaces and autoloading

All new PHP classes MUST use namespaces.

Preferred root namespace:

```php
namespace RRZE\PluginName;
```

One class, interface, trait, or enum SHOULD normally exist per PHP file.

Class and file naming MUST be consistent.

PSR-4-compatible autoloading SHOULD be used.

If Composer is used, prefer:

```php
require_once __DIR__ . '/vendor/autoload.php';
```

If Composer is not required, a small project-local PSR-4 autoloader MAY be used.

Do not introduce Composer solely because an AI template happens to prefer it.

---

# 10. PHP code style

RRZE project formatting takes precedence over generic formatter defaults.

Follow PSR-12 Standards.

Correct:

```php
if ($condition) {
    do_something();
} else {
    do_something_else();
}
```

Correct:

```php
class Example 
{
    public function run() 
    {
        if ($this->isReady()) {
            $this->process();
        }
    }
}
```

Incorrect:

```php
if ($condition)
{
    do_something();
}
```

Opening braces MUST remain on the same line as:
- `if`;
- `elseif`;
- `else`;
- `for`;
- `foreach`;
- `while`;
- `switch`;
- `try`;
- `catch`.

Indent with four spaces unless a repository-specific existing rule requires otherwise.

Code MUST be readable over clever.

Avoid deeply nested logic.

Use early returns where they improve clarity.

---

# 11. Anonymous functions and arrow functions in PHP

Arrow functions SHOULD NOT be used in PHP.

Anonymous functions SHOULD NOT be used.

Hooks SHOULD use named methods or named functions.

Preferred:

```php
add_action('init', [Plugin::getInstance(), 'init']);
```

or:

```php
add_action('init', __NAMESPACE__ . '\\init');
```

Avoid:

```php
add_action('init', function () {
    // ...
});
```

Anonymous functions MAY only be used when there is a concrete technical reason and a named callable would materially worsen the implementation.

The reason MUST be documented.

AI-generated anonymous PHP callbacks MUST be treated as a code smell and rewritten by default.

---

# 12. WordPress hooks

WordPress hooks MUST be registered deliberately.

Do not register expensive callbacks globally when they are only needed on specific screens or requests.

Prefer standard WordPress actions and filters over custom interception mechanisms.

Custom hooks SHOULD be provided when they allow other plugins to alter meaningful behavior without modifying source code.

Custom hook names MUST be prefixed or namespaced.

Example:

```php
$value = apply_filters('rrze_example_result', $value, $context);
```

Actions and filters MUST be documented where they form part of the plugin's supported extension API.

---

# 13. Configuration hierarchy

Plugins MUST distinguish between:

1. hard-coded defaults;
2. network-wide configuration;
3. site-specific configuration;
4. runtime overrides.

## 13.1 Option names, option keys and stored structures

All option names created by a plugin MUST use the plugin slug as a prefix.

Example for plugin slug `rrze-example`:

```text
rrze_example_settings
rrze_example_api
rrze_example_display
```

Because WordPress option names conventionally use underscores, the slug MAY be normalized from:

```text
rrze-example
```

to:

```text
rrze_example
```

for option identifiers.

Nested keys, values representing named settings, and associative array keys stored inside plugin-owned options SHOULD also use a plugin-specific prefix where collisions, exports, merges, filters, or shared structures could otherwise create ambiguity.

Example:

```php
$settings = [
    'rrze_example_api_url' => '',
    'rrze_example_api_key' => '',
    'rrze_example_timeout' => 10,
];
```

The plugin MUST NOT create generic global option names such as:

```text
settings
api_key
timeout
display_options
```

Option names are persistent global identifiers and MUST be treated as part of the plugin's compatibility surface.

## 13.2 Transient names

All WordPress transient and site-transient names created by the plugin MUST use the plugin slug as a prefix.

This applies to:

```php
set_transient()
get_transient()
delete_transient()

set_site_transient()
get_site_transient()
delete_site_transient()
```

Example:

```php
set_transient('rrze_example_remote_data', $data, HOUR_IN_SECONDS);
set_site_transient('rrze_example_network_status', $status, 15 * MINUTE_IN_SECONDS);
```

Generic transient names such as:

```text
cache
api_response
status
remote_data
```

MUST NOT be used.

Transient identifiers MUST be centralized where practical so cache invalidation remains maintainable.

A recommended precedence is:

```text
runtime/filter override
        ↓
site option
        ↓
network option
        ↓
plugin default
```

The exact precedence MUST be documented.

For security-sensitive global configuration, site-level override MAY intentionally be prohibited.

---

# 14. Single Site and Multisite

As a general engineering rule, plugins SHOULD work on both WordPress Single Site and WordPress Multisite.

For plugins intended for the RRZE CMS, full Multisite support is REQUIRED and cannot be waived by a project-local exception. Where a plugin is also distributed outside the RRZE CMS, it SHOULD additionally remain usable on Single Site.

Code MUST NOT assume that Multisite functions are always available in a meaningful context without checking `is_multisite()`.

The plugin MUST define explicitly:

- whether it may be activated per site;
- whether it may be network activated;
- whether both modes are supported;
- where settings are stored;
- how network defaults interact with site values;
- which capabilities are required.

---

# 15. Network settings

Values that represent infrastructure-wide configuration SHOULD normally be stored as network options on Multisite.

Examples:

- central API endpoint;
- tenant identifier;
- shared service URL;
- organization-wide feature flag;
- centrally administered API key;
- central authentication configuration.

Use:

```php
get_site_option()
update_site_option()
delete_site_option()
```

for Multisite-wide values.

Use:

```php
get_option()
update_option()
delete_option()
```

for site-specific values.

The naming is unfortunately historical: WordPress "site options" are network-level options. Document this clearly in code.

---

# 16. Configuration resolver

Configuration lookup SHOULD be centralized.

Do not spread calls to `get_option()` and `get_site_option()` across unrelated classes.

Preferred conceptual API:

```php
$value = Settings::get('keyname');
```

The settings component decides whether the value comes from:

- network configuration;
- site configuration;
- default value;
- filter override.

Example:

```php
namespace RRZE\Example\Config;

class Settings {
    public static function get(string $key, mixed $default = null): mixed {
        if (is_multisite()) {
            $networkValue = get_site_option('rrze_example_' . $key, null);

            if ($networkValue !== null) {
                return $networkValue;
            }
        }

        $siteValue = get_option('rrze_example_' . $key, null);

        if ($siteValue !== null) {
            return $siteValue;
        }

        return $default;
    }
}
```

The final implementation MUST distinguish between "not configured" and legitimate false-like values such as:

```text
0
false
""
[]
```

where necessary.

---

# 17. API keys and secrets

Secrets MUST NOT be:

- committed to Git;
- printed into HTML;
- exposed in JavaScript;
- placed in REST responses;
- logged in plaintext;
- embedded in source maps;
- returned in debug endpoints.

On Multisite, centrally managed API keys SHOULD normally be stored as network options if that is the intended operational model.

Only authorized network administrators MUST be able to view or modify network-level credentials.

Site administrators MUST NOT automatically gain access to centrally stored credentials.

When possible, sensitive fields SHOULD display masked values rather than echoing the stored secret.

Updating a secret MUST NOT require resubmitting the old secret in clear text.


Centrally managed API keys will be managed by the network plugin https://github.com/RRZE-Webteam/rrze-settings and obtained by using the `rrze_settings` option.

Example:

```php
 public static function getKey(): string {
        if (is_multisite()) {
            $settingsOptions = get_site_option('rrze_settings');

            if (is_object($settingsOptions)) {
                return (string) ($settingsOptions->plugins->rrze_example_apiKey ?? '');
            }
            if (is_array($settingsOptions)) {
                return (string) ($settingsOptions['plugins']['rrze_example_apiKey'] ?? '');
            }
        }
        $options = get_option('rrze_example_options');
        if (!is_array($options)) {
            return '';
        }

        $key = $options['api_key'] ?? '';
        return is_string($key) ? trim($key) : '';
    }
```  

---

# 18. Settings pages

Settings pages MUST use appropriate WordPress capabilities.

Do not assume that every administrator should have every capability.

Typical distinctions include:

```text
manage_options
manage_network_options
activate_plugins
manage_network_plugins
```

Choose the capability that matches the operation.

Network settings MUST be exposed in Network Admin when they are network-owned.

Site settings MUST be exposed in site admin when they are site-owned.

Forms MUST use WordPress nonces.

Input MUST be validated and sanitized.

Output MUST be escaped.

---

# 19. Security baseline

Every feature MUST be reviewed against the principle:

> Never trust input. Never assume authorization. Escape output at the point of output.

At minimum:

- validate expected data type and structure;
- sanitize incoming values;
- use capability checks;
- use nonces for state-changing browser actions;
- escape output late;
- use `$wpdb->prepare()` for variable SQL;
- avoid raw SQL when WordPress APIs are sufficient;
- validate REST request parameters;
- implement REST `permission_callback`;
- protect AJAX actions with capability and nonce checks;
- prevent direct file access where appropriate;
- do not expose secrets;
- do not trust filenames, MIME types, URLs, IDs, or user-provided HTML;
- do not deserialize untrusted data;
- avoid `eval()`;
- avoid dynamic PHP execution;
- avoid shell execution;
- avoid arbitrary file inclusion;
- use WordPress HTTP APIs instead of ad-hoc remote requests.

---

# 20. Sanitization, validation and escaping

These concepts MUST NOT be confused.

## Validation

Determine whether data is acceptable.

Examples:

```php
is_email()
rest_validate_value_from_schema()
filter_var()
```

## Sanitization

Normalize or clean data before storage or processing.

Examples:

```php
sanitize_text_field()
sanitize_key()
sanitize_email()
sanitize_url()
absint()
wp_kses_post()
```

## Escaping

Escape data at output time.

Examples:

```php
esc_html()
esc_attr()
esc_url()
esc_textarea()
wp_kses()
```

Do not pre-escape data before storage unless there is a specific reason.

Store canonical data and escape for the actual output context.

---

# 21. Nonces are not authorization

A nonce verifies intent/context, not permission.

A state-changing request generally requires both:

```php
check_admin_referer(...);
```

and:

```php
current_user_can(...);
```

Do not treat a valid nonce as sufficient authorization.

---

# 22. Database access

Prefer native WordPress APIs:

- Options API;
- Metadata API;
- Posts API;
- Taxonomy API;
- User API;
- Transients API;
- REST API.

Custom database tables MAY be used when the data model or expected scale justifies them.

Before creating a table, document why existing WordPress storage mechanisms are insufficient.

Custom SQL MUST use `$wpdb->prepare()` for external values.

Database schema migrations MUST be versioned.

Plugin updates MUST be able to migrate existing installations safely.

---

# 23. Data deletion and uninstall

Deactivation MUST NOT automatically delete user data.

If cleanup on uninstall is appropriate, use an explicit `uninstall.php` or supported uninstall hook.

Destructive cleanup MUST be deliberate.

Document:

- what data the plugin stores;
- where it is stored;
- what is removed on uninstall;
- what is intentionally retained.

Network-wide cleanup MUST consider every site if data is stored per site.

Do not iterate over a very large Multisite network synchronously without considering runtime limits.

---

# 24. External HTTP services and runtime third-party resources

Remote communication MUST use the WordPress HTTP API where server-side communication is required.

## 24.1 Runtime-loaded resources

Plugins MUST NOT load executable, visual, tracking, font, media, library, stylesheet, JavaScript, or other runtime resources directly from third-party hosts or public CDNs unless this has been explicitly approved for the specific plugin and use case.

Prohibited by default are, for example:

```text
Google Fonts
jsDelivr
cdnjs
unpkg
remote JavaScript libraries
remote CSS frameworks
third-party tracking pixels
remote fonts
third-party embeds that initiate requests immediately
```

Required libraries, fonts, icons, CSS, and JavaScript MUST normally be shipped locally with the plugin or obtained from WordPress Core if WordPress already provides them.

A third-party runtime request MAY only occur when at least one of the following applies:

1. RRZE has explicitly approved the external resource and processing; or
2. the resource is subject to an appropriate consent mechanism and is loaded only after valid consent; or
3. the resource is functionally required as an explicitly approved external service and its privacy/security implications have been documented.

A plugin MUST NOT circumvent this rule by dynamically injecting remote `<script>`, `<link>`, `<iframe>`, image, font, or module URLs.

Consent-dependent resources MUST NOT be fetched before consent.

The README MUST document every approved runtime third-party resource, its purpose, host/provider, data transfer, and consent behavior.

For server-side remote communication use:

```php
wp_remote_get()
wp_remote_post()
wp_remote_request()
```

Every request MUST define sensible timeout behavior.

Failure MUST be handled explicitly.

A remote service failure MUST NOT normally make the entire WordPress backend unusable.

External API responses MUST be treated as untrusted input.

Cache remote results where appropriate.

Rate limits MUST be respected.

Do not retry aggressively.

Avoid synchronous remote API calls during normal frontend rendering when a cached or asynchronous approach is possible.

---

# 25. REST API

REST endpoints MUST:

- use a unique namespace;
- register explicit methods;
- define arguments;
- validate and sanitize arguments;
- implement `permission_callback`;
- return `WP_REST_Response`, normal REST-compatible data, or `WP_Error`;
- avoid leaking internal information.

Example route namespace:

```text
rrze-example/v1
```

Do not expose administrator-only information merely because an endpoint URL is difficult to guess.

---

# 26. WP-Cron and background processing

Long-running work SHOULD NOT occur during normal page rendering.

Use background processing where appropriate.

Cron tasks MUST:

- use unique hook names;
- avoid duplicate scheduling;
- tolerate retries;
- be idempotent where practical;
- clean up scheduled events on deactivation if they are plugin-specific;
- not assume exact execution time.

For large Multisite installations, avoid unbounded synchronous loops over all sites.

Use batches.

---

# 27. Retrieval of external websites

If a plugin performs automated retrieval of external websites, feeds, APIs,
files, or other web resources on behalf of an FAU organizational unit,
project, or service, it MUST follow the FAU crawler rules defined in
`RRZE-Crawler-Rules.md`.

The plugin MUST explicitly set an appropriate HTTP `User-Agent` for such
requests instead of relying on the default User-Agent of WordPress, PHP, wget,
curl, Guzzle, Requests, or any other HTTP library.

The User-Agent MUST identify the responsible FAU organization, the crawler or
plugin component, its version, a stable information URL, and a monitored
contact address according to the schema defined in `RRZE-Crawler-Rules.md`.

The plugin MUST also respect applicable crawler behavior rules, including
`robots.txt`, declared sitemaps where relevant, target-specific access
restrictions, per-origin rate limits, `Retry-After`, HTTP 429 and HTTP 503
backoff behavior, and the restrictions on cookies and session state.

Do not use wget, curl, PHP streams, Guzzle, Requests, browser automation, or
similar retrieval mechanisms to bypass WordPress HTTP API requirements,
identity requirements, access controls, crawler restrictions, or rate limits.

---

# 28. Caching

Caching SHOULD use WordPress-supported mechanisms unless the infrastructure explicitly provides another abstraction.

Potential mechanisms:

- object cache;
- transients;
- site transients;
- request-local in-memory cache.

Cache keys MUST be prefixed.

Cache invalidation MUST be defined.

Never cache secrets into publicly accessible output.

Network-wide data MAY use site transients where appropriate.

---

# 29. Internationalization

All user-visible strings MUST be translatable unless they are externally supplied data.

Use the plugin text domain consistently.

Examples:

```php
__('Text', 'rrze-example');
esc_html__('Text', 'rrze-example');
_x('Post', 'noun', 'rrze-example');
```

Dynamic values MUST use placeholders.

Correct:

```php
$message = sprintf(
    /* translators: %s: User name. */
    __('Hello %s', 'rrze-example'),
    $name
);
```

Do not concatenate sentence fragments where that prevents correct translation.

---

# 30. Accessibility implementation rules

All UI generated by the plugin MUST be operable without a mouse.

Use native HTML controls before ARIA-based custom widgets.

Correct:

```html
<button type="button">Open details</button>
```

Avoid:

```html
<div role="button" tabindex="0">Open details</div>
```

unless native HTML cannot implement the requirement.

Forms MUST provide programmatically associated labels.

Validation errors MUST be:

- understandable;
- associated with the relevant field;
- perceivable by screen readers;
- not represented only by color.

Dynamic status messages SHOULD use suitable live regions when needed.

Focus MUST be managed deliberately when dialogs, overlays, or dynamic editor components are created.

Do not remove focus outlines without providing an accessible replacement.

---

# 31. Editor UX

The target users of administrative interfaces are editors and administrators, not plugin developers.

UI design MUST NOT expose implementation concepts unnecessarily.

Avoid requiring users to understand:

- post meta keys;
- REST routes;
- JSON;
- internal IDs;
- API payload structures;
- WordPress hook names;
- database terminology.

Prefer WordPress-native controls and interaction patterns. Use the WordPress Native @wordpress/components Library for UI Components used inside the BlockEditor Interface.

Before implementing a complex administration workflow, define the actual user workflow.

Do not assume that technically detectable state is equivalent to editorial intent.

---

# 32. WordPress Blocks

For new plugins that add content to posts, pages, or custom post types, blocks are the mandatory content integration mechanism. Blocks MUST use current WordPress block APIs. New shortcodes MUST NOT be introduced for content-authoring functionality. Classic metaboxes SHOULD NOT be introduced for new functionality on block-enabled post types unless no appropriate Block Editor mechanism exists and the reason is documented.

## 32.1 Block category and name

Every plugin block SHOULD be assigned to the `RRZE`, `FAU`, or an agreed institutional category, such as the abbreviation of another higher education institution.

The `RRZE` category is reserved for blocks developed by or commissioned and accepted by RRZE or the RRZE Webteam. External developers MUST obtain prior agreement from the RRZE Webteam before assigning a block to that category.

Blocks from other teams that are intended for institution-wide FAU use SHOULD use the `FAU` category. Blocks developed for another higher education institution SHOULD use that institution's agreed abbreviation as their category.

The block name SHOULD use a stable namespaced slug. The preferred pattern is:

```text
category-or-plugin-name/block-name
```

Examples:

```text
rrze/block-name
fau/block-name
plugin-name/block-name
```

If a plugin provides multiple blocks, the plugin slug MAY be used as the namespace. Block names are a public compatibility surface and MUST NOT be renamed casually.

## 32.2 Block metadata, structure and backwards compatibility

New blocks SHOULD follow the current WordPress-recommended block file structure. `block.json` MUST remain the primary definition of block metadata.

Block updates MUST preserve existing editorial content. The WordPress Block Deprecation API MUST be used whenever a change affects stored block markup or block metadata compatibility. This includes:

- changes to relevant `block.json` values for static and dynamic blocks;
- changes to the `save` implementation of a static block;
- changes to attributes, attribute defaults, or serialization that affect existing content.

Static blocks MUST provide appropriate deprecated block definitions for every supported historical markup or attribute format. Dynamic blocks MUST likewise preserve compatible parsing of existing saved block comments and attributes when `block.json` or attributes change.

## 32.3 Block controls and editorial usability

Block controls MUST follow the WordPress interaction model. Settings required to insert, fill, or quickly configure a block belong in the Block Toolbar. Complex, secondary, or advanced settings belong in the Settings Sidebar.

The normal editorial workflow of a block SHOULD be self-explanatory without requiring separate documentation. Contextual descriptions, help text, and explanations for non-obvious settings SHOULD be provided in the Settings Sidebar at the point where they are needed. This does not replace the mandatory user documentation for the plugin as a whole.

Blocks MUST be delivered in English (`en_US`) and additionally provide German (`de_DE`) and formal German (`de_DE_formal`) translations for all user-visible strings.

## 32.4 Block quality and styling

Blocks MUST work without errors with the currently operated WordPress version. They MUST NOT create browser-console errors during normal editor or frontend use. Defects MUST be corrected promptly.

Editor CSS and styling adjustments SHOULD be scoped so they affect only the block that owns them. Block frontend output SHOULD NOT rely on inline `style` attributes except where a specific, documented WordPress API or accessibility requirement makes this unavoidable.

Recommended structure:

```text
src/
└── example-block/
    ├── components/
    │   └── example-selector.tsx
    ├── block.json
    ├── index.ts
    ├── edit.tsx
    ├── editor.scss
    ├── style.scss
    └── view.ts
```

Thus JS is allowed. TS is recommended.

Reusable components may be used to keep edit.tsx clean.
Instead of using save.tsx, the plugin SHOULD prefer Dynamic Rendering via PHP. Static blocks are not recommended due to deprecation and maintenance.

If save.tsx is used, always provide a dedicated deprecated.tsx.

Generated block assets SHOULD be written to:

```text
build/blocks/example-block/
```

`block.json` SHOULD be the primary block metadata definition.

Use server-side rendering for dynamic blocks where appropriate instead of static renders via save.tsx

Inside the Editor always mimic the PHP render output, avoid using ServerSideRender Component.

Do not store generated HTML in attributes when structured attributes or dynamic rendering are more appropriate.

Accessibility MUST apply in both editor and frontend output.

---

# 33. JavaScript source structure

Human-authored JavaScript & TypeScript MUST live below `src/`, or another explicitly documented source directory.

Example:

```text
src/
├── js/
│   ├── admin.js
│   ├── frontend.js
│   └── modules/
├── ts/
│   ├── admin.ts
│   ├── frontend.ts
│   └── components/
└── scss/
    ├── admin.scss
    ├── frontend.scss
    └── components/
```

Compiled files MUST be written to `build/`.

Example:

```text
build/
├── js/
│   ├── admin.js
│   └── frontend.js
└── css/
    ├── admin.css
    └── frontend.css
```

Never edit files in `build/` manually.

---

# 34. JavaScript style

Use modern JavaScript supported by the configured WordPress build pipeline.

Arrow functions are permitted and normal in JavaScript and TypeScript. A clearly named arrow function assigned to a constant is acceptable. Prefer named function declarations or clearly named arrow functions over inline anonymous callbacks where this improves readability, reuse, testability, or hook cleanup.

Project-specific examples:

Preferred:

```js
const handleClick = (event) => {
	event.preventDefault();
}

function handleClick(event) {
    event.preventDefault();
}
```

Prefer named functions or clearly named arrow functions.

Inline anonymous JavaScript callbacks SHOULD be avoided unless technically justified. This rule does not prohibit arrow functions assigned to a clearly named constant.

Use WordPress packages where they provide the intended functionality.

Do not bundle another copy of React for Gutenberg code. Use wordpress Element package instead.

Use WordPress-provided dependencies where appropriate.

---

# 35. CSS and SCSS

SCSS or source CSS MUST live in the source tree.

Example:

```text
src/scss/
```

Compiled CSS MUST live below:

```text
build/css/
```

Block styles MAY live beside their block source:

```text
src/example-block/style.scss
src/example-block/editor.scss
```

and compile into the block build directory.

CSS MUST:

- be scoped to the plugin;
- use the plugin-slug wrapper class hierarchically for selectors that modify plugin output;
- scope block-editor styles to the owning block and avoid changing other editor blocks or WordPress editor UI;
- avoid overly broad selectors;
- avoid global element resets;
- avoid unnecessary `!important`;
- respect WordPress admin styles;
- support responsive layouts;
- maintain accessible contrast;
- preserve focus indicators;
- not encode semantic meaning by color alone.

Prefix classes where practical, for example:

```text
.rrze-example-
```

---

# 36. Asset loading

CSS and JavaScript MUST only be loaded where required.

## 36.1 Development and production asset modes

The build system MUST distinguish clearly between development and production assets.

### Development (`dev` branch)

For development builds:

- CSS MUST NOT be minified;
- JavaScript MUST NOT be minified;
- source maps SHOULD be generated;
- source maps SHOULD reference the actual source files;
- readable symbol/function names SHOULD be preserved where technically possible;
- development builds MAY contain additional diagnostics that are prohibited in production.

Typical outputs:

```text
build/css/rrze-example.css
build/css/rrze-example-admin.css
build/js/rrze-example.js
build/js/rrze-example-admin.js
*.map
```

### Production (`main` branch / release)

For production builds:

- all CSS MUST be minified;
- all JavaScript MUST be minified;
- source map files MUST NOT be included in the production release;
- production CSS/JS MUST NOT reference missing `.map` files through `sourceMappingURL`;
- development-only diagnostics MUST be removed;
- the production package MUST contain only assets intended for runtime use.

The build/release process SHOULD fail if source maps are accidentally included in the production artifact.

## 36.2 Consolidation and file naming

Production assets SHOULD be consolidated as far as technically sensible.

Frontend CSS SHOULD normally be delivered as a single plugin stylesheet named after the plugin slug:

```text
rrze-example.css
```

Backend/admin CSS SHOULD normally be delivered as:

```text
rrze-example-admin.css
```

Frontend JavaScript SHOULD normally be delivered as:

```text
rrze-example.js
```

Backend/admin JavaScript SHOULD normally be delivered as:

```text
rrze-example-admin.js
```

Even when files are physically minified, the canonical production filename SHOULD remain based on the plugin slug unless the existing release process explicitly requires `.min.css` or `.min.js`.

Multiple CSS or JS files MAY remain separate only where there is a concrete runtime benefit, such as:

- avoiding loading large feature-specific bundles;
- block-specific lazy loading;
- significantly different frontend contexts;
- WordPress-generated block asset metadata;
- independently cacheable components.

Do not split assets merely because source files are organized into multiple modules.

Source organization and production bundling are different concerns.

Use:

```php
wp_enqueue_script()
wp_enqueue_style()
```

Do not print `<script>` or `<link>` tags manually when enqueue APIs are suitable.

Admin assets SHOULD be restricted to plugin screens where possible.

Frontend assets SHOULD only load when needed.

Dependencies MUST be declared correctly.

Asset versions SHOULD use the plugin version or build-generated asset metadata.

During development, `filemtime()` MAY be useful for cache busting, but production release behavior MUST be deterministic.

---

# 37. Node.js build system

Node.js MUST be used only for development/build tooling unless runtime JavaScript genuinely requires Node outside WordPress, which would be unusual.

`package.json` MUST define:

- project name;
- project version;
- license;
- structured repository metadata (`type`, `url`, `issues`, `clone`);
- author metadata;
- compatibility metadata (`wprequires`, `wptestedup`, `phprequires`);
- the plugin main file where tooling requires it;
- supported Node/npm engines;
- development dependencies;
- build scripts;
- version/metadata synchronization scripts;
- start/watch scripts;
- lint scripts;
- test scripts where applicable.

`@wordpress/scripts` (`wp-scripts`) is the preferred build tool for WordPress projects. It MUST be installed and used for projects that build WordPress blocks, Block Editor extensions, or other WordPress-specific JavaScript that relies on the WordPress build pipeline.

In particular, `wp-scripts` MUST be used for block-specific build tasks, including where applicable:

- automatic `block.json` entry-point detection and generated block assets;
- JSX/TSX transformation for Block Editor code;
- WordPress package handling, including packages such as `@wordpress/element`;
- WordPress dependency extraction and generated asset dependency metadata;
- block-specific development, linting, testing, and production build workflows.

A custom script based only on `esbuild` and `sass`, for example `scripts/build-assets.js`, MUST NOT replace `wp-scripts` for these WordPress- or block-specific tasks unless equivalent behavior is explicitly implemented, documented, and verified.

## 37.1 Asset and metadata-only builds

When a plugin provides no blocks, no Block Editor extensions, and no other WordPress-specific JavaScript build requirements, it MAY omit `@wordpress/scripts`.

In this limited case, a project-local Node.js script MAY use `esbuild` and `sass` to generate and minify normal JavaScript and CSS assets. The same script or companion Node.js scripts MAY also synchronize the version, compatibility metadata, plugin header, `readme.txt`, and `README.md` metadata.

This exception is for asset and metadata generation only. It does not authorize a custom `esbuild`/`sass` script to build blocks or replace WordPress-specific `wp-scripts` behavior.

## 37.2 Mixed projects

Projects MAY combine both approaches. A project-local Node.js script MAY generate ordinary assets and synchronize version or documentation metadata, while `wp-scripts` builds all WordPress- and Block Editor-specific source code. Build commands MUST make this separation explicit and remain reproducible from a clean checkout.

### WordPress and Block Editor baseline

```json
{
    "scripts": {
        "build:wordpress": "wp-scripts build",
        "start:wordpress": "wp-scripts start",
        "build:assets": "node scripts/build-assets.js",
        "sync:metadata": "node scripts/build-version.js",
        "build": "npm run sync:metadata && npm run build:assets && npm run build:wordpress",
        "lint:js": "wp-scripts lint-js",
        "lint:css": "wp-scripts lint-style",
        "format": "wp-scripts format"
    }
}
```

### Asset and metadata-only baseline

For a plugin covered by section 37.1, the build scripts MAY instead remain limited to the required asset and metadata tasks:

```json
{
    "scripts": {
        "build:assets": "node scripts/build-assets.js",
        "sync:metadata": "node scripts/build-version.js",
        "build": "npm run sync:metadata && npm run build:assets"
    }
}
```

For multiple entry points, use an explicit build configuration or clearly named scripts.

Builds MUST be reproducible from a clean checkout using documented commands.

---

# 38. package-lock.json

When Node/npm dependencies are used, `package-lock.json` MUST be committed.

CI and release builds SHOULD use:

```bash
npm ci
```

instead of:

```bash
npm install
```

to produce reproducible dependency installations.

Do not manually modify `package-lock.json`.

---

# 39. Build commands

The repository SHOULD support at least:

```bash
npm ci
npm run build
npm run start
```

If PHP tooling is present, also provide suitable commands for:

```text
PHP lint
PHPCS
PHPUnit
```

A single command such as:

```bash
npm run check
```

or:

```bash
composer check
```

SHOULD run the relevant static checks.

---

# 40. Production repository and deployment model

The plugin is deployed from the Git repository by an updater mechanism. Production installations operated by RRZE obtain the plugin directly from the `main` branch.

Therefore:

- no manually uploaded release ZIP is required;
- no ZIP exchange is part of the standard deployment process;
- the `main` branch itself MUST contain a directly executable, production-ready WordPress plugin;
- all runtime dependencies required by the plugin MUST already be present in `main`;
- generated production assets required at runtime MUST already be built and committed to `main`;
- `main` MUST NOT depend on an additional local build step on the production server.

The `main` branch is the deployable artifact.

A fresh checkout of `main` into the WordPress plugins directory MUST be sufficient for WordPress to load and execute the plugin, assuming the documented WordPress/PHP/environment requirements are fulfilled.

## 40.1 Files that belong in `main`

Files required for runtime operation MUST be committed.

Typically this includes:

```text
plugin-slug.php
includes/
build/
languages/
vendor/        if runtime Composer dependencies are required and deployment does not run Composer
readme.txt
README.md
LICENSE
package.json
```

Additional runtime directories MAY be present where required.

Production CSS and JavaScript in `main` MUST follow the production asset rules defined elsewhere in this standard:

- minified;
- no source maps;
- deployable without an additional build step.

## 40.2 Files that MUST NOT be committed

Files and directories that are purely local, temporary, IDE-specific, operating-system-specific, generated only for development, or otherwise not part of the maintained repository MUST be excluded through `.gitignore`.

Examples include:

```text
.idea/
.vscode/              if it contains only local IDE state
.nbproject/private/
**/nbproject/
.DS_Store
Thumbs.db
*.tmp
*.log
mode_modules/
local environment files
local credentials
temporary build directories
test output
coverage output
developer-specific configuration
```

Secrets MUST never be committed, regardless of `.gitignore`.

The purpose of `.gitignore` is to keep non-project and local build artifacts out of the repository. It MUST NOT be used to exclude production runtime files that `main` requires in order to operate.

Development source files and build scripts MAY remain in the repository if they are part of the maintained source and required for future development. The decisive rule is not "build-related files must never be committed", but rather:

> `main` must contain everything required to run the plugin, while local/temporary/IDE artifacts that do not belong to the maintained project must be excluded.

## 40.3 Documentation

If repository-local technical documentation is required, Markdown documentation SHOULD be placed below:

```text
doc/
```

Example:

```text
doc/
├── architecture.md
├── configuration.md
├── development.md
└── api.md
```

Where practical, linking to an authoritative maintained documentation URL is preferred over duplicating extensive documentation in the repository.

`README.md` SHOULD therefore reference the canonical documentation URL where such documentation exists.

Documentation URLs MUST point to maintained, durable project documentation rather than ephemeral personal locations.

## 40.4 `package.json` repository metadata

`package.json` MUST contain repository metadata sufficient to identify the canonical source repository, issue tracker, and clone URL.

Recommended structure:

```json
{
    "repository": {
        "type": "git",
        "url": "https://github.com/RRZE-Webteam/rrze-faudir",
        "issues": "https://github.com/RRZE-Webteam/rrze-faudir/issues",
        "clone": "git+https://github.com/RRZE-Webteam/rrze-faudir.git"
    }
}
```

The URLs MUST reference the actual canonical repository.

## 40.5 `package.json` author metadata

Author information MUST be represented as structured metadata.

Recommended form:

```json
{
    "author": {
        "name": "RRZE-Webteam <webmaster@fau.de> (https://www.rrze.fau.de)",
        "url": "https://www.wordpress.rrze.fau.de"
    }
}
```

The actual project MUST use the responsible organization or maintainer information appropriate to that plugin.

Do not claim RRZE authorship for plugins that are not commissioned, accepted, or maintained by RRZE.

## 40.6 `package.json` compatibility metadata

The canonical compatibility metadata MUST be maintained in `package.json`.

Recommended structure:

```json
{
    "compatibility": {
        "wprequires": "6.7",
        "wptestedup": "6.9.4",
        "phprequires": "8.3"
    }
}
```

Meanings:

- `wprequires`: minimum supported WordPress version;
- `wptestedup`: highest WordPress version currently tested;
- `phprequires`: minimum supported PHP version.

These values MUST be treated as authoritative project metadata and MUST be synchronized into every other location where WordPress or project tooling requires them.

Typical target locations include:

- plugin header `Requires at least`;
- plugin header `Requires PHP`;
- `readme.txt` `Requires at least`;
- `readme.txt` `Tested up to`;
- `readme.txt` `Requires PHP`;
- compatibility constants used by the plugin;
- generated metadata where applicable.

## 40.7 Central metadata synchronization script

Version and compatibility metadata MUST NOT be maintained independently by hand in multiple files.

`package.json` is the canonical source for at least:

- plugin version;
- WordPress minimum version;
- WordPress tested-up-to version;
- PHP minimum version;
- repository metadata;
- author metadata where relevant.

A Node.js build/update script MUST propagate the canonical values into all required plugin files.

The implementation SHOULD follow the principle used by:

```text
https://github.com/RRZE-Webteam/rrze-faudir/blob/main/scripts/build-version.js
```

A typical script location is:

```text
scripts/build-version.js
```

The script MUST support the project's development/release workflow and MUST update all relevant metadata consistently.

At minimum it SHOULD:

1. read `package.json`;
2. validate the version as Semantic Versioning;
3. update the version according to the requested mode;
4. write the resulting version back to `package.json`;
5. synchronize the plugin header version;
6. synchronize `readme.txt` stable/version metadata where present;
7. synchronize WordPress/PHP compatibility values from `package.json`;
8. synchronize other project constants or metadata that duplicate these values;
9. fail with a non-zero exit status when expected files or metadata are inconsistent or cannot be updated.

The exact version increment policy MAY differ by project, but it MUST be deterministic and documented.

Example commands MAY be:

```bash
node scripts/build-version.js dev
node scripts/build-version.js prod
node scripts/build-version.js release
```

Equivalent npm scripts SHOULD be provided:

```json
{
    "scripts": {
        "version:dev": "node scripts/build-version.js dev",
        "version:prod": "node scripts/build-version.js prod",
        "version:release": "node scripts/build-version.js release"
    }
}
```

Because metadata synchronization modifies tracked files, it MUST normally be performed in the development/release process before the final promotion from `dev` to `main`.

The production updater MUST NOT modify versions or compatibility metadata after checkout.

# 41. Version and compatibility synchronization

`package.json` is the canonical source for version and compatibility metadata.

The synchronization script described in section 40 MUST ensure that all duplicated values are identical before promotion to `main`.

A production state MUST be rejected if, for example:

```text
package.json version:       1.2.0
plugin header Version:      1.1.0
readme.txt Stable tag:      1.2.0
```

or:

```text
package.json wprequires:    6.8
plugin header Requires:     6.7
readme.txt Requires:        6.8
```

The build/check process MUST fail on such inconsistencies.

The same rule applies to:

- `wptestedup`;
- `phprequires`;
- version constants;
- generated compatibility metadata.

Manual editing of derived metadata SHOULD be avoided. Change canonical values in `package.json` and execute the synchronization script instead.

# 42. Source maps

Production source maps MUST be considered deliberately.

If source maps could expose internal source paths, development comments, secrets, or non-public code, they MUST NOT be deployed publicly.

Never allow credentials to appear in source code regardless of source-map policy.

---

# 43. PHP dependencies

Third-party PHP libraries SHOULD be minimized.

Before introducing a library, evaluate:

- maintenance status;
- license compatibility;
- security record;
- package size;
- transitive dependencies;
- whether WordPress already provides equivalent functionality.

Composer dependencies MUST be locked with `composer.lock` for applications/plugins where reproducible builds are required.

---

# 44. JavaScript dependencies

Third-party frontend dependencies SHOULD be minimized.

Before adding an npm dependency, ask:

1. Does WordPress already ship a suitable package?
2. Is this dependency maintained?
3. What is its bundle size?
4. Does it create accessibility problems?
5. Does it introduce a security or supply-chain risk?
6. Is the license compatible?
7. Is the feature worth the maintenance burden?

Do not install large UI frameworks for trivial controls.

---

## 44.1 Cookies and browser storage

If a plugin creates, reads, or modifies cookies, `localStorage`, or `sessionStorage`, this MUST be documented in `README.md`.

The documentation MUST identify at least:

- storage mechanism;
- exact name/key;
- purpose;
- data stored;
- lifetime or expiration behavior;
- whether the value is technically necessary;
- whether consent is required;
- whether the value is site-specific or network-wide.

Cookie names MUST contain the plugin slug or an unambiguous normalized form of it.

Example:

```text
rrze-example-preference
rrze_example_preference
```

Generic names such as:

```text
settings
preferences
accepted
state
```

MUST NOT be used.

For each cookie, the README MUST document its lifetime explicitly, for example:

```text
Name: rrze-example-preference
Purpose: Stores the selected display preference.
Lifetime: 30 days
Type: First-party
```

For `localStorage`, which has no automatic expiration, the README MUST explicitly state that it persists until deleted and SHOULD document the plugin's cleanup behavior.

For `sessionStorage`, the README MUST document that storage is scoped to the browser tab/session.

Client-side storage MUST NOT contain secrets, authentication credentials, privileged API keys, or sensitive personal data unless a separate security/privacy review explicitly permits it.

# 45. Privacy and data protection

Plugins MUST document personal-data processing.

Before sending data to external services, determine:

- what data is sent;
- whether personal data is included;
- legal/organizational permission;
- retention;
- third-country transfer implications;
- logging;
- consent or other legal basis where applicable.

Do not send full post content, usernames, email addresses, IP addresses, or identifiers to external AI/API services merely because the API can accept them.

Transmit only what is required.

---

# 46. Logging and diagnostics

Logging MUST be purposeful.

On RRZE-operated WordPress installations, plugins MUST integrate with the logging interface provided by the `rrze-log` plugin:

```text
https://github.com/RRZE-Webteam/rrze-log/
```

Diagnostic information MUST be emitted through the actions defined by `rrze-log`.

Available log actions currently include:

```php
do_action('rrze.log.error', $message, $context);
do_action('rrze.log.warning', $message, $context);
do_action('rrze.log.notice', $message, $context);
do_action('rrze.log.info', $message, $context);
```

The payload SHOULD identify the originating plugin in structured context whenever practical.

In a production release:

- plugin-generated debug messages MUST NOT be written directly to PHP standard error;
- plugin-generated warnings MUST NOT be written directly to PHP standard error;
- `error_log()` MUST NOT be used as the plugin's production logging mechanism;
- raw `trigger_error()` diagnostics MUST NOT be used as an operational logging mechanism;
- debugging output MUST NOT be printed into HTML, REST responses, AJAX output, or JavaScript console output;
- relevant diagnostic, warning, notice, and error information MUST use the `rrze-log` actions.

If `rrze-log` is not active, the plugin MUST continue operating safely. Logging integration MUST NOT introduce a hard runtime dependency unless explicitly declared and approved.

Logs MUST NOT contain:

- passwords;
- API keys;
- authentication headers;
- access tokens;
- unnecessary personal data;
- complete sensitive payloads.

Centralize plugin logging where significant logging is required.

---


## 46.1 Plugin-owned log files

The preferred operational logging mechanism on RRZE installations remains the `rrze-log` action interface.

If a plugin has an approved technical requirement to create its own logfile or log directory, that data MUST be stored below:

```text
wp-content/log/
```

The logfile name or plugin-specific subdirectory MUST contain the plugin slug.

Examples:

```text
wp-content/log/rrze-example.log
wp-content/log/rrze-example/
wp-content/log/rrze-example/events.log
```

Generic paths such as the following MUST NOT be used:

```text
wp-content/log/plugin.log
wp-content/log/debug.log
wp-content/log/custom/
```

Plugin-owned log files MUST NOT be written into:

```text
wp-content/plugins/
wp-content/uploads/
the plugin directory
the web root
/tmp
```

unless a separately approved infrastructure requirement explicitly mandates another location.

The plugin MUST:

- create directories only when required;
- use safe filesystem permissions;
- tolerate missing or unwritable log directories gracefully;
- avoid exposing log files through frontend URLs;
- never log secrets or unnecessary personal data;
- document retention and cleanup behavior where the plugin owns persistent logs.

## 46.2 Plugin-owned upload directories

If a plugin stores uploaded media or files, these MUST be placed inside the WordPress uploads directory of the respective installation/site.

The plugin MUST use WordPress APIs such as:

```php
wp_upload_dir()
```

to determine the correct uploads base directory.

A plugin-specific subdirectory MUST use the plugin slug.

Example:

```text
wp-content/uploads/rrze-example/
```

In Multisite, the actual physical uploads path MUST be resolved through WordPress APIs and MUST NOT be hard-coded, because WordPress may use site-specific upload paths.

Files MUST NOT be stored directly in:

```text
wp-content/
wp-content/plugins/
the plugin directory
the WordPress root
```

The plugin MUST NOT assume that the uploads path is always literally:

```text
wp-content/uploads/
```

The logical requirement is:

```text
WordPress uploads base directory
└── plugin-slug/
```

Example PHP concept:

```php
$uploadDir = wp_upload_dir();
$pluginDir = trailingslashit($uploadDir['basedir']) . 'rrze-example';
```

If files are user-uploaded, the plugin MUST additionally apply all file validation and security requirements defined elsewhere in this standard.

The README MUST document:

- what files are stored;
- the plugin-specific upload directory;
- whether files are public or protected;
- retention and deletion behavior;
- Multisite behavior where relevant.

# 47. Error handling

External failures MUST not normally result in white screens or fatal errors.

Use:

- `WP_Error`;
- exceptions where architecturally appropriate;
- admin notices;
- structured REST errors;
- logging;
- graceful fallback.

User-facing errors MUST explain what the user can do next.

Do not show stack traces, credentials, raw API responses, SQL, or internal filesystem paths to normal users.

---

# 48. Performance

Plugins MUST be designed for large installations.

Assume that Multisite may contain hundreds or thousands of sites.

Avoid:

- unbounded queries;
- `posts_per_page => -1` on potentially large datasets;
- loading all sites into memory;
- repeated uncached remote requests;
- expensive logic on every request;
- repeated option calls inside large loops where results can be cached;
- unnecessary asset loading;
- full-network synchronous migrations.

Database queries SHOULD be measurable and bounded.

Background processing SHOULD use batches.

---

# 49. Multisite scale

Never assume that code acceptable for 5 sites is acceptable for 900 sites.

Operations that iterate through a network MUST:

- be batched;
- be resumable where practical;
- record progress if long running;
- handle errors per site;
- avoid leaving the current blog context changed.

When using:

```php
switch_to_blog($blogId);
```

always restore:

```php
restore_current_blog();
```

Use `try/finally` where it improves safety.

---

# 50. Activation and migrations

Activation hooks MUST remain fast.

Do not perform large remote operations during activation.

Schema/data migrations SHOULD run incrementally and use a stored schema version.

A migration MUST be safe to run more than once where feasible.

The plugin MUST handle upgrades from supported prior versions.

Do not assume every user installs only the newest version on a fresh site.

---

# 51. Compatibility

The plugin MUST declare minimum supported versions.

Compatibility policy SHOULD identify:

- minimum WordPress version;
- tested WordPress version;
- minimum PHP version;
- supported browsers if frontend functionality depends on them;
- Node version used for builds.

Do not use newer PHP syntax than the declared minimum PHP version supports.

Do not use WordPress APIs newer than the declared minimum WordPress version without compatibility guards.

---

# 52. Deprecated APIs

Do not introduce deprecated WordPress APIs.

When touching existing code that uses deprecated APIs, evaluate whether the code should be modernized as part of the change.

Do not perform unrelated large refactors merely to satisfy stylistic preferences.

---

# 53. Backwards compatibility

Public hooks, stored option names, REST routes, block names, post types, taxonomies, and serialized data may form part of the plugin's compatibility surface.

Do not rename them casually.

If a breaking change is necessary:

- document it;
- provide migration where feasible;
- increment the MAJOR version where SemVer applies.

---

# 54. Testing

Every non-trivial feature SHOULD have an explicit test strategy.

Depending on the plugin, tests MAY include:

- PHP unit tests;
- JavaScript unit tests;
- REST tests;
- integration tests;
- end-to-end tests;
- Multisite tests;
- accessibility tests;
- manual editor workflow tests.

At minimum, test:

- activation;
- deactivation;
- Single Site;
- Multisite site activation;
- Multisite network activation where supported;
- permissions;
- invalid input;
- missing configuration;
- external API failure;
- frontend output;
- admin output.

---

# 55. Accessibility testing

Automated accessibility tests are useful but insufficient.

Where UI is added or significantly modified, perform at least:

- keyboard-only test;
- visible focus test;
- accessible-name test;
- form label/error test;
- semantic structure review;
- color/contrast check;
- screen-reader-oriented DOM review.

Complex widgets SHOULD be manually tested with assistive technology when feasible.

## 55.1 WordPress Plugin Check (PCP)

Before a plugin is considered ready for production, it MUST be tested with the official WordPress **Plugin Check** plugin (PCP).

Reference:

```text
https://wordpress.org/plugins/plugin-check/
```

The production candidate MUST pass Plugin Check without errors.

All checks relevant to the plugin MUST be executed against the same code state that is intended to be promoted to `main`.

Warnings and recommendations SHOULD also be reviewed and either corrected or explicitly documented with a technically justified reason.

An unresolved **error** from Plugin Check blocks production release.

The release checklist MUST record that Plugin Check was executed successfully.

---

# 56. Code quality tools

Recommended tools include, where appropriate:

```text
PHP_CodeSniffer / WordPress Coding Standards
PHPStan
Psalm
PHPUnit
@wordpress/scripts
ESLint
Stylelint
Jest
Playwright
axe-core
```

Project-specific K&R formatting MUST not be automatically destroyed by a formatter configured for another brace style.

If PHPCS is used, configure or selectively apply rules so that WordPress semantic/security standards remain enforced even where RRZE formatting intentionally differs.

---

# 57. Continuous Integration

Repositories SHOULD use CI.

CI SHOULD verify at least:

- installability;
- PHP syntax;
- JavaScript lint;
- CSS lint;
- tests where present;
- build success;
- version consistency;
- release packaging where relevant.

Recommended matrix testing MAY include multiple supported PHP and WordPress versions.

A failed mandatory check MUST block release.

---

# 58. Git workflow and deployment branches

Every plugin MUST be developed in a Git repository.

The repository MUST use at least:

```text
dev
main
```

Rules:

- `dev` is the integration and development branch.
- `main` is the production branch.
- Active development MUST take place against `dev`.
- Feature and fix branches MAY branch from `dev` and MUST be merged back into `dev`.
- Code MUST NOT be developed directly in `main`.
- A version becomes production-ready only after the tested and fully built state from `dev` has been deliberately promoted into `main`.
- `main` MUST always represent the currently approved, directly executable production state.
- Required production build artifacts MUST already be committed to `main`.

For installations operated by RRZE, the deployed/current plugin code is always obtained from `main` by the Git-based updater mechanism. No release ZIP upload or ZIP exchange is part of the standard deployment workflow.

Recommended flow:

```text
feature/* or fix/*
        ↓
       dev
        ↓
review + build + tests + Plugin Check
        ↓
       main
        ↓
RRZE production deployment
```

The `main` branch SHOULD be protected against unreviewed direct pushes.

Commit messages SHOULD describe meaningful changes.

Do not commit generated noise unrelated to the actual change.

Do not mix mass formatting changes with functional changes.

---

# 59. AI-assisted development rules

When an AI is asked to implement a task in this repository, it MUST follow this process:

1. Read this document completely.
2. Inspect the existing repository before changing code.
3. Identify existing architecture and conventions.
4. Search for existing functionality before adding new functionality.
5. Identify Single Site and Multisite implications.
6. Identify security implications.
7. Identify accessibility implications.
8. Identify editor/admin UX implications.
9. Identify data migration implications.
10. Identify external service/privacy implications.
11. Propose the smallest maintainable change.
12. Implement in source files, not generated build files.
13. Run the relevant build.
14. Run lint/tests.
15. Review the resulting diff.
16. Update documentation and changelog where required.
17. Verify version handling if the change is a release.
18. Report unresolved risks or assumptions.

The AI MUST NOT claim a feature is finished if required tests were not run.

The AI MUST explicitly state which checks were actually executed.

---

# 60. AI prohibition against speculative implementation

If requirements are ambiguous, the AI MUST NOT silently invent business rules.

It SHOULD infer only technically harmless defaults.

For decisions affecting:

- user workflow;
- permissions;
- data retention;
- network policy;
- legal compliance;
- accessibility behavior;
- meaning of content;
- external data transfer;
- API costs;
- destructive migration;

the AI MUST identify the assumption explicitly.

Where repository context already answers the question, inspect the repository instead of asking.

---

# 61. AI code review requirements

After implementation, the AI MUST review its own diff for:

```text
Security
Accessibility
Multisite
Performance
Internationalization
Backwards compatibility
Error handling
Editor UX
Version consistency
Documentation
Testing
```

The review MUST look for generated-code pathologies such as:

- duplicated functions;
- unused abstractions;
- generic helper classes with no purpose;
- excessive comments that merely repeat code;
- broad exception swallowing;
- arbitrary retry loops;
- hard-coded URLs;
- hard-coded IDs;
- hard-coded credentials;
- excessive option reads;
- unescaped output;
- unsanitized input;
- missing capability checks;
- missing REST permission callbacks;
- anonymous callbacks;
- giant classes;
- unnecessary JavaScript dependencies.
- clean Code principles followed;

---

# 62. Comments and documentation

Comments SHOULD explain why, constraints, side effects, or non-obvious behavior.

Avoid comments such as:

```php
// Increment counter.
$counter++;
```

Prefer documentation for architectural intent:

```php
// Network configuration takes precedence because API credentials are
// centrally administered and must not be overridden by site admins.
```

Public or non-obvious methods SHOULD use PHPDoc where useful.

JavaScript APIs SHOULD use JSDoc where useful.

Do not generate paragraphs of boilerplate documentation for self-explanatory private methods.

---

# 63. Maintainability

Optimize for the developer who must understand the code two years later.

Prefer:

- explicit dependencies;
- clear names;
- short methods;
- cohesive classes;
- documented data ownership;
- conventional WordPress APIs;
- predictable build steps.

Avoid:

- meta-programming;
- hidden global state;
- magic configuration;
- excessive service containers;
- framework-style architecture without need;
- generated complexity.

A 30-line clear implementation is preferable to a 200-line abstraction generated because "enterprise architecture" sounded impressive.

---

# 64. Dependency on themes

Plugins MUST NOT depend on a specific theme unless the plugin's purpose explicitly requires it.

Functional behavior belongs in plugins.

Visual integration MAY use theme-supported CSS variables, hooks, block styles, or patterns where available, but the plugin MUST fail gracefully when those facilities are absent unless they are a declared dependency.

Do not hard-code FAU theme DOM structures unless explicitly required and documented.

---

# 65. Content ownership

A plugin MUST not silently rewrite editorial content merely to make its internal state simpler.

When a plugin manages or transforms content, define:

- which content remains user-owned;
- which values are generated;
- which values are authoritative;
- whether generated content may be manually edited;
- how conflicts are resolved.

Technically detectable differences MUST NOT automatically be interpreted as editorial errors.

---

# 66. Blocks vs custom UI vs content

Before implementing a new UI feature, determine whether the requirement is already satisfied by:

- Core blocks;
- block patterns;
- reusable patterns;
- block variations;
- post meta;
- editor sidebar controls;
- existing RRZE blocks;
- normal page content.

Do not create a plugin feature for something editors can already express reliably and accessibly using existing blocks unless automation or governance provides a concrete benefit.

---

# 67. API abstraction

Third-party APIs SHOULD be isolated behind a client/service class.

Example:

```text
includes/API/Client.php
```

The rest of the plugin SHOULD not depend directly on vendor-specific request/response formats.

This makes it possible to:

- replace providers;
- mock API calls;
- test failures;
- centralize authentication;
- centralize timeouts;
- centralize logging;
- centralize rate-limit handling.

---

# 68. Feature flags

Experimental or infrastructure-dependent functionality SHOULD use explicit feature flags.

Feature flags SHOULD be centrally resolvable and documented.

A network administrator MAY control global feature flags in Multisite.

Do not implement hidden behavior based on arbitrary URL parameters or undocumented constants.

---

# 69. Admin notices

Admin notices MUST be actionable and shown only to users who can act on them.

Do not show network configuration errors to ordinary editors.

Do not display a permanent dismissible warning on every backend page when the issue only concerns one plugin settings screen.

Use network admin notices for network-owned problems.

Use site admin notices for site-owned problems.

---

# 70. Capability model

Capabilities MUST model operations, not job titles. Advanced functionality MUST be exposed only to users with the required capability. On Multisite, network-owned configuration MUST be restricted to Network Admin/Super Admin authority; significant site-owned configuration MUST be restricted to Site Admin authority. Hiding a control is not authorization; privileged reads and writes MUST be protected server-side.

Do not rely only on:

```php
current_user_can('administrator')
```

WordPress roles are not capabilities.

Use capabilities appropriate to the task.

Custom capabilities MAY be introduced if the plugin needs finer-grained authorization.

---

# 71. Output and HTML

Prefer semantic HTML.

Do not generate invalid nested interactive elements.

Do not place buttons inside links or links inside buttons.

All URLs MUST be escaped.

All text output MUST be escaped according to context.

HTML intended to allow formatting MUST use a defined allowlist, for example with `wp_kses()` or `wp_kses_post()` where appropriate.

---

## 71.1 Structured data and Schema.org

Whenever a plugin outputs domain-specific information, the developer MUST determine whether Schema.org defines a suitable vocabulary/type for that information.

Reference:

```text
https://schema.org/
```

If an applicable Schema.org definition exists, the rendered HTML MUST expose the corresponding information as semantically correct structured data.

For plugins governed by this standard, the required representation is **Schema.org Microdata embedded in HTML**.

Use:

```html
itemscope
itemtype
itemprop
```

Example:

```html
<article itemscope itemtype="https://schema.org/Person">
    <h2 itemprop="name">Max Mustermann</h2>
    <a itemprop="email" href="mailto:max.mustermann@example.org">
        max.mustermann@example.org
    </a>
</article>
```

Structured data MUST truthfully represent visible or legitimately represented content.

Do not invent unavailable values.

Do not misuse a vaguely related Schema.org type when no semantically correct type exists.

Microdata markup MUST preserve valid, accessible HTML.

If no appropriate Schema.org definition exists, no artificial structured-data mapping is required.


## 71.2 Output scoping and CSS namespace

Every plugin-generated frontend or backend output MUST be wrapped in a container whose CSS class corresponds to the plugin slug.

Example for plugin slug `rrze-example`:

```html
<div class="rrze-example">
    ...
</div>
```

If a more specific context is required, additional classes MAY be added, but the plugin-slug class MUST remain present.

Examples:

```html
<div class="rrze-example rrze-example-admin">
    ...
</div>
```

```html
<section class="rrze-example rrze-example-search-results">
    ...
</section>
```

This requirement applies to:

- frontend output;
- wp-admin pages;
- settings pages;
- metaboxes;
- custom editor interfaces;
- shortcode output;
- block frontend output where a plugin-controlled wrapper exists;
- dynamically rendered plugin UI.

### CSS scoping requirement

Plugin CSS MUST assume this plugin-slug wrapper hierarchically.

Selectors that change the appearance or behavior of HTML elements MUST be scoped below the plugin wrapper.

Preferred:

```css
.rrze-example .button {
    ...
}

.rrze-example .result-list li {
    ...
}

.rrze-example-admin input[type="text"] {
    ...
}
```

Avoid:

```css
.button {
    ...
}

.result-list li {
    ...
}

input[type="text"] {
    ...
}
```

Global selectors that can affect WordPress Core, themes, or other plugins MUST NOT be introduced unless there is an explicit, documented requirement.

This rule exists to prevent style collisions in large WordPress installations with many simultaneously active plugins.

Even when using highly specific internal class names, the plugin-slug wrapper SHOULD remain the outer CSS namespace.

# 72. Forms

Forms MUST:

- have labels;
- use correct input types;
- indicate required fields accessibly;
- preserve user input after validation failure where reasonable;
- identify errors;
- use nonce protection for state changes;
- check capabilities;
- sanitize values before storage;
- escape values on redisplay.

Placeholders MUST NOT be the only form labels.

---

# 73. JavaScript enhancement

Prefer progressive enhancement where reasonable.

Basic content MUST remain understandable when optional JavaScript behavior fails.

Do not make server-generated information inaccessible solely because a JavaScript bundle failed to load.

For application-like interfaces where JavaScript is essential, failure handling MUST be explicit.

Prefer TypeScript for complex tasks.

---

# 74. AJAX

Prefer REST API for new structured endpoints unless WordPress admin-ajax provides a simpler justified fit.

For AJAX:

- verify nonce;
- verify capability;
- sanitize input;
- return structured JSON;
- escape only at the eventual output context;
- handle failure codes correctly.

Avoid exposing unauthenticated `wp_ajax_nopriv_*` actions without deliberate security analysis.

---

# 75. File handling

File uploads MUST use WordPress APIs where possible.

Validate:

- capability;
- nonce;
- file type;
- MIME type;
- file size;
- expected extension;
- destination.

Never trust the original filename.

Prevent path traversal.

Do not execute uploaded files.

---

# 76. URL handling

Use WordPress URL helpers.

Do not concatenate administrative URLs manually when helpers exist.

Examples:

```php
admin_url()
network_admin_url()
site_url()
home_url()
plugins_url()
plugin_dir_url()
```

For external URLs, validate allowed schemes and domains where appropriate.

---

# 77. Dates and time

Use WordPress timezone-aware APIs for user-visible/local site time.

Do not assume server timezone equals WordPress site timezone.

For database/internal timestamps, document whether values are UTC or site-local.

Avoid mixing Unix timestamps, MySQL local time, and browser-local time without explicit conversion.

---

# 78. Serialization and structured data

Prefer arrays or JSON-compatible structures.

Avoid storing PHP object instances in options.

Do not call `unserialize()` on untrusted external input.

Be aware that WordPress options and metadata may be serialized automatically.

Schema changes to stored arrays MUST remain backwards-compatible or be migrated.

---

# 79. Feature lifecycle

Every feature SHOULD have an owner.

For externally contributed functionality, determine before production use:

- maintainer;
- code repository;
- issue tracker;
- release responsibility;
- security contact;
- update responsibility;
- support responsibility.

A one-time AI-generated code contribution without a maintenance owner is not sufficient for production acceptance.

---

# 80. Definition of Done

A feature is complete only when all applicable items are satisfied.

## Functional

- [ ] Requirement is implemented.
- [ ] Existing functionality was checked before duplicating it.
- [ ] Editor/admin workflow is understandable for non-technical users.
- [ ] Normal and advanced settings are separated where applicable.
- [ ] WordPress-native GUI patterns are used.
- [ ] New content functionality uses blocks rather than shortcodes.
- [ ] New classic metaboxes were avoided or a necessary exception is documented.
- [ ] Block controls use the Block Toolbar for necessary quick configuration and the Settings Sidebar for complex or advanced settings.
- [ ] A block's normal workflow is self-explanatory and contextual help is provided for non-obvious settings.
- [ ] Error paths are implemented.

## Code

- [ ] Frontend and backend output is wrapped in a plugin-slug CSS class.
- [ ] CSS selectors that modify plugin output are scoped below the plugin-slug wrapper.
- [ ] Option names use the plugin slug as prefix.
- [ ] Plugin-owned option array keys are namespaced/prefixed where applicable.
- [ ] Transient and site-transient names use the plugin slug as prefix.
- [ ] K&R brace style is used.
- [ ] Anonymous PHP functions are avoided unless justified; JavaScript/TypeScript callbacks are named where practical.
- [ ] Source code is organized by responsibility.
- [ ] Generated files were not edited manually.
- [ ] No unnecessary dependencies were introduced.
- [ ] Block names use the agreed `category-or-plugin-name/block-name` pattern and the correct institutional category.
- [ ] Block editor CSS is scoped to the owning block and frontend block output does not use avoidable inline styles.

## WordPress

- [ ] WordPress APIs are used where appropriate.
- [ ] Strings are translatable.
- [ ] Blocks provide `en_US`, `de_DE`, and `de_DE_formal` translations.
- [ ] Assets are enqueued correctly.
- [ ] Capabilities are correct.
- [ ] Nonces are used where required.

## Security

- [ ] Plugin-owned log files, if required, are stored below `wp-content/log/` with the plugin slug.
- [ ] Plugin-owned uploaded files are stored below the WordPress uploads directory in a plugin-slug subdirectory.
- [ ] No unauthorized runtime CDN or third-party resources are loaded.
- [ ] Consent-dependent resources are not loaded before consent.
- [ ] Cookies/localStorage/sessionStorage are documented when used.
- [ ] Cookie/storage keys use the plugin slug where applicable.
- [ ] Input is validated/sanitized.
- [ ] Output is escaped.
- [ ] REST/AJAX authorization is correct.
- [ ] SQL is prepared.
- [ ] No secrets are exposed.
- [ ] External responses are treated as untrusted.

## Accessibility

- [ ] Keyboard use works.
- [ ] Focus is visible.
- [ ] Controls have accessible names.
- [ ] Forms have labels and usable errors.
- [ ] Semantic HTML is used.
- [ ] WCAG 2.2 AA implications were reviewed.
- [ ] Form input workflows were reviewed against WCAG 2.2 AAA where reasonable and technically practical.

## Multisite

- [ ] Single Site was considered/tested.
- [ ] RRZE CMS plugin is fully Multisite-capable and Multisite was tested.
- [ ] Network-global configuration is restricted to Network Admin/Super Admin authority.
- [ ] Significant site-level configuration is restricted to Site Admin authority.
- [ ] Network-wide license/API keys are network options and are neither visible nor editable at site level.
- [ ] Site vs network settings are clearly defined.
- [ ] Network-admin capabilities are correct.
- [ ] Large-network performance was considered.

## Build

- [ ] `package.json` contains repository, author, and compatibility metadata.
- [ ] Version/compatibility synchronization script completes successfully.
- [ ] Plugin header/readme/compatibility metadata matches `package.json`.
- [ ] `npm ci` succeeds.
- [ ] Development assets are unminified and have source maps.
- [ ] Production CSS and JavaScript are minified.
- [ ] Production package contains no source maps.
- [ ] Production CSS/JS is consolidated and named according to the plugin slug where technically sensible.
- [ ] build succeeds.
- [ ] lint succeeds.
- [ ] tests succeed where present.
- [ ] WordPress Plugin Check (PCP) completes without errors.
- [ ] Blocks were tested against the currently operated WordPress version without browser-console errors.
- [ ] Block deprecations preserve existing content after relevant `block.json`, attribute, serialization, or static `save` changes.
- [ ] production assets are generated from source.
- [ ] release package contains only intended files.

## Release

- [ ] A competent named maintainer/support contact is documented.
- [ ] Maintained web-based end-user documentation exists and is understandable without technical training.
- [ ] README.md references the canonical user-documentation URL.
- [ ] Current FAU/RRZE CMS plugin admission rules were reviewed.
- [ ] Development occurred in Git against `dev`.
- [ ] Metadata was synchronized from `package.json` before release.
- [ ] The approved, fully built production state was promoted from `dev` to `main`.
- [ ] `main` contains a directly executable plugin without requiring a production-server build.
- [ ] RRZE deployment will use the Git updater against `main`.
- [ ] No ZIP upload/exchange is required by the deployment process.
- [ ] `.gitignore` excludes local, IDE, temporary, credential, and non-project artifacts.
- [ ] Version numbers are consistent.
- [ ] README is current.
- [ ] readme.txt is current.
- [ ] changelog is current.
- [ ] migration impact is documented.
- [ ] maintenance responsibility is assigned.

---

# 81. Initial AI prompt for creating a new plugin

The following section can be copied as the immediate task prompt after this document has been provided to an AI system.

---

## Task prompt

Create or extend a WordPress plugin according to the **RRZE WordPress Plugin Engineering Standard** in this repository.

Before writing code:

1. Inspect the complete repository.
2. Identify its existing architecture, namespace, slug, text domain, versioning scheme, build process, and coding conventions.
3. Check whether the requested feature can be implemented with existing WordPress Core functionality or existing plugin components.
4. Identify effects on Single Site and Multisite.
5. Identify whether configuration belongs at site or network level.
6. Identify security, privacy, performance, accessibility, and editor-UX implications.
7. Do not invent business rules that are not supported by the repository or requirements.

Implementation requirements:

- Use PHP namespaces and clear class responsibilities.
- Use K&R brace style.
- Avoid anonymous PHP functions unless technically justified. JavaScript and TypeScript arrow functions are permitted; avoid inline anonymous callbacks where a named callable is clearer.
- Use WordPress APIs and hooks.
- Follow WordPress security best practices.
- Target WCAG 2.2 AA for UI.
- Keep source assets in `src/` or block-specific `src/` directories.
- Generate runtime CSS/JS into `build/`.
- Development assets must remain unminified and should include source maps.
- Production assets must be minified and production source maps must be removed.
- Consolidate production frontend CSS/JS into slug-named files where technically sensible; use `-admin` for backend equivalents.
- Do not manually edit generated build files.
- Ensure the `main` branch itself contains the complete directly executable production plugin.
- Do not rely on ZIP-based deployment or a production-server build step.
- Exclude local/IDE/temporary artifacts through `.gitignore`.
- Store repository-local Markdown documentation under `doc/` when needed, but prefer a canonical documentation URL where available.
- Use `wp-scripts` for all WordPress- and Block Editor-specific build tasks. A project-local Node.js script using `esbuild` and `sass` MAY generate ordinary assets and synchronize version, compatibility, plugin-header, and README metadata alongside `wp-scripts`; it MAY replace `wp-scripts` only when the project has no corresponding WordPress-specific build requirement.
- Treat `package.json` as canonical for version and compatibility metadata.
- Use a `scripts/build-version.js`-style synchronization script to propagate version, WordPress compatibility, PHP compatibility, and duplicated metadata.
- Keep plugin header, readme metadata, constants, and package metadata synchronized.
- For RRZE CMS plugins, full Multisite support is mandatory. Preserve Single Site compatibility where the plugin is also intended for Single Site use.
- Design normal workflows for users without technical training; separate advanced settings.
- Use WordPress-native GUI patterns and integrate editorial controls into the Block Editor.
- For new content functionality use blocks; do not introduce new shortcodes and avoid new classic metaboxes.
- Place necessary quick block controls in the Block Toolbar and complex or advanced block settings in the Settings Sidebar.
- Make block workflows self-explanatory and provide contextual help for non-obvious block settings in the Settings Sidebar.
- Assign blocks to the agreed institutional category and use stable `category-or-plugin-name/block-name` names.
- Follow the current WordPress block file structure and use `block.json` as the primary metadata definition.
- Use the WordPress Block Deprecation API whenever block metadata, attributes, serialization, or static `save` markup changes could affect existing content.
- Provide `en_US`, `de_DE`, and `de_DE_formal` translations for all block UI strings.
- Scope editor styles to the owning block, avoid frontend inline styles, and ensure blocks create no browser-console errors.
- Protect advanced functionality with WordPress capabilities and separate network-global from site-level administration.
- For Multisite, treat centrally administered infrastructure values such as API endpoints, API keys, or organization-wide settings as network settings when appropriate.
- Site administrators must not be able to read, reveal, edit or override network-owned secrets or network-wide license keys; site-level settings must not display their input fields.
- Prefix option names and transient names with the plugin slug.
- Wrap all plugin-generated frontend/backend UI in a CSS class matching the plugin slug.
- Scope plugin CSS hierarchically below that plugin-slug wrapper.
- Store plugin-owned logs, if explicitly required, below `wp-content/log/` using the plugin slug in the filename or directory.
- Store plugin-owned uploads below the WordPress uploads base directory in a plugin-slug subdirectory resolved through WordPress APIs.
- Document cookies, localStorage, and sessionStorage in README.md when used; cookie/storage names must be plugin-specific.
- Do not load third-party/CDN runtime resources without explicit approval or an applicable consent mechanism.
- Ensure all new user-facing strings are translatable.
- Do not add unnecessary dependencies.
- Do not alter unrelated code.

After implementation:

1. Run all applicable build commands.
2. Run linting.
3. Run tests.
4. Review the diff.
5. Verify security.
6. Verify accessibility.
7. Verify Multisite behavior.
8. Verify version consistency.
9. Update documentation/changelog if required.
10. Report exactly which checks were executed and which could not be executed.

Do not declare the work complete merely because code was generated.

A production-ready result must be maintainable, testable, accessible, secure, documented, and suitable for long-term operation.

---

# 82. Project initialization checklist

When creating a completely new plugin, initialize these values explicitly:

```text
PLUGIN_NAME=
PLUGIN_SLUG=
PHP_NAMESPACE=
TEXT_DOMAIN=
DESCRIPTION=
PLUGIN_URI=
AUTHOR=
AUTHOR_URI=
LICENSE=
MIN_WORDPRESS_VERSION=
MIN_PHP_VERSION=
INITIAL_VERSION=
MULTISITE_SUPPORT=
RRZE_CMS_TARGET=
USER_DOCUMENTATION_URL=
ADVANCED_SETTINGS=
NETWORK_LICENSE_KEYS=
BLOCK_EDITOR_INTEGRATION=
BLOCK_CATEGORY=
BLOCK_NAMES=
BLOCK_DEPRECATION_STRATEGY=
NETWORK_ACTIVATION_SUPPORT=
NETWORK_SETTINGS=
SITE_SETTINGS=
EXTERNAL_SERVICES=
THIRD_PARTY_RUNTIME_RESOURCES=
COOKIES=
LOCAL_STORAGE=
SESSION_STORAGE=
MAINTAINER=
USER_SUPPORT_CONTACT=
```

Do not begin implementation until these values are either known or safely derivable from the repository/project context.

---

# 83. Recommended default RRZE baseline

Unless a project explicitly specifies otherwise, use the following conceptual defaults:

```text
Repository hosting: GitHub or GitLab
Default branch: main
License: GPL-compatible; RRZE project decision applies
PHP architecture: namespaced OOP
PHP brace style: K&R
PHP arrow functions: should not be used
JavaScript/TypeScript arrow functions: allowed and normal
Anonymous inline callbacks: avoid where a named callable is clearer
Frontend build: Node/npm
WordPress and Block Editor build tooling: @wordpress/scripts required where applicable
Asset/metadata-only build tooling: project-local Node.js scripts using esbuild and sass permitted when no WordPress-specific build is required
Source assets: src/
Generated assets: build/
Translations: languages/
Accessibility target: WCAG 2.2 AA generally; WCAG 2.2 AAA desired for form input workflows where reasonable and technically practical
Development branch: dev
Production branch: main, directly executable
RRZE production source: Git updater against main
ZIP deployment: not used
Canonical metadata: package.json
Version/compatibility propagation: scripts/build-version.js-style script
Production logging: rrze-log actions
Output wrapper: plugin-slug CSS class mandatory
CSS scope: hierarchically below plugin-slug wrapper
Option names: plugin slug prefix mandatory
Transient names: plugin slug prefix mandatory
Plugin-owned logs: wp-content/log/ + plugin slug
Plugin-owned uploads: WordPress uploads base + plugin slug
Dev assets: unminified + source maps
Production assets: minified, no source maps
Production bundles: slug-based names, `-admin` for backend
Third-party/CDN runtime resources: prohibited by default
Cookie/browser storage documentation: mandatory when used
Structured domain data: Schema.org Microdata where applicable
Plugin Check: mandatory, zero errors
Single Site: supported where required by distribution/use
RRZE CMS Multisite support: mandatory
Content integration: blocks; no new shortcodes
Classic metaboxes: avoid for new Block Editor functionality
Block category: RRZE, FAU, or agreed institutional category
Block name: stable `category-or-plugin-name/block-name`
Block controls: toolbar for essential quick configuration; sidebar for complex or advanced settings
Block compatibility: WordPress Block Deprecation API for breaking block metadata or markup changes
Block translations: en_US, de_DE, de_DE_formal
Block editor CSS: scoped to the owning block
Block frontend styles: avoid inline styles
Block quality: no browser-console errors
User UX: optimized for non-technical operators
End-user web documentation: mandatory
Network-wide license/API keys: Network Admin only and never visible/editable at site level
Network settings: supported where infrastructure-wide configuration exists
Versioning: Semantic Versioning
Release artifact: reproducible ZIP
Production owner: mandatory
```

---

## 83.1 Reference implementations

When architectural examples are needed, inspect current maintained RRZE plugins before inventing a new project structure.

Designated reference plugins:

```text
https://github.com/RRZE-Webteam/rrze-faudir
https://github.com/RRZE-Webteam/rrze-elements-blocks
https://github.com/RRZE-Webteam/rrze-block-control
https://github.com/RRZE-Webteam/rrze-multisite-manager
```

These repositories are examples, not immutable specifications.

An AI MUST NOT blindly copy historical code, outdated dependencies, accidental inconsistencies, or project-specific architecture from a reference plugin.

If a reference implementation conflicts with a mandatory rule in this document, this document takes precedence.

For logging, the dedicated reference is:

```text
https://github.com/RRZE-Webteam/rrze-log/
```

Its documented `rrze.log.*` actions define the required operational logging interface on RRZE installations.


For multisite settings, the dedicated reference plugin is:

```text
https://github.com/RRZE-Webteam/rrze-settings/
```


# 84. Final architectural rule

The goal of this standard is not maximum abstraction and not maximum code generation.

The goal is:

> **the smallest architecture that remains secure, accessible, understandable, maintainable, testable, Multisite-capable, and supportable over the expected lifetime of the plugin.**

When in doubt, prefer established WordPress mechanisms and existing RRZE infrastructure over new custom code.
