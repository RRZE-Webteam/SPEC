# FAU Crawler User-Agent Specification --- LLM/Agent Project Rules

**Specification version:** 1.5\
**Scope:** All software that performs automated HTTP retrieval or
crawling on behalf of an FAU organizational unit, project or service.

This file is intended to be consumed as a normative project instruction
by LLMs, coding agents and automated development systems.

## 1. Normative requirement

Any crawler covered by this specification **MUST** identify itself using
this HTTP `User-Agent` schema:

``` text
FAU-<ORG>-<BOT>/<VERSION> (+<INFO-URL>;mailto:<CONTACT>)
```

Preferred version syntax:

``` text
FAU-<ORG>-<BOT>/<MAJOR.MINOR> (+<INFO-URL>;mailto:<CONTACT>)
```

Canonical example:

``` http
User-Agent: FAU-RRZE-Legalcheck/1.0 (+https://www.rrze.fau.de/bots/legalcheck/;mailto:webmaster@fau.de)
```

The syntax above is canonical. New implementations **SHOULD** generate
the separator exactly as `;mailto:` without whitespace.

For compatibility with earlier implementations, the form `; mailto:`
with a single space after the semicolon is also valid. Parsers and
validators **MUST NOT** reject an otherwise valid FAU crawler User-Agent
solely because it uses this legacy-compatible whitespace form.

Generators **SHOULD** emit the canonical `;mailto:` form.

## 2. Field requirements

### `FAU`

-   **MUST** be exactly `FAU`.
-   **MUST** be the first component.
-   **MUST NOT** be used for systems that are not operated under
    responsibility of an FAU organizational unit or FAU project.
-   **MUST NOT** be interpreted as authentication.

### `<ORG>`

-   **MUST** identify the responsible FAU organizational unit.
-   **SHOULD** use an established organizational abbreviation.
-   **MUST NOT** contain whitespace.
-   **SHOULD** match `[A-Za-z0-9_-]+`.
-   **SHOULD** remain stable for the lifetime of the crawler.

Example:

``` text
RRZE
```

### `<BOT>`

-   **MUST** uniquely identify the crawler/service within the
    responsible organization.
-   **MUST NOT** contain whitespace.
-   **SHOULD** match `[A-Za-z0-9_-]+`.
-   **SHOULD** remain stable across releases.
-   **MAY** use generic functional identifiers such as `Bot`, `Crawler`
    or `Scraper` as the complete bot name.
-   **SHOULD NOT** use identifiers that merely name the implementation
    technology, programming language, runtime, HTTP client library or
    generic retrieval tool, such as `Python`, `PHP`, `Java`, `curl`,
    `wget`, `python-requests` or `Guzzle`, as the complete bot name.

Example:

``` text
Legalcheck
```

### `<VERSION>`

-   **MUST** be present.
-   **MUST** be numeric and dot-separated.
-   **SHOULD** contain at least `MAJOR.MINOR`.
-   **MAY** contain additional numeric components.
-   **SHOULD** change when crawler behavior changes materially.

Valid examples:

``` text
1.0
1.4
2.0
2.3.1
```

### `<INFO-URL>`

-   **SHOULD** be present for production crawlers.
-   **SHOULD** use HTTPS.
-   **SHOULD** be a stable public URL.
-   The target page **SHOULD** identify the crawler, responsible
    organization, purpose, contact information and crawl behavior.
-   The target page **SHOULD** document relevant rate limits and source
    networks where appropriate.

### `<CONTACT>`

-   **MUST** be present.
-   **MUST** be represented as `mailto:<address>`.
-   **MUST** be a monitored contact address.
-   **SHOULD** be a functional/team mailbox rather than a personal
    mailbox.

## 3. Construction algorithm

When implementing a crawler:

1.  Determine the responsible FAU organization abbreviation as `ORG`.
2.  Choose a stable and unique crawler name as `BOT`.
3.  Set the crawler implementation/behavior version as `VERSION`.
4.  Set a stable HTTPS documentation URL as `INFO-URL`.
5.  Set a monitored contact mailbox as `CONTACT`.
6.  Construct exactly:

``` text
FAU-{ORG}-{BOT}/{VERSION} (+{INFO-URL};mailto:{CONTACT})
```

7.  Configure the HTTP client to send this value in the `User-Agent`
    header for every crawler request.
8.  Verify that the HTTP library does not replace it with its default
    User-Agent.

## 4. Request behavior

The specified User-Agent **SHOULD** be sent for all crawler-originated
HTTP requests, including:

-   HTML pages,
-   WordPress REST API requests,
-   other HTTP APIs,
-   XML sitemaps,
-   RSS/Atom feeds,
-   media/files,
-   requests following redirects.

Do **NOT** intentionally switch to browser impersonation such as
`Mozilla/5.0` to conceal crawler identity.

## 5. Crawl Discovery and Politeness

### 5.1 robots.txt

Before systematic crawling of an origin, the crawler **MUST** retrieve
and evaluate the applicable `robots.txt`.

The crawler:

-   **MUST** respect applicable `User-agent`, `Allow`, and `Disallow`
    directives unless an explicitly authorized and documented exception
    exists for the concrete deployment.
-   **MUST NOT** treat FAU affiliation as an implicit exemption from
    `robots.txt`.
-   **SHOULD** cache `robots.txt` for a reasonable period instead of
    retrieving it before every request.
-   **MUST** re-evaluate applicable rules when a refreshed `robots.txt`
    is retrieved.

The stable crawler product identifier is:

``` text
FAU-<ORG>-<BOT>
```

Example `robots.txt` targeting:

``` text
User-agent: FAU-RRZE-Legalcheck
Disallow: /intern/
```

`ORG` and `BOT` **MUST NOT** be randomly altered between requests or
executions.

### 5.2 Sitemap discovery

The crawler **SHOULD** inspect `Sitemap:` directives declared in
`robots.txt`.

If one or more sitemaps are declared:

-   the crawler **SHOULD** use them as the preferred source for URL
    discovery;
-   the crawler **SHOULD** avoid unnecessary recursive link discovery
    when the sitemap already provides the required URL inventory;
-   the crawler **SHOULD** support sitemap index files and recursively
    process referenced sitemap files;
-   URLs discovered through a sitemap **MUST** remain subject to
    applicable `robots.txt` rules.

A sitemap declaration does **NOT** imply permission to retrieve a URL
that is otherwise disallowed for the crawler.

### 5.3 Machine-readable site metadata and AI discovery

Before or during systematic crawling, the crawler **SHOULD** check for
machine-readable site metadata and content representations relevant to
discovery or efficient content retrieval.

The preferred discovery and retrieval order is:

1.  `/robots.txt`
2.  sitemaps and sitemap indexes declared by `robots.txt`
3.  `/llms.txt` and, where referenced or available, `/llms-full.txt`
4.  suitable APIs or explicitly provided structured machine-readable
    content representations
5.  regular HTML crawling

This order expresses a preference for efficient discovery and retrieval.
It does **NOT** override access restrictions or crawler policies.

The crawler:

-   **MUST** treat applicable `robots.txt` rules, HTTP access controls,
    authentication requirements and target-specific rate limits as
    authoritative constraints.
-   **MUST NOT** interpret `/llms.txt`, `/llms-full.txt`, an API, a
    sitemap or another metadata resource as permission to bypass an
    applicable restriction.
-   **SHOULD** use `/llms.txt` as an AI-oriented discovery and guidance
    resource when available and relevant.
-   **MAY** use `/llms-full.txt` when it is referenced, available and
    useful for the crawler's purpose.
-   **MUST NOT** assume that `llms.txt` or `llms-full.txt` is
    universally supported or authoritative; their absence **MUST NOT**
    be treated as an error.
-   **SHOULD** prefer an explicitly offered API or structured
    machine-readable representation when it provides the required
    content with less redundant retrieval than HTML crawling.
-   **SHOULD** avoid downloading and parsing redundant HTML pages when
    an appropriate machine-readable representation already provides the
    content required for the crawler's task.
-   **MUST** apply the same per-origin/host request-rate limits to
    metadata, sitemap, API, structured-content and HTML requests.

#### HTML-advertised machine-readable representations

When an HTML document has been retrieved, the crawler **SHOULD** inspect
its document metadata for links to alternative or machine-readable
representations of the same resource.

This includes, where applicable:

-   HTML `<link>` elements such as `rel="alternate"` that advertise
    JSON, API or other structured representations;
-   CMS-specific discovery links to REST APIs or JSON representations;
-   WordPress REST API discovery information exposed in HTML metadata;
-   other explicitly linked machine-readable representations whose media
    type or relation indicates that they represent the current resource.

When such a representation is available:

-   the crawler **SHOULD** use it when it provides the content and
    metadata required for the crawler's task more directly or
    efficiently than parsing the HTML representation;
-   the crawler **SHOULD** avoid redundant retrieval or processing of
    equivalent HTML content once a suitable structured representation
    has been obtained;
-   the crawler **MUST NOT** assume that every advertised API or JSON
    endpoint contains a complete representation of the HTML resource;
-   the crawler **SHOULD** verify that the structured representation
    contains the information required for its task before treating it as
    a replacement for HTML processing;
-   the linked resource **MUST** remain subject to applicable
    `robots.txt` rules, HTTP access controls, authentication
    requirements and per-origin/host request-rate limits.

HTML metadata discovery is a mechanism for locating a preferred
machine-readable representation after an HTML document has been
encountered. It does **NOT** change the general preference for an
already-known API or structured representation over HTML crawling.

### 5.4 `.well-known` resources

The crawler **MAY** use explicitly supported and relevant resources
below `/.well-known/` when required for its defined purpose.

The crawler:

-   **MUST NOT** enumerate `/.well-known/` indiscriminately.
-   **MUST NOT** recursively crawl the `/.well-known/` namespace.
-   **MUST NOT** request arbitrary `/.well-known/` resource names merely
    to test for their existence.
-   **SHOULD** request a `/.well-known/` resource only when the crawler
    explicitly supports the corresponding specification or when the
    resource has been referenced by another authoritative site resource.
-   **MUST** continue to apply applicable `robots.txt`, HTTP
    access-control and rate-limit rules to such requests.

The existence of the `/.well-known/` namespace does not by itself
indicate that it contains content relevant to crawling or AI processing.

### 5.5 Request rate

A crawler **MUST** limit its request rate independently for each target
origin/host.

Unless a lower target-specific limit applies:

-   the crawler **MUST NOT** initiate more than **3 requests per second
    per origin/host**;
-   this limit **MUST** apply to the crawler as a whole, including all
    concurrent workers, threads, processes and asynchronous requests;
-   parallelization **MUST NOT** be used to circumvent the
    per-origin/host limit;
-   the crawler **MAY** use a lower request rate;
-   a rate above 3 requests per second **MUST** require explicit
    configuration and authorization for the affected target service.

Implementations **SHOULD** use a shared per-origin/host rate limiter
when concurrency is used.

### 5.6 Server overload and backoff

The crawler **MUST** react to server-side rate limiting and temporary
unavailability.

-   A `Retry-After` response header **MUST** be respected when present.
-   On HTTP `429 Too Many Requests`, the crawler **MUST** reduce or
    suspend requests to the affected origin/host and **SHOULD** apply
    exponential backoff.
-   On HTTP `503 Service Unavailable`, the crawler **MUST** avoid
    immediate aggressive retries and **SHOULD** apply exponential
    backoff.
-   A target-specific restriction that is stricter than the FAU default
    **MUST** take precedence.
-   Retry logic **MUST NOT** cause the effective request rate to exceed
    the configured per-origin/host limit.

## 6. Cookies, Sessions and Client State

Crawler implementations **SHOULD** be stateless by default.

The crawler:

-   **SHOULD NOT** persist or return cookies unless they are technically
    required for the explicitly defined crawling purpose.
-   **SHOULD NOT** store or return cookies whose purpose is tracking,
    analytics, advertising, personalization or consent management.
-   **SHOULD NOT** interact with cookie-consent dialogs intended for
    human users.
-   **SHOULD NOT** signal consent to optional tracking, analytics,
    advertising or personalization solely to obtain or crawl content.
-   **MUST NOT** use cookies or session state to bypass authentication
    requirements, access restrictions, paywalls, crawler restrictions,
    security controls or anti-bot protections.
-   **MAY** use technically required cookies when they are necessary for
    the explicitly authorized crawling function.
-   **SHOULD** scope technically required cookies to the relevant origin
    and session.
-   **SHOULD** retain technically required cookies only for as long as
    necessary for the crawling operation.
-   **MUST NOT** transfer cookies received from one origin to an
    unrelated origin.

If a target service requires authenticated or stateful access for an
authorized crawler, that access **SHOULD** be explicitly configured and
documented rather than obtained by simulating human consent or browser
behavior.

## 7. Security constraints

The `User-Agent` header is client-controlled and can be spoofed.

Therefore:

-   **MUST NOT** use an `FAU-*` User-Agent as authentication.
-   **MUST NOT** grant access to protected resources based only on the
    User-Agent.
-   **MUST NOT** bypass authentication based only on the User-Agent.
-   **MUST NOT** disable security controls based only on the User-Agent.
-   **MUST NOT** whitelist a crawler solely by User-Agent when the
    whitelist grants additional privileges.
-   **MUST NOT** assume that a request originates from FAU merely
    because the User-Agent starts with `FAU-`.

Where verified identity is required, use an additional verifiable
mechanism such as authentication or controlled source IP
addresses/networks.

## 8. Validation

The product/version prefix **SHOULD** satisfy:

``` regex
^FAU-[A-Za-z0-9_-]+-[A-Za-z0-9_-]+/[0-9]+(?:\.[0-9]+)+
```

A validator **SHOULD** additionally verify:

-   `INFO-URL` is a syntactically valid HTTPS URL;
-   `CONTACT` contains a syntactically valid email address;
-   the separator is either the canonical `;mailto:` form or the
    legacy-compatible `; mailto:` form;
-   generators use the canonical `;mailto:` form for newly generated
    User-Agent values;
-   no required component is empty;
-   `ORG` and `BOT` contain no whitespace;
-   version contains at least two numeric components for new
    implementations.

## 9. Valid examples

``` text
FAU-RRZE-Legalcheck/1.0 (+https://www.rrze.fau.de/bots/legalcheck/;mailto:webmaster@fau.de)
FAU-RRZE-SearchBot/2.3 (+https://www.rrze.fau.de/bots/searchbot/;mailto:webmaster@fau.de)
FAU-UB-CatalogCrawler/1.1 (+https://www.ub.fau.de/bots/catalogcrawler/;mailto:webmaster@ub.fau.de)
```

## 10. Invalid or non-compliant examples

``` text
Mozilla/5.0
python-requests/2.32
curl/8.0
Crawler
FAU-Bot
FAU-RRZE-Legalcheck
FAU-RRZE-Legalcheck/1.0
```

Reasons include missing organizational identity, missing unique bot
name, missing version, missing required contact information, or
deviation from the canonical separator syntax.

## 11. Implementation acceptance criteria

An implementation is compliant only if all applicable criteria below are
met:

-   [ ] User-Agent starts with `FAU-`.
-   [ ] `ORG` identifies the responsible FAU organization.
-   [ ] `BOT` uniquely and stably identifies the crawler.
-   [ ] A version is supplied.
-   [ ] A monitored `mailto:` contact is supplied.
-   [ ] A stable information URL is supplied for production deployment.
-   [ ] Newly generated User-Agent values use the canonical `;mailto:`
    separator.
-   [ ] Parsers and validators also accept the legacy-compatible
    `; mailto:` separator.
-   [ ] The configured User-Agent is actually transmitted on crawler
    requests.
-   [ ] Applicable `robots.txt` rules are honored.
-   [ ] `Sitemap:` directives in `robots.txt` are evaluated and used for
    URL discovery where applicable.
-   [ ] Sitemap index files are supported where sitemap discovery is
    implemented.
-   [ ] Machine-readable discovery follows the preferred order:
    `robots.txt` → declared sitemaps → `llms.txt`/`llms-full.txt` →
    suitable APIs/structured representations → HTML crawling.
-   [ ] AI-specific metadata never overrides `robots.txt`, HTTP access
    controls, authentication requirements or rate limits.
-   [ ] Suitable machine-readable content representations are preferred
    over redundant HTML crawling when they fully satisfy the crawler's
    purpose.
-   [ ] Retrieved HTML documents are inspected for advertised
    machine-readable alternatives, including relevant `rel` links and
    CMS-specific API/JSON discovery metadata.
-   [ ] Advertised structured representations are preferred over
    redundant HTML processing when they provide the content required for
    the crawler's task.
-   [ ] `/.well-known/` is not enumerated or recursively crawled; only
    explicitly supported or referenced resources are requested.
-   [ ] The crawler initiates no more than 3 requests per second per
    origin/host unless explicitly authorized otherwise.
-   [ ] Concurrent workers share the same per-origin/host rate limit.
-   [ ] `Retry-After` is respected.
-   [ ] HTTP 429 and 503 responses trigger appropriate rate
    reduction/backoff behavior.
-   [ ] The crawler is stateless by default and retains cookies only
    where technically required for its explicitly defined purpose.
-   [ ] Tracking, analytics, advertising, personalization and consent
    cookies are not intentionally persisted or returned.
-   [ ] Cookie-consent dialogs are not used to simulate human consent.
-   [ ] Cookies or session state are not used to bypass authentication,
    access restrictions or security/anti-bot controls.
-   [ ] No authentication or privileged trust decision relies solely on
    the User-Agent.

## 12. Canonical project rule

When generating, modifying or reviewing crawler code for an FAU project,
**do not accept a library default User-Agent**. Explicitly configure:

``` text
FAU-<ORG>-<BOT>/<VERSION> (+<INFO-URL>;mailto:<CONTACT>)
```

If one or more required project-specific values are unknown, **do not
invent them**. Mark the value as requiring project configuration or
request the missing information.
