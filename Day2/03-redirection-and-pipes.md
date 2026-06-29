# Redirection & Pipes

This is the heart of Unix power: small tools connected together. Master this and the
text-processing tools (next section) become superpowers.

## The Three Standard Streams
| Stream | FD | Default | Meaning |
|---|---|---|---|
| **stdin** | 0 | keyboard | input |
| **stdout** | 1 | screen | normal output |
| **stderr** | 2 | screen | error output |

---

## Redirecting Output
```bash
ls > files.txt        # write stdout to file (OVERWRITE)
ls >> files.txt       # APPEND stdout to file
ls 2> errors.txt      # redirect stderr only
ls > out.txt 2>&1     # stdout AND stderr to same file
ls &> all.txt         # shorthand for both (bash)
command > /dev/null 2>&1   # discard all output (the "black hole")
```

> `>` overwrites, `>>` appends. `2>&1` means "send stderr to wherever stdout is going."
> `/dev/null` throws output away — common in cron jobs and scripts.

## Redirecting Input
```bash
sort < names.txt      # feed file as stdin
wc -l < access.log    # count lines from a file via stdin
```

## Here-doc / Here-string
```bash
cat <<EOF > config.yml
key: value
env: prod
EOF

grep "error" <<< "$logline"   # feed a string as stdin
```

---

## Pipes `|` — Chain Commands
A pipe sends the **stdout of one command into the stdin of the next**.
```bash
cat access.log | grep ERROR | wc -l        # count error lines
ps -ef | grep nginx                        # find nginx processes
ls -l | sort -k5 -n                        # sort files by size
history | grep ssh                         # recall past ssh commands
du -h | sort -rh | head                    # biggest items, top of list
```

> Build pipelines incrementally: run the first command, then add `| next` one piece at a
> time and watch how the output changes.

## `tee` — Split Output (screen + file)
```bash
deploy.sh | tee deploy.log          # see output AND save it
deploy.sh | tee -a deploy.log       # append instead of overwrite
echo "data" | sudo tee /etc/file    # write to a root-owned file via sudo
```
> `sudo tee` is the idiom for writing to protected files, because `sudo echo > file`
> does **not** work (the redirect runs as your user, not root).

---

## TL;DR
- `>` overwrite, `>>` append, `2>` errors, `2>&1` merge, `/dev/null` discard.
- `|` chains commands — the core Unix pattern (`grep | sort | uniq | wc`).
- `tee` shows output and saves it; `sudo tee` writes to protected files.
