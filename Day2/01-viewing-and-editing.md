# Viewing & Editing Files

## Viewing
```bash
cat file.txt          # dump whole file
cat -n file.txt       # with line numbers
less file.txt         # scrollable pager (q quit, / search, n next)
head -n 20 file.txt   # first 20 lines
tail -n 50 file.txt   # last 50 lines
tail -f app.log       # follow live (great for watching logs)
tail -f app.log | grep ERROR   # follow + filter
```

> Use `less` for big files (it doesn't load the whole thing). Use `tail -f` to watch a
> log while you reproduce a bug.

---

## Editing: `nano` (beginner-friendly)
```bash
nano file.txt
```
- `Ctrl+O` save (write Out), `Enter` confirm.
- `Ctrl+X` exit.
- `Ctrl+W` search, `Ctrl+K` cut line, `Ctrl+U` paste.

Bottom-bar shows shortcuts. Best choice when you just need a quick edit.

---

## Editing: `vim` (everywhere on servers)
`vim` is preinstalled on virtually every server, so learn the survival basics.

**Modes:** you start in **Normal** mode (keys = commands), press `i` for **Insert**
mode (type text), press `Esc` to go back.

| Goal | Keys |
|---|---|
| Enter insert mode | `i` |
| Back to normal mode | `Esc` |
| Save | `:w` |
| Save & quit | `:wq` or `ZZ` |
| Quit without saving | `:q!` |
| Search | `/word` then `n` |
| Go to line 50 | `:50` |
| Undo / redo | `u` / `Ctrl+r` |
| Delete a line | `dd` |

> **The #1 panic question:** "How do I exit vim?" → press `Esc`, then type `:q!` and Enter.

---

## Quick Edits Without an Editor
```bash
echo "hello" > file.txt        # overwrite file with text
echo "more"  >> file.txt       # append a line
sed -i 's/old/new/g' file.txt  # find/replace in place (Day 2 text processing)
```

---

## TL;DR
- View: `cat`, `less`, `head`, `tail -f`.
- Edit easily with `nano`; know enough `vim` to save (`:wq`) and quit (`:q!`).
- `>` overwrites, `>>` appends, `sed -i` edits in place.
