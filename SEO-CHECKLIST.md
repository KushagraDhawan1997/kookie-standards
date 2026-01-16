# SEO Checklist for Next.js Projects

A comprehensive checklist to ensure production-ready SEO across all projects.

---

## Quick Status Overview

Use this table to track SEO implementation across multiple projects:

| Feature | Project 1 | Project 2 | Project 3 |
|---------|:---------:|:---------:|:---------:|
| Favicons (complete set) | | | |
| OG Image (1200x630 PNG) | | | |
| Web Manifest | | | |
| metadataBase | | | |
| OpenGraph config | | | |
| Twitter cards | | | |
| robots.ts | | | |
| sitemap.ts | | | |
| JSON-LD Schema | | | |
| Security headers | | | |
| 404/Error pages | | | |
| Search Console | | | |

---

## 1. Favicon & Icon Set

Every project needs a complete favicon set for cross-browser and cross-device compatibility.

### Required Files

**Modern approach (recommended):**

| File | Size | Purpose |
|------|------|---------|
| `favicon.ico` | 48x48 | Legacy browsers |
| `favicon.svg` | scalable | Modern browsers (supports dark mode) |
| `favicon-96x96.png` | 96x96 | High-DPI displays |
| `apple-touch-icon.png` | 180x180 | iOS home screen |
| `web-app-manifest-192x192.png` | 192x192 | Android/PWA |
| `web-app-manifest-512x512.png` | 512x512 | Android splash screen |

**Traditional approach:**

| File | Size | Purpose |
|------|------|---------|
| `favicon.ico` | 32x32 | Legacy browsers |
| `favicon-16x16.png` | 16x16 | Small browser UI |
| `favicon-32x32.png` | 32x32 | Standard tabs |
| `apple-touch-icon.png` | 180x180 | iOS home screen |
| `android-chrome-192x192.png` | 192x192 | Android |
| `android-chrome-512x512.png` | 512x512 | Android splash |

### Location

Place in `/app` (auto-discovered by Next.js) or `/public` (referenced in metadata).

### Naming Convention

All favicon and logo files should use lowercase, hyphenated names:

| Type | Filename Pattern |
|------|------------------|
| Primary logo | `{project}-logo.svg` |
| Logo PNG | `{project}-logo.png` |
| Icon only | `{project}-icon.svg` |

### Generation Tool

Use [RealFaviconGenerator.net](https://realfavicongenerator.net/) to generate a complete set from your logo.

### Checklist

- [ ] All favicon files created
- [ ] Lowercase, hyphenated naming
- [ ] Icons referenced in metadata
- [ ] Old/unused icon files removed

---

## 2. Open Graph Image

Required for social media sharing previews (Twitter, LinkedIn, Discord, Slack).

### Specifications

| Property | Requirement |
|----------|-------------|
| Dimensions | 1200x630 pixels |
| Format | PNG or JPG (not SVG) |
| File size | < 200KB recommended |
| Location | `/app/opengraph-image.png` (auto-discovered) |

### Content Guidelines

- Include project logo prominently
- Use brand colors consistently
- Add tagline or brief description
- Ensure text is readable at small sizes
- Test at https://opengraph.xyz

### Dynamic OG Images (for content sites)

```tsx
// app/blog/[slug]/opengraph-image.tsx
import { ImageResponse } from "next/og";

export const runtime = "edge";
export const alt = "Blog Post Title";
export const size = { width: 1200, height: 630 };
export const contentType = "image/png";

// Load Google Fonts dynamically
async function loadGoogleFont(font: string, weight: number, text: string) {
  const url = `https://fonts.googleapis.com/css2?family=${font}:wght@${weight}&text=${encodeURIComponent(text)}`;
  const css = await (await fetch(url)).text();
  const resource = css.match(/src: url\((.+)\) format\('(opentype|truetype)'\)/);
  if (resource) {
    const response = await fetch(resource[1]);
    if (response.status === 200) {
      return await response.arrayBuffer();
    }
  }
  throw new Error("Failed to load font");
}

export default async function Image({ params }: { params: { slug: string } }) {
  const post = await getPost(params.slug);
  const text = post.title + post.description;

  const interMedium = await loadGoogleFont("Inter", 500, text);
  const interRegular = await loadGoogleFont("Inter", 400, text);

  return new ImageResponse(
    (
      <div
        style={{
          background: "#fafafa",
          width: "100%",
          height: "100%",
          display: "flex",
          flexDirection: "column",
          justifyContent: "center",
          padding: "80px",
          fontFamily: "Inter",
        }}
      >
        <div
          style={{
            fontSize: 64,
            fontWeight: 500,
            color: "#171717",
            lineHeight: 1.1,
            marginBottom: 24,
            letterSpacing: "-0.02em",
          }}
        >
          {post.title}
        </div>
        <div
          style={{
            fontSize: 28,
            fontWeight: 400,
            color: "#737373",
          }}
        >
          {post.description}
        </div>
      </div>
    ),
    {
      ...size,
      fonts: [
        { name: "Inter", data: interMedium, weight: 500 },
        { name: "Inter", data: interRegular, weight: 400 },
      ],
    }
  );
}
```

**Note:** The `loadGoogleFont` function fetches font CSS from Google, parses it to extract the actual font binary URL, then loads that. This is required because Satori (used by `ImageResponse`) only supports TTF/OTF formats, not WOFF2.

### Checklist

- [ ] Static OG image at 1200x630
- [ ] PNG format (not SVG)
- [ ] Dynamic OG images for content pages (if applicable)
- [ ] Tested at https://opengraph.xyz

---

## 3. Web App Manifest

Required for PWA support and "Add to Home Screen" functionality.

### Location

`/public/site.webmanifest`

### Template

```json
{
  "name": "Project Full Name",
  "short_name": "Short Name",
  "description": "Brief project description",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    {
      "src": "/web-app-manifest-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/web-app-manifest-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

### Checklist

- [ ] Manifest file created
- [ ] Referenced in metadata: `manifest: "/site.webmanifest"`
- [ ] Icons match favicon set

---

## 4. Metadata Configuration

### Complete layout.tsx Metadata

```tsx
import type { Metadata } from "next";

const baseUrl = process.env.NEXT_PUBLIC_BASE_URL || "https://your-domain.com";

export const metadata: Metadata = {
  metadataBase: new URL(baseUrl),

  title: {
    default: "Project Name — Tagline",
    template: "%s | Project Name",
  },

  description:
    "Compelling 150-160 character description with primary keywords.",

  keywords: ["primary-keyword", "secondary-keyword", "brand-name"],

  authors: [{ name: "Author Name", url: "https://author-site.com" }],
  creator: "Creator Name",
  publisher: "Publisher Name",

  // Canonical URL
  alternates: {
    canonical: "/",
  },

  // Search engine verification
  verification: {
    google: "your-google-verification-code",
    other: {
      "msvalidate.01": "your-bing-verification-code",
    },
  },

  // Open Graph
  openGraph: {
    type: "website",
    locale: "en_US",
    url: "/",
    siteName: "Project Name",
    title: "Project Name — Tagline",
    description: "Same or similar to main description",
    images: [
      {
        url: "/opengraph-image.png",
        width: 1200,
        height: 630,
        alt: "Project Name — descriptive alt text",
      },
    ],
  },

  // Twitter/X Cards
  twitter: {
    card: "summary_large_image",
    title: "Project Name — Tagline",
    description: "Same or similar to main description",
    creator: "@twitterhandle",
    site: "@twitterhandle",
    images: ["/opengraph-image.png"],
  },

  // Robots
  robots: {
    index: true,
    follow: true,
    googleBot: {
      index: true,
      follow: true,
      "max-video-preview": -1,
      "max-image-preview": "large",
      "max-snippet": -1,
    },
  },

  // Icons (modern approach)
  icons: {
    icon: [
      { url: "/favicon.ico", sizes: "48x48" },
      { url: "/favicon.svg", type: "image/svg+xml" },
      { url: "/favicon-96x96.png", sizes: "96x96", type: "image/png" },
    ],
    apple: "/apple-touch-icon.png",
    shortcut: "/favicon.ico",
  },

  manifest: "/site.webmanifest",
  applicationName: "Project Name",
};
```

### Per-Page Metadata

```tsx
// app/about/page.tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: "About Us",
  description: "Learn about our mission and team.",
  alternates: {
    canonical: "/about",
  },
  openGraph: {
    title: "About Us | Project Name",
    description: "Learn about our mission and team.",
    url: "/about",
  },
};
```

### Checklist

- [ ] metadataBase configured
- [ ] Title with template pattern
- [ ] Description under 160 characters
- [ ] authors/creator/publisher set
- [ ] Canonical URL set
- [ ] OpenGraph fully configured
- [ ] Twitter card configured
- [ ] Icons referenced
- [ ] Verification codes added (if applicable)

---

## 5. URL Structure & Routing

### next.config.ts

```ts
const nextConfig = {
  // Pick one and be consistent
  trailingSlash: false,

  // Preserve link equity from old URLs
  async redirects() {
    return [
      {
        source: "/old-page",
        destination: "/new-page",
        permanent: true, // 301 redirect
      },
    ];
  },
};
```

### Best Practices

| Do | Don't |
|-------|---------|
| `/blog/my-post-title` | `/blog/post?id=123` |
| `/products/widget` | `/products/Widget` |
| `/about` | `/about/` (pick one) |
| `/docs/getting-started` | `/docs/getting_started` |

### Checklist

- [ ] Trailing slash policy configured
- [ ] No uppercase letters in URLs
- [ ] No query parameters for indexable pages
- [ ] 301 redirects for changed URLs

---

## 6. Content SEO & On-Page Optimization

### Heading Hierarchy

```tsx
// Correct
<h1>Page Title</h1>           // One per page
  <h2>Major Section</h2>
    <h3>Subsection</h3>
  <h2>Another Section</h2>

// Incorrect - skipping levels
<h1>Title</h1>
  <h4>Subsection</h4>         // Skipped h2 and h3
```

### Title Tag Guidelines

| Rule | Guideline |
|------|-----------|
| Length | 50-60 characters |
| Keywords | Primary keyword near front |
| Uniqueness | Never duplicate across pages |
| Branding | Include brand name |

### Meta Description Guidelines

| Rule | Guideline |
|------|-----------|
| Length | 150-160 characters |
| Content | Compelling, includes call-to-action |
| Keywords | Include naturally, don't stuff |
| Uniqueness | Unique per page |

### Image Alt Text

```tsx
// Good - descriptive, contextual
<Image
  src="/dashboard.png"
  alt="Analytics dashboard showing monthly revenue chart with 23% growth"
/>

// Bad
<Image src="/dashboard.png" alt="screenshot" />
<Image src="/dashboard.png" alt="dashboard image picture analytics" />
```

### Internal Linking

```tsx
// Good - descriptive anchor text
<p>
  Learn more about our <Link href="/components/button">Button component</Link>.
</p>

// Bad - generic anchor text
<p>
  Click <Link href="/components/button">here</Link> to learn more.
</p>
```

### Checklist

- [ ] One H1 per page
- [ ] Proper heading hierarchy (no skipped levels)
- [ ] Title tags under 60 characters
- [ ] Meta descriptions under 160 characters
- [ ] All images have descriptive alt text
- [ ] Internal links use descriptive anchor text

---

## 7. robots.ts

### Location

`/app/robots.ts`

### Template

```tsx
import type { MetadataRoute } from "next";

export default function robots(): MetadataRoute.Robots {
  const baseUrl = process.env.NEXT_PUBLIC_BASE_URL || "https://your-domain.com";

  return {
    rules: [
      {
        userAgent: "*",
        allow: "/",
        disallow: ["/api/", "/private/", "/_next/", "/admin/"],
      },
    ],
    sitemap: `${baseUrl}/sitemap.xml`,
    host: baseUrl,
  };
}
```

### Checklist

- [ ] robots.ts created
- [ ] Sitemap URL included
- [ ] Private routes disallowed

---

## 8. sitemap.ts

### Location

`/app/sitemap.ts`

### Template

```tsx
import type { MetadataRoute } from "next";

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const baseUrl = process.env.NEXT_PUBLIC_BASE_URL || "https://your-domain.com";

  // Static routes
  const staticRoutes: MetadataRoute.Sitemap = [
    {
      url: baseUrl,
      lastModified: new Date(),
      changeFrequency: "weekly",
      priority: 1.0,
    },
    {
      url: `${baseUrl}/about`,
      lastModified: new Date(),
      changeFrequency: "monthly",
      priority: 0.7,
    },
  ];

  // Dynamic routes (fetch from CMS/database)
  const posts = await getAllPosts();
  const dynamicRoutes: MetadataRoute.Sitemap = posts.map((post) => ({
    url: `${baseUrl}/blog/${post.slug}`,
    lastModified: new Date(post.updatedAt),
    changeFrequency: "weekly" as const,
    priority: 0.8,
  }));

  return [...staticRoutes, ...dynamicRoutes];
}
```

### Checklist

- [ ] sitemap.ts created
- [ ] All public pages included
- [ ] Dynamic content pages generated
- [ ] Sitemap submitted to Search Console

---

## 9. JSON-LD Structured Data

### Reusable Component

```tsx
// components/json-ld.tsx
export function JsonLd({ data }: { data: Record<string, unknown> }) {
  return (
    <script
      type="application/ld+json"
      dangerouslySetInnerHTML={{ __html: JSON.stringify(data) }}
    />
  );
}
```

### Schema Types by Project Type

**Organization/Company:**

```tsx
const organizationSchema = {
  "@context": "https://schema.org",
  "@type": "Organization",
  name: "Company Name",
  url: "https://your-domain.com",
  logo: "https://your-domain.com/logo.png",
  sameAs: ["https://github.com/username", "https://twitter.com/handle"],
};
```

**Software/Library:**

```tsx
const softwareSchema = {
  "@context": "https://schema.org",
  "@type": "SoftwareSourceCode",
  name: "Library Name",
  description: "Brief description",
  url: "https://your-domain.com",
  codeRepository: "https://github.com/username/repo",
  programmingLanguage: ["TypeScript", "React"],
  author: {
    "@type": "Person",
    name: "Author Name",
    url: "https://author-site.com",
  },
  license: "https://opensource.org/licenses/MIT",
};
```

**Person/Portfolio:**

```tsx
const personSchema = {
  "@context": "https://schema.org",
  "@type": "Person",
  name: "Your Name",
  url: "https://your-site.com",
  image: "https://your-site.com/photo.png",
  jobTitle: "Your Title",
  worksFor: {
    "@type": "Organization",
    name: "Company Name",
  },
  sameAs: [
    "https://github.com/username",
    "https://linkedin.com/in/username",
    "https://twitter.com/handle",
  ],
  knowsAbout: ["Skill 1", "Skill 2", "Skill 3"],
};
```

**Article/Blog Post:**

```tsx
const articleSchema = {
  "@context": "https://schema.org",
  "@type": "Article",
  headline: post.title,
  description: post.excerpt,
  image: post.ogImage,
  datePublished: post.publishedAt,
  dateModified: post.updatedAt,
  author: {
    "@type": "Person",
    name: "Author Name",
  },
};
```

### Checklist

- [ ] Appropriate schema type chosen
- [ ] Schema added to layout or page
- [ ] Validated at https://validator.schema.org
- [ ] Tested with Google Rich Results Test

---

## 10. Error Pages

### 404 Not Found

```tsx
// app/not-found.tsx
import Link from "next/link";
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: "Page Not Found",
  robots: { index: false, follow: true },
};

export default function NotFound() {
  return (
    <div className="flex min-h-screen flex-col items-center justify-center">
      <h1 className="text-4xl font-bold">404</h1>
      <p className="mt-4 text-gray-600">Page not found</p>
      <Link href="/" className="mt-8 underline">
        Go home
      </Link>
    </div>
  );
}
```

### Error Boundary

```tsx
// app/error.tsx
"use client";

export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <div className="flex min-h-screen flex-col items-center justify-center">
      <h1 className="text-4xl font-bold">Something went wrong</h1>
      <button onClick={reset} className="mt-8 underline">
        Try again
      </button>
    </div>
  );
}
```

### Checklist

- [ ] Custom 404 page created
- [ ] Custom error page created
- [ ] 404 page has `index: false` in robots

---

## 11. Security Headers

### next.config.ts

```ts
const securityHeaders = [
  { key: "X-DNS-Prefetch-Control", value: "on" },
  {
    key: "Strict-Transport-Security",
    value: "max-age=63072000; includeSubDomains; preload",
  },
  { key: "X-Content-Type-Options", value: "nosniff" },
  { key: "Referrer-Policy", value: "strict-origin-when-cross-origin" },
  { key: "X-Frame-Options", value: "SAMEORIGIN" },
  { key: "X-XSS-Protection", value: "1; mode=block" },
  {
    key: "Permissions-Policy",
    value: "camera=(), microphone=(), geolocation=()",
  },
];

const nextConfig = {
  async headers() {
    return [{ source: "/:path*", headers: securityHeaders }];
  },
};
```

### Checklist

- [ ] Security headers configured
- [ ] HTTPS enforced (HSTS)
- [ ] Clickjacking prevented (X-Frame-Options)

---

## 12. Performance & Core Web Vitals

### Image Optimization

```ts
// next.config.ts
const nextConfig = {
  images: {
    formats: ["image/avif", "image/webp"],
    minimumCacheTTL: 31536000,
  },
};
```

### Font Loading

```tsx
// app/layout.tsx
import { Inter } from "next/font/google";

const inter = Inter({
  subsets: ["latin"],
  display: "swap", // Prevents Flash of Invisible Text
  variable: "--font-inter",
});
```

### Script Loading

```tsx
import Script from "next/script";

// Analytics - after page is interactive
<Script src="https://analytics.com/script.js" strategy="afterInteractive" />

// Non-critical - when browser is idle
<Script src="https://widget.com/embed.js" strategy="lazyOnload" />
```

### Core Web Vitals Targets

| Metric | Target | Measures |
|--------|--------|----------|
| LCP | < 2.5s | Loading |
| INP | < 200ms | Interactivity |
| CLS | < 0.1 | Visual stability |

### Checklist

- [ ] Image optimization configured
- [ ] Fonts use `display: swap`
- [ ] Scripts use appropriate loading strategy
- [ ] Above-fold images use `priority`
- [ ] Lighthouse score 90+ on all metrics
- [ ] Core Web Vitals passing in Search Console

---

## 13. Search Console & Webmaster Tools

### Setup Steps

1. **Google Search Console**
   - Go to https://search.google.com/search-console
   - Add property (URL prefix)
   - Verify via HTML meta tag or DNS
   - Submit sitemap

2. **Bing Webmaster Tools**
   - Go to https://www.bing.com/webmasters
   - Import from Google Search Console
   - Submit sitemap

### What to Monitor

| Item | Frequency | Action |
|------|-----------|--------|
| Index coverage | Weekly | Fix excluded/error pages |
| Core Web Vitals | Monthly | Address "Poor" URLs |
| Mobile usability | Monthly | Fix mobile issues |
| Security issues | Immediately | Address flagged issues |

### Checklist

- [ ] Google Search Console verified
- [ ] Sitemap submitted to Google
- [ ] Bing Webmaster Tools set up
- [ ] Sitemap submitted to Bing
- [ ] Monthly monitoring scheduled

---

## 14. Ongoing Monitoring

### Weekly

- [ ] Check Search Console for crawl errors
- [ ] Monitor Core Web Vitals
- [ ] Review new 404s in analytics

### Monthly

- [ ] Full Lighthouse audit
- [ ] Review Search Console performance
- [ ] Check for broken links
- [ ] Update sitemap if pages added

### Quarterly

- [ ] Comprehensive SEO audit
- [ ] Content refresh for outdated pages
- [ ] Review and update meta descriptions
- [ ] Validate structured data

---

## File Structure Reference

```
/app
├── favicon.ico
├── favicon.svg
├── favicon-96x96.png
├── apple-touch-icon.png
├── web-app-manifest-192x192.png
├── web-app-manifest-512x512.png
├── opengraph-image.png
├── robots.ts
├── sitemap.ts
├── not-found.tsx
├── error.tsx
└── layout.tsx

/public
├── site.webmanifest
└── {project}-logo.svg

/components
└── json-ld.tsx
```

---

## Tools & Resources

### Testing Tools

| Tool | Purpose |
|------|---------|
| [opengraph.xyz](https://opengraph.xyz) | OG preview |
| [PageSpeed Insights](https://pagespeed.web.dev) | Performance audit |
| [Schema Validator](https://validator.schema.org) | JSON-LD validation |
| [Rich Results Test](https://search.google.com/test/rich-results) | Google rich results |
| [Facebook Debugger](https://developers.facebook.com/tools/debug/) | Facebook/Meta OG |
| [Twitter Card Validator](https://cards-dev.twitter.com/validator) | Twitter cards |

### Generation Tools

| Tool | Purpose |
|------|---------|
| [RealFaviconGenerator](https://realfavicongenerator.net/) | Complete favicon set |
| [Squoosh](https://squoosh.app/) | Image optimization |
| [SVGOMG](https://jakearchibald.github.io/svgomg/) | SVG optimization |

---

## Implementation Priority

| Priority | Item | Impact | Effort |
|----------|------|--------|--------|
| 1 | metadataBase + metadata | High | Low |
| 2 | Canonical URLs | High | Low |
| 3 | Favicon set | Medium | Low |
| 4 | OG image (PNG, 1200x630) | High | Medium |
| 5 | robots.ts + sitemap.ts | High | Low |
| 6 | Search Console setup | High | Low |
| 7 | Heading hierarchy | Medium | Medium |
| 8 | JSON-LD schema | Medium | Medium |
| 9 | Error pages | Medium | Low |
| 10 | Security headers | Low | Low |
| 11 | Performance optimization | High | High |

---

## Quick Checklist Summary

### Critical

- [ ] metadataBase configured
- [ ] Canonical URLs on all pages
- [ ] Complete favicon set
- [ ] OG image (1200x630 PNG)
- [ ] robots.ts + sitemap.ts
- [ ] Search Console verified

### Important

- [ ] Full metadata (OG, Twitter, icons)
- [ ] authors/creator/publisher
- [ ] One H1 per page
- [ ] Descriptive image alt text
- [ ] JSON-LD structured data
- [ ] Custom 404/error pages
- [ ] Security headers

### Maintenance

- [ ] Web manifest
- [ ] Bing Webmaster Tools
- [ ] Performance optimization
- [ ] Monthly monitoring schedule

---

*Last updated: January 2026*
