# How EU websites handle cookie consent in 2026: open data

Aggregate results of a scan of 300 public homepages in the Netherlands, Germany, France, Italy and Spain on 20 November 2026: which sites show a consent banner, what loads before anyone clicks, how Google Consent Mode v2 is set up and which consent tools sites use. Totals only: no site is named, and there is no per-site data in this repository.

- **Full study, charts and method:** https://privacy-pill-consent-studio.lovable.app/research/eu-cookie-consent-2026
- **Check your own site with the same checks (free, no sign-up):** https://privacy-pill-consent-studio.lovable.app/scanner
- **Licence:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), credit “Kalmwerk / PrivacyPill” and link the study page

The study, the scanner and this repository are made by Kalmwerk, which also makes PrivacyPill, a cookie consent tool. The links above go to our own site.

## Headline findings (20 November 2026)

- **57%** (171 of 300) show a consent banner in their page code. 43% through a consent tool, 14% with a banner of their own.
- **21%** (64 of 300) load an advertising or marketing tracker before any choice. Confirmed in the page code. Most often Meta Pixel, Google Ads & Floodlight and HubSpot.
- **48%** (143 of 300) load at least one tracker that needs consent before any choice, analytics included. Some regulators allow privacy-friendly analytics without consent; a page scan can't see that setting.
- **25%** (76 of 300) show no banner, but load or hold back trackers that need consent. 13% of all sites with advertising or marketing trackers among them.
- **19%** (33 of 174) of sites with a Google tag deny all four Consent Mode v2 signals by default. 33% set no default we could find; 40% leave it to a consent tool or Tag Manager.
- **41%** (123 of 300) have a tracker whose timing a page scan can't confirm. For example a Tag Manager tag whose trigger the page doesn't show. We count these apart instead of guessing.
- **Consent tools are local:** Cookiebot leads in the Netherlands, Usercentrics leads in Germany, Axeptio leads in France, Complianz leads in Italy and CookieYes leads in Spain.

Shares are of all 300 sites unless the line says otherwise. The results say what we detected, not whether a site follows the law. This isn't legal advice.

## Files

| File | What it is |
| --- | --- |
| [`data/eu-cookie-consent-2026.csv`](data/eu-cookie-consent-2026.csv) | The aggregates: 377 rows, one per metric, group and answer (sha256 `44730125100d8a292d5e48ed5553f41ec6ffea9d84a68926a02235fb6725931a`) |
| [`datapackage.json`](datapackage.json) | [Frictionless Data](https://datapackage.org/) package: fields, types, allowed values and licence |
| [`CITATION.cff`](CITATION.cff) | Citation metadata (GitHub's “Cite this repository”) |
| [`LICENSE`](LICENSE) | CC BY 4.0 legal code |

The CSV is a byte-for-byte copy of the one published with the study, at https://privacy-pill-consent-studio.lovable.app/research/eu-cookie-consent-2026.csv. It is also attached to release [v2026.11](https://github.com/kalmwerk/eu-cookie-consent-2026/releases/tag/v2026.11). Published files never change, so figures cited from them stay checkable.

```sh
curl -LO https://github.com/kalmwerk/eu-cookie-consent-2026/releases/download/v2026.11/eu-cookie-consent-2026.csv
```

## Columns

| Column | Type | Meaning |
| --- | --- | --- |
| `metric` | string | What is counted; see the metrics table. |
| `dimension` | string | How the sites are grouped: all, country or category (type of site). |
| `group` | string | The group: all; a country (nl, de, fr, it, es); or a type of site (news, ecommerce, government, saas, local). |
| `answer` | string | The answer counted, such as tool, own or none; a tool or tracker name for consent_tool and tracker_confirmed_before_consent. |
| `sites` | integer | Sites with that answer. |
| `base` | integer | Sites the share is of. |
| `share_pct` | number | sites / base in percent, one decimal. |
| `ci95_low_pct` | number | Lower end of the 95% Wilson score interval, in percent. |
| `ci95_high_pct` | number | Upper end of the 95% Wilson score interval, in percent. |

## Metrics

| `metric` | `answer` | Share of | Meaning |
| --- | --- | --- | --- |
| `banner` | `tool`, `own`, `none` | all sites in the group | Consent banner in the page code: through a consent tool, a banner of the site's own, or none found. |
| `no_banner_but_consent_needed` | `yes` | all sites in the group | No banner found, while the page loads or holds back trackers or cookies that need consent. |
| `no_banner_but_marketing_tracker` | `yes` | all sites in the group | Of those: an advertising or marketing tracker among them (loading, unclear or held back). |
| `first_layer_reject` | `button`, `continue_without`, `none`, `unreadable` | sites with a banner | Way to refuse on the banner's first layer: reject button, “continue without” link, none, or settings not in the page (unreadable). |
| `pre_consent` | `confirmed`, `unconfirmed`, `held`, `none` | all sites in the group | Trackers that need consent before any choice. Each site counts once, under its strongest finding: confirmed, couldn't confirm (unconfirmed), held back until consent, or nothing that needs consent. |
| `marketing_tracker_confirmed_before_consent` | `yes` | all sites in the group | At least one advertising or marketing tracker confirmed before consent. |
| `analytics_tracker_confirmed_before_consent` | `yes` | all sites in the group | At least one analytics tracker confirmed before consent. |
| `consent_cookie_on_first_response` | `yes` | all sites in the group | The server sets a cookie that needs consent on the first response. |
| `could_not_confirm` | `yes` | all sites in the group | At least one tracker that needs consent whose timing the page code doesn't settle. |
| `low_confidence` | `yes` | all sites in the group | The scanner read the page with low confidence (for example an app shell or a Tag Manager container it couldn't read). |
| `google_tag` | `yes` | all sites in the group | A Google tag: Google Analytics (gtag.js), Google Tag Manager or Google Ads. |
| `consent_mode_default` | `all_denied`, `denied_without_v2`, `regional_denied`, `grants`, `set_elsewhere`, `no_default` | sites with a Google tag | Google Consent Mode v2 default for EEA visitors: all four signals denied, only the two older ones denied, denied for a list of regions that doesn't clearly cover every EU country, at least one granted, left to a consent tool or Tag Manager, or no default found. |
| `consent_mode_default_after_google_tag` | `yes` | sites whose default position could be read | The Consent Mode default comes after the first Google tag. |
| `consent_tool` | a name | sites with a recognised consent tool in the group | Main consent tool. Tools found on only one site of the whole sample are grouped as “Other tools”. Only for dimension all and country. |
| `tracker_confirmed_before_consent` | a name | all sites | Trackers (vendors, not sites) most often confirmed before consent, ranked only when found on at least two sites. Only for dimension all. |

## Method

### The sample

300 homepages: 12 for each of 5 types of site in each of 5 countries. News sites, online shops, government bodies and software companies are organisations that Wikidata lists for that country and whose website is also among the sites most visited from that country in Chrome's user experience report (October 2026; top 100,000, or top million for software companies).

Local businesses are shops, crafts, restaurants, cafés and similar places with their own website in OpenStreetMap, within about 6 km of the centre of Groningen, Utrecht and Eindhoven (the Netherlands), Leipzig, Münster and Freiburg (Germany), Lyon, Nantes and Montpellier (France), Bologna, Turin and Bari (Italy) and Valencia, Seville and Zaragoza (Spain). Chains are left out.

Each list is shuffled with a fixed seed, so the draw can be repeated. Each organisation and each domain is in the sample once.

When a site couldn't be scanned, the next site of the same list took its place: we tried 359 sites for 300 scans. Skipped: 24 refused our request, 16 had a robots.txt we couldn't read, which counts as “don't visit”, 8 didn't answer or no longer exist, 6 returned an error page and 5 ask crawlers like ours not to visit in robots.txt, at their own address or the one they redirect to (59 in all).

### The visit

One visit per site, on 20 November 2026: its robots.txt, then the homepage (following redirects; when one led to another site, that site's robots.txt had to allow us too), then the Google Tag Manager containers it loads, from googletagmanager.com. Two sites at a time with pauses in between, from servers in the EU, in the country's language, with a user agent that names us and links to the study page.

### The analysis

The same analysis as our free scanner and Consent Mode v2 checker. It reads the page and its Tag Manager containers without running them, and sorts each tracker into “loads before consent”, “waits for consent” or “couldn't confirm”. Trackers count as needing consent unless they're strictly necessary or cookieless; analytics counts.

### The numbers

Shares are of all 300 sites unless a chart says otherwise. Every country and every type of site has the same number of sites, so “all sites” weighs them equally; it isn't an estimate for the web as a whole. The ranges are 95% Wilson score intervals: with 60 sites in a group, a difference of less than about 15 points can be chance.

### Sources

- [Chrome UX Report country top lists, October 2026 (crux-top-lists)](https://github.com/zakird/crux-top-lists)
- [Wikidata (instance of, industry, country, official website)](https://www.wikidata.org/)
- [OpenStreetMap data © OpenStreetMap contributors (ODbL), via Geofabrik and BBBike extracts](https://www.openstreetmap.org/copyright)
- [RFC 9309: Robots Exclusion Protocol](https://www.rfc-editor.org/rfc/rfc9309)

## Limitations

In short: one visit, one location, not legal advice.

- One visit, one moment: sites change, and tests or regional settings can show something else to other visitors.
- One location: our servers are in the EU. Visitors elsewhere may see a different banner, or none.
- Page code, not a browser: banners and tags that scripts add later can be missed, which is why “couldn't confirm” exists.
- Homepages only: other pages of a site can load other things.
- Types of site come from Wikidata and OpenStreetMap and are sometimes rough.
- 300 sites are a sample. Small groups, such as one tool in one country, have wide ranges.
- The results say what we detected, not whether a site follows the law. This isn't legal advice.

## How to cite

Kalmwerk / PrivacyPill (2026). How EU websites handle cookie consent in 2026: a scan of 300 homepages in the Netherlands, Germany, France, Italy and Spain. https://privacy-pill-consent-studio.lovable.app/research/eu-cookie-consent-2026 (data: CC BY 4.0)

```bibtex
@misc{kalmwerk_eu_cookie_consent_2026,
  author       = {{Kalmwerk}},
  title        = {{How EU websites handle cookie consent in 2026}},
  year         = {2026},
  publisher    = {PrivacyPill},
  version      = {2026.11},
  url          = {https://privacy-pill-consent-studio.lovable.app/research/eu-cookie-consent-2026},
  note         = {Aggregate data of 300 homepages, CC BY 4.0. https://github.com/kalmwerk/eu-cookie-consent-2026}
}
```

GitHub's “Cite this repository” button reads the same details from `CITATION.cff`.

## For site owners

If your site was in the sample: we publish totals only and never name a site. We kept no page content, cookies or anything about visitors, only the findings per site (such as “banner found”), privately, to check the totals.

To stay out of future runs, add this to your robots.txt; we read it before every visit. Or email kalmwerk@gmail.com and we'll leave your domain out.

```
User-agent: PrivacyPillResearch
Disallow: /
```

Our crawler identifies itself as: `Mozilla/5.0 (compatible; PrivacyPillResearch/1.0; +https://privacy-pill-consent-studio.lovable.app/research/eu-cookie-consent-2026#for-site-owners)`

## Next run

We repeat the study every three months with the same method; the next run is planned for February 2027. Each run adds its own CSV to this repository and gets its own release; earlier files stay as they were.

## Summaries in other languages

- [Deutsch: Zusammenfassung mit Blick auf Deutschland](https://privacy-pill-consent-studio.lovable.app/de/cookie-banner-studie-2026)
- [Français : synthèse centrée sur la France](https://privacy-pill-consent-studio.lovable.app/fr/etude-bandeaux-cookies-2026)
- [Italiano: sintesi con focus sull'Italia](https://privacy-pill-consent-studio.lovable.app/it/studio-cookie-banner-2026)
- [Español: resumen centrado en España](https://privacy-pill-consent-studio.lovable.app/es/estudio-banners-cookies-2026)

## Licence

The data is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (see [`LICENSE`](LICENSE)): share and adapt it, also commercially, as long as you credit “Kalmwerk / PrivacyPill” and link https://privacy-pill-consent-studio.lovable.app/research/eu-cookie-consent-2026. Questions or corrections: kalmwerk@gmail.com.
