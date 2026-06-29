# Getting Help (Don't Memorize — Look It Up)

Great Linux users don't memorize every flag; they know how to **find** the answer fast.

## `man` — the manual
```bash
man ls               # full manual for ls
man 5 crontab        # section 5 = file formats (vs section 1 = commands)
```
Inside `man` / `less`: `↑/↓` scroll, `/word` search, `n` next match, `q` quit.

> Man sections: **1** user commands, **5** file formats/config, **8** admin commands.

## `--help` — the quick reference
```bash
ls --help            # condensed flag list, faster than man
tar --help | less    # pipe long help into a scrollable viewer
```

## `tldr` — practical examples
```bash
tldr tar             # real-world example commands (install: apt install tldr)
```
Best when you think *"I know the tool, I just forgot the exact syntax."*

## `type`, `which`, `whatis`
```bash
type ll              # is it a command, alias, or function?
which python3        # full path of the binary
whatis grep          # one-line description
```

## `apropos` — search by keyword
```bash
apropos compress     # find commands related to "compress"
```

---

## Online Helpers
- **explainshell.com** — paste a full command, get each piece explained.
- **`man7.org`** — clean online manuals.
- Official docs for the specific tool (e.g. `nginx`, `systemd`).

---

## A Repeatable Workflow When Stuck
1. `tldr <cmd>` → get a working example fast.
2. `<cmd> --help` → confirm the exact flags you need.
3. `man <cmd>` → read deeply if behavior is surprising.
4. Search the **error message** verbatim if it still fails.

---

## TL;DR
- `man` = deep docs, `--help` = quick flags, `tldr` = copy-paste examples.
- `which` / `type` tell you *what* a command actually is.
- explainshell.com decodes any command you don't understand.
