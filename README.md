# Hikayat Keyboard Library

A production-ready media library for the **Hikayat Keyboard** wallpaper application.

## Source

- Pinterest board: [كيبورد دار الحكايات](https://www.pinterest.com/mohamed01140251843sayed/%D9%83%D9%8A%D8%A8%D9%88%D8%B1%D8%AF-%D8%AF%D8%A7%D8%B1-%D8%A7%D9%84%D8%AD%D9%83%D8%A7%D9%8A%D8%A7%D8%AA/)
- Board size at extraction time: **29 pins**
- Generated manifest: [`metadata/manifest.json`](metadata/manifest.json)

## Repository layout

```text
assets/
  images/       Highest CDN image variant available for each unique asset
  videos/       Reserved for original video files when an authenticated export is available
metadata/
  manifest.json Machine-readable catalog with checksums and source URLs
  pin_ids.txt   Source Pinterest pin IDs
```

## App integration

Use the `source_url` and repository-relative `file` fields in `metadata/manifest.json` to build a remote catalog. For a release-pinned catalog, use the raw GitHub URL for a commit or tag instead of the mutable `main` branch:

```text
https://raw.githubusercontent.com/msayed-io/hikayat-keyboard-library/main/metadata/manifest.json
```

Each image has a SHA-256 checksum so the client can verify a downloaded file before caching it on-device.

## Media quality and extraction notes

- Images were downloaded from Pinterest's CDN using the best available `originals` variant, falling back to `736x`, `474x`, or `236x` only when necessary.
- The board page exposed 27 unique image assets from the 29 pins at extraction time.
- Pinterest did not expose downloadable original MP4 URLs in the authenticated board HTML or sampled pin pages. The original pin IDs and source board are preserved so video exports can be added later without changing the catalog contract.
- See [`assets/videos/README.md`](assets/videos/README.md) for the video status.

## Usage rights

Confirm that you own or have permission to redistribute every source asset before shipping them in a public application. Pinterest source URLs are retained for attribution and auditing.
