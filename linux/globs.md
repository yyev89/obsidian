- `?` replace any 1 character
- `*` replace 0 or any amount of characters
- `[]` create a character class (range)
- `!` or `^` create negated character class (not)
- `\` - escape symbol

```bash
# Produce exact result:
ls \?ail
ls "?ail"
# Exclude from output:
ls file[^A-C]
ls file[!A-C]
# Case sensetive, ascending order matters:
rm file[aA-eE]
```
