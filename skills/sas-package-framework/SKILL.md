---
name: sas-packages-framework
description: Using the SAS Packages Framework (SPF) as a user and creating packages as a developer
license: MIT
metadata:
  version: "0.1"
---

# SAS Packages Framework

## Overview
The SAS Packages Framework is a framework for managing packages for the SAS language.
Since the SAS language does not have an official package management system, this is a user-created mechanism.
The official repository is https://github.com/yabwon/SAS_PACKAGES

## Package Mechanism
Packages bundle SAS macros, formats, FCMP functions, along with help and license information into a zip file.
To use packages, reference the storage location with `filename packages "<directory/containing/packages/>";` using packages as the fileref name.
To utilize framework features such as loading packages or creating packages, SPFinit.sas must be loaded.
SPFinit.sas is typically stored in the same folder as packages, and if so, can be loaded with `%include packages(SPFinit.sas);`
While adding packages and backups can be done through file operations, it is recommended to prioritize using framework-provided features when available.

## Installing and Updating the Framework
You can download and save the SAS program from the repository, or reference it directly as shown below.
With the framework features available, `%installPackage(SPFinit)` can save and update SPFinit.sas.

```sas
filename SPFinit url "https://raw.githubusercontent.com/yabwon/SAS_PACKAGES/main/SPF/SPFinit.sas";
%include SPFinit;

filename packages "<directory/containing/packages/>"; 
%installPackage(SPFinit) /* install the framework */
```

Versions are managed in YYYYMMDD format.

## Usage Details
When calling framework features, ensure SPFinit.sas is loaded.
When using package features, ensure filename `packages` is specified.

For descriptions of framework features for using packages as a user and creating packages as a developer, refer to the following:  
- **Guide For Package Users**: forUser.md 
- **Guide For Package Developers**: forDeveloper.md
- **Packages for pharmaceutical industry**: PharmaForest.md
- **Framework Macro Documentation**: reference/SPF.md

Corresponding SPF version: `20260216`