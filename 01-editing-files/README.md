# Neovim 101 — edit real files

Practice files for varCodeFather Back to Basics. Video in production; these files were rehearsed with Neovim 0.12.6 in a local Arch VM. No Docker daemon or AWS account needed for this editing lesson.

## Start

From this tutorial folder, copy starter files into a disposable practice folder:

```bash
cp -r start practice
cd practice
```

Use a fresh name if `practice` already exists; don't overwrite work you want to keep. `expected` contains final Compose and HTML examples. Notes are your own practice space and have no required final wording.

Open files with `nvim --clean` to follow without custom plugins; this skips your usual configuration for that session. It does not remove it. Commands use built-in editing features. On Arch: `sudo pacman -S --needed neovim`; on macOS: `brew install neovim`; on Ubuntu: `sudo apt install neovim`.

## Key notation

`<Esc>`, `<Enter>`, `<Ctrl+r>` and `<Ctrl+w>` mean keys, not literal text. Other characters in key sequences are typed in order. Start each Normal-mode sequence with Escape if unsure of mode; don't press Escape halfway through an Insert-mode replacement. Uppercase and lowercase differ. `:w` saves; `:q` closes a window; `:wq` saves and quits; `:q!` deliberately discards unsaved changes in current buffer.

## Walkthrough

### 01 — Get comfortable before changing files

**Use a practice copy**

```bash
pwd
ls -R
```

### 02 — Install and open the editor

**Install Neovim**

```bash
sudo pacman -S --needed neovim
```

**Check the installed command**

```bash
nvim --version | head -3
```

**Open notes without custom plugins**

```bash
nvim --clean notes.md
```

### 03 — Make a change, save it and recover

**Enter Insert mode**

Keys:

```text
iPractice copy<Enter>
<Esc>
```

Buffer contains: `Practice copy`.

**Save without leaving**

Keys:

```text
:w<Enter>
```

Saved file contains: `Practice copy`.

**Undo an accidental edit**

Keys:

```text
ggjdd
```

Buffer no longer contains: `# Local development notes`.

**Bring the heading back**

Keys:

```text
u
```

Buffer contains: `# Local development notes`.

**Save and quit**

Keys:

```text
:wq<Enter>
```

**Check the saved text**

```bash
head -4 notes.md
```

**Make one unsaved change**

Open `notes.md` with `nvim --clean notes.md`.

Then keys:

```text
ggSDo not keep this line<Esc>
```

Buffer contains: `Do not keep this line`.

**See the unsaved-change message**

Read unsaved-change message, then press Enter to dismiss before continuing.

Keys:

```text
:q<Enter>
<Enter>
```

Buffer contains: `Do not keep this line`.

**Discard only the unsaved edit**

Keys:

```text
:q!<Enter>
```

**Confirm the earlier save survived**

```bash
head -1 notes.md
```

### 04 — Move to text and change it

**Open a real configuration file**

Open `compose.yaml` with `nvim --clean compose.yaml`.

Then keys:

```text
:set number<Enter>
```

**Jump to a line**

Keys:

```text
3G0
```

**Move by words**

Keys:

```text
wwbb
```

**Search for the value you need**

Keys:

```text
/8080<Enter>
```

**Change the four digits**

Keys:

```text
c4l8090<Esc>
```

Buffer contains: `127.0.0.1:8090:80`.

**Undo and redo the port change**

Keys:

```text
u<Ctrl+r>:w<Enter>
```

Saved file contains: `8090`.

**Keep the saved port for later**

Keys:

```text
:q<Enter>
```

### 05 — Edit lines, use Visual mode and search

**Work on the notes again**

Open `notes.md` with `nvim --clean notes.md`.

Then keys:

```text
:set number<Enter>
ggdd
```

Buffer no longer contains: `Practice copy`.

**Copy a whole line**

Keys:

```text
/Check Docker<Enter>
yyp
```

**Remove the duplicate with undo**

Keys:

```text
u
```

**Select whole lines**

Keys:

```text
/Check Docker<Enter>
Vj
```

**Indent a selection and undo it**

Keys:

```text
>u
```

**Change a word under the cursor**

Keys:

```text
/draft<Enter>
ciwready<Esc>
```

Buffer contains: `Status: ready`.

**Search forwards and backwards**

Keys:

```text
?Project<Enter>
/Status<Enter>
```

**Clear search highlighting**

Keys:

```text
:nohlsearch<Enter>
:w<Enter>
```

Saved file contains: `Status: ready`.

### 06 — Work across files without losing your place

**Open the HTML file in this session**

Keys:

```text
:edit site/index.html<Enter>
```

Current file: `site/index.html`.

**Change text between the heading tags**

Keys:

```text
/Hello<Enter>
citEdited with Neovim<Esc>:w<Enter>
```

Buffer contains: `<h1>Edited with Neovim</h1>`.

**List the buffers**

Keys:

```text
:ls<Enter>
```

**Return to the previous file**

Keys:

```text
<Enter>
:b#<Enter>
```

Buffer contains: `Status: ready`.

Current file: `notes.md`.

**See two files together**

Keys:

```text
:set splitright<Enter>
:vsplit compose.yaml<Enter>
```

Buffer contains: `8090`.

Current file: `compose.yaml`.

Open editor windows: 2.

**Move between windows**

Keys:

```text
<Ctrl+w>h<Ctrl+w>l
```

Current file: `compose.yaml`.

Open editor windows: 2.

**Close a view without deleting a file**

Keys:

```text
:close<Enter>
```

Current file: `notes.md`.

Open editor windows: 1.

**Leave the editor**

Keys:

```text
:q<Enter>
```

**Check the saved project changes**

```bash
grep -n '8090' compose.yaml
head -1 site/index.html
grep -n 'Status:' notes.md
```

### More saving and quitting options

These examples follow the two-file/split-window section. `:q!` closes the current window without saving; `:qa!` quits all windows and abandons unsaved changes; `:wqa` writes changed files and quits all windows. Check save errors before assuming files were written.

Open a new note:

```bash
nvim --clean save-one.txt
```

Type `i`, enter `First note to keep`, then Escape. Open the second note:

```vim
:vsplit save-two.txt
```

Type `i`, enter `Second note to keep`, then Escape. Both files now have unsaved changes. Type `:wqa`, read the command, then press Enter. Verify both saves:

```bash
cat save-one.txt
cat save-two.txt
```

Reopen `save-one.txt`, press `Go`, add `Unwanted change one`, then Escape. Open `save-two.txt` with `:vsplit save-two.txt`, press `Go`, add `Unwanted change two`, then Escape. Type `:qa!`, read the command, then Enter to discard both unsaved additions.

```bash
cat save-one.txt
cat save-two.txt
```

Expected: only `First note to keep` and `Second note to keep`; unwanted additions were not written. Website files remain unchanged by this exercise.

### 07 — A small configuration and a practical exercise

**Set indentation for this session**

Open `compose.yaml` with `nvim --clean compose.yaml`.

Then keys:

```text
:set number expandtab shiftwidth=2 softtabstop=2<Enter>
```

Buffer contains: `8090`.

**Indent the nested block together**

Keys:

```text
/ports:<Enter>
Vj>u
```

Buffer contains: `    ports:`.

**Find the built-in help**

Keys:

```text
:help iw<Enter>
```

Open editor windows: 2.

**Return from help**

Keys:

```text
:q<Enter>
```

Buffer contains: `8090`.

Current file: `compose.yaml`.

Open editor windows: 1.

**Try this change yourself**

Keys:

```text
/8090<Enter>
c4l8080<Esc>:w<Enter>
```

Saved file contains: `8080`.

**Restore the target port**

Keys:

```text
/8080<Enter>
c4l8090<Esc>:w<Enter>
```

Saved file contains: `127.0.0.1:8090:80`.

**Save and leave the finished project**

Keys:

```text
:q<Enter>
```

**Compare the finished files**

```bash
diff -u ../expected/compose.yaml compose.yaml
diff -u ../expected/site/index.html site/index.html
```

## Final check

From `practice`, both commands finish without differences:

```bash
diff -u ../expected/compose.yaml compose.yaml
diff -u ../expected/site/index.html site/index.html
```

Host port should be 8090; container port stays 80. HTML heading should read `Edited with Neovim`. Lesson does not start containers. Existing Docker tutorial explains how to run this web service.

Tutorial sets `splitright` before `:vsplit` so new file opens on right; default split direction may differ in your own configuration. Session settings such as `number`, `expandtab`, `shiftwidth` and `softtabstop` do not write an init file.

## Built-in help and references

- `:help iw` — inner-word text object.
- `:Tutor` — guided practice inside Neovim.
- [First steps](https://neovim.io/doc/user/usr_02/)
- [Moving around](https://neovim.io/doc/user/usr_03/)
- [Editing more files](https://neovim.io/doc/user/usr_07/)
- [Inserting quickly](https://neovim.io/doc/user/usr_24/)
- [Editor settings](https://neovim.io/doc/user/usr_05/)
