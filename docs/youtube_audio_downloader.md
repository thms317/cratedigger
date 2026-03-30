# YouTube Audio Downloader

A guide for downloading audio from YouTube videos using yt-dlp. This tool allows you to extract high-quality audio from YouTube videos and convert them to various formats including MP3.

## Quick Start

**Important:** YouTube now requires browser cookies to verify you're not a bot. Use `--cookies-from-browser` with your browser name (chrome, safari, firefox, brave, or edge).

```bash
# Download audio as MP3 (most common)
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome "https://www.youtube.com/watch?v=VIDEO_ID"

# Download to specific directory
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome -o "~/Downloads/%(title)s.%(ext)s" "YOUTUBE_URL"

# Download playlist as MP3
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome "https://www.youtube.com/playlist?list=PLAYLIST_ID"
```

**Note:** Replace `chrome` with your browser: `safari`, `firefox`, `brave`, or `edge`. Make sure you're logged into YouTube in that browser.

## Prerequisites

### Required Software

- **yt-dlp**: Modern YouTube downloader (successor to youtube-dl)
- **ffmpeg**: Required for audio conversion

### Installation

**Check if already installed:**
```bash
which yt-dlp
```

**Install yt-dlp:**
```bash
# macOS (using Homebrew)
brew install yt-dlp

# Linux (using pip)
pip install yt-dlp

# Windows (using Chocolatey)
choco install yt-dlp
```

**Install ffmpeg (if not already installed):**
```bash
# macOS (using Homebrew)
brew install ffmpeg

# Ubuntu/Debian
sudo apt update && sudo apt install ffmpeg

# Windows (using Chocolatey)
choco install ffmpeg
```

## Basic Usage

### Important: Browser Cookies Required

YouTube now requires authentication to prevent bot access. You must use `--cookies-from-browser` to import cookies from your browser.

**Supported browsers:** chrome, safari, firefox, brave, edge

Make sure you're logged into YouTube in the browser you specify.

### Download Audio as MP3

```bash
# Download single video as MP3
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome "https://www.youtube.com/watch?v=dQw4w9WgXcQ"

# Download with best audio quality
yt-dlp -x --audio-format mp3 --audio-quality 0 --cookies-from-browser chrome "YOUTUBE_URL"
```

### Custom Output Location

```bash
# Download to specific directory
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome -o "~/Music/YouTube/%(title)s.%(ext)s" "YOUTUBE_URL"

# Use custom filename pattern
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome -o "%(uploader)s - %(title)s.%(ext)s" "YOUTUBE_URL"
```

## Command-Line Options

### Essential Options

| Option | Description |
|--------|-------------|
| `-x` or `--extract-audio` | Extract audio only (no video) |
| `--audio-format FORMAT` | Convert to format (mp3, m4a, opus, vorbis, flac, wav) |
| `--audio-quality QUALITY` | Audio quality: 0 (best) to 9 (worst) |
| `--cookies-from-browser BROWSER` | Import cookies from browser (chrome, safari, firefox, brave, edge) - **Required** |
| `-o TEMPLATE` | Output filename template |
| `--embed-thumbnail` | Embed video thumbnail as cover art |
| `--add-metadata` | Add metadata (title, artist, etc.) |

### Output Templates

Available template variables:

- `%(title)s` - Video title
- `%(uploader)s` - Channel/uploader name
- `%(artist)s` - Artist name (if available)
- `%(album)s` - Album name (if available)
- `%(id)s` - Video ID
- `%(ext)s` - File extension
- `%(upload_date)s` - Upload date (YYYYMMDD)

## Practical Examples

### Download Music with Metadata

```bash
# Download with thumbnail and metadata embedded
yt-dlp -x --audio-format mp3 \
  --cookies-from-browser chrome \
  --embed-thumbnail \
  --add-metadata \
  "YOUTUBE_URL"
```

### Download Entire Playlist

```bash
# Download all videos in a playlist as MP3
yt-dlp -x --audio-format mp3 \
  --cookies-from-browser chrome \
  -o "~/Music/%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s" \
  "https://www.youtube.com/playlist?list=PLAYLIST_ID"
```

### Download Multiple Videos

```bash
# Create a file with URLs (one per line)
cat > urls.txt <<EOF
https://www.youtube.com/watch?v=VIDEO_ID1
https://www.youtube.com/watch?v=VIDEO_ID2
https://www.youtube.com/watch?v=VIDEO_ID3
EOF

# Download all URLs
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome -a urls.txt
```

### Download Channel's Audio

```bash
# Download all videos from a channel
yt-dlp -x --audio-format mp3 \
  --cookies-from-browser chrome \
  -o "~/Music/%(uploader)s/%(title)s.%(ext)s" \
  "https://www.youtube.com/@CHANNEL_NAME/videos"
```

### Keep Original Audio Format

```bash
# Don't convert, keep original format (usually opus or m4a)
yt-dlp -x --audio-quality 0 --cookies-from-browser chrome "YOUTUBE_URL"
```

## Audio Formats

### Format Comparison

| Format | Quality | File Size | Compatibility | Use Case |
|--------|---------|-----------|---------------|----------|
| **MP3** | Good | Medium | Universal | Best compatibility |
| **M4A** | Better | Medium | Very Good | Apple devices, modern players |
| **OPUS** | Best | Small | Good | Best quality-to-size ratio |
| **FLAC** | Lossless | Large | Good | Archival, audiophiles |
| **WAV** | Lossless | Very Large | Universal | Professional editing |

### Format Examples

```bash
# MP3 (most compatible)
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome "YOUTUBE_URL"

# M4A (better quality, smaller size)
yt-dlp -x --audio-format m4a --cookies-from-browser chrome "YOUTUBE_URL"

# OPUS (best quality-to-size ratio)
yt-dlp -x --audio-format opus --cookies-from-browser chrome "YOUTUBE_URL"

# FLAC (lossless)
yt-dlp -x --audio-format flac --cookies-from-browser chrome "YOUTUBE_URL"
```

## Advanced Usage

### Quality Settings

```bash
# Best possible quality (recommended)
yt-dlp -x --audio-format mp3 --audio-quality 0 --cookies-from-browser chrome "YOUTUBE_URL"

# Balanced quality and size
yt-dlp -x --audio-format mp3 --audio-quality 5 --cookies-from-browser chrome "YOUTUBE_URL"

# Smaller file size
yt-dlp -x --audio-format mp3 --audio-quality 9 --cookies-from-browser chrome "YOUTUBE_URL"
```

### Filter Videos

```bash
# Download only videos shorter than 10 minutes
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome --match-filter "duration < 600" "YOUTUBE_URL"

# Download only videos uploaded after specific date
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome --dateafter 20231001 "YOUTUBE_URL"
```

### Download Speed and Rate Limiting

```bash
# Limit download speed to 1MB/s
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome --limit-rate 1M "YOUTUBE_URL"

# Add delay between downloads in playlist
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome --sleep-interval 5 "PLAYLIST_URL"
```

### Resume Failed Downloads

```bash
# Continue interrupted downloads
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome --continue "YOUTUBE_URL"

# Retry failed downloads
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome --retries 10 "YOUTUBE_URL"
```

## Organizing Downloads

### Folder Structure Examples

**By Artist:**
```bash
yt-dlp -x --audio-format mp3 \
  --cookies-from-browser chrome \
  -o "~/Music/%(artist)s/%(title)s.%(ext)s" \
  "YOUTUBE_URL"
```

**By Playlist:**
```bash
yt-dlp -x --audio-format mp3 \
  --cookies-from-browser chrome \
  -o "~/Music/%(playlist)s/%(playlist_index)02d - %(title)s.%(ext)s" \
  "PLAYLIST_URL"
```

**By Date:**
```bash
yt-dlp -x --audio-format mp3 \
  --cookies-from-browser chrome \
  -o "~/Music/%(upload_date)s/%(title)s.%(ext)s" \
  "YOUTUBE_URL"
```

## Troubleshooting

### Common Issues

**"yt-dlp: command not found"**
```bash
# Install yt-dlp
brew install yt-dlp  # macOS
pip install yt-dlp   # Linux/Windows
```

**"ffmpeg not found"**
```bash
# Install ffmpeg
brew install ffmpeg  # macOS
sudo apt install ffmpeg  # Linux
```

**Video is unavailable or private**
- Check if the video is publicly accessible
- Try updating yt-dlp: `brew upgrade yt-dlp` or `pip install -U yt-dlp`

**Download is very slow**
```bash
# Try different download method
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome --downloader aria2c "YOUTUBE_URL"

# Or limit concurrent fragments
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome --concurrent-fragments 1 "YOUTUBE_URL"
```

### "Sign in to confirm you're not a bot" error

This means you need to use cookies from your browser (REQUIRED for most videos now):

```bash
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome "YOUTUBE_URL"

# Try different browsers if one doesn't work
yt-dlp -x --audio-format mp3 --cookies-from-browser safari "YOUTUBE_URL"
```

### macOS password prompt every download

When using `--cookies-from-browser chrome` on macOS, you may be prompted for your password each time because yt-dlp accesses Chrome's cookies from the macOS Keychain.

#### Solutions

#### Option 1: Export cookies to a file (one-time password prompt)

```bash
# Install a browser extension to export cookies:
# - "Get cookies.txt LOCALLY" for Chrome/Firefox
# - Export cookies for youtube.com to a file: cookies.txt

# Then use the cookies file instead
yt-dlp -x --audio-format mp3 --cookies cookies.txt "YOUTUBE_URL"
```

#### Option 2: Use Safari (may not prompt as often)

```bash
# Safari integration sometimes requires fewer prompts
yt-dlp -x --audio-format mp3 --cookies-from-browser safari "YOUTUBE_URL"
```

#### Option 3: Grant Terminal/iTerm full disk access

1. Go to System Settings → Privacy & Security → Full Disk Access
2. Add your Terminal app (Terminal.app or iTerm.app)
3. Restart your terminal

This may reduce the frequency of password prompts.

### Updating yt-dlp

YouTube frequently changes its structure, so keep yt-dlp updated:

```bash
# macOS (Homebrew)
brew upgrade yt-dlp

# Linux/Windows (pip)
pip install -U yt-dlp
```

## Best Practices

### Recommended Default Command

```bash
# Best quality MP3 with metadata and thumbnail
yt-dlp -x \
  --audio-format mp3 \
  --audio-quality 0 \
  --cookies-from-browser chrome \
  --embed-thumbnail \
  --add-metadata \
  -o "~/Music/%(uploader)s - %(title)s.%(ext)s" \
  "YOUTUBE_URL"
```

### Batch Download Script

Create a reusable script:

```bash
#!/bin/bash
# save as: youtube-audio.sh

yt-dlp -x \
  --audio-format mp3 \
  --audio-quality 0 \
  --cookies-from-browser chrome \
  --embed-thumbnail \
  --add-metadata \
  --output "~/Music/YouTube/%(uploader)s/%(title)s.%(ext)s" \
  "$@"
```

Make it executable and use:
```bash
chmod +x youtube-audio.sh
./youtube-audio.sh "YOUTUBE_URL"
```

### Shell Alias

Add to your `~/.bashrc` or `~/.zshrc`:

```bash
# Quick YouTube audio download
alias ytmp3='yt-dlp -x --audio-format mp3 --audio-quality 0 --cookies-from-browser chrome --embed-thumbnail --add-metadata'

# Usage:
ytmp3 "YOUTUBE_URL"
```

## Legal Considerations

- Only download content you have permission to download
- Respect copyright laws in your jurisdiction
- YouTube's Terms of Service prohibit downloading videos without permission
- This tool is intended for personal, educational, or fair use purposes
- Always support content creators through official channels when possible

## Additional Resources

- **yt-dlp Documentation**: [https://github.com/yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp)
- **Supported Sites**: yt-dlp supports 1000+ sites beyond YouTube
- **Full Option List**: Run `yt-dlp --help` for all options

## Quick Reference

```bash
# Basic MP3 download (cookies required)
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome "URL"

# Best quality with metadata (recommended)
yt-dlp -x --audio-format mp3 --audio-quality 0 --cookies-from-browser chrome --embed-thumbnail --add-metadata "URL"

# Playlist download
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome -o "%(playlist)s/%(title)s.%(ext)s" "PLAYLIST_URL"

# Multiple URLs from file
yt-dlp -x --audio-format mp3 --cookies-from-browser chrome -a urls.txt

# Update yt-dlp
brew upgrade yt-dlp
