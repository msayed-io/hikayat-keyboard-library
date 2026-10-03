# Hikayat Keyboard Library

A production-ready media library for the **Hikayat Keyboard** wallpaper application.

## Source and catalog

- Pinterest board: [كيبورد دار الحكايات](https://www.pinterest.com/mohamed01140251843sayed/%D9%83%D9%8A%D8%A8%D9%88%D8%B1%D8%AF-%D8%AF%D8%A7%D8%B1-%D8%A7%D9%84%D8%AD%D9%83%D8%A7%D9%8A%D8%A7%D8%AA/)
- Machine-readable catalog: [`metadata/manifest.json`](metadata/manifest.json)
- Current visual preview: [`curated-preview.jpg`](curated-preview.jpg)

## Current curated collection

The curated collection now contains **50 images** in total. The latest release adds **20 new horizontal landscape images** selected to avoid near-duplicates:

- Coastlines and sea cliffs
- Forest paths and waterfalls
- Mountain lakes and alpine valleys
- Rivers, reflections, and mist
- Desert dunes and wide open terrain
- Sunrise, sunset, and blue-hour landscapes

The replacement batch is landscape-only with a minimum aspect ratio of **1.45:1**, minimum downloaded width of **1000px**, and no visible people, girls, women, cartoons, anime characters, or illustrations in the visual review.

Five images were extracted from the user's authenticated Pinterest search results in My Browser. The remaining fifteen are real photographic files from Wikimedia Commons, with exact source URLs and Commons titles retained in the manifest. This mixed provenance was used because Pinterest stopped loading additional search results reliably during the session; no AI image generation was used.

## Repository layout

```text
assets/
  images/            Original board assets
  images/curated/    50 curated landscape images
  videos/            Reserved for original video files
metadata/
  manifest.json      Machine-readable catalog, checksums, sources, and categories
  pin_ids.txt        Source Pinterest pin IDs
curated-preview.jpg  Visual preview sheet for the current curated batch
```

## App integration

Use the `source_url` and repository-relative `file` fields in `metadata/manifest.json` to build a remote catalog. For a release-pinned catalog, use a tag or commit instead of the mutable `main` branch:

```text
https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/main/metadata/manifest.json
```

Each image has a SHA-256 checksum so the client can verify a downloaded file before caching it on-device.

## Quality and provenance notes

- The original board assets remain under `assets/images/`.
- The `assets/images/curated/` directory contains the cleaned, landscape-only collection.
- Pinterest CDN URLs, authenticated-search provenance, Wikimedia Commons titles, and source URLs are retained in the manifest for auditing.
- The Wikimedia files are photographic source files; Pinterest results were visually screened for obvious illustrations, people, and cartoon content, but source licensing and authenticity should still be reviewed before public redistribution.
- No AI-generated images were created for this release.
- Original MP4 URLs were not exposed by Pinterest during the earlier board extraction; the `assets/videos/` directory remains ready for authenticated video exports.

## Usage rights

Confirm that you own or have permission to redistribute every source asset before shipping them in a public application. Pinterest and Wikimedia source URLs are retained for attribution and auditing.
