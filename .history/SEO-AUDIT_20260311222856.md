# Full SEO Audit — jordanschilling.me

**Audit Date:** March 11, 2026  
**Site URL:** https://jordanschilling.me  
**Platform:** Jekyll (GitHub Pages) with customized Minimal theme  
**Prepared for:** Professional SEO Agent Review

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Technical SEO](#2-technical-seo)
3. [On-Page SEO](#3-on-page-seo)
4. [Content SEO](#4-content-seo)
5. [Structured Data & Schema](#5-structured-data--schema)
6. [Site Architecture & Internal Linking](#6-site-architecture--internal-linking)
7. [Performance & Core Web Vitals Factors](#7-performance--core-web-vitals-factors)
8. [Social & Open Graph](#8-social--open-graph)
9. [Issues by Priority](#9-issues-by-priority)
10. [File-by-File Inventory](#10-file-by-file-inventory)

---

## 1. Executive Summary

The site has solid foundational SEO with `jekyll-seo-tag` and `jekyll-sitemap` plugins, proper canonical tags, JSON-LD structured data, and Open Graph meta tags. However, there are significant issues around **keyword stuffing / over-optimization of the author's name**, **missing meta descriptions on blog posts**, **missing favicon**, **no og:image**, **inconsistent post front matter**, and **pages with no SEO value** (e.g., `another-page.md`). The site also lacks mobile-specific optimizations, performance optimizations (no image optimization, no lazy loading), and has no analytics tracking ID configured.

### Overall SEO Health: **6/10** — Strong foundation, needs refinement

---

## 2. Technical SEO

### 2.1 Crawlability

| Item | Status | Details |
|------|--------|---------|
| robots.txt | ✅ Present | `Allow: /` — all pages crawlable. Sitemap URL declared. |
| XML Sitemap | ✅ Present | Custom `sitemap.xml` with manual entries + auto-generated posts/pages. Includes `<lastmod>`, `<changefreq>`, and `<priority>`. |
| Canonical Tags | ✅ Present | `<link rel="canonical">` set in `_includes/head-custom.html` via `{{ page.url | absolute_url }}` |
| `jekyll-sitemap` plugin | ✅ Installed | Listed in `_config.yml` and `Gemfile`. However, both a manual `sitemap.xml` AND the plugin exist — **potential duplicate sitemap conflict**. |
| `jekyll-seo-tag` plugin | ✅ Installed | `{% seo %}` tag present in `default.html` head. |

#### ISSUES:
- **CRITICAL: Duplicate sitemap generation.** The manual `sitemap.xml` in the root AND the `jekyll-sitemap` plugin will both try to generate sitemaps. The manual file will override the plugin, but this creates maintenance burden and potential conflicts. **Recommendation:** Remove the manual `sitemap.xml` and rely solely on `jekyll-sitemap` plugin, OR remove the plugin and maintain the manual file.
- **WARNING: `jekyll-sitemap` plugin may generate a second sitemap** at a different path if not properly configured, leading to crawl confusion.

### 2.2 Indexability

| Item | Status | Details |
|------|--------|---------|
| `<html lang>` attribute | ✅ Present | `lang="en-US"` (defaults correctly) |
| Meta charset | ✅ Present | `UTF-8` |
| Meta viewport | ✅ Present | `width=device-width, initial-scale=1` |
| `noindex` tags | ✅ None found | All pages are indexable by default |

#### ISSUES:
- **MEDIUM: `another-page.md` is indexable** but contains no useful content ("Welcome to another page" / "yay"). This is a test/placeholder page that should either be removed, given `noindex`, or populated with real content. It gets indexed and dilutes crawl budget.
- **MEDIUM: Dev log index pages** (`focus-buddy.md`, `python-playwright.md`, `portfolio-dev.md`, `inventory-management.md`) have **no front matter `title` or `description`**. They will inherit site defaults, producing duplicate/generic meta tags across multiple pages.

### 2.3 URL Structure

| Page | URL (permalink) | Assessment |
|------|-----------------|------------|
| Homepage | `/` | ✅ Clean |
| About | `/about-jordan-schilling` | ⚠️ Keyword-stuffed URL |
| Bio | `/jordan-schilling-bio` | ⚠️ Keyword-stuffed URL |
| Projects | `/jordan-schilling-projects` | ⚠️ Keyword-stuffed URL |
| Writing | `/jordan-schilling-writing` | ⚠️ Keyword-stuffed URL |
| Media | `/jordan-schilling-media` | ⚠️ Keyword-stuffed URL |
| Blog posts | `/category/YYYY/MM/DD/slug.html` | ✅ Standard Jekyll format |

#### ISSUES:
- **MEDIUM: Keyword-heavy URLs.** Every top-level page URL contains "jordan-schilling". While name recognition is important for a personal portfolio, having the full name in every URL can be perceived as over-optimization. Typical portfolio URLs would be `/about`, `/projects`, `/writing`, `/media`. **However**, if the explicit goal is name-brand SEO, this is a deliberate strategy that an SEO professional should evaluate in context.

### 2.4 HTTPS & Domain

| Item | Status |
|------|--------|
| HTTPS | ✅ Assumed (GitHub Pages enforces HTTPS) |
| Custom domain | ✅ `jordanschilling.me` configured in `_config.yml` |
| www vs non-www | ⚠️ Not explicitly declared — should verify DNS/redirect configuration |

### 2.5 Favicon

| Item | Status |
|------|--------|
| favicon.ico | ❌ **MISSING** |
| favicon link tag | ❌ **Commented out** in `head-custom.html` |

#### ISSUES:
- **HIGH: No favicon.** The `<link rel="shortcut icon">` tag is commented out, and no `favicon.ico` file exists. Browsers will return 404s for `/favicon.ico` requests, which shows in server logs and hurts perceived professionalism. Some search engines display favicons in results.

---

## 3. On-Page SEO

### 3.1 Title Tags

| Page | Title | Length | Assessment |
|------|-------|--------|------------|
| Homepage | "Jordan Schilling \| Technologist, Software Builder, and Engineer" | 64 chars | ✅ Good length, includes target keywords |
| About | "About Jordan Schilling \| Technologist and Software Engineer" | 59 chars | ✅ Good |
| Bio | "Jordan Schilling Bio \| Software Engineer and Technologist" | 58 chars | ⚠️ Very similar to About page title — cannibalization risk |
| Projects | "Jordan Schilling Projects \| Software and Automation Portfolio" | 62 chars | ✅ Good |
| Writing | "Jordan Schilling Writing \| Articles on Engineering and Automation" | 66 chars | ✅ Good (slightly long) |
| Media | "Jordan Schilling Media \| Mentions, Talks, and Public References" | 65 chars | ✅ Good |
| focus-buddy.md | ❌ **No title in front matter** | N/A | Uses site default |
| python-playwright.md | ❌ **No title in front matter** | N/A | Uses site default |
| portfolio-dev.md | ❌ **No title in front matter** | N/A | Uses site default |
| inventory-management.md | ❌ **No title in front matter** | N/A | Uses site default |
| another-page.md | ❌ **No title in front matter** | N/A | Uses site default |

#### ISSUES:
- **HIGH: 5 pages have no `title` in front matter** — they will all render with the same site-wide default title, creating duplicate title tags across the site. Search engines penalize duplicate titles.
- **MEDIUM: "About" and "Bio" pages have near-identical titles** and very similar content. This creates keyword cannibalization — both pages compete for the same search queries. Consider merging them or differentiating their title/content focus.

### 3.2 Meta Descriptions

| Page | Description | Length | Assessment |
|------|-------------|--------|------------|
| Homepage | "Jordan Schilling is a technologist and software engineer focused on building automation systems, experimental tools, and civic technology projects. This site documents the work, ideas, and projects created by Jordan Schilling." | 224 chars | ⚠️ Too long (recommended: 150-160 chars). Will be truncated in SERPs. |
| About | "Learn about Jordan Schilling — a technologist..." | ~130 chars | ✅ Good |
| Bio | "Structured biography of Jordan Schilling..." | ~90 chars | ⚠️ Short, could be more descriptive |
| Projects | "Index of all projects built by Jordan Schilling..." | ~120 chars | ✅ Good |
| Writing | "Technical articles, project breakdowns..." | ~95 chars | ✅ Acceptable |
| Media | "Media mentions, talks, public references..." | ~90 chars | ✅ Acceptable |
| All blog posts | ❌ **No `description` in any post front matter** | N/A | Will fall back to site description |
| Dev log index pages | ❌ **No `description` in front matter** | N/A | Will fall back to site description |

#### ISSUES:
- **CRITICAL: No blog post has a `description` field.** All 15 blog posts will use the site-wide default description in meta tags, meaning every post shares the exact same meta description. This is a major SEO problem.
- **HIGH: Homepage description is 224 characters** — exceeds the ~155-160 char recommended limit and will be truncated in search results.
- **HIGH: Dev log index pages (4 pages) have no description** — same duplication issue.

### 3.3 Heading Structure

| Page | H1 | Assessment |
|------|----|----|
| Homepage | "Jordan Schilling — Technologist, Software Builder, and Engineer" | ✅ Single H1, keyword-rich |
| About | "About Jordan Schilling" | ✅ |
| Bio | "Jordan Schilling Bio" | ✅ |
| Projects | "Jordan Schilling Projects" | ✅ |
| Writing | "Jordan Schilling Writing" | ✅ |
| Media | "Jordan Schilling Media" | ✅ |
| Blog posts (via post.html layout) | `{{ page.title }}` | ✅ Dynamically set |
| Dev log pages | "Project Updates" | ⚠️ Generic, non-descriptive H1 |

#### ISSUES:
- **MEDIUM: Four dev log index pages all share the same H1** ("Project Updates"). These should be differentiated (e.g., "FocusBuddy Project Updates by Jordan Schilling").
- **LOW: The `<header>` in `default.html` contains an `<h1>` for the site title** that appears on every page. Combined with the content H1, **every page has two H1 tags**. The header H1 wraps an `<a>` linking back home. Consider changing the header to use a `<div>` or `<p>` instead of `<h1>` to avoid duplicate H1s.

---

## 4. Content SEO

### 4.1 Keyword Over-Optimization

**This is the most significant content SEO issue on the site.**

The name "Jordan Schilling" appears with extreme frequency across all pages. A rough count:

| Page | Approximate "Jordan Schilling" mentions | Assessment |
|------|----------------------------------------|------------|
| Homepage (index.md) | ~30+ times | 🔴 Severely over-optimized |
| About page | ~50+ times | 🔴 Severely over-optimized |
| Bio page | ~30+ times | 🔴 Severely over-optimized |
| Projects page | ~20+ times | 🔴 Over-optimized |
| Writing page | ~20+ times | 🔴 Over-optimized |
| Media page | ~20+ times | 🔴 Over-optimized |

#### ISSUES:
- **CRITICAL: Extreme keyword stuffing of the author's name.** While it's appropriate for a personal portfolio to mention the owner's name, the current density is far beyond natural. Nearly every sentence on every page begins with or contains "Jordan Schilling." Modern search engines (especially Google's Helpful Content Update and SpamBrain) can detect and penalize this pattern. This reads as if optimized for a search engine rather than for humans.
- **Recommendation:** Reduce name mentions dramatically. Use pronouns ("he," "his"), implied subjects, and natural sentence flow. The name should appear in key positions (title, H1, first paragraph, schema markup) but not in every sentence.
- **SEO professional should assess:** Whether the current density crosses Google's spam threshold and what the target keyword density should be.

### 4.2 Content Duplication / Cannibalization

#### ISSUES:
- **HIGH: "About" page vs "Bio" page.** These two pages cover essentially the same content — who Jordan Schilling is, background, projects, interests. They will compete against each other in search results for queries like "Jordan Schilling software engineer." **Recommendation:** Merge into a single authoritative page, or sharply differentiate the content angle.
- **MEDIUM: The homepage, About page, and Bio page all repeat** the same project descriptions, career summary, and technical interests. This internal duplication dilutes page authority.

### 4.3 Thin Content

| Page | Word Count (approx) | Assessment |
|------|---------------------|------------|
| another-page.md | ~5 words | 🔴 Extremely thin — should be removed or noindexed |
| Dev log index pages (4) | ~10 words + auto-generated links | ⚠️ Thin — consider adding introductory content |
| Media page | ~300 words, mostly placeholder | ⚠️ Several sections say "will be updated" — placeholder content |

### 4.4 Blog Post SEO

| Post | Has title | Has description | Has tags | Has category |
|------|-----------|-----------------|----------|--------------|
| 2025-12-06 focus-buddy | ✅ | ❌ | ❌ | ✅ |
| 2025-12-08 portfolio-dev | ✅ | ❌ | ❌ | ✅ |
| 2025-12-20 portfolio-dev | ✅ | ❌ | ❌ | ✅ |
| 2025-12-21 portfolio-dev | ✅ | ❌ | ❌ | ⚠️ `my-portfolio` (inconsistent) |
| 2025-12-28 focus-buddy | ✅ | ❌ | ❌ | ✅ |
| 2025-12-30 focus-buddy | ✅ | ❌ | ❌ | ✅ |
| 2025-12-31 focus-buddy | ✅ | ❌ | ❌ | ✅ |
| 2026-01-01 focus-buddy | ✅ | ❌ | ❌ | ✅ |
| 2026-01-11 focus-buddy | ✅ | ❌ | ❌ | ✅ |
| 2026-01-14 focus-buddy | ✅ | ❌ | ❌ | ✅ |
| 2026-01-26 inventory-mgmt | ✅ | ❌ | ❌ | ✅ |
| 2026-03-05 python-playwright | ✅ | ❌ | ❌ | ✅ |
| 2026-03-06 python-playwright | ✅ | ❌ | ❌ | ✅ |
| 2026-03-07 python-playwright | ✅ | ❌ | ❌ | ✅ |
| 2026-03-08 python-playwright | ✅ | ❌ | ❌ | ✅ |

#### ISSUES:
- **CRITICAL: Zero blog posts have a `description` front matter field.** Every post shares the same meta description.
- **HIGH: Zero blog posts have `tags`.** Tags help with content categorization and internal linking.
- **MEDIUM: Inconsistent front matter** — 3 portfolio-dev posts use `by:` instead of `author:`, and one uses a different category name (`my-portfolio` vs `portfolio-dev`).

---

## 5. Structured Data & Schema

### 5.1 JSON-LD (in `head-custom.html`)

**Person Schema:** ✅ Present
```json
{
  "@type": "Person",
  "name": "Jordan Schilling",
  "url": "https://jordanschilling.me",
  "sameAs": ["https://github.com/jschilling12"],
  "jobTitle": "Software Engineer",
  "knowsAbout": [...]
}
```

**WebSite Schema:** ✅ Present
```json
{
  "@type": "WebSite",
  "name": "Jordan Schilling",
  "url": "https://jordanschilling.me"
}
```

#### ISSUES:
- **MEDIUM: No `BlogPosting` schema** for blog posts. Adding structured data to posts would enable rich snippets in search results (date, author, headline).
- **MEDIUM: No `image` property** in Person schema. Adding a profile image URL would help with Knowledge Panel eligibility.
- **LOW: `sameAs` array only includes GitHub.** Consider adding LinkedIn, Twitter/X, or other professional profiles if they exist.
- **LOW: WebSite schema lacks `potentialAction` for SearchAction** (sitelinks search box) — minor since this is a small static site.

### 5.2 `jekyll-seo-tag` Output

The `{% seo %}` tag in the default layout auto-generates:
- `<title>` tag
- Meta description
- Open Graph tags (og:title, og:description, og:locale, og:site_name, og:url)
- Twitter card tags
- JSON-LD (additional)
- Canonical URL

**Note:** There is potential **duplication** between the auto-generated tags from `jekyll-seo-tag` and the manually added tags in `head-custom.html`:
- **Duplicate canonical tag** — `jekyll-seo-tag` generates one, AND `head-custom.html` adds another manually.
- **Duplicate og:title, og:description, og:url, og:site_name** — both sources generate these.
- **Duplicate JSON-LD** — `jekyll-seo-tag` generates its own JSON-LD AND `head-custom.html` has manual JSON-LD blocks.

#### ISSUES:
- **HIGH: Duplicate meta tags.** Having two canonical tags, two sets of OG tags, and multiple JSON-LD blocks can confuse search engines and social media crawlers. **Recommendation:** Either rely on `jekyll-seo-tag` for all meta/OG/schema output and remove manual duplicates from `head-custom.html`, or disable `jekyll-seo-tag` and maintain everything manually.

---

## 6. Site Architecture & Internal Linking

### 6.1 Navigation

The sidebar nav (in `default.html`) links to:
- Home (`/`)
- About Jordan Schilling (`/about-jordan-schilling`)
- Jordan Schilling Bio (`/jordan-schilling-bio`)
- Projects (`/jordan-schilling-projects`)
- Writing (`/jordan-schilling-writing`)
- Media (`/jordan-schilling-media`)

✅ `<nav>` element has `aria-label` for accessibility.

#### ISSUES:
- **MEDIUM: No breadcrumb navigation.** Breadcrumbs help search engines understand site hierarchy and can appear as rich results.
- **LOW: Blog posts are not directly accessible from nav** — they're only reachable through the Writing page or dev log index pages. Consider adding a "Blog" or "Dev Logs" direct link.

### 6.2 Internal Link Audit

| Link Pattern | Assessment |
|--------------|------------|
| Homepage → About, Bio, Projects, Writing, Media | ✅ Good hub-and-spoke |
| Dev log pages → Individual posts | ✅ Auto-generated via Liquid loops |
| Blog posts → Other pages | ⚠️ Not audited individually — likely minimal cross-linking |
| Footer links | ✅ Links to About and Projects |

#### ISSUES:
- **MEDIUM: Internal links use `.html` extension** in some places (e.g., `./focus-buddy.html`, `./python-playwright.html`) and no extension in others. This should be consistent. If using Jekyll's `permalink` setting, links should match the configured URL format.
- **LOW: `another-page.md` contains a link `[text](another-page.html)` pointing to itself** — circular/useless link.
- **MEDIUM: Dev log index pages don't link back to parent project pages** or the projects index.

### 6.3 Orphan Pages

| Page | Linked from navigation? | Linked from other pages? |
|------|------------------------|-------------------------|
| another-page.md | ❌ | ❌ | 
| inventory-management.md | ❌ | ❌ |

These are **orphan pages** — not linked from anywhere in the site navigation and likely not discoverable by crawlers unless they appear in the sitemap.

---

## 7. Performance & Core Web Vitals Factors

### 7.1 Assets

| Item | Status | Details |
|------|--------|---------|
| CSS | Single stylesheet (`style.scss` → compiled) | ✅ Minimal |
| JavaScript | Single `scale.fix.js` | ✅ Minimal |
| Images | Only `logo.png` used across site | ⚠️ No content images at all |
| Fonts | 4 Noto Sans variants (regular, italic, 700, 700italic) loaded locally | ⚠️ Check if all 4 are needed |
| External scripts | html5shiv (IE conditional) | ✅ Conditional load |

#### ISSUES:
- **LOW: No content images across the entire site.** While not a direct SEO ranking factor, image-rich content tends to rank better in universal search, appears in Google Images, and improves engagement metrics. Consider adding project screenshots, diagrams, or architecture visuals.
- **MEDIUM: Four font weight/style variants loaded** — verify all are actually used in the CSS. Unused font files add download weight.
- **LOW: IE conditional comment** (`<!--[if lt IE 9]>`) loads an external CDN script. IE is completely dead — this can be removed.

### 7.2 Google Analytics

| Item | Status |
|------|--------|
| GA tracking code | ✅ Present in template |
| GA tracking ID | ❌ **Not configured** — `google_analytics:` in `_config.yml` has no value |
| GA version | ⚠️ Uses **Universal Analytics (analytics.js)** — deprecated July 2023 |

#### ISSUES:
- **HIGH: Google Analytics is not functional.** The `google_analytics` key in `_config.yml` has no value, so the conditional in the template never fires. No traffic data is being collected.
- **HIGH: Uses deprecated Universal Analytics.** Even if a tracking ID were added, `analytics.js` (Universal Analytics) has been sunset. Must migrate to **GA4 (gtag.js)**.

---

## 8. Social & Open Graph

### 8.1 Open Graph Tags

From `head-custom.html` (manual) + `jekyll-seo-tag` (auto):

| Tag | Status | Value |
|-----|--------|-------|
| og:title | ✅ Present (duplicated) | `{{ page.title | default: site.title }}` |
| og:description | ✅ Present (duplicated) | `{{ page.description | default: site.description }}` |
| og:url | ✅ Present (duplicated) | `{{ page.url | absolute_url }}` |
| og:site_name | ✅ Present (duplicated) | "Jordan Schilling" |
| og:type | ✅ Present | "website" |
| og:image | ❌ **MISSING** | No og:image tag exists |
| og:locale | Depends on jekyll-seo-tag | Should auto-generate |

### 8.2 Twitter Card

From `_config.yml`:
```yaml
twitter:
  card: summary
```

#### ISSUES:
- **HIGH: No `og:image` tag.** When someone shares the site on social media (LinkedIn, Twitter/X, Facebook, Slack, Discord), no image preview will appear. This dramatically reduces click-through rates on shared links. Add an `og:image` meta tag pointing to a branded social sharing image (recommended: 1200x630px).
- **MEDIUM: No Twitter/X username configured.** The twitter card is set to `summary` but no `twitter:site` or `twitter:creator` is configured.
- **MEDIUM: `og:type` is hardcoded to "website"** for all pages including blog posts. Blog posts should use `og:type: article`.
- **HIGH: Duplicate OG tags** from both manual (`head-custom.html`) and `jekyll-seo-tag`. Social media crawlers may parse the wrong one.

---

## 9. Issues by Priority

### 🔴 CRITICAL

| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| 1 | **Keyword stuffing** — "Jordan Schilling" used 30-50+ times per page | All content pages | Reduce to natural density. Use pronouns, implied subjects. Keep name in title, H1, first paragraph, and schema. |
| 2 | **No meta descriptions on any blog post** (15 posts) | All `_posts/` files | Add `description:` to front matter of every post |
| 3 | **Duplicate meta tags** from jekyll-seo-tag + manual head-custom.html | `_includes/head-custom.html` + `_layouts/default.html` | Choose one source of truth. Remove manual OG/canonical tags or remove jekyll-seo-tag. |
| 4 | **Duplicate sitemap** — manual sitemap.xml + jekyll-sitemap plugin | `sitemap.xml` + `_config.yml` | Remove one. Recommend keeping the plugin. |

### 🟠 HIGH

| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| 5 | **No favicon** | Missing file + commented-out tag | Create favicon.ico and uncomment/add the link tag |
| 6 | **No og:image** | `head-custom.html` | Create social sharing image and add og:image tag |
| 7 | **5 pages missing title in front matter** | `focus-buddy.md`, `python-playwright.md`, `portfolio-dev.md`, `inventory-management.md`, `another-page.md` | Add unique titles |
| 8 | **4 dev log pages missing description** | Same as above | Add unique descriptions |
| 9 | **Homepage description too long** (224 chars) | `index.md` | Trim to ≤155 characters |
| 10 | **Google Analytics not functional** | `_config.yml` | Add GA4 tracking ID and migrate to gtag.js |
| 11 | **About vs Bio page cannibalization** | `about-jordan-schilling.md`, `jordan-schilling-bio.md` | Merge or sharply differentiate |
| 12 | **Duplicate OG tags** cause social sharing unpredictability | `head-custom.html` | Remove manual OG tags, let jekyll-seo-tag handle it |

### 🟡 MEDIUM

| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| 13 | **Inconsistent blog post front matter** — 3 posts use `by:` instead of `author:` | `_posts/2025-12-08`, `2025-12-20`, `2025-12-21` | Standardize to `author:` |
| 14 | **Inconsistent category** — one post uses `my-portfolio` instead of `portfolio-dev` | `_posts/2025-12-21-portfolio-dev.md` | Change to `portfolio-dev` |
| 15 | **No tags on any blog post** | All `_posts/` files | Add relevant tags |
| 16 | **Dev log H1s are generic** ("Project Updates") | 4 dev log index pages | Add specific, keyword-rich H1s |
| 17 | **Double H1 on every page** — header + content | `_layouts/default.html` | Change header site title from `<h1>` to `<p>` or `<div>` |
| 18 | **No BlogPosting schema** on blog posts | `_layouts/post.html` or `head-custom.html` | Add JSON-LD BlogPosting schema |
| 19 | **No breadcrumb navigation** | Site-wide | Add BreadcrumbList schema and visible breadcrumbs |
| 20 | **Inconsistent link extensions** (.html vs no extension) | Various content pages | Standardize |
| 21 | **No content images** across entire site | Site-wide | Add project screenshots, diagrams |
| 22 | **Orphan pages** not linked from navigation | `another-page.md`, `inventory-management.md` | Link or remove |
| 23 | **og:type hardcoded to "website"** for all pages | `head-custom.html` | Use "article" for blog posts |
| 24 | **4 font variants loaded** — verify usage | `_sass/fonts.scss` | Audit and remove unused variants |
| 25 | **Media page has placeholder content** | `jordan-schilling-media.md` | Fill in or noindex until ready |
| 26 | **No Twitter/X username** configured | `_config.yml` | Add if applicable |

### 🟢 LOW

| # | Issue | Location | Recommendation |
|---|-------|----------|----------------|
| 27 | `sameAs` in Person schema only has GitHub | `head-custom.html` | Add LinkedIn, Twitter if they exist |
| 28 | IE conditional script load | `_layouts/default.html` | Remove — IE is dead |
| 29 | `another-page.md` self-referential link | `another-page.md` | Remove page entirely |
| 30 | `show_downloads: true` in config but download section commented out | `_config.yml` + `default.html` | Set to `false` or remove |

---

## 10. File-by-File Inventory

### Configuration Files

| File | SEO Role | Status |
|------|----------|--------|
| `_config.yml` | Site title, description, URL, plugins, social config, defaults | ✅ Well-configured with minor issues |
| `Gemfile` | Plugin dependencies | ✅ Both SEO plugins listed |
| `robots.txt` | Crawl directives | ✅ Allows all, declares sitemap |
| `sitemap.xml` | Search engine sitemap | ⚠️ Conflicts with jekyll-sitemap plugin |

### Layout & Include Files

| File | SEO Role | Status |
|------|----------|--------|
| `_layouts/default.html` | Base template with `{% seo %}`, nav, semantic HTML | ⚠️ Double H1 issue |
| `_layouts/post.html` | Blog post template | ⚠️ No BlogPosting schema |
| `_includes/head-custom.html` | Manual canonical, OG, JSON-LD | ⚠️ Duplicates jekyll-seo-tag output |
| `_includes/head-custom-google-analytics.html` | GA tracking | ❌ Non-functional (no ID + deprecated UA) |

### Content Pages

| File | Has Title | Has Description | Has Permalink | Issues |
|------|-----------|-----------------|---------------|--------|
| `index.md` | ✅ | ✅ (too long) | N/A | Description exceeds 160 chars |
| `about-jordan-schilling.md` | ✅ | ✅ | ✅ | Cannibalization with bio page |
| `jordan-schilling-bio.md` | ✅ | ✅ | ✅ | Cannibalization with about page |
| `jordan-schilling-projects.md` | ✅ | ✅ | ✅ | — |
| `jordan-schilling-writing.md` | ✅ | ✅ | ✅ | — |
| `jordan-schilling-media.md` | ✅ | ✅ | ✅ | Placeholder content |
| `focus-buddy.md` | ❌ | ❌ | ❌ | Missing all SEO front matter |
| `python-playwright.md` | ❌ | ❌ | ❌ | Missing all SEO front matter |
| `portfolio-dev.md` | ❌ | ❌ | ❌ | Missing all SEO front matter |
| `inventory-management.md` | ❌ | ❌ | ❌ | Missing all SEO front matter |
| `another-page.md` | ❌ | ❌ | ❌ | Thin/test content, should be removed |

### Blog Posts (15 total)

All posts have: `layout: post`, `title`, `author` (except 3), `date`, `category`  
No posts have: `description`, `tags`, `image`

---

## Appendix: Raw Configuration Reference

### _config.yml
```yaml
title: "Jordan Schilling | Technologist, Software Builder, and Engineer"
author: Jordan Schilling
logo: /assets/img/logo.png
description: "Jordan Schilling is a technologist and software engineer..."
url: "https://jordanschilling.me"
baseurl: ""
plugins:
  - jekyll-seo-tag
  - jekyll-sitemap
twitter:
  card: summary
social:
  name: Jordan Schilling
  links:
    - https://github.com/jschilling12
defaults:
  - scope:
      path: ""
    values:
      author: Jordan Schilling
      image: /assets/img/logo.png
```

### head-custom.html — Manual SEO Tags
- Canonical URL: `<link rel="canonical" href="{{ page.url | absolute_url }}">`
- OG tags: og:title, og:description, og:url, og:site_name, og:type
- JSON-LD: Person + WebSite schemas
- **Missing:** og:image, twitter:site, twitter:creator

### Structured Data Schemas Present
1. **Person** (JSON-LD) — name, url, sameAs, jobTitle, description, knowsAbout
2. **WebSite** (JSON-LD) — name, url, description, author

### Structured Data Schemas Missing
1. **BlogPosting** — for individual blog posts
2. **BreadcrumbList** — for site navigation hierarchy
3. **ImageObject** — no images to mark up currently

---

*End of SEO Audit — Prepared for professional SEO review*
