# Working with Shell

## Working with files

* View file size (Disk usage)

```bash
du -sk test.img
```

```bash
du -sh test.img
```

```bash
ls -lh test.img
```

### Archiving Files

`TAR` - utility used to group multiple files into one

* Create tar

```bash
tar -cf test.tar file1 file2 file3
```

* See contents of the tar file

```bash
tar -tf test.tar
```

* Extract the contents of the tar file

```bash
tar -xf test.tar
```

* Compress the tar file

```bash
tar -zcf test.tar file1 file2 file3
```

### Compressing files

* `bzip2` `gzip` and `xz` - are popular compressing tools
* `bunzip2`, `gunzip` and `unxz` - uncompress the files

### Reading compressed files without decompressing

```bash
zcat /usr/share/man/man1/tail.1.gz | tee /home/bob/pipes
```

## Searching for Files and Directories

```bash
locate City.txt
```

To update the db if locate does not return anything

```bash
updatedb
```

```bash
find /home/michael -n City.txt
```

## Search within Files | GREP

```bash
grep second sample.txt
```

To search case insensitive

```bash
grep -i capital sample.txt
```

To search recursively

```bash
grep -r "third line" /home/michael
```

Print the lines that don't contain a string

```bash
grep -v "printed" sample.txt
```

Print exact matches

```bash
grep -w examp example.txt
```

Print the linse that don't contain the entire word

```bash
grep -vw exam examples.txt
```

Print one line after the match

```bash
grep -A1 Arsenal premier-league-table.txt
```

Print one line before a match

```bash
grep -B1 Arsenal premier-league-table.txt
```

Print one line after and before a match


```bash
grep -A1 -B1 Arsenal premier-league-table.txt
```

## IO Redirection

### Standard Streams

* Standard input
* Standard output
* Standard error

with IO redirection, std streams can be redirected to text files

#### Redirect std output: `>` and `>>`

Overwrite with the file with the command output

```bash
echo $SHELL > shell.txt
```

Append to file

```bash
echo 'hello world' >> shell.txt
```

#### Redirect stderr: `2>` and `2>>`

```bash
cat missing_file 2> error.txt
```

To not print any errors:

```bash
cat missing_file 2> /dev/null
```

## Command line pipes (`|`)

Allows chaining commands, where the first's cmd std out will be used as the std input for the second command.

### The `tee` command

Writes text to file, can be used with `|`

```bash
echo "Hello world" | tee [-a to append] shell.txt
```

## The VI Editor

```bash
vi /hom/michael/sample.txt
```

### Operation Modes

* When you open a file you'll be in "command mode" by default
* 

#### The command mode

* Issue commands to the editor (copy, paste, delete line etc.)
* Allows highlighting using the mouse
* Copy a line: `y y`
* To paste: `p`
* To save `z z`
* To delete a letter `x`
* to delete a line `d d`
* to delete 3 lines from the current line: `d 3 d`
* undo: `u`
* redo: `ctrl + r`
* to find forward: `/whatever text`. to go forward: `n`, to previous `N`
* to find backward: `?whatevertext`

#### The insert mode

* to switch: lower case `i`
* write text to file
* escape button: switch back to command mode

#### Last line mode

* while in command mode: `:` to switch to last line mode
* save file
* discard changes
* exit vi editor
* `escape` button: switch back to command mode
* to save: `:w`
* to quit: `q`
* to save and quit: `:wq`
* to save without confirmation `:q!`