<div align="center">

# Aniwrapper

[**_Setup_**](#setup) | [**_Usage_**](#usage) | [**_Screenshots_**](#screenshots)

![aniwrapper stream gif - konosuba](https://imgur.com/HNRphY0.gif)

</div>

# Introduction

This is a fork of [Dink4n's ani-cli](https://github.com/Dink4n/ani-cli),
which itself is a fork of
pystardust's [old-ani-cli](https://github.com/pystardust/ani-cli/tree/old-ani-cli)

This fork is a wrapper around a modified version of ani-cli, which uses [rofi](https://github.com/davatorium/rofi)
to gather information and control the program flow.
In addition to `rofi`, I've also changed the way saving history works by
integrating a local [sqlite3](https://www.sqlite.org/index.html) database with a table for
search, watch, and file history

While this is a fork of a fork of pystardust's old-ani-cli, I would call this more of a light fork of the [the main ani-cli](https://github.com/pystardust/ani-cli).
I have implemented most of the main features from the original script and continue to use the same scraping logic

This tool scrapes the site [gogoanime](https://gogoanime.cm).

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->

**Table of Contents**

-   [Aniwrapper](#aniwrapper)
-   [Introduction](#introduction)
-   [MPV Extenstion - Skip Intro Script](#mpv-extenstion---skip-intro-script)
    -   [Installing](#installing)
        -   [Manual Install](#manual-install)
-   [Aniwrapper Menus](#aniwrapper-menus)
-   [Dealing with conflicting search queries / rofi grabbing from search list](#dealing-with-conflicting-search-queries--rofi-grabbing-from-search-list)
-   [Usage](#usage)
    -   [aniwrapper](#aniwrapper-1)
        -   [Option 1: Streaming](#option-1-streaming)
        -   [Option 2: Download](#option-2-download)
        -   [Option 3: Continue](#option-3-continue)
        -   [Option 4: Play from File](#option-4-play-from-file)
        -   [Option 5: Sync History](#option-5-sync-history)
        -   [Option 6: Choose Theme](#option-6-choose-theme)
    -   [ani-cli](#ani-cli)
-   [Themes](#themes)

<!-- markdown-toc end -->

# MPV Extenstion - Skip Intro Script

_This repo comes packaged with and will install the
[skip-intro.lua](https://github.com/rui-ddc/skip-intro)
script for MPV during setup if it is not already installed_

**The script is activated with the `TAB` key**

Upon activation, the skip-intro script will try its best to skip the
episode introduction by skipping to the next moment of silence in the video

-   If the video has not pre-loaded past the introduction, the script will not
    know what to do. Press `TAB` again to stop the script until the video
    has loaded enough, or just manually skip past the intro.
-   If the video does not have a pause in audio (or a significant enough drop in
    audio volume) between the end of the introduction and the beginning of the
    video/episode, then the script may fail and skip to a random point in the
    video

## Installing

These are the minimum dependences required to run `aniwrapper`

```
aria2 curl grep mpv rofi sed sqlite3
```

**Arch Linux**

`aniwrapper-git` is available on the [AUR](https://aur.archlinux.org/packages/aniwrapper-git/) for Arch users

```sh
paru -S aniwrapper-git
or
yay -S aniwrapper-git
```

### Manual Install

Install the Dependencies

```sh
# Arch
pacman -S --needed aria2 curl grep mpv rofi sed sqlite3

# Debian
apt install aria2 curl grep mpv rofi sed sqlite3
```

Clone and switch into the repo directory

```sh
git clone https://github.com/ksyasuda/aniwrapper && cd aniwrapper
```

Then, from the `aniwrapper` directory, run the following commands to set up and install the script

```sh
chmod +x setup.sh
./setup.sh && sudo make install
```

# Aniwrapper Menus

See [aniwrapper menus](docs/aniwrapper-menus.md)

# Dealing with conflicting search queries / rofi grabbing from search list

In this program, rofi is configured to search with case insensitivity and select the best match from the list if there are matches. This can make it difficult at times to write a search query that does not trigger a selection from the rofi menu

<div align="center">

![selection with query 'isekai'](https://imgur.com/c2U4kdn.png)
Once your history starts filling up, it becomes progressively more difficult to form unique search queries

![selection with dash](https://imgur.com/vSyaoG6.png)
A workaround for this is to append a dash ` -` to the end of the search query<br/>
The above output was produced by searching: `isekai -`

</div>

# Usage

## aniwrapper

```
# Launch menu (default video quality: best)
aniwrapper

# Launch menu with quality selection
aniwrapper -q

# Enable verbose logging
aniwrapper -v

# Connect to history database
aniwrapper -C

# Enable silent mode (suppress output to stdout) [cannot be used with -v]
aniwrapper -S

# Query the history database
aniwrapper -Q <query>

# Choose rofi theme from presets
aniwrapper -t <aniwrapper (default)|dracula|doomone|fancy|flamingo|material|nord|onedark>

# Specify custom rofi config
aniwrapper -T <path_to_config>

# Specify starting directory for play_from_file mode, bypassing main menu
aniwrapper -f <starting_directory>

# Use ani-cli command-line mode (rofi disabled)
aniwrapper -c

# Download anime in command-line mode
aniwrapper -d
```

### Option 1: Streaming

Streaming is the default option for the `aniwrapper` script. See _[aniwrapper menus](docs/aniwrapper-menus.md)_ for more information about each menu

<details>

<summary>Example</summary>

<div align="center">

![example](https://imgur.com/wNoXjLX.gif)

</div>

</details>

### Option 2: Download

The default download location is `$HOME/Videos/sauce` and will be chosen as the download directory unless otherwise specified

After specifying the download directory (or leaving it blank for the default). See _[aniwrapper Menus](docs/aniwrapper-menus.md)_ for more information about each menu

<details>

<summary>Example</summary>

<div align="center">

![example](https://imgur.com/itdGCsI.gif)

</div>

</details>

### Option 3: Continue

The continue option queries the `sqlite3` history databse and pulls the list of distinct anime names from the `watch_history` table. Select an option from the list and the most recently watched episode of the selected anime will play

<details>

<summary>Example</summary>

<div align="center">

![example](https://imgur.com/d23iYy7.gif)

</div>

</details>

### Option 4: Play from File

This option prompts you to provide a starting directory for the search function. From there, follow the prompts to select the video (or song) you want to play and it will be opened in `mpv`

<details>

<summary>Example</summary>

<div align="center">

![example](https://imgur.com/ODB3lBu.gif)

</div>

</details>

### Option 5: Sync History

This option allows you to sync your search/watch history across devices. It queries the database on the remote machine and inserts/updates the necessary rows

At the moment, the requirements are as follows:

-   You must be able to `ssh` into the remote machine
-   The username must be the same across both devices
-   The `history.sqlite3` file must be in the default location: `$XDG_CONFIG_HOME/aniwrapper/history.sqlite3`

### Option 6: Choose Theme

Allows for changing the aniwrapper theme from the main menu

<details>

<summary>Example</summary>

<div align="center">

![example](https://imgur.com/o97YqLe.gif)

</div>

</details>

## ani-cli

```
# watch anime
ani-cli <query>

# verbose logging
ani-cli -v

# download anime
ani-cli -d <download_directory>

# resume watching anime
ani-cli -H

# sync history across devices
ani-cli -s

# choose quality
ani-cli -q <best (default)|1080p|720p|480p|360p|worst>

# choose rofi theme from presets
ani-cli -t <aniwrapper (default)|dracula|fancy|flamingo|material|nord|onedark>

# Specify starting directory for play_from_file mode (does not work with -c)
ani-cli -f <starting_directory>

# run ani-cli in command-line mode (rofi disabled)
ani-cli -c
```

# Themes

<div align="center">

Default theme

![aniwrapper main menu](https://imgur.com/HFq5gCq.png)

|                                                                                           |                                                                                                                |
| :---------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------: |
| <details><summary>doom-one</summary> ![doomone](https://imgur.com/zMU0i8l.png) </details> |            <details><summary>dracula</summary> ![dracula](https://imgur.com/BPdIuX3.png)</details>             |
|    <details><summary>fancy</summary>![fancy](https://imgur.com/CZx73NL.png)</details>     |            <details><summary>material</summary>![material](https://imgur.com/eTWkxw2.png)</details>            |
|  <details><summary>onedark</summary>![onedark](https://imgur.com/rchqtBm.png)</details>   | <details><summary>flamingo (sidebar theme)</summary>![flamingo-theme](https://imgur.com/pxF7hRE.png)</details> |

</div>
