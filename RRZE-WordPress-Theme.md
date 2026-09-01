# RRZE WordPress Theme Engineering Standard and AI Development Prompt

( Version: 1.2,  Date: 01.09.2026,  Source: https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Theme.md )

> **Purpose:** This document defines the mandatory baseline for creating, extending, reviewing, and maintaining WordPress themes intended for operation in RRZE/FAU environments.
>
> It is designed both as an engineering standard for human developers and as a system/project prompt for AI-assisted development tools.
>
> **Normative language:** The terms **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are to be interpreted as binding priority levels. Where this document conflicts with generic AI-generated conventions, this document wins.

---

# 1. Core Principle

A WordPress theme is responsible for presentation, layout, templates, typography, color, spacing, responsive behavior, block styles, patterns, and editor/frontend visual integration.

A theme MUST NOT implement functionality that belongs in the plugin domain. Functional behavior belongs in plugins.

Examples of plugin-domain functionality include:

- custom business logic unrelated to presentation;
- data import, export, synchronization, or crawling;
- custom post types and taxonomies unless explicitly approved as part of a theme migration strategy;
- complex forms and workflows;
- authentication, authorization, or role management;
- SEO suites, analytics, tracking, consent management, or external service integrations;
- content transformation, editorial workflow, or persistent application state;
- custom plugin installers, marketplace installers, update installers, or dependency installers.

Before implementing a feature in a theme, the developer or AI MUST determine whether the requirement belongs in the theme, WordPress Core, an existing RRZE plugin, the full `RRZE-WordPress-Plugin.md` standard, or a new plugin.

The goal is the smallest theme architecture that remains accessible, maintainable, performant, Block Editor-compatible, Multisite-capable, documented, and supportable.

---

# 2. Reference Standards

Development MUST take the current WordPress and RRZE standards into account.

## 2.1 WordPress

Primary WordPress references:

- WordPress Theme Developer Handbook  
  https://developer.wordpress.org/themes/
- WordPress Block Themes Handbook  
  https://developer.wordpress.org/themes/block-themes/
- WordPress Theme Structure  
  https://developer.wordpress.org/themes/core-concepts/theme-structure/
- WordPress Main Stylesheet and theme header  
  https://developer.wordpress.org/themes/core-concepts/main-stylesheet/
- WordPress theme.json documentation  
  https://developer.wordpress.org/themes/global-settings-and-styles/
- WordPress Templates documentation  
  https://developer.wordpress.org/themes/templates/
- WordPress Block Editor Handbook  
  https://developer.wordpress.org/block-editor/
- WordPress Block API Reference  
  https://developer.wordpress.org/block-editor/reference-guides/block-api/
- WordPress block.json metadata  
  https://developer.wordpress.org/block-editor/reference-guides/block-api/block-metadata/
- WordPress Block Deprecation API  
  https://developer.wordpress.org/block-editor/reference-guides/block-api/block-deprecation/
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
- WordPress Multisite documentation  
  https://developer.wordpress.org/advanced-administration/multisite/

WordPress best practices concerning security, interoperability, escaping, sanitization, internationalization, hooks, APIs, template loading, capabilities, nonces, asset enqueueing, and compatibility MUST be followed.

Project-specific formatting rules in this document override WordPress formatting rules where they differ.

## 2.2 RRZE Specifications

The following RRZE specifications are part of the expected development context:

- `RRZE-WordPress-Plugin.md`: applies by reference wherever this document says that plugin-equivalent engineering rules also apply to themes.
- `RRZE-WordPress-Theme-LLM-Shortcut.md`: compact LLM baseline for this standard.
- `RRZE-WordPress-Entwicklungsumgebung.md`: reference development and test environment.
- `RRZE-Crawler-Rules.md`: mandatory for automated HTTP retrieval by themes.

For WordPress themes intended to be operated on or admitted to the central FAU/RRZE CMS, this SPEC repository is the canonical source for admission and engineering rules.

The RRZE WordPress development pages remain a useful public entry point for documentation, explanations, and related information about plugins, themes, blocks, and other web applications:

```text
https://www.wp.rrze.fau.de/entwicklung/
```

If documentation pages and this SPEC repository conflict, this SPEC repository wins.

## 2.3 Shared Rules from the Plugin Standard

Where a requirement is not theme-specific, the corresponding rule from `RRZE-WordPress-Plugin.md` applies analogously to themes.

At minimum, theme work MUST follow the plugin standard's rules on:

- coding style and avoidance of anonymous PHP callbacks, see `RRZE-WordPress-Plugin.md` chapters 10 and 11;
- WordPress hooks, see chapter 12;
- option names, transient names, configuration hierarchy, network settings, and secrets, see chapters 13 to 17;
- accessibility, see chapter 2.2 and the accessibility checklist;
- security, escaping, sanitization, nonces, capability checks, uploads, filesystem access, external services, cookies, browser storage, logging, REST/AJAX, and external data, where the theme uses comparable mechanisms;
- external website retrieval and User-Agent handling, see chapter 27 and `RRZE-Crawler-Rules.md`;
- WordPress blocks, see chapter 32 and chapter 8 of this document;
- Node/npm, `@wordpress/scripts`, asset builds, `package-lock.json`, deployment from `main`, and metadata synchronization, see chapters 37 to 41;
- testing and release reporting, including separation of executed tests, static review, manual review, and untested items.

These cross-references do not authorize plugin-domain functionality inside a theme. They only define how theme-owned code must be engineered when comparable code is legitimately part of a theme.

---

# 3. Admission and Maintenance

Themes for the CMS offerings of RRZE MUST satisfy the current RRZE theme admission rules.

Every production theme MUST have:

- a named, competent, reachable maintainer or support team;
- maintained web-based end-user documentation;
- compatibility with the WordPress and PHP versions operated by RRZE;
- timely maintenance for defects, security issues, accessibility issues, and WordPress/PHP compatibility changes.

A theme without durable maintenance ownership is not acceptable for production use.

Technical compliance alone does not create an entitlement to installation or continued operation on the RRZE CMS.

---

# 4. Block Editor and Theme Type

New themes for the RRZE CMS MUST be Block Editor themes.

Classic themes SHOULD NOT be created or newly admitted. A Classic Theme MAY only be accepted as a temporary, explicitly approved migration or compatibility exception. The exception MUST be documented with reason, scope, and planned replacement path.

Themes MUST NOT introduce, bundle, or require custom page builders outside the WordPress Block Editor.

Themes MUST NOT include their own plugin installers, marketplace installers, update installers, or comparable mechanisms that install plugins or executable code.

Theme-provided block styles, patterns, templates, and template parts MUST support the normal editorial workflow without requiring technical knowledge.

Where a theme restricts WordPress standard functionality, such as disabling Additional CSS, the restriction MUST be implemented in theme logic through supported WordPress mechanisms such as `functions.php`, `theme.json`, or appropriate filters. The restriction MUST be documented and justified.

---

# 5. Theme and Plugin Boundary

Themes MUST stay within the presentation domain.

Theme code MAY:

- define templates, template parts, patterns, block styles, style variations, and editor/frontend styling;
- configure theme supports;
- enqueue theme assets;
- provide navigation and layout behavior that is genuinely presentational;
- integrate visually with existing RRZE plugins without taking over their business logic.

Theme code MUST NOT:

- create substantial content functionality that should be reusable independent of the theme;
- make editorial content dependent on a specific theme for semantic meaning;
- silently rewrite user-owned content;
- register application-like REST or AJAX workflows unless explicitly approved as theme-owned presentation behavior;
- store secrets, license keys, tokens, or external service credentials;
- perform crawler-like retrieval unless explicitly approved and compliant with `RRZE-Crawler-Rules.md`;
- depend on a non-standard plugin without documenting and justifying the dependency.

If theme behavior requires durable functionality, data ownership, external service integration, or reuse across themes, implement it in a plugin.

The WordPress Theme Handbook's distinction between themes and plugins MUST be respected. Theme Check's "Plugin Territory" findings MUST be treated as release-relevant warnings or errors and either fixed or explicitly reviewed with RRZE.

---

# 6. Naming Conventions

The institutional prefix indicates responsibility and MUST NOT be chosen merely for convenience.

## 6.1 Examples and Placeholders

Examples in this standard that use names, namespaces, slugs, or prefixes such as RRZE, FAU, or UTN are placeholders only. The applicable naming convention MUST be determined before development and used consistently for the actual theme.

The AI MUST NOT invent multiple spellings of the theme slug, text domain, package name, namespace, constant prefix, repository name, or screenshot/build identifiers.

Names MUST be chosen once at project initialization and reused consistently.

## 6.2 RRZE-owned or RRZE-commissioned Themes

The prefix `RRZE` / `rrze-` is reserved for themes whose development has been commissioned, accepted, or is maintained under responsibility of RRZE.

Example:

```text
Theme Name: RRZE Example
Theme slug: rrze-example
Text domain: rrze-example
PHP namespace where required: RRZE\Example
PHP prefix where required: rrze_example_
Repository: rrze-example
```

A developer, chair, institute, project group, professor, contractor, or other FAU institution MUST NOT use the `RRZE` prefix merely because the theme is intended for use on RRZE infrastructure.

## 6.3 Themes Created by Other FAU Institutions

Themes originating from other FAU institutions SHOULD use the `FAU` / `fau-` prefix unless another approved institutional naming convention exists.

Because the central RRZE CMS operating rules reserve the prefixes `rrze-`, `fau-`, and `cms-` for coordinated use, the `FAU` / `fau-` prefix MUST be agreed with RRZE before a theme is admitted to the central RRZE CMS.

Example:

```text
Theme Name: FAU Example
Theme slug: fau-example
Text domain: fau-example
PHP namespace where required: FAU\Example
PHP prefix where required: fau_example_
Repository: fau-example
```

The repository and theme documentation MUST identify the actual responsible institution and maintainer.

## 6.4 Themes in Use at Other Higher Education Institutions

Themes in use for the Technical University of Nuremberg SHOULD use the `UTN` / `utn-` prefix unless another approved institutional naming convention exists.

---

# 7. Theme Metadata, Versioning, and README Files

Themes MUST keep metadata consistent across `style.css`, `package.json`, `readme.txt`, `README.md`, build metadata, and release artifacts where these files exist.

## 7.1 style.css Theme Header

The theme's main `style.css` MUST contain the WordPress theme header.

Recommended baseline:

```css
/*
Theme Name:        RRZE Example
Theme URI:         https://github.com/RRZE-Webteam/rrze-example
Author:            RRZE Webteam
Author URI:        https://www.wp.rrze.fau.de/
Description:       Short description of the theme.
Version:           1.0.0
Requires at least: 6.8
Tested up to:      6.9
Requires PHP:      8.2
License:           GNU General Public License Version 3
License URI:       https://www.gnu.org/licenses/gpl-3.0.html
Text Domain:       rrze-example
Tags:              full-site-editing, block-patterns, editor-style
*/
```

Adjust minimum WordPress and PHP versions according to the deployment target.

The `Text Domain` MUST match the theme slug unless WordPress or project-specific tooling requires a documented exception.

## 7.2 Versioning

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

The version number MUST have a single canonical source.

At release/build time the same version MUST be synchronized to all relevant locations, including where applicable:

- `style.css` theme header;
- `package.json`;
- `readme.txt`;
- build metadata;
- release ZIP filename if a ZIP is produced for external distribution;
- constants used by the theme;
- asset cache-busting logic.

A build or release MUST fail if inconsistent versions are detected.

## 7.3 readme.txt

`readme.txt` SHOULD follow WordPress theme readme conventions where applicable.

It SHOULD contain:

```text
=== Theme Name ===
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

`Stable tag` MUST match the released theme version if used.

## 7.4 README.md

`README.md` is the developer and repository documentation.

It SHOULD contain:

```text
# Theme name

Short description.

## Contributors

List of: Author/team, URL of author/team

## Copyright

GNU General Public License (GPL) Version 3

## Documentation

Canonical public documentation URL and end-user guidance.

## Feedback

* Canonical public documentation URL for issues and feedback
* Email contact

## Requirements

WordPress, PHP, browser, build, and Multisite requirements.

## Installation

Deployment and local development instructions.

## Configuration

Theme settings, Site Editor behavior, templates, patterns, styles, and restrictions.

## Multisite behavior

Network deployment, site activation, and site-specific behavior.

## Accessibility

Known accessibility limitations or a reference to accessibility metadata.

## External resources

Runtime third-party resources, if explicitly approved.

## Cookies and browser storage

Cookies, localStorage, and sessionStorage, if used.

## Data storage

Theme-owned options, transients, or other storage, if used.

## Hooks and APIs

Theme-provided hooks or APIs, if available.
```

The README MUST state all external dependencies and services.

If external APIs or runtime third-party resources are used, document:

- provider;
- purpose;
- transmitted data;
- authentication method;
- timeout behavior;
- failure behavior;
- privacy implications;
- whether data leaves FAU infrastructure;
- required API scopes where applicable.

If accessibility errors or problems exist, document them in `README.md` or in an `accessibility.json` file according to:

```text
https://github.com/xwolfde/Accessibility-Metadata-Format
```

The canonical public end-user documentation URL MUST be referenced from `README.md`.

## 7.5 Badge in GitHub Repository

By default, RRZE themes use a badge in the first line containing the version number, release, license, and issue information.

Example:

```text
[![Aktuelle Version](https://img.shields.io/github/package-json/v/rrze-webteam/rrze-legal/main?label=Version)](https://github.com/RRZE-Webteam/rrze-legal) [![Release Version](https://img.shields.io/github/v/release/rrze-webteam/rrze-legal?label=Release+Version)](https://github.com/rrze-webteam/rrze-legal/releases/) [![GitHub License](https://img.shields.io/github/license/rrze-webteam/rrze-legal)](https://github.com/RRZE-Webteam/rrze-legal) [![GitHub issues](https://img.shields.io/github/issues/RRZE-Webteam/rrze-legal)](https://github.com/RRZE-Webteam/rrze-legal/issues)
```

Replace the repository path with the actual canonical theme repository.

---

# 8. WordPress Blocks

The block requirements from `RRZE-WordPress-Plugin.md` chapter 32 apply to themes as well.

For themes this includes theme-owned custom blocks, block variations, block styles, block patterns, template parts, and editor integrations where they affect block-based content creation.

New theme-provided content functionality MUST use WordPress blocks or current Block Editor mechanisms. New shortcodes for content-authoring functionality are prohibited.

Classic metaboxes SHOULD NOT be introduced for new functionality on block-enabled post types unless no appropriate Block Editor mechanism exists and the reason is documented.

## 8.1 Block Category and Name

Every theme-owned block SHOULD be assigned to the `RRZE`, `FAU`, or an agreed institutional category, such as the abbreviation of another higher education institution.

The `RRZE` category is reserved for blocks developed by or commissioned and accepted by RRZE or the RRZE Webteam. External developers MUST obtain prior agreement from the RRZE Webteam before assigning a block to that category.

Blocks from other teams that are intended for institution-wide FAU use SHOULD use the `FAU` category. Blocks developed for another higher education institution SHOULD use that institution's agreed abbreviation as their category.

The block name SHOULD use a stable namespaced slug. The preferred pattern is:

```text
category-or-theme-name/block-name
```

Examples:

```text
rrze/block-name
fau/block-name
theme-name/block-name
```

If a theme provides multiple blocks, the theme slug MAY be used as the namespace. Block names are a public compatibility surface and MUST NOT be renamed casually.

## 8.2 Block Metadata, Structure, and Backwards Compatibility

New blocks SHOULD follow the current WordPress-recommended block file structure. `block.json` MUST remain the primary definition of block metadata.

Block updates MUST preserve existing editorial content. The WordPress Block Deprecation API MUST be used whenever a change affects stored block markup or block metadata compatibility. This includes:

- changes to relevant `block.json` values for static and dynamic blocks;
- changes to the `save` implementation of a static block;
- changes to attributes, attribute defaults, or serialization that affect existing content.

Static blocks MUST provide appropriate deprecated block definitions for every supported historical markup or attribute format. Dynamic blocks MUST likewise preserve compatible parsing of existing saved block comments and attributes when `block.json` or attributes change.

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

JavaScript is allowed. TypeScript is recommended.

Reusable components may be used to keep `edit.tsx` clean.

Instead of using `save.tsx`, themes SHOULD prefer dynamic rendering through PHP where this improves compatibility and maintainability. Static blocks are not recommended where frequent markup changes are expected. If `save.tsx` is used, always provide a dedicated `deprecated.tsx` when existing saved content could be affected by future changes.

## 8.3 Block Controls and Editorial Usability

Block controls MUST follow the WordPress interaction model.

Settings required to insert, fill, or quickly configure a block belong in the Block Toolbar.

Complex, secondary, or advanced settings belong in the Settings Sidebar.

The normal editorial workflow of a block SHOULD be self-explanatory without requiring separate documentation. Contextual descriptions, help text, and explanations for non-obvious settings SHOULD be provided in the Settings Sidebar at the point where they are needed.

Blocks MUST be delivered in English (`en_US`) and additionally provide German (`de_DE`) and formal German (`de_DE_formal`) translations for all user-visible strings.

## 8.4 Block Quality and Styling

Blocks MUST work without errors with the currently operated WordPress version. They MUST NOT create browser-console errors during normal editor or frontend use. Defects MUST be corrected promptly.

Editor CSS and styling adjustments SHOULD be scoped so they affect only the block that owns them. Block frontend output SHOULD NOT rely on inline `style` attributes except where a specific, documented WordPress API or accessibility requirement makes this unavoidable.

Theme-level editor CSS MUST NOT break unrelated Core blocks, plugin blocks, or administrative WordPress UI.

---

# 9. Multisite, Roles, and Configuration

Themes intended for RRZE CMS use MUST be suitable for WordPress Multisite.

A theme MUST consider:

- activation and switching on individual sites;
- network-wide deployment through the updater;
- site-specific customization;
- permissions for theme options, Customizer access, and Site Editor access;
- compatibility with large Multisite installations;
- interaction with site-specific uploads, menus, templates, and block settings.

Network-wide infrastructure values MUST NOT be hidden inside theme code. If centralized settings are required, use the approved RRZE infrastructure or a plugin.

If a theme creates options, transients, site options, files, logs, uploads, REST routes, AJAX endpoints, or comparable persistent identifiers, the naming and security requirements from `RRZE-WordPress-Plugin.md` apply analogously.

---

# 10. Accessibility and Usability

Theme frontend output, editor styles, templates, navigation, and generated UI MUST meet WCAG 2.2 AA as the general minimum.

WCAG 2.2 AAA for form input workflows is desirable where reasonable and technically practical, but AAA is not mandatory.

Themes MUST provide:

- semantic HTML;
- correct heading structure;
- keyboard-operable navigation and controls;
- visible focus indicators;
- sufficient contrast;
- accessible names for controls;
- responsive behavior without loss of content or functionality;
- accessible skip links and landmark structure where applicable;
- editor styles that do not harm accessibility in the Block Editor.

Corporate design requirements MUST NOT override accessibility requirements.

---

# 11. Security and Privacy

Themes MUST follow WordPress security best practices.

At minimum:

- validate and sanitize input;
- escape output at the point of output;
- use nonces and capability checks for state-changing actions;
- use WordPress APIs instead of direct filesystem, SQL, or HTTP shortcuts where suitable;
- avoid dynamic code execution, arbitrary includes, shell execution, and unsafe deserialization;
- do not expose internal paths, stack traces, secrets, or raw debug data.

Themes MUST NOT load executable, visual, tracking, font, media, library, stylesheet, JavaScript, or other runtime resources directly from third-party hosts or public CDNs unless explicitly approved for the specific theme and use case.

Required libraries, fonts, icons, CSS, and JavaScript MUST normally be shipped locally with the theme or obtained from WordPress Core.

Cookies, browser storage, external resources, and personal-data processing MUST be documented when used.

If a theme performs automated retrieval of external websites, feeds, APIs, files, or other web resources on behalf of an FAU organizational unit, project, or service, it MUST follow `RRZE-Crawler-Rules.md` and explicitly set a compliant HTTP `User-Agent`.

---

# 12. Code Structure and Style

Use WordPress APIs and coding standards unless this standard specifies a project-specific rule.

PHP code SHOULD be minimal and theme-appropriate. Non-trivial PHP logic MUST be organized by responsibility.

Use K&R brace style.

PHP arrow functions SHOULD NOT be used. Anonymous PHP functions SHOULD be avoided unless technically justified.

Human-authored JavaScript, TypeScript, CSS, and SCSS MUST live in source directories such as `src/`. Generated runtime assets MUST live in `build/` or another documented build directory.

Generated files MUST NOT be edited manually.

Theme-owned CSS MUST be scoped so it does not unintentionally affect WordPress admin UI, other themes, or plugin UIs. Block editor styles MUST be scoped to theme/editor behavior and must not break unrelated blocks.

---

# 13. Assets, Dependencies, and Build

JavaScript and CSS MUST only be loaded where required.

Use WordPress-provided scripts and packages where appropriate. Do not bundle another copy of libraries already provided by WordPress, such as jQuery or React for editor behavior.

`@wordpress/scripts` (`wp-scripts`) is the preferred build tool for WordPress projects. It MUST be installed and used for themes that build WordPress blocks, Block Editor extensions, or other WordPress-specific JavaScript that relies on the WordPress build pipeline.

Production CSS and JavaScript MUST be minified and free of source maps.

Development assets MAY be unminified and MAY include source maps.

SCSS/SASS or another documented preprocessing workflow SHOULD be used for non-trivial CSS. Generated vendor prefixes SHOULD be produced by tooling such as Autoprefixer rather than hand-maintained.

Node/npm dependencies require a committed `package-lock.json`.

`package.json` SHOULD be the canonical source for:

- theme version;
- WordPress compatibility;
- PHP compatibility;
- repository metadata;
- author/maintainer metadata;
- build scripts.

Duplicated metadata in `style.css`, `readme.txt`, documentation, and package metadata MUST be synchronized by tooling where practical.

---

# 14. Repository and Deployment

Every theme MUST be maintained in Git.

GitHub or GitLab MUST be used as the canonical repository.

The repository MUST contain at least:

```text
theme-slug/
├── .gitignore
├── LICENSE
├── README.md
├── readme.txt
├── package.json
├── style.css
├── functions.php
├── theme.json
├── templates/
├── parts/
├── patterns/
├── styles/
├── src/
├── build/
└── languages/
```

Additional directories MAY be added when justified, for example:

```text
assets/
inc/
includes/
doc/
tests/
.github/
vendor/
```

The `main` branch MUST contain a directly executable, production-ready theme. Production deployment MUST NOT require a build on the production server.

Required production assets MUST be committed to `main`.

Local, IDE, temporary, credential, test-output, and development-only artifacts MUST be excluded through `.gitignore`.

Manual ZIP exchange is not the standard deployment workflow for RRZE-operated installations.

---

# 15. Documentation

Every production theme MUST provide maintained web-based end-user documentation.

Documentation MUST explain:

- purpose and supported use cases;
- visual and editorial workflows;
- Block Editor and Site Editor usage;
- templates, template parts, patterns, style variations, and relevant restrictions;
- configuration and permissions;
- dependencies on plugins or RRZE infrastructure;
- accessibility considerations;
- external resources, cookies, storage, and privacy implications where applicable;
- support and maintainer contact.

The canonical documentation URL MUST be referenced from `README.md`.

Repository-only developer documentation is not sufficient as end-user documentation.

---

# 16. Testing and Release

Before a theme is considered ready for production, perform and record applicable checks:

- activation and switching in Single Site and Multisite;
- Block Editor and Site Editor workflows;
- frontend output with representative content;
- templates, template parts, patterns, navigation, menus, widgets where applicable, and responsive behavior;
- keyboard operation, visible focus, accessible names, heading structure, landmarks, and contrast;
- PHP syntax and project PHP checks;
- Node/npm installation and production build;
- JavaScript and CSS linting;
- Theme Check without blocking errors;
- browser console without errors during normal editor and frontend use;
- compatibility with the current RRZE reference themes/plugins where relevant;
- no custom page builder, plugin installer, or plugin-domain functionality;
- block names, `block.json`, block deprecations, block translations, and block styling where theme-owned blocks exist;
- README, `readme.txt`, `style.css`, `package.json`, and compatibility metadata consistency;
- direct executability of `main`.

## 16.1 Theme Check

Before a theme is considered ready for production, it MUST be tested with the official WordPress **Theme Check** plugin.

Reference:

```text
https://wordpress.org/plugins/theme-check/
```

The production candidate MUST pass Theme Check without blocking errors.

All checks relevant to the theme MUST be executed against the same code state that is intended to be promoted to `main`.

Warnings and recommendations SHOULD also be reviewed and either corrected or explicitly documented with a technically justified reason.

Unresolved Theme Check findings concerning security, plugin territory, text domain, stylesheet metadata, screenshot, CDN usage, deprecated APIs, or required WordPress theme behavior block production release unless RRZE explicitly accepts the finding as non-blocking for the specific theme.

The release checklist MUST record that Theme Check was executed successfully.

Separate executed tests, static review, manual review, and untested items in the result.

Do not claim that a test, accessibility check, Multisite check, or Theme Check was performed unless it actually ran.

---

# 17. Definition of Done

A theme is complete only when all applicable items are satisfied:

- the theme remains in the presentation domain;
- no plugin-domain functionality was introduced;
- it is a Block Editor theme unless an explicit temporary exception exists;
- no custom page builder or plugin installer is included or required;
- WordPress reference standards and RRZE admission rules were reviewed;
- block rules from `RRZE-WordPress-Plugin.md` chapter 32 and chapter 8 of this document were followed where applicable;
- accessibility was reviewed against WCAG 2.2 AA;
- assets are local, scoped, minified for production, and loaded only where needed;
- security, escaping, sanitization, nonces, and capabilities were reviewed;
- Multisite behavior was considered and tested where possible;
- Theme Check was executed or explicitly reported as unavailable;
- documentation and support ownership exist;
- `README.md`, `readme.txt`, `style.css`, `package.json`, version metadata, and compatibility metadata are consistent;
- `main` is directly executable as the production theme.
