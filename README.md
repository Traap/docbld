## docbld - Document Build 
The **docbld** repository simplifies compiling LaTeX files into pdf documents.

### Installation
```bash
$ cd $HOME
$ git clone git@github.com:Traap/docbld.git
```

### Add this function to .bashrc
```bash
DOCBLDPATH=${HOME}/git/docbld

export DOCBLDPATH

function docbld() {
  rake --rakefile ${DOCBLDPATH}/Rakefile $1
}

```

### Use
```
$ docbld -T
  rake clean           # Remove any temporary products.
  rake clobber         # Remove any generated files.
  rake copy_files      # Copy files to _build directory.
  rake default         # Default deploy task.
  rake deploy          # Build and deploy documents to _build directory.
  rake list_files      # List texx files to compile.
  rake remove_diskdir  # Remove _build directory.
  rake texx            # Compile tex to pdf.
```

### Convention
**docbld** recursively searches the current directory for files with a texx 
extension.  Upon finding them, it compiles them into pdf documents,  moves 
the pdf documents to the **_build** directory, and eliminates temporary files.
