# Command Line Video Clip Utility

The `clip` utility (a bash script) is for downloading managing and
playing clips from videos in full screen from the command line.

## Dependencies

Required:

* `bash` (4+)
* `mpv`
* `youtube-dl`

Will use if detected:

* `keyon` / `keyoff`
* `pandoc`

Environment variables:

```
: ${CLIP_DATA:="$HOME/.config/clip/data"}
: ${CLIP_DIR:="$HOME/Videos/clips"}
: ${CLIP_SCREEN:=1}
: ${CLIP_VOLUME:=-50}
: ${PAGER:=more}
: ${EDITOR:=vi}
: ${HELP_BROWSER:=}
```

## Usage

```
clip dir
clip data [<name>]
clip add <name> <url>
clip edit
clip help [<command>]
clip list
clip (play) [<name>]
clip usage
```
## Commands

* [usage](#usage) - print usage summary 
* [play](#play)   - play clip, randomize if duplicates, select menu if none
* [list](#list)   - print list of all clips by name
* [data](#data)   - print data for name (random if dupicates)
* [dir](#dir)     - print directory path containing clip videos
* [add](#add)     - download and add video from YouTube URL
* [edit](#edit)   - open data file for editing with `$EDITOR`
* [help](#help)   - display help for all or specified command

## Data File Format

The data file containing clip name, volume, and source information can be edited with the `edit` command and is updated by the `add` command when adding new clips. The first two numbers after the file name (separated by commas) are for the start second (including decimals) and the length. To add another segment from the same file, add a semicolon and another start and length, and so on. Clips that have the same name will be randomized. The `play` commands accepts a regular expression which can also be used to randomize between several clips.

```
working 100 dXjcvIPSBr4.mkv,19,20
revenge 100 _oyP0QHjty8.webm,17,30
rick 100 dQw4w9WgXcQr.mkv,0,9001
verse 240 VX58scb5_B0.mkv,-16.9,14
```

A comprehensive collection of clips is maintained in the [data](data)
file within this repo. Feel free to submit a PR with your own.

## `edit`

The `edit` command opens the clip data file (`CLIP_DATA`) with the current `$EDITOR` (default: `vi`).

## `data`

The `data` command returns a line from the data file (`$CLIP_DATA`) that
matches the argument passed, which can be simply a string or an extended
regular expression. If more than one line matches, then one will
randomly be returned.

## `play`

The `play` command (which is the default when no command is passed)
takes the basename of a video in the `$CLIP_DIR` and plays it with
passed volume (100), starting point (0), and length (10). If the name
passed refers to one or more clips, a random clip from among them will
be select. Also, rather than a specific, any Bash-compatible regular
expression may be passed allowing for interesting combinations. For
example, a `coin` alias could be created to select randomly from between
the *yes* and *no* clips, which could themselves also have multiple in
their groups. (See `man mpv` for more details on how the videos are
played.)

## `add`

The `add` command will download the provided YouTube URL into the
`$CLIP_DIR` directory and name it according the YouTube identifier
preserving the same file suffix. It then add an entry to the clips data
file (`$CLIP_DATA`) with default settings which can be changed easily
with `edit` later. Be sure to use the shareable URL (rather than the one
in the omnibox) so that the ID is extracted correctly.

## `list`

The `list` command display a space-delimited list of all possible, unique clip names sorted alphalexically.

## `dir`

The `dir` command prints the full path to the directory containing the videos used for all clips (`$CLIP_DIR`).

## `usage`

The `usage` command print a summary of usage for this command.

## `help`

The `help` command prints help information. If no argument is passed
displays general help information (main). Otherwise, the documentation
for the specific argument keyword is displayed, which usually
corresponds to a command name (but not necessarily). All documentation
is written in CommonMark (Markdown) and will displayed as Web page if
`pandoc` and `$HELP_BROWSER` are detected, otherwise, just the Markdown is
sent to `$PAGER` (default: more).

## Legal

Copyright 2021 Rob Muhlestein <rob@rwx.gg>
Released under Apache-2.0 License
Please mention https://youtube.com/rwxrob

