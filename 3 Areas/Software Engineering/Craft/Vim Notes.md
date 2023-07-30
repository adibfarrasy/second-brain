reference:
[https://www.youtube.com/watch?v=X6AR2RMB5tE&list=PLm323Lc7iSW_wuxqmKx_xxNtJC_hJbQ7R](https://www.youtube.com/watch?v=X6AR2RMB5tE&list=PLm323Lc7iSW_wuxqmKx_xxNtJC_hJbQ7R)

# What I Haven’t Done (Jan 2023)

1. yank straight away without having to highlight lines, e.g. use `y5j` or `yi{`
2. yank including the parens with `ya{` or `ya(`
3. use `I` to insert at the beginning of the line
4. create new windows to the top/bottom/left/right, navigate through windows
5. use `:Sex!`to open netrw on the left side window, or `:Ex` to spawn netrw in a separate buffer
6. load a word into the search term using `*`
7. use `viW` (capital w) to highlight contiguous text (also works for yank and delete). useful to navigate around word separators like `-`.
8. use `o` in visual mode to jump between ends
9. use `yap` to yank contiguous paragraphs
10. use regex to remove repetitions

# The 4 Modes

- Normal
- Insert
- Visual
- Command

# Anatomy of a Vim Keybind

Command Count Motion

- Command examples: d, c, y, v
- Count: number
- Motion: h, j, k, l, w, e, b

combination example: `d3j` deletes 3 lines from current line

# Entering Insert Mode

i - insert from the front of cursor

a - insert from behind the cursor

I - insert at the beginning of the line

A - insert at the end of the line

o - insert line below and go to insert mode

O - insert line above and go to insert mode

# Entering Visual Mode

v - highlight by character

V - highlight by line

# Horizontal Movements

f(character) - find character

t(character) - find and place cursor before the character

F(character) - find character behind the cursor

T(character) - find and place cursor before the character behind the cursor

use `;` to find next character, `,` to go to previous character

# Vertical Movements

Ctrl+d - jump half way down

Ctrl+u - jump half way up

gg - go to the top of the file

G - go to the bottom of the file

:(line number)/ (line number)gg - go to the exact line

- - load a word into the search term

# Advanced Vim Motions

- - load a word into the search term using

o in visual mode - jump to the start/ end of a highlighted block

yap - yank contiguous paragraph

## regex replace

```go
// given
Foo
Bar
Baz

// want
data[0] = "Foo";
data[1] = "Bar";
data[2] = "Baz";
```

steps:

1. highlight the words.
2. use `:'<,'>s/\\(w.*\\)/data[0] = "\\1"` to add the `data[0] = "..."` (the matched word inside the brackets can be called using `\\1`)
3. increment the number inside `[]` using `gCtrl+aCtrl+a`

# Anatomy of Vim Structure

Buffer: vim representation of a file. When a file is opened, a buffer is created

Window: a visual interface of a buffer. when changing the file without exiting vim, the window will be the same but the buffers will change