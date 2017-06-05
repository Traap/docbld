# docbld - Document Build 
The **docbld** repository simplifies compiling LaTeX files into pdf documents.

## Installation
```bash
$ cd $HOME
$ git clone git@github.com:Traap/docbld.git
```

## Add this function to .bashrc
```bash
function docbld() {
  rake -- rakefile ${HOME}/docbld/Rakefile $1
}
```

## Use
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

## Convention
**docbld** recursively searches the current directory for files with a texx 
extension.  Upon finding them, it compiles them into pdf documents,  moves 
the pdf documents to the **_build** directory, and eliminates temporary files.

# Project Management
Please refer to my [Lightweight Project Mangement](https://github.com/Traap/lpm)
for the project management strategy I use.
