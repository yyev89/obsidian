### files and buffers:
`:edit filname` or `:e` open an existing file
`:w filename` save current file to filename
`:x` save and exit, or `ZZ`
`:r filename` open another file in current
`:e filename` edit another file in parallel buffer
`:bp` move to previous buffer
`:bn` move to next buffer
`:bd` close current buffer
`:buffers` or `:ls` list buffers
`:enew` open new clean file
`:! cmd` run linux command
`:split filename` open two files in horizontal split
`:vsplit` in vertical, or `:vs`
`Ctrl` + `ww` move between splits

### moves horizontal:
`0` move cursor to start of line
`$` to end
`f` + character (find) set cursor on character
`F` + character same backwards
`;` repeat move forwards
`,` repeat move backwards
`t` + character (towards) set cursor right before character
`T` same backwards
`w` move to the next word start
`W` move to the next word, separated by space
`b` backwards to previous word start
`B` backwards to previous word start, separated by space
`e` move to the next end of word
`E` move to the next end of word, separated by space
`ge` backwards to previous word end
`gE` backwards to previous word end, separated by space

### moves vertical:
`}` move to the next paragraph start (separated by empty line)
`{` move backwards
`gg` move to the first line of document
`G` to the last one
`%` when on brace, go to closing one and backwards
`Ctrl` + `f` move one screen forward
`Ctrl` + `b` move one screen back
`Ctrl` + `d` move half-screen down
`Ctrl` + `u` move half-screen up
`Ctrl` + `v` vertical visual mode

### search:
`/pattern` search forward
`?pattern` search backwards
`n` move to next found
`N` to previous
`*` search for word under the cursor

### editing:
`d` + move delete
`D` delete till the end of line
`dd` delete line
`cc` delete line and enter insert mode, or `S`
`u` undo changes
`Ctrl` + `r` redo changes
`ce` change word after cursor
`.` repeat last change
`r` replace character under cursor
`R` replace until `Esq` is pressed
`x` delete character
`~` switch case of character
`y` copy
`yy` copy the whole line
`p` paste after cursor (down)
`P` paste before cursor (up)
`gU` set to uppercase
`gUU` make all line uppercase
`gu` set to lowercase
`guu` make all line lowercase
`>` indent to the right
`>>` indent whole line to the right
`<` indent to the right
`<<` indent whole line to the right

### text:
`i` "inside"
`a` "around"
`diw` delete inside word
`dip` delete inside paragraph
`yip` copy inside paragraph
`di"` delete inside ""
`da(` delete around () including parentheses
`%s/old/new/g` replace old with new in whole document
`%s/old/new/gc` same with confirmation
`%s/old/new/gi` same ignoring case
`i` insert text under cursor
`a` append text next to cursor
`I` insert on the start of current line
`A` append to the end of current line
`o` open line below
`O` open line above
`:norm A;<enter>` append ; to every selected line
`:norm Ivar <enter>` put "var" before selected lines




