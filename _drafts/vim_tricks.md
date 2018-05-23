# Vim Tricks

## Edit multiple files

### Method 1: Using buffers

Open both files directly: `vim file1.txt file2.txt`. The files are opened in vim in the same order.
The following commands can be issued:

Next file `:n`
Previous file `:N`
Save changes in current file: `ZZ`
Previous file with discard of changes: `:N!`
List open files: `:buffers` 
Change to other buffer by number: `: buffer 1`
Open additional file: `:e file3.txt`
Insert file into current buffer: `:r file3.txt`

Note that n and N cannot be used to cycle through files opened with :e.
Yanking and pasting can be done across buffers.

### Method 2: Screen splitting

Horizontal split: `vim -o file1.txt file2.txt`
Vertical split: `vim -O file1.txt file2.txt`

Switch windows: `CTRL+w, w`
Select top window: `CTRL+w k`
Select bottom window: `CTRL+w j`
Select left window: `CTRL+w h`
Select right window: `CTRL+w l`

Difference here is that ZZ closes the file as well. 
Issuing :wq causes only one window to close while the whole editor closed in method 1. 
