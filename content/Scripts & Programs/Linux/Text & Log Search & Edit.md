
# grep & tail
Use `grep`or `zgrep` (for zipped files) to filter text for relevant info. 
```bash
grep <flags> <text> <filename>
zgrep <flags>  <text> <filename>
```
Text does not need to be in quotes if it is a simple string. File name can contain `*` wildcard.
## Flags
- `-a` = *text* - process binary file as text
- `-i`  = *case-insensitive*
- `-v`  = *invert* - find non-matching
- `-w` = *word* - match exact word (start of line or preceded by non-word character)
- `-x`  = *exact* - match exact line
- `-c` = *count* - count of matches for each input file
- `-m <n>` = *max* - return first n matches
- `-n` = *line-number* - return line-number with match
- `-r` = *recursive* - read all files under each directory, not including symbolic links
- `-R` = *derefence-recursive* - read all files under each directory, including symbolic links
- `-E` = *RegEx* - use RegEx pattern searching
## Examples
```
grep -i vpn /var/log/syslog
grep " 18:00" /var/log/syslog
grep -i "ending" filer-20240105*.txt
grep -E -i "starting|ending" filer-20240105*
zgrep -i vpn /var/log/syslog.2.gz
```

Combine with other commands, particularly `tail` to return a more manageable number of lines.
```bash
<command> | grep <text>

sudo tail -50 syslog | grep -i vpn
```

# vim
```
i - INSERT mode
ESC - COMMAND mode
:wq - to save and exit
:q! - to trash all changes
```