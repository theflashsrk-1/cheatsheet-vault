#macos #fundamentals #homebrew

# macOS Fundamentals

Related: [fundamentals](fundamentals.md) · [ios](../Mobile/ios.md)

## Contents
- [Files](#files)
- [Networking](#networking)
- [Homebrew](#homebrew)
- [Shell](#shell)

## Files
```bash
defaults write com.apple.Finder AppleShowAllFiles true && killall Finder
```
Show hidden files in Finder.

```bash
sudo chown <user>:<group> <file>
```
Change ownership (`chmod -vv <octal> <file>` for perms).

## Networking
```bash
ifconfig en0 inet 192.168.1.1 netmask 255.255.255.0
```
Manually set an interface IP/mask.

```bash
lsof -n -i4TCP -P
```
List bound TCP ports with owning apps.

```bash
networksetup -listallnetworkservices
```
List network services/devices (`-getinfo <dev>` for details).

```bash
security find-generic-password -wa <SSID>
```
Retrieve a stored Wi-Fi password from the keychain.

## Homebrew
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Install Homebrew.

```bash
brew install <package>
```
Install a package (`--cask` for GUI apps, `brew search <x>` to find).

## Shell
```bash
chsh -s /bin/zsh
```
Change the login shell (`.zshrc` / `.bashrc` config files).

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Install Oh My Zsh.
