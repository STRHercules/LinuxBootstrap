# Packages and Manual Install Guide

This document mirrors what `bootstrap.sh` installs. The script is best-effort and will skip packages that are missing for a given distro.

If you only need the high-level install commands, scan the "Manual install" blocks. If you want to know what each package does, scan the "What it is" bullets.

---

## APT (Debian/Ubuntu/Pop!_OS)

### Prep and repos

Manual install:

```bash
sudo apt-get update
sudo apt-get install -y software-properties-common
sudo add-apt-repository -y universe
sudo add-apt-repository -y multiverse
```

### Required packages (APT_REQUIRED)

Manual install:

```bash
sudo apt-get install -y \
  curl wget gpg ca-certificates git \
  build-essential make cmake pkg-config \
  libsdl2-dev libsdl2-ttf-dev libsdl2-image-dev libsdl2-mixer-dev \
  tmux zip unzip \
  gdb valgrind ccache \
  ripgrep fzf zoxide \
  fonts-firacode
```

What it is:
- `curl`, `wget`, `gpg`, `ca-certificates` - download/signing basics.
- `git` - source control client.
- `build-essential`, `make`, `cmake`, `pkg-config` - core build toolchain.
- `libsdl2-*` - SDL2 dev headers for games/media projects.
- `tmux` - terminal multiplexer.
- `zip`, `unzip` - archive tools.
- `gdb`, `valgrind`, `ccache` - debug/profiling/build cache helpers.
- `ripgrep` (`rg`), `fzf`, `zoxide` - modern CLI helpers for search, fuzzy find, and directory jumping.
- `fonts-firacode` - Fira Code font family.

### Optional packages (APT_OPTIONAL)

Manual install:

```bash
sudo apt-get install -y \
  eza gh bat \
  unrar unrar-free \
  cmatrix cbonsai fortune-mod cowsay lolcat figlet \
  pipes.sh sl ninvaders nsnake pacman4console moon-buggy bastet \
  hollywood fastfetch btop npm qdirstat remmina vlc cool-retro-term
```

What it is:
- `eza` - modern `ls` replacement.
- `gh` - GitHub CLI.
- `bat` - `cat` with syntax highlighting.
- `unrar`, `unrar-free` - RAR archive support (one may be unavailable).
- `cmatrix`, `cbonsai`, `fortune-mod`, `cowsay`, `lolcat`, `figlet` - terminal fun/text toys.
- `pipes.sh`, `sl`, `ninvaders`, `nsnake`, `pacman4console`, `moon-buggy`, `bastet` - terminal games.
- `hollywood` - faux hacker terminal display.
- `fastfetch` - system info summary.
- `btop` - resource monitor.
- `npm` - Node package manager (note: Node itself is installed via NVM).
- `qdirstat` - disk usage analyzer.
- `remmina` - remote desktop client.
- `vlc` - media player.
- `cool-retro-term` - retro terminal emulator.

### GitHub CLI repo fallback (APT only)

If `gh` is missing from default repos, the script adds the official GitHub CLI repo:

```bash
sudo install -d -m 0755 /usr/share/keyrings
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg \
  | sudo tee /usr/share/keyrings/githubcli-archive-keyring.gpg >/dev/null
sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg
arch="$(dpkg --print-architecture)"
echo "deb [arch=${arch} signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
  | sudo tee /etc/apt/sources.list.d/github-cli.list >/dev/null
sudo apt-get update
sudo apt-get install -y gh
```

### Browsers (APT only)

Brave repo:

```bash
sudo install -d -m 0755 /usr/share/keyrings
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg \
  https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg
sudo curl -fsSLo /etc/apt/sources.list.d/brave-browser-release.sources \
  https://brave-browser-apt-release.s3.brave.com/brave-browser.sources
```

Edge repo:

```bash
sudo install -d -m 0755 /usr/share/keyrings
curl -fsSL https://packages.microsoft.com/keys/microsoft.asc \
  | gpg --dearmor \
  | sudo tee /usr/share/keyrings/microsoft.gpg >/dev/null
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/edge stable main" \
  | sudo tee /etc/apt/sources.list.d/microsoft-edge.list >/dev/null
```

Install both:

```bash
sudo apt-get update
sudo apt-get install -y brave-browser microsoft-edge-stable
```

---

## DNF (Fedora)

Manual install:

```bash
sudo dnf install -y curl wget gpg ca-certificates git
sudo dnf groupinstall -y "Development Tools"
sudo dnf install -y cmake pkgconf-pkg-config \
  SDL2-devel SDL2_ttf-devel SDL2_image-devel SDL2_mixer-devel \
  tmux zip unzip unrar gdb valgrind ccache \
  ripgrep fzf zoxide eza gh bat fira-code-fonts \
  cmatrix fortune-mod cowsay lolcat figlet sl ninvaders \
  hollywood fastfetch btop qdirstat remmina vlc cool-retro-term npm
```

What it is:
- Same purpose as the APT list, but with Fedora package names.
- Browsers (Brave/Edge) are not automated on DNF in the script.

---

## Pacman (Arch)

Manual install:

```bash
sudo pacman -Syu --noconfirm --needed \
  curl wget gnupg ca-certificates git base-devel cmake pkgconf \
  sdl2 sdl2_ttf sdl2_image sdl2_mixer \
  tmux zip unzip \
  unrar gdb valgrind ccache \
  ripgrep fzf zoxide eza bat github-cli ttf-fira-code \
  cmatrix fortune-mod cowsay lolcat figlet sl ninvaders \
  hollywood fastfetch btop qdirstat remmina vlc npm
```

What it is:
- Same purpose as the APT list, but with Arch package names.
- Browsers (Brave/Edge) are not automated on pacman in the script.

---

## Flatpak apps (Flathub)

Manual install:

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install -y flathub \
  dev.vencord.Vesktop \
  com.visualstudio.code \
  com.valvesoftware.Steam \
  net.lutris.Lutris \
  tv.plex.PlexDesktop \
  com.plexamp.Plexamp \
  org.prismlauncher.PrismLauncher \
  org.kde.dolphin \
  com.github.tchx84.Flatseal
```

What it is:
- `dev.vencord.Vesktop` - Vesktop (Discord client with Vencord).
- `com.visualstudio.code` - Visual Studio Code.
- `com.valvesoftware.Steam` - Steam client.
- `net.lutris.Lutris` - game launcher.
- `tv.plex.PlexDesktop` - Plex desktop app.
- `com.plexamp.Plexamp` - Plexamp music player.
- `org.prismlauncher.PrismLauncher` - Minecraft launcher.
- `org.kde.dolphin` - Dolphin file manager.
- `com.github.tchx84.Flatseal` - Flatpak permission manager.

---

## GNOME extensions (extensions.gnome.org)

Manual install:
- Install the Extensions app or use `gnome-extensions`, then install each UUID from https://extensions.gnome.org.

UUIDs and what they are:
- `add-to-desktop@tommimon.github.com` - add launchers to desktop.
- `appindicatorsupport@rgcjonas.gmail.com` - tray icons/app indicators.
- `azwallpaper@azwallpaper.gitlab.com` - wallpaper utilities.
- `blur-my-shell@aunetx` - blur effects.
- `burn-my-windows@schneegans.github.com` - window animations.
- `clipboard-indicator@tudmotu.com` - clipboard history.
- `compiz-alike-magic-lamp-effect@hermes83.github.com` - window minimize animation.
- `compiz-windows-effect@hermes83.github.com` - compiz-like effects.
- `CoverflowAltTab@palatis.blogspot.com` - coverflow-style alt-tab.
- `custom-accent-colors@demiskp` - accent color control.
- `dash2dock-lite@icedman.github.com` - dock (lite).
- `dash-to-dock@micxgx.gmail.com` - dock.
- `desktop-cube@schneegans.github.com` - workspace cube.
- `dynamic-panel@velhlkj.com` - dynamic top panel.
- `grand-theft-focus@zalckos.github.com` - focus management.
- `lockscreen-extension@pratap.fastmail.fm` - lock screen tweaks.
- `monitor@astraext.github.io` - system monitor.
- `notifications-alert-on-user-menu@hackedbellini.gmail.com` - notifications in user menu.
- `runcat@kolesnikov.se` - CPU load cat.
- `simple-weather@romanlefler.com` - weather.
- `transparent-top-bar@ftpix.com` - transparent top bar.
- `user-theme@gnome-shell-extensions.gcampax.github.com` - user themes.

---

## Direct downloads and installers

These are not distro packages, but are installed by the script.

### Nerd Font (default: FiraCode)

Manual install:

```bash
mkdir -p ~/.local/share/fonts/NerdFonts/FiraCode
curl -LfsS -o /tmp/FiraCode.zip \
  https://github.com/ryanoasis/nerd-fonts/releases/download/v3.3.0/FiraCode.zip
unzip -q -o /tmp/FiraCode.zip -d /tmp/FiraCode-unz
find /tmp/FiraCode-unz -maxdepth 1 -type f -name '*.ttf' \
  -exec cp -f {} ~/.local/share/fonts/NerdFonts/FiraCode/ \;
fc-cache -f ~/.local/share/fonts/NerdFonts/FiraCode
```

### Oh My Zsh

Manual install:

```bash
KEEP_ZSHRC=yes RUNZSH=no CHSH=no \
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended
```

### Starship

Manual install:

```bash
curl -fsSL https://starship.rs/install.sh | sh -s -- -y
```

### NVM + Node.js (LTS)

Manual install:

```bash
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
export NVM_DIR="$HOME/.nvm"
. "$NVM_DIR/nvm.sh"
nvm install --lts
nvm alias default 'lts/*'
corepack enable || true
```

### npm global tools

Manual install:

```bash
npm install -g @openai/codex @google/gemini-cli
```

### Cursor (AppImage)

Manual install:

```bash
mkdir -p ~/Applications
curl -LfsS -o ~/Applications/cursor.AppImage \
  "https://api2.cursor.sh/updates/download/golden/linux-x64-deb/cursor/2.3"
chmod +x ~/Applications/cursor.AppImage
```

If the AppImage does not launch, install FUSE:
- APT: `sudo apt-get install -y libfuse2`
- DNF: `sudo dnf install -y fuse-libs`
- Pacman: `sudo pacman -S --noconfirm --needed fuse2`

### GitHub Desktop (Shiftkey .deb, APT only)

Manual install:

```bash
curl -LfsS -o /tmp/GitHubDesktop-linux-amd64.deb \
  https://github.com/shiftkey/desktop/releases/download/release-3.4.13-linux1/GitHubDesktop-linux-amd64-3.4.13-linux1.deb
sudo apt-get install -y /tmp/GitHubDesktop-linux-amd64.deb
```

### Deskflow (.deb, APT only)

Manual install:

```bash
curl -LfsS -o /tmp/deskflow-x86_64.deb \
  https://github.com/deskflow/deskflow/releases/download/v1.25.0/deskflow-1.25.0-ubuntu-questing-x86_64.deb
sudo apt-get install -y /tmp/deskflow-x86_64.deb
```

### ckb-next

Manual install:

```bash
git clone --depth=1 https://github.com/ckb-next/ckb-next.git /tmp/ckb-next
cd /tmp/ckb-next
chmod +x ./quickinstall
./quickinstall
```

### Zsh plugins (fallback clone)

Manual install:

```bash
mkdir -p ~/.zsh/plugins
git clone --depth=1 https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/plugins/zsh-autosuggestions
git clone --depth=1 https://github.com/zsh-users/zsh-syntax-highlighting ~/.zsh/plugins/zsh-syntax-highlighting
```

### GNOME Shell (needed to apply extensions)

Manual install (if missing):
- APT: `sudo apt-get install -y gnome-shell gnome-shell-extension-prefs gnome-shell-extensions`
- DNF: `sudo dnf install -y gnome-shell gnome-extensions-app gnome-shell-extension-prefs`
- Pacman: `sudo pacman -Syu --noconfirm --needed gnome-shell gnome-shell-extensions gnome-extensions`
