# Guide for Package Users

## Installing Packages

Packages are primarily distributed through repositories in the following organizations:
- [SAS Packages Archive](https://github.com/SASPAC)
- [PharmaForest](https://github.com/PharmaForest)

For framework features, use `%installPackage`.
PharmaForest primarily distributes packages related to clinical development.
When installing packages from PharmaForest, specify `mirror=3` or `mirror=PharmaForest` as a parameter.

## Usage Examples

```sas
%installPackage(SQLinDS);  /* install the package from the Internet */
%helpPackage(SQLinDS);     /* get help about the package            */
%loadPackage(SQLinDS);     /* load the package content into the SAS session.  */
%unloadPackage(SQLinDS);   /* unload the package content from the SAS session */

%loadPackageS(GSM, baseplus(1.17), macroarray(1.0), dfa(0.5)) /* load the packages */
```

## Package Management
To check installed packages, use `%listPackages`.

If web access from SAS is restricted, installation is possible by saving zip files downloaded from a browser or files saved on another PC to the package folder.
The following files that can be obtained with installPackage are located at the root of each package's repository:
- `packagename.zip`
- `packagename.md` (optional)

There is no update feature for packages, so reinstall if needed.
There is also no delete feature for packages, so directly delete the zip file when removing.

If you want to switch package sets like virtual environments due to dependencies, you can prepare separate package folders.

To reproduce the environment on another PC, there are three methods:
- Use SPF's `%bundlePackages` and `%unbundlePackages`
- Simply copy zip files
- Output to a dataset with `%listPackages`, compile information from records with tags PACKAGE and VERSION, and obtain with `%installPackage`
