# CLAUDE.md — SBPN Website Rebuild

## 1. Project Mission

Rebuild the public website for the **Syrian British Professionals Network (SBPN)** at **https://sbpn.org** as a modern, professional, fast, secure, bilingual English/Arabic website with a complete content-management and administration system.

This is a production rebuild, not a prototype.

The new site must:

1. Preserve and migrate the useful structure, content, media, people, sectors, articles/news, initiative links, legal content, and forms from the existing `sbpn.org` website.
2. Present the same information architecture and equivalent content in **English and Arabic**.
3. Provide an excellent Arabic **RTL** experience and English **LTR** experience.
4. Include a powerful `/admin` dashboard that lets authorised SBPN staff manage the whole site without editing code.
5. Preserve SEO value by retaining important existing URLs or creating explicit permanent redirects.
6. Be designed for production deployment on a normal PHP/Plesk hosting environment.
7. Be accessible, responsive, performant, maintainable, and secure.
8. Use real migrated SBPN content. Do not use lorem ipsum or generic placeholder content in the finished build.

The public website is the primary deliverable. The admin dashboard is a core part of the deliverable, not an optional extra.

---

## 2. Operating Rules for Claude Code

### 2.1 Work autonomously

Do not stop after scaffolding. Continue through database design, frontend, admin resources, migration tooling, tests, seed data, documentation, and deployment preparation.

When information is missing, make the safest conventional implementation choice that is easy for an administrator to change later.

Do not ask for confirmation for routine implementation details.

### 2.2 Inspect before changing

Before modifying an existing repository:

- inspect the full directory structure;
- inspect `composer.json`, `package.json`, `.env.example`, routes, migrations, models, tests, and deployment files;
- identify existing conventions and reuse them where sensible;
- never destroy existing code or data merely to simplify implementation.

### 2.3 Use official AI development guidance

For Laravel work, fetch and follow the current Laravel agent guidance:

`https://laravel.com/for/agents`

Install Laravel Boost as a development dependency and configure it for the coding agent:

```bash
composer require laravel/boost --dev
php artisan boost:install
```

When prompted by Boost, enable the applicable Laravel and Filament guidance.

Filament's official AI guidance should also be treated as authoritative for Filament implementation patterns:

`https://filamentphp.com/docs/5.x/introduction/ai`

Use documentation search/tools provided by Boost when framework behaviour or package APIs are uncertain.

### 2.4 No unfinished implementation

Do not leave:

- `TODO` placeholders for required functionality;
- dummy buttons;
- fake analytics;
- fake form submissions;
- inaccessible controls;
- pages with copied English text under the Arabic locale;
- empty admin resources;
- temporary credentials in source control;
- default Laravel or Filament demo content exposed publicly.

---

## 3. Required Technology Stack

Use the following production-oriented stack unless the existing repository already has an equivalent compatible architecture.

### Backend

- **Laravel 13.x**, latest compatible patch release
- **PHP 8.3+**
- Eloquent ORM
- Laravel validation, authorization, queues, notifications, scheduler, cache, filesystem, mail, rate limiting, and security features

### Database

Primary target:

- **MariaDB / MySQL**, because the intended deployment environment is compatible with standard Plesk PHP hosting.

Do not require PostgreSQL-specific functionality.

### Admin CMS

- **Filament 5.x**
- Admin route: `/admin`
- Use Filament resources, forms, tables, widgets, notifications, and custom pages.

### Public frontend

Prefer:

- Laravel Blade
- Tailwind CSS
- Alpine.js only where interaction is required
- Vite for asset compilation

Do **not** make the production website require a persistent Node.js server.

Node/npm or Bun may be used at build time for Vite/Tailwind assets.

### Recommended packages

Use packages only after verifying compatibility with the installed Laravel/Filament versions.

Preferred packages where useful:

- `spatie/laravel-permission` — admin roles and granular permissions
- `spatie/laravel-translatable` — bilingual model attributes stored as JSON

Prefer native Laravel/Filament capability instead of adding unnecessary packages.

Avoid abandoned, unmaintained, low-quality, or duplicate packages.

---

## 4. Existing Website — Source of Truth for Migration

The existing public site is:

`https://sbpn.org`

Claude must inspect and migrate the current website before replacing it.

### Current English public structure to preserve

At minimum, the current site exposes:

- Home — `/`
- Who We Are
  - About Us — `/about-us/`
  - Teams — `/teams/`
- Join Us — `/join-us/`
- FAQ — `/faq/`
- Sectors — `/sbpn-sectors/`
- Initiatives
  - TECH50K — external/subdomain link to `https://tech50k.sbpn.org`
- News — `/news/`
- Contact Us — `/contacts/`
- Privacy Policy — `/privacy/`
- Terms & Conditions — `/tc/`
- Arabic site — under `/ar/...`

There are also current posts/articles and individual person/profile pages that must be included in the migration inventory.

### Current content themes to retain

The site currently contains content covering, among other things:

- SBPN's identity as an independent non-profit professional network;
- Companies House registration information;
- mission, vision, objectives, and values;
- global Syrian professional network;
- education, professional development and knowledge transfer;
- research and consultancy;
- rebuilding/reconstruction and sustainable development;
- founders and leadership;
- departmental/sector leadership;
- SBPN sectors;
- joining/membership;
- FAQs;
- initiatives such as TECH50K;
- news, announcements, articles and research-oriented posts;
- contact information;
- privacy and membership terms.

### Important bilingual migration rule

The current English and Arabic sites do **not** always contain perfectly equivalent navigation or text.

The new site must fix this.

For every public content entity:

- maintain an English version;
- maintain an Arabic version;
- make both versions structurally equivalent;
- link translations directly to one another;
- prevent accidental publishing of required content in only one language.

If migrated English and Arabic text differ materially:

1. preserve both original versions in migration staging/history;
2. mark the item `needs_translation_review`;
3. do not silently discard either version;
4. make it clear in the admin dashboard which language needs review;
5. before final public launch, both language versions must be complete.

For ordinary non-legal copy, a careful draft translation can be produced during migration and flagged for review.

For legal/privacy/terms content, do not silently rely on unreviewed machine translation. Preserve existing official wording and clearly flag missing legal translations for administrator review.

---

## 5. Migration Safety

The current production site must remain untouched until the replacement has passed acceptance tests.

Before deployment:

- take a full backup of the current website files;
- export the current database;
- capture the current XML sitemap if available;
- capture current page/post slugs;
- capture current page titles and metadata where available;
- capture current media URLs;
- capture current redirects if available;
- capture current forms and fields;
- capture all current team/person profile URLs;
- capture article dates, authors, categories, images, and slugs.

Develop a repeatable migration/import process instead of manually copy-pasting everything into code.

Where possible, inspect WordPress APIs such as:

- `/wp-json/wp/v2/...`
- sitemap indexes
- public HTML only as a fallback

Store migration snapshots under a non-public project path such as:

`storage/app/migration/`

Do not expose raw migration files through the public web root.

---

## 6. Target Public Information Architecture

English and Arabic must expose the same top-level structure.

Recommended main navigation:

1. Home
2. Who We Are
   - About SBPN
   - Our Team
3. Sectors
4. Initiatives
5. News & Insights
6. FAQ
7. Join SBPN
8. Contact
9. Language switcher

Do not add unnecessary top-level navigation items merely because the CMS supports them.

### English URL policy

Preserve existing English routes where practical to protect existing links and SEO.

Examples:

- `/`
- `/about-us`
- `/teams`
- `/join-us`
- `/faq`
- `/sbpn-sectors`
- `/news`
- `/contacts`
- `/privacy`
- `/tc`

### Arabic URL policy

Arabic must live under `/ar`.

Use clean stable Arabic routes and maintain explicit mappings to the corresponding English content.

All old Arabic URLs must be evaluated. If a route changes, add a **301 redirect** to its new canonical Arabic URL.

Do not allow old malformed/encoded Arabic slugs to become broken links.

### Language switcher behaviour

The language switcher must go to the translated equivalent of the current page, not merely to the other language's homepage.

Example:

- English About -> Arabic About
- Arabic sector page -> same sector in English
- English news article -> Arabic version of the same article

If a translation is deliberately unavailable, show a graceful message and fallback choice rather than a 404.

---

## 7. Public Page Requirements

### 7.1 Home

Build a modern institutional homepage with manageable sections.

Minimum section capability:

- Hero
- Primary CTA: Join SBPN
- Secondary CTA as configured by admin
- Short institutional introduction
- Global professional network/value proposition
- Research & consultancy/value proposition
- Education and workforce development/value proposition
- Mission
- Vision
- Core values
- Key sectors preview
- Initiatives preview
- Latest news/insights
- Impact/statistics block, if enabled and backed by real admin-managed values
- Partner/supporter logos, if populated
- Final CTA

All sections must be individually manageable from the CMS.

### 7.2 About SBPN

Must support:

- Who We Are
- registration/company information
- mission
- vision
- objectives
- values
- organisation story/background
- CTA to join
- optional governance/organisation diagram
- optional downloadable documents

### 7.3 Team

The existing team page must be rebuilt as data-driven content, not hard-coded HTML.

Support groups such as:

- Founders
- Board / Executive Team
- Sector/Department Heads
- Deputy Heads
- Staff/Volunteers
- Initiative-specific leadership if later required

Each person record should support:

- full name EN/AR
- honorific/title EN/AR
- role EN/AR
- biography EN/AR
- portrait
- display order
- group/team
- linked sector(s)
- LinkedIn URL optional
- other approved professional/social links optional
- status: active/inactive
- featured flag
- slug EN/AR
- SEO fields

Use accessible person cards and dedicated profile pages where the migrated site already has profile pages or where an administrator enables them.

### 7.4 Sectors

Build a database-driven sectors directory.

Each sector must support:

- name EN/AR
- short description EN/AR
- full content EN/AR
- icon or image
- colour/accent token optional
- slug EN/AR
- head/deputy relationships to People
- related news/articles
- related initiatives
- display order
- active/inactive status
- SEO metadata

The system must allow sectors to be added, renamed, reordered, hidden, or archived from the admin dashboard.

### 7.5 Initiatives

Create a manageable initiatives content type.

TECH50K must be represented as the existing SBPN initiative and link to:

`https://tech50k.sbpn.org`

Each initiative should support:

- name EN/AR
- strapline EN/AR
- summary EN/AR
- content EN/AR
- hero/thumbnail image
- internal page OR external URL
- start/end dates optional
- status: planned / active / completed / archived
- related sectors
- related people
- featured flag
- CTA label EN/AR
- CTA URL
- SEO metadata

### 7.6 News & Insights

Use one robust content model with a type/category field rather than several duplicate systems.

Supported editorial types may include:

- News
- Announcement
- Article
- Research / Insight
- Event
- Publication

Each post must support:

- title EN/AR
- excerpt EN/AR
- body EN/AR
- slug EN/AR
- featured image
- gallery/media where required
- author/person or editorial author
- publication date
- scheduled publication
- categories
- tags
- related sector(s)
- related initiative(s)
- related content
- downloadable attachments
- SEO metadata
- social share image
- draft/review/published/archived state

Public listing must provide:

- pagination
- type/category filters
- optional sector filter
- search
- useful empty states

### 7.7 FAQ

Create a manageable FAQ system.

Each FAQ item:

- question EN/AR
- answer EN/AR
- category
- display order
- active/inactive

FAQ page should support accessible accordion interaction.

Add `FAQPage` structured data only when the visible content and current search-engine guidance make it appropriate.

### 7.8 Join SBPN

Rebuild the current membership application as a secure, professional form.

Initial fields should preserve the currently useful application data, including as applicable:

- Full name
- Email
- Phone / WhatsApp number
- Syrian nationality question
- Other nationality question
- Country of residence
- Address fields if still required by SBPN policy
- Sector/specialty
- Other specialty
- Education level
- Additional education detail
- CV/qualification upload
- Identity/proof upload if still required
- consent to privacy policy and membership terms

Do not hard-code field labels in one language. The public form must be fully bilingual.

Admin must be able to manage application statuses:

- New
- Under Review
- More Information Required
- Approved
- Rejected
- Withdrawn
- Archived

Support:

- internal notes
- assignment to an administrator
- application timeline/history
- export to CSV/XLSX if required
- filters/search
- safe email notifications
- configurable acknowledgement message/email

Sensitive uploaded documents must be stored on a **private** filesystem/disk and never be accessible using a predictable public URL.

Use temporary authorised download responses or signed links.

Never send sensitive identity documents as normal email attachments.

Record access to sensitive application files if practical.

### 7.9 Contact

Provide a bilingual contact page and managed contact form.

Fields:

- name
- email
- sector/topic
- message
- consent if needed

Store submissions in the database and notify configured recipients.

Admin should support:

- status
- assignment
- notes
- reply tracking indicator
- spam flag
- archive

### 7.10 Privacy Policy and Terms

Keep public legal pages manageable in the CMS but restrict editing permissions to appropriate roles.

Preserve the current source documents during migration.

Include:

- revision/effective date
- version history
- publication status
- EN/AR content

Do not rewrite legal terms automatically without explicit instruction from an authorised user.

---

## 8. CMS / Admin Dashboard

The `/admin` experience must be professional enough for routine SBPN operation.

### 8.1 Dashboard home

Show useful widgets such as:

- new membership applications
- applications awaiting review
- new contact enquiries
- recent content changes
- drafts awaiting review
- content missing Arabic translation
- content missing English translation
- scheduled posts
- recently published posts
- total active people
- total active sectors
- total initiatives
- broken/flagged links if link-checking is implemented

Do not show vanity metrics unless backed by real data.

### 8.2 Admin navigation groups

Recommended groups:

**Content**
- Pages
- News & Insights
- FAQs
- Initiatives

**Organisation**
- People / Team
- Sectors
- Partners

**Applications & Enquiries**
- Membership Applications
- Contact Enquiries
- Newsletter Subscribers, only if enabled

**Media**
- Media Library
- Documents

**Website**
- Menus
- Redirects
- Global Settings
- Homepage Settings / Page Builder
- Footer
- SEO Defaults
- Social Links

**Administration**
- Users
- Roles & Permissions
- Audit Log
- System / Health summary

### 8.3 Bilingual editing experience

For all translatable resources:

- use clear English and Arabic tabs or side-by-side sections;
- display translation completeness badges;
- enforce RTL input direction for Arabic rich-text fields;
- show validation messages clearly;
- show a `Translation Review Required` flag;
- allow authorised translator/editor workflow.

For required public bilingual content, block normal publishing when one language is missing.

Super Admin may override only with an explicit warning and audit entry.

### 8.4 Content workflow

Implement:

- Draft
- In Review
- Scheduled
- Published
- Archived

Support:

- created by
- updated by
- published by
- timestamps
- optional reviewer
- publication date/time
- scheduled publication
- revision history for important editorial content

Use soft deletes for significant CMS records where appropriate.

### 8.5 Preview

Editors must be able to preview draft content in both English and Arabic before publishing.

Preview URLs must be protected/unlisted and expire or be access-controlled.

### 8.6 Media library

Provide a proper media library for:

- images
- documents/PDFs
- logos
- profile images
- article media

Metadata:

- title
- alt text EN/AR
- caption EN/AR
- copyright/source optional
- filename
- file type
- dimensions
- size
- uploaded by
- created date

Generate optimised image variants/thumbnails where practical.

Prevent accidental use of giant original images on listing cards.

SVG uploads must be sanitised or restricted to trusted administrators.

---

## 9. Page Builder

Administrators need to manage page composition without editing templates.

Create a controlled block-based page builder rather than unrestricted raw HTML.

Supported reusable blocks should include:

- Hero
- Rich Text
- Text + Image
- Image + Text
- CTA Banner
- Statistics
- Card Grid
- Sector Grid
- Team/People Grid
- Initiative Grid
- Latest Posts
- Selected Posts
- FAQ Block
- Logo/Partners Grid
- Quote/Testimonial
- Timeline
- Gallery
- Video Embed
- Document/Download List
- Feature Columns
- Contact CTA
- Membership CTA
- Custom spacer/divider only if actually needed

Every relevant field must be bilingual.

Blocks should support sensible layout options, but do not expose arbitrary CSS classes to normal editors.

Only trusted Super Admin users may use narrowly controlled custom code/embed fields, if such a feature is enabled at all.

Sanitise rich text and embedded content.

---

## 10. Data Model

Use a clean relational model. Exact migration names may adapt to repository conventions.

Expected primary models/tables include:

- `users`
- `roles` / `permissions` through the selected permission implementation
- `pages`
- `page_blocks` or an equivalent structured JSON block implementation
- `posts`
- `categories`
- `tags`
- `people`
- `team_groups`
- `sectors`
- `initiatives`
- `faqs`
- `partners`
- `media`
- `membership_applications`
- `membership_application_notes`
- `contact_submissions`
- `redirects`
- `site_settings`
- `navigation_menus`
- `navigation_items`
- `audit_logs` or equivalent activity history

Use pivot tables for many-to-many relationships such as:

- person <-> sector
- post <-> sector
- post <-> category
- post <-> tag
- initiative <-> sector
- initiative <-> person

### Translatable database fields

For content such as title, name, summary, body, excerpt, alt text and SEO description, use JSON translation fields where appropriate, e.g.:

```json
{
  "en": "About SBPN",
  "ar": "عن الشبكة"
}
```

Use explicit casts and validation.

Avoid duplicating whole rows purely by locale unless there is a strong architectural reason.

### Slugs

Maintain locale-aware slugs and uniqueness.

Store enough routing metadata to:

- resolve the correct localized page;
- switch language to the same content entity;
- produce canonical/hreflang tags;
- build old-to-new redirects.

---

## 11. Admin Roles and Permissions

Implement least-privilege access.

Suggested roles:

### Super Admin

Full platform access including users, roles, settings, legal pages and sensitive membership records.

### Administrator

Manage most website content, submissions and operational settings, but not system-level security unless granted.

### Editor

Create/edit/review/publish ordinary pages and posts.

### Author

Create and edit own content; submit for review; no unrestricted publishing.

### Translator

Edit Arabic/English translations and mark translation review state; cannot change security/settings.

### Membership Manager

Access membership applications and permitted private documents; limited public CMS access.

### Communications Manager

Manage news, announcements, contact enquiries, homepage promotions and social metadata.

### Viewer / Auditor

Read-only access to permitted admin records.

Implement permissions as granular capabilities, not hard-coded role-name checks throughout the application.

Examples:

- `pages.view`
- `pages.create`
- `pages.update`
- `pages.publish`
- `pages.delete`
- `legal.update`
- `posts.publish`
- `people.manage`
- `sectors.manage`
- `applications.view`
- `applications.view_sensitive_documents`
- `applications.update_status`
- `contacts.manage`
- `settings.manage`
- `users.manage`
- `roles.manage`
- `audit.view`

---

## 12. Auditability

Track important administrative actions.

At minimum record:

- authentication events where feasible
- content publish/unpublish
- important content edits
- legal page edits
- settings changes
- user/role changes
- membership application status changes
- sensitive document access where feasible

Audit records must identify:

- actor
- action
- target
- timestamp
- useful before/after context where appropriate

Normal admins must not be able to silently delete audit history.

---

## 13. Design Direction

The new site must look like a credible international professional/educational/non-profit network, not a generic WordPress theme.

### 13.1 Brand

- Preserve the existing SBPN logo and organisation identity.
- Derive the core UI palette from approved SBPN branding/assets.
- Use a controlled design-token system.
- Do not recolour the official logo unless an approved alternate logo asset exists.

### 13.2 Visual language

Use:

- clean white/light surfaces;
- strong dark text hierarchy;
- restrained brand accents;
- generous whitespace;
- subtle borders and shadows;
- structured editorial layouts;
- professional photography and real SBPN imagery where available;
- modest animation only where it improves comprehension.

Avoid:

- excessive gradients;
- glassmorphism everywhere;
- oversized rounded cards on every element;
- excessive animations;
- decorative clutter;
- stock photos where real SBPN media is available;
- a giant hero that hides useful content below the fold.

### 13.3 Typography

Use a professional, highly legible sans-serif pairing with excellent Arabic support.

Recommended direction:

- English: Inter or equivalent professional sans-serif
- Arabic: Noto Sans Arabic or equivalent high-quality Arabic sans-serif

Prefer self-hosted/font-package delivery rather than blocking third-party font calls.

Use appropriate Arabic line-height and letter spacing; do not apply Latin typography rules blindly to Arabic.

### 13.4 Layout

- mobile-first responsive design;
- sensible max content width around 1200–1320px;
- consistent 8px-based spacing scale;
- responsive 12-column grid where appropriate;
- text measure limited for readability;
- clear section rhythm;
- strong header/navigation usability on mobile.

### 13.5 RTL

Arabic is not just translated text.

When locale is Arabic:

- set `<html lang="ar" dir="rtl">`;
- reverse appropriate layout direction;
- mirror directional icons where semantically necessary;
- keep logos and non-directional assets unchanged;
- ensure menus, breadcrumbs, pagination, forms and validation align naturally in RTL;
- verify mixed Arabic/English names and URLs remain readable.

English must use `lang="en" dir="ltr"`.

---

## 14. Header and Navigation

Build a responsive sticky or semi-sticky header with:

- SBPN logo
- desktop navigation
- accessible dropdowns
- Join SBPN CTA
- language switcher
- mobile menu
- optional site search

Navigation must be keyboard usable.

Do not open ordinary internal links in new tabs.

External initiative links may clearly indicate they lead to another SBPN site/subdomain.

Menus should ultimately be manageable from the admin dashboard.

---

## 15. Footer

Build a structured bilingual footer containing configurable sections such as:

- SBPN summary
- primary navigation
- sectors or useful links
- initiatives
- contact details
- social links
- registered company information
- privacy
- terms
- copyright year automatically generated

Use the official organisation information stored in site settings rather than hard-coding it across templates.

---

## 16. Search

Add public site search for published content.

Search should cover relevant fields from:

- pages
- posts
- sectors
- initiatives
- people where profiles are public

Search only the current locale's text.

Do not expose private admin/submission content.

For current scale, database-based indexed/full-text search is acceptable. Do not introduce Elasticsearch/OpenSearch unless demonstrably required.

---

## 17. SEO

SEO is a launch requirement.

Implement:

- per-content SEO title EN/AR
- per-content meta description EN/AR
- canonical URLs
- `hreflang` links for `en`, `ar`, and appropriate `x-default`
- XML sitemap(s)
- robots.txt
- Open Graph metadata
- Twitter/X card metadata where appropriate
- configurable social image
- semantic headings
- clean URLs
- breadcrumbs
- breadcrumb structured data where appropriate
- Organisation structured data
- Article structured data on suitable posts
- Person structured data for public professional profiles where appropriate
- WebSite structured data
- correct noindex rules for admin, previews, search results if desired, and non-public pages

### Redirect manager

Create an admin-manageable redirects resource.

Fields:

- source path
- destination URL/path
- status code, default 301
- active flag
- hit count optional
- notes

Import all required old-to-new URL mappings before launch.

Avoid redirect chains.

---

## 18. Accessibility

Target **WCAG 2.2 AA** quality.

At minimum:

- keyboard navigation
- visible focus states
- skip-to-content link
- semantic landmarks
- logical heading structure
- labels associated with inputs
- useful validation errors
- accessible accordion/menu/dialog behaviour
- sufficient colour contrast
- meaningful alt text support in both languages
- no reliance on colour alone
- reduced-motion support
- responsive zoom without broken layouts
- accessible language and direction attributes

Run automated checks and manually test core flows with keyboard navigation.

---

## 19. Performance

Target a fast content site.

Implement:

- server-rendered HTML
- minimal public JavaScript
- image resizing/optimisation
- lazy loading below-the-fold images
- eager/high-priority loading only for true hero/LCP media
- local/static compiled CSS and JS
- cacheable assets with hashed filenames
- application/config/route/view caching in production where safe
- database indexes for high-use queries
- eager loading to avoid N+1 query problems
- pagination for content directories

Do not load large animation libraries for minor effects.

Aim for strong Core Web Vitals on normal mobile connections.

---

## 20. Forms, Spam and Abuse Protection

For public forms implement layered protection:

- CSRF protection
- server-side validation
- rate limiting
- honeypot field or equivalent low-friction protection
- optional Cloudflare Turnstile support via environment configuration
- upload MIME/type/size restrictions
- secure filename handling
- never trust browser MIME type alone

For sensitive document uploads:

- private storage
- randomised stored filenames
- strict allowed extensions and MIME checks
- size limits
- never execute uploaded files
- optional malware scan integration when the hosting environment supports it

Do not make CAPTCHA/Turnstile a hard dependency for local development.

---

## 21. Security

Treat security as a first-class requirement.

Implement and verify:

- framework CSRF protection
- escaped output by default
- sanitised rich content
- strong authorization policies
- least-privilege admin access
- password hashing using Laravel defaults
- email verification or admin-controlled accounts as appropriate
- optional admin MFA/2FA if supported cleanly by the chosen authentication approach
- login throttling
- session security
- secure cookies in production
- HTTPS-only production configuration
- `APP_DEBUG=false` in production
- no secrets in repository
- `.env` excluded from version control
- private uploaded files outside public web root
- safe response headers
- `X-Content-Type-Options`
- sensible Content Security Policy where compatible
- frame protection / `frame-ancestors`
- referrer policy
- permissions/policies for every Filament resource containing sensitive data

Never expose stack traces to the public in production.

Never log passwords, access tokens, identity files, or raw sensitive upload contents.

---

## 22. Privacy and Data Protection

The website processes membership/contact data and must be built with UK privacy requirements in mind.

Engineering requirements:

- data minimisation;
- collect only fields configured/required for a legitimate operational purpose;
- explicit consent where appropriate;
- retention-ready timestamps/statuses;
- private document storage;
- role-controlled access;
- export capability for a person's stored application data if operationally required;
- deletion/anonymisation capability subject to organisational/legal retention rules;
- clear privacy links at data collection points;
- no non-essential analytics cookies before consent where consent is legally required.

Do not invent legal policy wording in code.

Legal pages are CMS content managed by authorised SBPN users.

---

## 23. Cookies and Analytics

Build a configurable privacy-aware analytics integration.

Preferred behaviour:

- support an analytics provider configured by environment/admin settings;
- do not hard-code Google Analytics IDs;
- allow analytics to be disabled;
- if non-essential cookies are used, present a bilingual consent interface before loading them;
- store only necessary consent state.

Do not create fake analytics dashboard numbers.

If privacy-friendly cookieless analytics are used later, keep the integration modular.

---

## 24. Email and Notifications

Use Laravel mail/notifications.

All addresses and provider settings must be environment/admin configurable.

Implement templates for:

- membership application acknowledgement
- internal new membership application notification
- membership status update where enabled
- contact acknowledgement if desired
- internal contact notification
- password reset/admin authentication emails

Templates should be bilingual based on user/application locale.

Use queues for email when possible.

Gracefully handle mail failure and log operational errors without logging sensitive content.

---

## 25. Global Site Settings

Admin should be able to manage at least:

- organisation name EN/AR
- abbreviated name
- registered company name
- company number
- registered address
- general email
- phone if used
- social media URLs
- default SEO title/description
- default social image
- logo variants
- favicon
- footer description EN/AR
- copyright label
- analytics configuration flags/IDs
- membership form notification recipients
- contact form notification recipients
- site maintenance mode message EN/AR

Do not store secrets such as SMTP passwords in database settings when environment variables are more appropriate.

---

## 26. Navigation Management

Provide admin-manageable menus while preventing accidental site breakage.

Support:

- EN/AR label
- internal route/content link
- external URL
- parent/child relationship
- display order
- active flag
- target behaviour

Validate internal links.

Top-level required pages should have sensible protection against accidental deletion while still allowing content editing.

---

## 27. Partners / Supporters

Create a small reusable partner model for future growth.

Fields:

- name
- logo
- URL
- description EN/AR optional
- display order
- active

Only show partner sections when populated.

---

## 28. Content Quality Rules

Migrated content must not be silently rewritten.

During migration:

- preserve factual claims;
- preserve names and professional titles accurately;
- preserve publication dates;
- preserve legal text;
- clean obvious HTML artefacts;
- fix display-only encoding issues;
- flag obvious spelling/grammar problems for review rather than changing sensitive meaning silently.

Do not invent biographies, qualifications, affiliations, statistics, partners, or achievements.

Do not invent impact numbers.

Any homepage statistic must be explicitly managed and sourced by an administrator.

---

## 29. Current Content Migration Inventory

At minimum create import/migration coverage for:

### Core pages

- Home
- About Us
- Team
- Join Us
- FAQ
- Sectors
- News
- Contact
- Privacy
- Terms

### People

Migrate founders, existing department heads/deputies and other public profiles from the current site, retaining portraits and profile content where available.

### Sectors/departments

Current content includes sector/department areas such as:

- Health and Medical
- Engineering, Infrastructure and Construction
- Information Technology
- Law, Relations and Diplomacy
- Economy, Administration and Governance
- Environmental and Agricultural Development
- Education and Academic Research
- Media and Communications

Also inspect the current sectors page and existing FAQ taxonomy for any additional sector labels that must be preserved or reconciled.

Do not assume the above list is exhaustive without inspecting the source site.

### Initiatives

- TECH50K — external site/subdomain

### News/articles

Import all currently published news/articles that are part of the SBPN site, preserving:

- title
- date
- author
- language
- slug
- content
- featured image
- category/tag if available
- original URL for redirect mapping

### Legal

- Terms & Conditions
- Privacy Policy

---

## 30. Importer Requirements

Create Artisan commands or service classes that make migration reproducible.

Suggested commands:

```bash
php artisan sbpn:snapshot-current-site
php artisan sbpn:import-content
php artisan sbpn:import-media
php artisan sbpn:generate-redirects
php artisan sbpn:translation-report
php artisan sbpn:verify-migration
```

Exact command names can change, but equivalent capability is required.

Importer must:

- be idempotent where practical;
- not duplicate records when rerun;
- log imported/skipped/failed records;
- save original source URL/ID metadata;
- use database transactions where appropriate;
- continue safely after an isolated bad record where possible;
- generate a migration report.

---

## 31. Translation Parity Report

Create an admin page or command/report showing:

- English-only content
- Arabic-only content
- missing titles
- missing bodies
- untranslated SEO metadata
- translation review flags
- mismatched publish status
- missing localized slugs

Before launch, all required navigation/public content must have complete bilingual parity.

---

## 32. Error Handling

Create custom, branded bilingual pages for:

- 404
- 403
- 419/session expired where relevant
- 500
- maintenance mode

404 should offer:

- homepage
- search
- useful navigation

Do not expose technical error details publicly.

---

## 33. Testing Requirements

Use automated tests for critical behaviour.

### Unit/feature coverage must include

- locale routing
- language switcher relationship
- public content visibility rules
- draft content is not public
- scheduled content behaviour
- admin authorization
- role permissions
- membership form validation
- secure/private document authorization
- contact form validation
- redirects
- sitemap only contains public content
- canonical/hreflang generation
- search excludes private content
- Arabic/LTR locale attributes
- required translation publishing rule

### Integration/smoke tests

Verify at least:

- Home EN/AR
- About EN/AR
- Team EN/AR
- Sectors EN/AR
- News listing and article EN/AR
- FAQ EN/AR
- Join form EN/AR
- Contact form EN/AR
- Privacy/Terms
- Admin login
- CRUD for one major content type
- membership application review flow

Run the full test suite before claiming completion.

---

## 34. Manual QA Checklist

Before deployment, manually verify:

- no lorem ipsum
- no broken images
- no dead menu links
- no `#` placeholder links
- no console errors on primary public pages
- no mixed incorrect RTL/LTR blocks
- all mobile menus work
- keyboard navigation works
- language switching retains equivalent content
- all required form labels/errors are translated
- email flow works in staging
- uploaded sensitive files cannot be accessed publicly
- all existing important URLs resolve or redirect
- social share previews have sensible metadata
- 404 works
- sitemap works
- robots.txt works
- production admin is not indexable

---

## 35. SEO Migration Checklist

Produce a machine-readable and human-readable redirect report containing:

- old URL
- new URL
- HTTP status
- reason

Prioritise preserving:

- homepage
- About
- Team
- Sectors
- Join
- FAQ
- News
- individual posts
- individual profiles
- Arabic pages
- privacy/terms

Do not bulk redirect unrelated missing pages to the homepage.

Use 410 only when content is deliberately removed and no meaningful replacement exists.

---

## 36. Deployment Target — Plesk/PHP Hosting

Prepare the application for Plesk deployment.

### Web root

The domain's document root must point to Laravel's `public/` directory.

Never expose the Laravel project root directly as the public web directory.

### Production commands

Deployment should support a sequence equivalent to:

```bash
composer install --no-dev --prefer-dist --optimize-autoloader
npm ci
npm run build
php artisan migrate --force
php artisan optimize
php artisan filament:optimize
```

Run `storage:link` only for approved **public** media storage if required.

Sensitive membership files must remain on private storage and must not be linked into `public/storage`.

### Scheduler

Configure Plesk Scheduled Tasks/cron to execute Laravel's scheduler every minute:

```bash
php /path/to/project/artisan schedule:run
```

### Queue

Use the database queue driver as a safe default unless a managed queue service is configured.

If Supervisor is unavailable on the host, implement a Plesk-compatible scheduled queue processing strategy, for example controlled short-lived workers rather than relying on a permanently attached terminal session.

Never require an interactive shell session to keep production jobs alive.

### Environment

Production must use:

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://sbpn.org
```

Generate a unique application key.

Force HTTPS and secure cookies.

Do not commit `.env`.

### Staging

Deploy to a staging subdomain first, for example:

`staging.sbpn.org`

Protect staging from indexing using both authentication/access controls where appropriate and `noindex` headers/meta.

Only switch production after migration, redirects and acceptance tests pass.

---

## 37. Backup and Recovery

Before launch establish:

- daily database backup
- media/file backup
- retention policy
- restore instructions
- pre-deployment backup

Create `docs/RECOVERY.md` explaining how to restore:

1. application code;
2. `.env`/secrets from the secure source;
3. database;
4. public media;
5. private membership uploads.

Do not store production backups in a public web directory.

---

## 38. Documentation Deliverables

Create and maintain:

- `README.md` — developer setup
- `CLAUDE.md` — this implementation contract
- `.env.example`
- `docs/ARCHITECTURE.md`
- `docs/CONTENT-MODEL.md`
- `docs/ADMIN-GUIDE.md`
- `docs/DEPLOYMENT-PLESK.md`
- `docs/MIGRATION.md`
- `docs/REDIRECTS.md`
- `docs/RECOVERY.md`
- `docs/SECURITY.md`

Admin guide must explain in plain language how SBPN staff can:

- edit a page
- translate a page
- publish a post
- add a team member
- add/edit a sector
- add an initiative
- manage FAQs
- review a membership application
- manage contact enquiries
- upload media
- update menus
- update footer/contact/social settings
- create a redirect

---

## 39. Code Quality

Follow Laravel conventions.

Use:

- Form Requests or equivalent clean validation boundaries
- Policies/Gates for authorization
- service/action classes when business logic is non-trivial
- Eloquent relationships
- enums for statuses where useful
- query scopes for repeated filters
- database indexes
- clear model casts
- factories for test data
- seeders for controlled site defaults
- Pint for PHP formatting
- ESLint/formatting for frontend code if configured

Avoid:

- giant controllers
- business logic in Blade templates
- raw SQL unless justified
- repeated locale conditionals everywhere
- duplicated English/Arabic templates
- insecure mass assignment
- arbitrary HTML injection
- environment-specific constants in source code

---

## 40. Localisation Implementation

Support exactly these initial locales:

```php
['en', 'ar']
```

Default locale for legacy public routes is English unless current production behaviour requires otherwise.

Arabic prefix:

`/ar`

English existing routes should remain unprefixed to preserve SEO unless a migration analysis proves a different routing strategy is safer.

Translate:

- navigation
- buttons
- validation messages
- system messages
- form labels
- empty states
- pagination
- cookie consent
- error pages
- admin-facing custom labels where practical

Do not store reusable interface strings inside database page content when they belong in language files.

---

## 41. Date, Number and Text Formatting

Use locale-aware presentation.

- English: professional British English style where applicable.
- Arabic: formal Modern Standard Arabic for institutional content.

Names should not be automatically translated unless a curated translated/display version exists.

Store dates in UTC and display according to configured site timezone.

Use Unicode-safe truncation and slug handling.

---

## 42. Contact/Social Configuration

Migrate the public social links currently in use where verified, then make them admin-configurable.

Do not hard-code social usernames throughout the templates.

All external links should use safe `rel` values when opened in a new tab.

---

## 43. Homepage Content Strategy

The public home page currently contains themes around:

- uniting Syrian professionals;
- collaboration and knowledge sharing;
- research and consultancy;
- education and professional development;
- Syria's future and sustainable development;
- mission;
- vision;
- core values;
- latest news.

The rebuilt homepage should preserve these themes but present them in a cleaner hierarchy.

Do not create two competing homepages with different mission statements by language.

Use the translation workflow to establish one current bilingual content set.

---

## 44. Current Registration Information

The current About page displays the organisation as registered with Companies House under:

**SYRIAN B PROFESSIONALS NETWORK (SBPN)**

Company Number:

**16272073**

Migrate this into configurable organisation settings rather than scattering it across templates.

Do not change this information automatically during content clean-up.

---

## 45. Current Address / Contact Content

The current FAQ includes the address:

**85 Great Portland Street, London, England, W1W 7LT**

Migrate current address/contact data into global settings and display it only where appropriate.

Do not duplicate address strings in multiple hard-coded views.

If source pages contain inconsistent address/contact details, flag them in the migration report for administrator verification.

---

## 46. Admin Safety UX

For destructive or high-impact actions:

- require explicit confirmation;
- clearly describe the affected records;
- prevent deleting core navigation pages without elevated permission;
- use soft delete/restore where sensible;
- warn before unpublishing pages linked from navigation;
- warn before changing a published slug;
- offer to create a redirect when a published URL changes.

When a published slug is changed, automatically suggest/create a 301 redirect from the previous URL unless the administrator explicitly declines with suitable permission.

---

## 47. Content Revisions

For pages, posts, legal documents and other significant editorial records, keep useful revision history.

An editor should be able to see:

- who changed content;
- when;
- publication state;
- previous values for important fields;
- rollback capability where implemented safely.

At minimum, preserve enough history to recover from accidental editorial changes.

---

## 48. Scheduled Publishing

Support future publication of posts and pages where appropriate.

The scheduler must:

- publish scheduled records at/after scheduled time;
- invalidate relevant caches;
- ensure no premature public visibility;
- handle both locales as one content entity.

Do not schedule one translation independently in a way that produces accidental public mismatch unless explicitly designed and authorised.

---

## 49. Cache Strategy

Use caching deliberately.

Good candidates:

- global settings
- navigation trees
- homepage derived data
- sector lists
- frequently used footer data

Invalidate cache automatically on relevant admin changes.

Do not cache private membership/contact data into public caches.

---

## 50. Structured Content over Raw HTML

Do not migrate WordPress page-builder HTML blindly into the new frontend.

Migration process should:

1. extract meaningful text/media;
2. map it to structured models and page blocks;
3. preserve original source snapshots;
4. avoid importing theme shortcodes, Elementor/Visual Composer wrappers, obsolete CSS classes, or broken inline scripts.

The final markup should be clean Laravel-rendered HTML.

---

## 51. WordPress Legacy Separation

The new site should not require WordPress at runtime.

After migration and launch, WordPress can remain only as a backup/archive until SBPN chooses to remove it.

Do not build new frontend pages by proxying the old WordPress site.

---

## 52. External TECH50K Site

TECH50K is a separate initiative site under:

`https://tech50k.sbpn.org`

For this SBPN rebuild:

- display TECH50K in Initiatives;
- use appropriate imagery/content migrated or entered by admin;
- link to its external/subdomain site;
- do not duplicate or rebuild the entire TECH50K application as part of this scope;
- do not break existing TECH50K routes or LMS integrations.

---

## 53. Optional Future-Ready Features

Architect so the following can be added later without a rewrite, but do not let optional features delay the core launch:

- events calendar
- publications/reports library
- newsletter integration
- partner directory
- member portal
- authenticated member profiles
- opportunities/jobs board
- volunteer opportunities
- multilingual expansion beyond EN/AR
- API access for selected public content
- advanced search

Do not expose unfinished menu items for these features.

---

## 54. Priority Order for Urgent Rebuild

### P0 — Launch blockers

1. Laravel/Filament project foundation
2. database and admin authentication
3. bilingual locale architecture
4. global settings
5. pages/page builder
6. people/team
7. sectors
8. initiatives
9. posts/news
10. FAQ
11. membership application
12. contact form
13. privacy/terms
14. current-site migration
15. SEO/hreflang/sitemap
16. redirects
17. responsive/RTL QA
18. permissions/security
19. staging deployment
20. backups and production cutover documentation

### P1 — Immediately after core functionality

- revision polish
- deeper editorial analytics
- link checker
- newsletter integration
- advanced audit visualisation
- additional optional content block types

Never trade security, bilingual correctness, redirects, or backup safety for visual polish.

---

## 55. Definition of Done

The project is not complete until all of the following are true:

- [ ] English public website is complete.
- [ ] Arabic public website is complete and genuinely RTL.
- [ ] Both languages have equivalent main navigation and content entities.
- [ ] Existing important SBPN content is migrated.
- [ ] Existing team/people content is migrated.
- [ ] Existing news/articles are migrated.
- [ ] Existing sectors are migrated.
- [ ] TECH50K is correctly represented under Initiatives.
- [ ] Privacy and terms are migrated.
- [ ] Membership form works securely.
- [ ] Contact form works securely.
- [ ] `/admin` manages all primary content types.
- [ ] Roles and permissions work.
- [ ] Sensitive uploads are private.
- [ ] Translation completeness is visible to admins.
- [ ] Draft content is not public.
- [ ] Scheduled publication works if enabled.
- [ ] SEO metadata works.
- [ ] Canonical and hreflang tags are correct.
- [ ] Sitemap is correct.
- [ ] Legacy redirects are implemented.
- [ ] Arabic legacy routes are not left broken.
- [ ] Core pages meet accessibility requirements.
- [ ] Core pages are responsive on mobile/tablet/desktop.
- [ ] Automated tests pass.
- [ ] Migration verification passes.
- [ ] No important broken links remain.
- [ ] Staging is verified before production cutover.
- [ ] Production deployment documentation exists.
- [ ] Backup and recovery documentation exists.
- [ ] No required functionality is left as a placeholder.

---

## 56. Final Implementation Behaviour

When Claude Code begins implementation, follow this sequence:

1. Inspect the repository and environment.
2. Read this entire `CLAUDE.md`.
3. Fetch current Laravel agent guidance.
4. Install/configure Laravel Boost if not already installed.
5. Verify PHP/Laravel/Filament package compatibility.
6. Create a migration snapshot of the live SBPN website before changing production.
7. Produce a concise implementation checklist in the development notes.
8. Build the application in P0 order.
9. Add automated tests during implementation, not only at the end.
10. Run migrations and seed/import current SBPN content locally/staging.
11. Generate the translation parity and redirect reports.
12. Run formatting, tests and static checks.
13. Perform the manual QA checklist.
14. Prepare Plesk deployment instructions.
15. Do not switch production until staging, redirects, forms, bilingual content, backups and security checks pass.

The goal is a site SBPN staff can operate independently after launch: **modern public experience, complete bilingual parity, and a safe professional CMS.**
