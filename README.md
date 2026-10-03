# Hikayat Keyboard Library

A production-ready media library for the **Hikayat Keyboard** wallpaper application.

## Source

- Pinterest board: [كيبورد دار الحكايات](https://www.pinterest.com/mohamed01140251843sayed/%D9%83%D9%8A%D8%A8%D9%88%D8%B1%D8%AF-%D8%AF%D8%A7%D8%B1-%D8%A7%D9%84%D8%AD%D9%83%D8%A7%D9%8A%D8%A7%D8%AA/)
- Machine-readable catalog: [`metadata/manifest.json`](metadata/manifest.json)
- Current visual preview: [`curated-preview.jpg`](curated-preview.jpg)

## Current curated collection

The previous curated collection containing anime/illustrated material was removed and replaced with **30 new landscape images** selected for this brief:

- Realistic-looking nature scenes only: mountains, lakes, rivers, waterfalls, forests, valleys, deserts, coastlines, and sunsets
- No girls, women, visible people, anime characters, cartoons, or illustrations in the replacement batch
- Landscape orientation only, minimum aspect ratio **1.45:1**
- High-resolution CDN variant selected where Pinterest exposed one; minimum downloaded width is **700px**
- Clear categories are represented in the manifest through the source-search fields and file ordering

## Repository layout

```text
assets/
  images/            Original board assets
  images/curated/    30 replacement realistic landscape images
  videos/            Reserved for original video files
metadata/
  manifest.json      Machine-readable catalog, checksums, and source URLs
  pin_ids.txt        Source Pinterest pin IDs
curated-preview.jpg  Visual preview sheet for the current curated batch
```

## App integration

Use the `source_url` and repository-relative `file` fields in `metadata/manifest.json` to build a remote catalog. For a release-pinned catalog, use a tag or commit instead of the mutable `main` branch:

```text
https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/main/metadata/manifest.json
```

Each image has a SHA-256 checksum so the client can verify a downloaded file before caching it on-device.

## Quality and extraction notes

- The original board assets remain under `assets/images/`.
- The `assets/images/curated/` directory is the cleaned replacement batch; the former 33-image mixed realistic/anime batch is no longer present there.
- Pinterest CDN URLs and search provenance are retained in the manifest for auditing.
- “Realistic” here means a natural photographic/photorealistic visual appearance; Pinterest source provenance should still be reviewed before public redistribution.
- Original MP4 URLs were not exposed by Pinterest during the earlier board extraction; the `assets/videos/` directory remains ready for authenticated video exports.

## Usage rights

Confirm that you own or have permission to redistribute every source asset before shipping them in a public application. Pinterest source URLs are retained for attribution and auditing.
