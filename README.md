# c(d)
atuin, autojump, bookmarks, CDPATH, McFly, z, zoxide... Let's settle this once and for all.

Y'all need to stop this directory bookmarking thing. Nevermind `z`. Don't bother with `autojump`. Leave aside `zoxide`. Skip `McFly`. No, don't invent your own. No, we don't need 400 lines of Go, or 700 lines of Rust, in another GitHub repo of 16 files and 5 pages of AI-generated documentation. Y'all fools just need to grasp the shell a little and embrace files. No, I'm not joking.

```sh
CDHISTFILE="$HOME/.cd_history"
cd() {
  command cd "$@" && printf '%s\n' "$PWD" >> "$CDHISTFILE"
}
```

POSIX. 2 fking lines. Bulletproof. Unbeatable.

`tac` it, dedupe with `awk`, and feed it to your fuzzy picker. Un. Beatable. Here, I'll do it for you:

```sh
c() {
  cd "$(tac "$CDHISTFILE" | awk '$0 != ENVIRON["PWD"] && !seen[$0]++' | "$MY_FUZZY_PICKER")"
}
``` 

You can't outdo this. No, I'm not joking. This needs to be PSA'd and stickied for all eternity.

3 fking lines. Stop this overengineering bullshit.

Whatever, whoever, is responsible for these brains confabulating this complexity hell needs to suffer. Y'all are duped the fuck outta your minds. Use a little creativity, kill a little complexity. Is it so fucking hard?

I'm feeling friendly. Here, have some color for that awesome little utility. Makes it easier to parse the paths. And I'll put it all in one package. Get your addon downloader! Your plugin manager! Your dotfile organizer! Pin it, lock it to a SHA; gotta track those 17 dependencies! Enshittify the fuck outta it! Oh wait, you can't; it's just a fking little script–albeit with lots of power. Dependency manage your way out of that!

```sh
ESC=$'\x1b'
YELLOW="$ESC[33m"
RESET="$ESC[0m"
# sed -r: colorize last path component
COLORIZE="s,([^/]+/?)$,$YELLOW\1$RESET,"
# sed: strip ansi color sequences
DECOLORIZE="s,$ESC[[0-9;]*[mK],,g"

CDHISTFILE="$HOME/.cd_history"
cd() {
  command cd "$@" && printf '%s\n' "$PWD" >> "$CDHISTFILE"
}

c() {
  dir=$(
    tac "$CDHISTFILE" |
    awk '$0 != ENVIRON["PWD"] && !seen[$0]++' |
    sed -r "$COLORIZE" |
    "$MY_FUZZY_PICKER" |
    sed "$DECOLORIZE"
  )
  [ "$dir" ] && cd "$dir"
}
```

You never saw a better directory hopper in your life. You'll know if you actually try.

Your shell life just changed. You're welcome.

[Source](https://www.reddit.com/r/commandline/comments/1stjetf/comment/oi5x65v/)

# Bonus

If you really need bookmarks (you don't):

```sh
alias cddownloads='cd $HOME/Downloads'
alias cdprojects='cd $HOME/some/path/Projects'
alias cdmything='cd /another/path/to/something'
```

Now type `cd` and press Tab. To add more just… add more.

You could also `alias cd.mything='…'` or one of several other accepted punctuation as delimiter, but really, why.

[Source](https://www.reddit.com/r/commandline/comments/1stjetf/comment/oi5p2k1/)
