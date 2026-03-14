# Guide for Package Developers

For those creating packages.
Packages do not need to be published; they can be used personally or internally.
When publishing, GitHub should be used as a rule.

## Naming Constraints
Package names must be 3 to 24 characters, using only letters, underscores, and numbers, and cannot start with a number.
As a regular expression pattern: [a-zA-Z_]\w{2,23}
Versions use numbers like 1.0, 0.10.1, etc.

File and folder names under the package root must use only lowercase letters, not uppercase.
Excluded folders and non-target folders are also subject to naming rule checks.


## Folder and File Structure
Due to naming conventions, if you want to place various files unrelated to the package such as PDFs, it is recommended to separate the package source.

Simple structure:
```
root
 ├─ description.sas      /* Required: Package metadata */
 ├─ license.sas          /* Optional: Package license. If absent, defaults to MIT license */
 ├─ readme.md            /* When publishing on GitHub */
 ├─ macro/               /* Optional: Macro definitions */
 ├─ function/            /* Optional: FCMP functions (with Proc FCMP header) */
 └─ !image/              /* Folders starting with `!` are excluded from the package */
```

Flexible structure:
```
root
 ├─ LICENSE         /* Outside package, so file and folder names are free  */
 ├─ README.md           
 ├─ docs/               
 ├─ image/               
 └─  <packageName>
        ├─ description.sas      /* Required: Package metadata */
        ├─ license.sas          /* Optional: Package license. Required separately from repository license. If absent, defaults to MIT license */  
        ├─ macro/               
        └─ function/            
```

### Supported Features and Order

Create folders for each feature and save corresponding programs in them.
All programs must be placed directly under each feature folder; subfolders cannot be used.
addcnt is for various files including non-program files, and subfolders can be used.
For features other than addcnt, files other than SAS programs are excluded from packaging.

Supported features are:

- libname
- macro   
- function
- functions
- format  
- formats  
- imlmodule
- proto   
- exec    
- clean   
- lazydata
- test    
- casludf 
- addcnt  
- kmfsnip 
- ds2pck  
- ds2thr  

When there are dependencies, such as formats using macros, setting a number_ prefix like 00_libname will load them in the order of the number part.
The order in documentation can also be adjusted.
Except for addcnt, you can use multiple folders by changing the number part.
If these are not particularly needed, there is no need to add numbers to the beginning.

The difference between function/functions and format/formats is whether you write the `proc ...` part yourself.
If there is no particular need to write it, use the ones with 's' at the end.

exec is used when you want to execute code directly when there is no suitable type.
The code can be executed directly when loading the package.
In that case, a file with the same name must be created in clean, which is executed when unloading.


## Package Information
Package information requires description.sas at the package root.
With the following structure, all items except Required and ReqPackages are mandatory:

```
Type: Package
Package: PackageName
Title: A title/brief info for log note about your packages.
Version: X.Y
Author: Firstname1 Lastname1 (xxxxxx1@yyyyy.com), Firstname2 Lastname2 (xxxxxx2@yyyyy.com)
Maintainer: Firstname Lastname (xxxxxx@yyyyy.com)
License: MIT
Encoding: UTF8

Required: "Base SAS Software"
ReqPackages: "xxx", "yyy (0.1)"  

DESCRIPTION START:
  Xxxxxxxxxxx xxxxxxx xxxxxx xxxxxxxx xxxxxxxx. 
DESCRIPTION END:
```

## Notes on Publishing to GitHub
When publishing to GitHub, place the package file (packageName.zip) at the repository root.
Package documentation (packageName.md) is optional, but it is recommended to also place it at the root when publishing.  

If you are considering publishing packages related to the pharmaceutical field, we recommend publishing or collaborating through PharmaForest.  
For more information about PharmaForest, see [PharmaForest.md](PharmaForest.md).  

### Managing Past Versions
Framework features allow you to save past versions with different names.
Previously, package files were placed in a hist folder, but now version tags are set in releases.
The hist folder is not recommended for publication as it causes repository bloat; for personal use, exclude it from version control with .gitignore.

### Handling README and Other Files
For simple structures where the repository root is the package root, it is recommended not to create README and LICENSE on the web when creating the repository, as they will be uppercase.
If created, problems may occur such as being treated as separate files on GitHub even after simple renaming, so rename using git mv.  

## Documentation and Comments
Each program, except for test programs, requires comments for help purposes.
Package documentation is generated by collecting help comments.
Since documentation is treated as markdown format, it is recommended to use markdown syntax appropriately in comments.
The strings `/*** HELP START ***/` and `/*** HELP END ***/` are targeted.
In addition to block comments, it is also possible to write normal code and comments in between.
For simple creation, writing together with code is acceptable, but it becomes a bit unclear in documentation, so if possible, block comments that are easy to use with markdown syntax are better for documentation.

Block comment:
```sas
/*** HELP START ***//* 
Everything inside is a comment
*//*** HELP END ***/
```

Combined with macro specification:
```sas
%macro sample(
/*** HELP START ***/
  param =  /* Parameter description */
/*** HELP END ***/
) ;
```

Each file becomes a unit in help and documentation, so especially for features that users call, keep files separate like one file per macro.

Internal programs that users do not call can be excluded from help and documentation.
To exclude, write one of the following comments at the beginning of the file:

- `/*##ExcludeFromDocumentation##*/`
- `/*##ExcludeFromDocumentation##*/`
- `/*##ExcludeFromMarkdownDoc##*/`

If you want more rich documentation, refer to Advanced Documentation: Documentation.md

### Markdown Formatting Guidelines

When writing documentation comments, follow these guidelines to ensure compatibility with both plain Markdown readers and documentation engines (MyST/Sphinx):

**Use Standard Markdown Syntax:**
- Use plain Markdown that renders well both as-is and when built with documentation tools
- Use standard Markdown tables for structured data
- Use H3 or lower for section headings (e.g., `### Parameters`)

**Special Characters:**
- MyST/Sphinx may misinterpret certain characters: `%`, `&`, `{`, `}`, `<`, `>`
- Wrap these in backticks or escape them when used in documentation (e.g., `%macro`, `&var`)
- Use backticks for code elements: parameter names, dataset names, file paths, function names

**Documentation Structure Example:**
````markdown
Brief description

### Parameters

- **paramName**: Description with default value if applicable
- **anotherParam**: Use backticks for special chars like `%macro` 

### Output

Description of output or results

### Usage Example

```sas
%macroName(param1=value1, param2=value2);
```
````

## Testing
Framework features allow testing to be performed simultaneously when building packages.
Testing is performed using X commands, so in environments where X commands cannot be used, such as SAS OnDemand, a warning message will appear and it will be skipped.
Tests are executed in a new session, and by default, a folder with a name like test_YYYYMMDDtHHMMSS is created in the WORK of the current session, where logs and listings are saved.
Testing is performed by counting the number of Errors and Warnings from the log, so if unexpected behavior occurs, it is good to ensure messages appear in the log.

## Package Creation Features

### Framework Features and Services

Use `%generatePackage` to create packages.
Basically, documentation should also be generated.
By default, packages are created in the package root folder `filesLocation`.
If not publishing to GitHub or if you want to verify operation before publishing, it needs to be placed in the package folder.
Also, when the package root and repository root differ, it is good to set the `buildLocation`.  

It's good to save build code in the project, but since it contains local paths, exclude it with .gitignore.
If testing is not performed or file operations are needed, it is good to document build settings in the README.

**Execution Example:**
```sas
%generatePackage(filesLocation=/path/to/packagename, buildLocation=package_save_destination, markdownDoc=1)
```

If you want to handle multiple macros in one file during development, `%splitCodeForPackage` can split them into multiple files.

**Tag Specification:**
```sas
/*##$##-code-block-start-##$## <type>(<name>) */
/* Code */
/*##$##-code-block-end-##$## <type>(<name>) */
```

**Usage Examples:**
```sas
%splitCodeForPackage(codeFile=program_path, packagePath=package_path)
```

Splits a single code file into multiple files and directories based on tags.

### Support Package
If unfamiliar with git or IDEs, installing [SASPACer](https://github.com/SASPAC/saspacer) allows you to manage package information in Excel, create package files and folders, and even create packages using `%generatePackage`.

## References
- Supported features and examples: reference/SupprtedType.md
- Framework features: reference/SPF.md
- About PharmaForest: PharmaForest.md
- Advanced Documentation: Documentation.md