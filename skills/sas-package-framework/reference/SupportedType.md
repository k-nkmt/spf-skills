# Package Supported Feature Types and Examples

Note: Although omitted here, each program file actually requires help comments `/*** HELP START ***/` and `/*** HELP END ***/`

## libname
For library assignments.
When distributing code outside the organization, mainly used when you want to set a dedicated libname in WORK.  

```
options DLCREATEDIR; 
libname mylib "%sysfunc(pathname(work)）/mylib" ;
options NODLCREATEDIR;
```

## macro   
Folder for macros.
As a rule, use one file per macro, and set the file name to the macro name.
There are no particular constraints compared to regular macros.

## function
When the function type is used a definition of a function has to be enclosed in the following template of the FCMP procedure:

```
proc fcmp
  inlib = work.&packageName.fcmp /* optional */
  outlib = work.&packageName.fcmp.package
  <... other options ...>
  <... function or subroutine body ...>
run;
quit;
```

The inlib= and outlib= options are, literally, set to: "work.&packageName.fcmp" and "work.&packageName.fcmp.package" respectively.   
In case when functions from other packages are required to be used the inlib= option may be extended e.g. inlib=(work.&packageName.fcmp work.OnePackagefcmp work.SecondPackagefcmp).

## functions
When the functionstype (mind the "s"!) is used a definition of a function must not to be enclosed in the FCMP procedure.  
In this case it has to be plain code of the function. All functions defined in the ..._functions subfolder will be compiled with one Proc FCMP execution.  
An example of such "plain" subroutine code could be as follows:

```
subroutine increaseByOne(a[*]);
  outargs a;
  do i = 1 to dim(a);
    a[i] = a[i] + 1;
  end;
  return;
endsub;
```

## format  
When type format is used then a definition of a format/informat has to be enclosed in the following template of the FORMAT procedure:
```
proc format
lib = work.&packageName.format
<... other options ...>
;
<... format/informat definition ...>
run;
```
The lib= option is, literally, set to: "work.&packageName.format".

## formats  
When the formats type (mind the "s"!) is used, a definition of a format/informat must not be enclosed in the FORMAT procedure.  
In this case it has to be plain code of the format/informat.  
All formats/informats defined in the ..._formats subfolder will be compiled with one Proc FORMAT execution.  
An example of such "plain" format/informat code could be as follows:
```
value abc
  low -< 0 = "negative"
         0 = "zero"
 0 <- high = "positive"
     other = "missing"
;
```

## imlmodule
A definition of an imlmodule has to be plain code of the IML module.  
All modules defined in the ..._imlmodule subfolder are compiled with one Proc IML execution and are stored in the work.&packageName.iml catalog.

## proto   
A definition of a proto external C function has to be plain code of the PROTO C function.  
All functions defined in the ..._proto subfolder are compiled with one Proc Proto execution and are stored in the work.&packageName.proto dataset.  
Function code has to contain the header, and the body of the function between externc and externcend, for example:

```
int doublePlusOne(int x);

externc doublePlusOne;
  int doublePlusOne(int x)
    return (2*x + 1);
externcend;
```

## exec    
exec folders are for so-called "free code", i.e., if a package requires some additional code to be run to be ready and usable (code not fitting provided types), this code has to be inserted into a file inside one of the exec subfolders.

## clean   
clean folders are for cleaning after execs, i.e., if code from one of the exec folders creates some object (e.g., a catalog, a macro, or a dataset), the appropriate code inside a clean subfolder has to be developed to remove that created object.  

Note: Each exec file should have a clean file counterpart and vice versa.  
exec is executed during `%loadPackages`, and clean is executed during `%unloadPackages`.
If the number of exec files and clean files differs but both are positive, a warning is issued.  
But if execs are positive and cleans are zero (or the other way around), an error is issued!

## lazydata
lazydata folders are for data generation.  
Code can be data steps, PROC SQL, etc.
lazydata are not automatically loaded when the %loadPackage() macro is executed.   
To load such dataset the user has to call the %loadPackage() macro with non missing lazyData= parameter, e.g %loadPackage(PiPackage, lazyData=work.first1E6digits) (list of multiple elements separated by space is allowed, an asterisk(*) means "load all data").  
In case when regular dataset provided during loading process has to be reloaded the lazyData= parameter can be used to do that.

## test  
test folders contain developer code with package tests.
Place files containing code you want to execute when creating a package with `%generatePackage`.
Within test files, there is no need to specify the packages path or load the package being tested.
Tests are executed in a new session using X commands, and listings and logs are saved in a folder created in the WORK of the running session in the format test_YYYYMMDDtHHMMSS.
The number of warnings and errors in the log is also checked.

## casludf 
A definition of a casludf has to be plain code of the CASL user defined function. An example of such "plain" CASL UDF code could be as follows:

```
function myFunction(x, y, z);
  result = x + y + z;
  return (result);
end func;
```

All UDFs defined in all ..._casludf subfolders are included with into Proc CAS by execution of single utility macro (see "User’s utility macros for loading package content" subsection for details).  
UDFs definitions are stored inside the package and are loaded "on the fly" when the utility macro is called.

## addcnt 
The addcnt folder (there can be only one such folder) contains all possible additional content that the developer wants to add to the package, e.g., a PDF file with documentation, files with graphs, plots, and figures, an HTML page, a markdown file, etc.  
The structure inside that folder can be arbitrary; it can contain files and subfolders, including nested ones. The content of the addcnt folder is transported into the package zip in binary form.

## kmfsnip 
kmfsnip folders are dedicated for kmf (keyboard-macro-file) files.
The structure of this type of file has the following form:

```
kmfCodeDesc: <The snippet single line description text.>
kmfCodeStart:
  <Code of the snippet,
  can be in multiple
  lines long.>
kmfCodeEnd:
```

## ds2pck  
ds2pckfolders are dedicated for the code of PROC DS2 packages. Structure of the file should be:

```
package PackageName / overwrite=yes;
<... DS2 package code ...>
endpackage;
```

By default, if there exist a SAS data set (which is not a DS2 package file) a warning is issued and the package data set is not generated.  
To force overwrite, set the DS2force= parameter of the %loadPackage() macro to 1.

## ds2thr  
ds2thr folders are dedicated for the code of PROC DS2 threads.  
Structure of the file should be:

```
thread ThreadName (<...parameters...>) / overwrite=yes;
<... DS2 thread code ...>
endthread;
```

By default, if there exist a SAS data set (which is not a DS2 thread file) a warning is issued and the thread data set is not generated.  
To force overwrite, set the DS2force= parameter of the %loadPackage() macro to 1.