# Podcast RSS Feeds

Multi-podcast RSS feed management system with self-contained podcast directories. Each podcast keeps all its assets (audio, images, metadata) in one place.

## Structure

Each podcast is completely self-contained:
```
podcasts/your-podcast-name/
├── feed.xml           # Main RSS feed
├── metadata.json      # Podcast metadata
├── episodes/          # Episode XML files
├── images/            # Podcast cover art and episode thumbnails
└── audio/             # Audio files
```

## Setup Instructions

### 1. Install VSCode Extensions
```bash
code --install-extension redhat.vscode-xml
code --install-extension scholarlyxml.scholarly-xml
```

### 2. Clone and Configure

```bash
git clone https://github.com/yourusername/podcast-feeds.git
cd podcast-feeds
```

Update all URLs in feed.xml files to match your GitHub username:
```
https://yourusername.github.io/podcast-feeds/podcasts/tech-talk/...
```

### 3. Audio File Management

**Option A: Store in Repository (Small Files)**
- Keep audio files in `podcasts/your-podcast/audio/`
- Git handles files up to 100MB
- Use Git LFS for larger files

**Option B: External Hosting (Recommended for Large Files)**
- Host on CDN (CloudFlare, AWS S3, Bunny CDN)
- Update audio file paths in feed.xml to external URLs
- Add audio folder to .gitignore

### 4. Validate Feeds

VSCode will automatically validate as you edit. Or run:
```bash
./validate.sh
```

## Adding a New Podcast

```bash
# Copy template structure
mkdir -p podcasts/my-new-podcast/{episodes,images,audio}

# Copy template files
cp templates/feed-template.xml podcasts/my-new-podcast/feed.xml
cp templates/metadata-template.json podcasts/my-new-podcast/metadata.json

# Edit the files with your podcast details
```

Your new podcast structure:
```
podcasts/my-new-podcast/
├── feed.xml
├── metadata.json
├── episodes/
├── images/
│   └── cover.jpg (add your cover art)
└── audio/
```

## Adding a New Episode

1. **Add audio file**:
```bash
cp your-audio.mp3 podcasts/your-podcast/audio/episode-003.mp3
```

2. **Add episode thumbnail** (optional):
```bash
cp thumbnail.jpg podcasts/your-podcast/images/episode-003-thumbnail.jpg
```

3. **Create episode XML** (optional for reference):
```bash
cp templates/episode-template.xml podcasts/your-podcast/episodes/episode-003.xml
```

4. **Update feed.xml** with new `<item>` entry:
```xml

  Your Episode Title




```

5. **Get file size** for enclosure length:
```bash
# macOS/Linux
stat -f%z podcasts/your-podcast/audio/episode-003.mp3

# Or
ls -l podcasts/your-podcast/audio/episode-003.mp3
```

## GitHub Pages Deployment

1. **Enable GitHub Pages**:
   - Go to repository Settings → Pages
   - Source: Deploy from branch
   - Branch: main, folder: / (root)
   - Save

2. **Your feed URLs will be**:
   - `https://yourusername.github.io/podcast-feeds/podcasts/tech-talk/feed.xml`
   - `https://yourusername.github.io/podcast-feeds/podcasts/business-insights/feed.xml`

3. **Submit to podcast directories**:
   - Apple Podcasts: https://podcastsconnect.apple.com
   - Spotify: https://podcasters.spotify.com
   - Google Podcasts: https://podcastsmanager.google.com

## File Size Considerations

**GitHub Limits**:
- Individual files: 100MB max (strict)
- Repository size: 1GB recommended (soft limit)
- Use Git LFS for files over 50MB

**Audio File Sizes** (approximate):
- 1 hour @ 64kbps = ~29MB ✅ OK for GitHub
- 1 hour @ 128kbps = ~58MB ✅ OK for GitHub
- 1 hour @ 192kbps = ~86MB ✅ OK for GitHub
- 1 hour @ 320kbps = ~144MB ❌ Too large

**Recommendations**:
- For podcasts under 1 hour @ 128kbps → Store in GitHub
- For longer podcasts or higher quality → Use external hosting

## Metadata.json Format

```json
{
  "title": "Tech Talk Podcast",
  "author": "Your Name",
  "email": "your.email@example.com",
  "description": "Weekly tech discussions",
  "category": "Technology",
  "language": "en-us",
  "explicit": false,
  "copyright": "© 2024 Your Name",
  "episodeCount": 3,
  "website": "https://example.com"
}
```

## Validation

The RedHat XML extension validates automatically:
- ✅ Green checkmark = valid
- ❌ Red underline = error (hover for details)

**Online validators**:
- https://podba.se/validate/
- https://www.castfeedvalidator.com/
- https://validator.w3.org/feed/

## Common Issues

**Issue**: Audio file not playing
- Check file size in `<enclosure length="...">` matches actual file
- Verify URL is publicly accessible
- Ensure MIME type is correct (`audio/mpeg` for MP3)

**Issue**: Cover art not showing
- Image should be square (1400x1400 to 3000x3000 recommended)
- Use JPG or PNG format
- File size under 512KB

**Issue**: Feed not updating
- Check GitHub Pages deployment status
- Clear cache: add `?v=timestamp` to feed URL
- Verify `<lastBuildDate>` is updated

## License

[Your chosen license]
