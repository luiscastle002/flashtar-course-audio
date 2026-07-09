# Flashtar Course Audio Repository

This repository serves as the dedicated, decoupled storage for all official Flashtar course pronunciation audio assets. By separating these static resources from the main codebase, we keep the main repository lightweight and simplify CDN caching/deployments.

---

## 📂 Repository Structure

The audio files mirror the official course hierarchy:

```text
flashtar-course-audio/
├── README.md
├── LICENSE
├── .gitignore
├── audio/
│   ├── japanese/
│   │   ├── hiragana/
│   │   ├── katakana/
│   │   ├── n5/
│   │   ├── n4/
│   │   ├── n3/
│   │   ├── n2/
│   │   └── n1/
│   ├── english/
│   │   ├── a1/
│   │   ├── a2/
│   │   ├── b1/
│   │   ├── b2/
│   │   ├── c1/
│   │   └── c2/
│   ├── spanish/
│   │   ├── a1/
│   │   ├── a2/
│   │   ├── b1/
│   │   ├── b2/
│   │   ├── c1/
│   │   └── c2/
│   └── (future-language)/
│       └── (future-course)/
└── metadata/
    └── (reserved for manifests and course mapping files if needed)
```

---

## 🏷️ Audio Naming Convention

All audio files in the `audio/` directory must adhere strictly to these rules:

1. **UUID-based Naming**: The filename **MUST** correspond exactly to the card's UUID in the Flashtar course database (e.g., `a0000000-0000-0000-0000-000000000061.mp3`).
2. **Strictly Lowercase**: All hex characters in the UUID and the `.mp3` extension must be lowercase.
3. **No Language/Vocabulary Metadata in Filename**: Avoid names like `ohayou.mp3` or `hello.mp3`. Decoupling transcription from filenames avoids breaking links when cards are updated or edited.

---

## 🌐 Serving Assets & CDN Integration

This repository is designed to be hosted statically (e.g., via GitHub Pages) and optionally proxied behind a CDN (such as Cloudflare R2, custom domains, or Supabase Storage).

### GitHub Pages URL Structure
When hosted, assets are served at:
```text
https://<github-user>.github.io/flashtar-course-audio/audio/<language>/<course_level>/<card_uuid>.mp3
```

### Decoupled CDN Migration
Because the folder hierarchy is logical and flat, transitioning to a custom domain (e.g., `audio.flashtar.app`) or object storage (Cloudflare R2/Supabase Storage) requires only modifying the base URL in the main client application. No path transformations are necessary.

---

## 📈 Scalability Guidelines (20,000+ Files)

To keep this repository performant and easy to maintain as it grows:

1. **Git Large File Storage (Git LFS)**: 
   * As the audio library grows past a few thousand files, local clones can become slow. We recommend configuring Git LFS to track `audio/**/*.mp3` files.
2. **Caching Strategy**:
   * Statically served audio rarely changes. Set aggressive `Cache-Control` headers (e.g., `public, max-age=31536000, immutable`) on the CDN to minimize egress bandwith and optimize load times for students.
3. **Directory Constraints**:
   * Keeping files organized within language/course directories helps avoid OS/file system slowdowns associated with holding tens of thousands of files in a single flat directory.
