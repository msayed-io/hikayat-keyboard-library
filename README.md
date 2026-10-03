# Hikayat Keyboard Library

A production-ready media library for the **Hikayat Keyboard** wallpaper application.

## Source

- Pinterest board: [كيبورد دار الحكايات](https://www.pinterest.com/mohamed01140251843sayed/%D9%83%D9%8A%D8%A8%D9%88%D8%B1%D8%AF-%D8%AF%D8%A7%D8%B1-%D8%A7%D9%84%D8%AD%D9%83%D8%A7%D9%8A%D8%A7%D8%AA/)
- Board size at initial extraction: **29 pins**
- Machine-readable catalog: [`metadata/manifest.json`](metadata/manifest.json)
- Curated visual preview: [`curated-preview.jpg`](curated-preview.jpg)

## Visual style

The curated set follows the visual language of the original library:

- Wide landscape compositions, suitable for wallpaper cropping and keyboard backgrounds
- Nature-led scenes: forests, lakes, mountains, meadows, mist, sunsets, and quiet outdoor moments
- Cinematic light: golden hour, soft haze, sunbeams, reflective water, and calm blue/green palettes
- A balanced mix of photographic landscapes and peaceful anime/illustrated scenes
- Portrait and narrow vertical pins are excluded; curated images use an aspect ratio of at least **1.45:1**

## Repository layout

```text
assets/
  images/            Original board assets
  images/curated/    33 additional Pinterest-discovered landscape images
  videos/            Reserved for original video files
metadata/
  manifest.json      Machine-readable catalog, checksums, and source URLs
  pin_ids.txt        Source Pinterest pin IDs
curated-preview.jpg  Visual preview sheet for the curated batch
```

## App integration

Use the `source_url` and repository-relative `file` fields in `metadata/manifest.json` to build a remote catalog. For a release-pinned catalog, use a tag or commit instead of the mutable `main` branch:

```text
https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/main/metadata/manifest.json
```

Each image has a SHA-256 checksum so the client can verify a downloaded file before caching it on-device.

## Quality and extraction notes

- Original board images were downloaded using the best available Pinterest CDN variant.
- The curated batch contains **33 additional horizontal images**: 14 cinematic landscape results and 19 peaceful anime/illustrated landscape results.
- Search sources used: `cinematic nature landscape wallpaper 16:9` and `anime peaceful nature landscape wallpaper 16:9`.
- Pinterest source URLs are retained in the manifest for attribution and auditing.
- Original MP4 URLs were not exposed by Pinterest during the earlier board extraction; the `assets/videos/` directory remains ready for authenticated video exports.

## Usage rights

Confirm that you own or have permission to redistribute every source asset before shipping them in a public application. Pinterest source URLs are retained for attribution and auditing.
