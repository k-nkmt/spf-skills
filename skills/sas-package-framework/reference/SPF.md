# SAS Packages Framework Macro Reference

## Overview
This document is a technical reference for each macro in the SAS Packages Framework (SPF).

---

## Package Usage Macros

### `%installPackage()`
Installs packages from the Internet.

**Parameters:**
- `packagesNames` - Space-separated list of package names (required)
- `sourcePath=` - Source URL for packages
- `mirror=` - Mirror site specification (`0`=SASPAC, `1`=yabwon, `2`=mini.pw.edu.pl, `3`=PharmaForest)
- `version=` - Historical version to install
- `replace=` - Whether to replace existing files (default: `1`)
- `backup=` - When set to `1` and a package file exists, it creates a backup copy like `_BCKP_yyyymmddJJMMSS` (default: `0`)  
- `URLuser=` - Username for password-protected URLs
- `URLpass=` - Password for password-protected URLs
- `URLoptions=` - Options for URL filename
- `loadAddCnt=` - Whether to load additional content (`0`=No, `1`=Yes)
- `instDoc=` - Whether to also download documentation (.md) (`0`=No, `1`=Yes)
- `SFRCVN=` - Macro variable name to store return value (`<successes>.<failures>` format)
- `github=` - GitHub username or organization name
- `githubRepo=` - A name of a repository in GitHub, Use this option for repository names with uppercase letters.                                                                                   

**Usage Examples:**
```sas
%installPackage(SQLinDS)
%installPackage(baseplus(1.17) macroarray(1.0))
%installPackage(OncoPlotter, mirror=3)  /* From PharmaForest */
```

---

### `%loadPackage()`
Loads a package into the SAS session (makes macros, functions, formats, etc. available for use).

**Parameters:**
- `packageName` - Package name (required)
- `path=` - Package location (default: `packages` fileref)
- `options=` - ZIP filename options (default: `LOWCASE_MEMNAME`)
- `source2=` - Whether to print loading details
- `requiredVersion=` - Required version specification
- `lazyData=` - List of lazy datasets to load (`*`=all)
- `zip=` - Format other than zip file (e.g., `disk`)
- `cherryPick=` - List of elements to selectively load (default: `*`=all)
- `loadAddCnt=` - Whether to load additional content (`0`=No, `1`=Yes)
- `suppressExec=` - Whether to suppress `exec` type files (default: `0`)
- `DS2force=` - Whether to overwrite datasets in PROC DS2 packages (default: `0`)

**Utility Macros for IML/CASL:**
Automatically generated when loading packages containing IML modules or CASL UDFs:
- `%<packageName>IML()` - Used in Proc IML
- `%<packageName>CASLudf()` - Used in Proc CAS

**Usage Examples:**
```sas
%loadPackage(SQLinDS)
%loadPackage(BasePlus, cherryPick=getVars)
%loadPackage(myPackage, lazyData=dataset1 dataset2)

proc IML;
  %myPackageIML()
  /* Use IML modules */
quit;
```

---

### `%loadPackageS()`
Wrapper macro to load multiple packages at once (with default options only).

**Parameters:**
- `packagesNames` - Comma-separated list of package names (required)
  - Version specification available: `myPackage1(1.7), myPackage2(4.2)`

**Usage Examples:**
```sas
%loadPackageS(SQLinDS, DFA)
%loadPackageS(baseplus(1.17), macroarray(1.0), dfa(0.5))
```

---

### `%unloadPackage()`
Unloads a package from the SAS session.

**Parameters:**
- `packageName` - Package name (required)
- `path=` - Package location (default: `packages` fileref)
- `options=` - ZIP filename options (default: `LOWCASE_MEMNAME`)
- `source2=` - Whether to print details
- `zip=` - Format other than zip file (e.g., `disk`)

**Usage Examples:**
```sas
%unloadPackage(SQLinDS)
```

---

### `%loadPackageAddCnt()`
Explicitly loads additional content for a package.

**Parameters:**
- `packageName` - Package name (required)
- `path=` - Package location (default: `packages` fileref)
- `target=` - Location to extract additional content (default: WORK)
- `source2=` - Whether to print details
- `requiredVersion=` - Required version specification

**Usage Examples:**
```sas
%loadPackageAddCnt(SQLinDS)
%loadPackageAddCnt(SQLinDS, target=/my/custom/location)
```



---

### `%helpPackage()`
Prints help information about a package to the SAS Log.

**Parameters:**
- `packageName` - Package name (required)
- `helpKeyword` - Phrase to search (empty=description, `*`=all, `license`=license)
- `path=` - Package location (default: `packages` fileref)
- `options=` - ZIP filename options (default: `LOWCASE_MEMNAME`)
- `source2=` - Whether to print details
- `zip=` - Format other than zip file (e.g., `disk`)
- `packageContentDS=` - Whether to generate dataset of package content (`0`=No, `1`=Yes)

**Usage Examples:**
```sas
%helpPackage(SQLinDS)
%helpPackage(SQLinDS, *)
%helpPackage(SQLinDS, license)
```

---

### `%previewPackage()`
Previews package content to the SAS Log.

**Parameters:**
- `packageName` - Package name (required)
- `helpKeyword` - Phrase to search (empty=description, `*`=all, `license`=license)
- `path=` - Package location (default: `packages` fileref)
- `options=` - ZIP filename options (default: `LOWCASE_MEMNAME`)
- `source2=` - Whether to print details
- `zip=` - Format other than zip file (e.g., `disk`)

**Usage Examples:**
```sas
%previewPackage(SQLinDS)
```

---

## Package Development Macros

### `%generatePackage()`
Generates a SAS package. Wraps content into a ZIP file and generates required metadata.

**Parameters:**
- `filesLocation=` - Package files location (required)
- `buildLocation=` - ZIP file generation location (default: `filesLocation`)

**Test Parameters:**
- `testPackage=` - Whether to execute tests (default: `Y`)
- `packages=` - Location of dependent packages for testing
- `testResults=` - Test results storage location (default: WORK)
- `workInTestResults=` - Whether to place test session WORK in same location as results (`0`/`1`)
- `testWorkPath=` - Test session WORK directory location
- `sasexe=` - SAS binary location (default: `!SASROOT`)
- `sascfgFile=` - Test session config file (`DEF`=`!SASROOT/sasv9.cfg`)
- `delTestWork=` - Whether to delete test WORK (default: `1`)

**Documentation Parameters:**
- `markdownDoc=` - Whether to generate Markdown documentation (`0`/`1`)
- `easyArch=` - Whether to create versioned copy (`0`/`1`)
- `archLocation=` - Versioned ZIP file location

**Usage Examples:**
```sas
%generatePackage(filesLocation=/path/to/packagename)
%generatePackage(filesLocation=/path/to/packagename, markdownDoc=1, easyArch=1)
```


---

### `%splitCodeForPackage()`
Splits a single code file into multiple files and directories based on tags.

**Parameters:**
- `codeFile=` - Code file name to split (required)
- `packagePath=` - Location for split files (required, uses WORK if not exists)
- `debug=` - Debug code output (optional)
- `nobs=` - Technical parameter (default: `0`, do not change)

**Tag Specification:**
```sas
/*##$##-code-block-start-##$## <type>(<name>) */
/* Code */
/*##$##-code-block-end-##$## <type>(<name>) */
```

**Usage Examples:**
```sas
%splitCodeForPackage(codeFile=C:/lazy/myPackageCode.sas, packagePath=C:/split/)
```

---


## Package Management Macros

### `%listPackages()`
Prints a list of available packages to the SAS Log.

**Parameters:**
- `listDataSet` - Dataset name to store results
- `quiet=` - Whether to suppress log output (`0`=print, other=suppress)

**Usage Examples:**
```sas
%listPackages()
%listPackages(ListDS, quiet=1)
```

---

### `%verifyPackage()`
Generates SHA256 hash of a package and compares it with the provided hash for verification.
*Minimum SAS version: 9.4M6*

**Parameters:**
- `packageName` - Package name (required)
- `hash=` - SHA256 hash value for verification. If not provided, print hash digest to log. 
- `path=` - Package location (default: `packages` fileref)

**Usage Examples:****
```sas
%verifyPackage(SQLinDS, hash=HDA478ANJ3HKHRY327FGE88HF89VH89HFFFV73GCV98RF390VB4)
```

---

### `%relocatePackage()`
Copies or moves packages locally.

**Parameters:**
- `packageName` - Package name or space-separated list (required)
- `source=` - Source location (mutually exclusive with `target=`)
- `target=` - Target location (mutually exclusive with `source=`)
- `sDevice=` - Source device type (`DISK`/`ZIP`/`FILESRVC`, default: `DISK`)
- `tDevice=` - Target device type (`DISK`/`ZIP`/`FILESRVC`, default: `DISK`)
- `checksum=` - Copy only if checksum differs (default: `0`)
- `move=` - Move instead of copy (deletes source after copying, default: `0`)
- `debug=` - Print debug information (default: `0`)
- `try=` - Number of retries on copy failure (1-9, default: `3`)

**Usage Examples:****
```sas
%relocatePackage(SQLinDS, source=/files/packages/, sDevice=FILESRVC)
%relocatePackage(BasePlus SQLinDS MacroArray, target=D:/archive/bundle.zip, tDevice=ZIP, move=1)
```

---

### `%bundlePackages()`
Bundles multiple packages into a single bundle file.

**Parameters:**
- `bundleName` - Bundle name (empty=auto-generated, `.bundle.zip` extension auto-added)
- `path=` - Bundle location (required, takes precedence over `pathRef=`)
- `pathRef=` - Fileref to bundle location
- `packagesList=` - Space-separated list of packages to bundle (empty=all)
- `packagesPath=` - Packages location (takes precedence over `packagesRef=`)
- `packagesRef=` - Fileref to packages location (default: `packages`)
- `ods=` - Report dataset name

**Usage Examples:**
```sas
%bundlePackages(myLittleBundle, path=/home/user/bundles, 
                packagesList=basePlus SQLinDS macroarray)
```

---

### `%unbundlePackages()`
Extracts packages from a bundle.

**Parameters:**
- `bundleName` - Bundle name (required, `.bundle.zip` extension auto-added)
- `path=` - Bundle location (required, takes precedence over `pathRef=`)
- `pathRef=` - Fileref to bundle location
- `packagesPath=` - Extraction destination (takes precedence over `packagesRef=`)
- `packagesRef=` - Fileref to extraction destination (default: `packages`)
- `ods=` - Report dataset name
- `verify=` - Execute verification after extraction (`1`=Yes, `0`=No)

**Usage Examples:**
```sas
%unbundlePackages(myLittleBundle, path=/home/user/bundles, 
                  verify=1, packagesRef=PACKAGES)
```


## Utility Macros

### `%isPackagesFilerefOK()`
Checks if the `packages` fileref is correctly configured (macro function).

**Parameters:**
- `vERRb` - Whether to print details (optional)

**Return Values:**
- `1` = OK
- `0` = Problem exists

**Usage Examples:**
```sas
%if %isPackagesFilerefOK() %then %do;
  %installPackage(SQLinDS)
%end;
```

---

### `%extendPackagesFileref()``
Returns the list of directories pointed to by the `packages` fileref. Used when adding new directories.

**Parameters:**
- `packages` - Valid fileref name (optional, uses "packages" if empty)

**Usage Examples:**
```sas
filename packages ("D:/NEW_DIR" %extendPackagesFileref());
```

---

### `%SasPackagesFrameworkNotes()`
Prints help notes for SPF macros.

**Parameters:**
- `SPFmacroName` - SPF component name(s) (space-separated, `*`=all, `HELP`=this help, empty=list)

**Usage Examples:**
```sas
%SasPackagesFrameworkNotes(*)                          /* All help */
%SasPackagesFrameworkNotes()                           /* Macro name list */
%SasPackagesFrameworkNotes(generatePackage helpPackage) /* Specific macros */
```


## Notes

### Reserved Keywords
- `packages` - Fileref pointing to package folder (user-configured)
- `package` - Used internally by framework (do not modify)

### Minimum SAS Version
- Basic functionality: SAS 9.4
- `%verifyPackage()`: SAS 9.4M6 or later

### Environments Without Zip Support
For environments that do not support ZIP file references, use the following approach:
```sas
/* After extracting package to packagename.disk folder */
%loadPackage(packageName, zip=disk, options=)
%helpPackage(packageName, , zip=disk, options=)  /* Note the double comma */
%unloadPackage(packageName, zip=disk, options=)
```

---

## Reference Links
- Documentation: https://github.com/yabwon/SAS_PACKAGES/tree/main/SPF/Documentation
- Tutorials: https://github.com/yabwon/HoW-SASPackages
- Package Repository: https://github.com/SASPAC/
- PharmaForest: https://github.com/PharmaForest/
