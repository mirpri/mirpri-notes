# My Favorite Softwares

## Neovim

Try out my configuration of Neovim based on LazyVim:  
https://github.com/mirpri/MyLazyVim


### clangd config
Clangd is the C/C++ language server you may use for C/C++ developing with neovim. In windows, it can be configured globally in `C:\Users\xxx\AppData\Local\clangd\config.yaml`. You need to adjust the settings based on your default compiler and set your preferred standard. Basically, if you are using the latest version of `mingw`, set to:
```yaml
CompileFlags:
  Add:
    - -std=c++17 # set standard
    - --target=x86_64-w64-windows-gnu # Universal Headers will work
```

## nushell
I use Nushell on Windows as it is much faster than PowerShell and behaves more (though not exactly) like a Unix shell. It fast, beautiful and flexible.

### conda
`conda init`  doesn't support nushell. You will need script from https://github.com/nushell/nu_scripts/blob/main/modules/virtual_environments/conda.nu. and use it in `config.nu`:
```
use ./conda.nu
```

### oh-my-posh
omp can work seemlessly with nushell.

## Fish
Fish is a **very user-friendly** shell. It literally have the best auto-completions out of the box.

### Disable welcome message
add this line:
```
set -g fish_greeting
```
to `~/.config/fish/config.fish` (This is fish's config file like bash's `~/.bashrc`)

Change welcome message:

```
set -g fish_greeting "Hello"
```

Display system info:

```
set -l os_info (grep "PRETTY_NAME" /etc/os-release | cut -d '"' -f 2)
set -g fish_greeting "Welcome to $os_info!"
```

### Environment variables
Fish uses `set -x` (instead of `export` in bash) to set environment variables.

Add to path:

```
set -x PATH $PATH /path/to/your/bin
```

Set http proxy:

```
set -x http_proxy http://127.0.0.1:7890
set -x https_proxy http://127.0.0.1:7890
set -x all_proxy socks5://127.0.0.1:7890
```

## mpv

A minimal yet powerful video player. Blazing fast and fully customizable.

Official site: https://mpv.io

My configuration: https://github.com/mirpri/mpv-config

It's recommended to install from scoop and build file association with scripts from [this repo](https://github.com/rossy/mpv-install).
