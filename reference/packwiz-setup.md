# Packwiz Setup Guide for Pelican/Pterodactyl

## What is packwiz?

packwiz is a command-line tool for managing Minecraft modpacks using git-friendly TOML configuration files. Instead of distributing large zip archives, you maintain a small text-based config that references mods from CurseForge, Modrinth, or direct URLs.

## Quick Start

### 1. Create your packwiz configuration

On your local machine (or via the panel's file editor), create these files in your server directory:

**packwiz.toml** (main config):
```toml
mod-provider = "curseforge"  # or "modrinth", "url", "github", etc.
remote-repo = ""              # Your git repository URL (for hosting)

[modrinth]
project-id = ""              # Modrinth project ID (if using modrinth provider)

[curseforge]
project-id = 0               # CurseForge project ID (if using curseforge provider)
```

### 2. Add mods to your pack

Use the packwiz CLI locally, or manually create entries:

```bash
# Add a mod from Modrinth
packwiz modrinth add <project-id> --file-type=release

# Add a mod from CurseForge
packwiz curseforge add <project-id> --file-type=release

# Add a mod from a direct URL
packwiz url add <url> --file-type=jar
```

This creates files in the `mods/` directory with references to the actual mod files.

### 3. Upload to your Pelican server

Upload these files to your server via the panel's file manager:
- `packwiz.toml`
- `mods/` directory (contains the reference files)
- `config/` directory (optional, for config overrides)

### 4. The egg handles the rest

When you start your server:
1. packwiz is automatically installed (if not present)
2. `packwiz install` downloads all referenced mods on first run
3. On every subsequent start, `packwiz update` checks for mod updates

## Using a Git Repository (Recommended)

For easy updates, host your packwiz config in a git repository:

1. Create a repo with `packwiz.toml` and `mods/` directory
2. Push to GitHub/GitLab/etc.
3. In Pelican, set the `PACKWIZ_REPO` egg variable to your repo URL
4. Set `PACKWIZ_BRANCH` if using a branch other than main

The egg will clone the repo during installation.

## Supported Mod Providers

- **Modrinth** - Modern mod platform
- **CurseForge** - Classic mod platform  
- **URL** - Direct download links
- **GitHub Releases** - GitHub-hosted mods
- **GitLab Releases** - GitLab-hosted mods

## Troubleshooting

### "packwiz install failed"
- Check your `packwiz.toml` is valid TOML syntax
- Verify the mod provider settings are correct
- Check your server has internet access

### Mods not appearing
- Ensure `mods/` directory exists and contains reference files
- Check the server console for packwiz output during startup

### Update not applying
- The egg runs `packwiz update` before each server start
- Check the console for any warnings

## Resources

- Official docs: https://packwiz.infra.link/
- GitHub: https://github.com/packwiz/packwiz
- Discord: https://discord.gg/DcSkRF4
