# PharmaForest

PharmaForest is a collaborative repository of SAS packages for the pharmaceutical industry, powered by PHUSE Japan OST (Open Source Technology) WG members.  
https://github.com/PharmaForest  
It primarily covers pharmaceutical industry workflows, but also includes utility packages and more.

## Using Packages

The hosted packages are mainly categorized into the following four types:
output/visualization: OncoPlotter, vis_review_kit …  
data utility: sashash, sas_dataset_json …  
checker: sas_compare, saslogchecker …  
other: SASPACer, misc …  

### Navigators
If you want to find a package that suits your needs or learn how to use a package, asking a navigator is a quick and effective approach.
The navigators are built on OpenAI's GPTs. You need to sign up to ChatGPT (at least a free user account) to talk to them.

- [Dr.Forest](https://chatgpt.com/g/g-699610471260819196be8f76e324dafa-dr-forest)  
  Master navigator; provides detailed guidance for #1-#15 packages. 
- [Dr. Apple](https://chatgpt.com/g/g-6996134ec0848191958c0dd931333f8c-dr-apple)  
  Support navigator; provides detailed guidance for #16 - #30 packages. 
- [Rio](https://chatgpt.com/g/g-69961574d1d48191bf402340734acf25-rio)  
  Support navigator; provides detailed guidance for package #31 and newer.
- [SAS Package Lady (Oba-chan)](https://chatgpt.com/g/g-6996168447ec8191a238046fbba86451-sas-package-lady-oba-chan)  
  Dedicated assistant for creating and managing SAS packages.

## Contribute to PharmaForest
There are many ways to contribute to PharmaForest.

- As a User  
  Simply engage with the packages as an active user, provide feedback, and stay up to date.
  When submitting feedback via GitHub, please direct it to the main repository.  
  (Some packages may have a separate developer repository.)
  Use the repository listed in the README if available; otherwise, use the PharmaForest repository.
  If you are unfamiliar with GitHub features, you may also reach out via the contact information listed below.

- As a PharmaForest Manager
  PharmaForest looking for people who can take on leadership roles beyond programming, such as promotion, outreach, and project management.

### Develop a Package
- Contribute Macros and Tools to the "Devil Package"  
  "Devil" is a package for casually sharing playful or simple programs.
- Co-develop Original Packages
  Collaborate on building new tools such as "OncoPlotter," "misc", and more.
- Mirror or Host Your Own Packages

Those considering development can also receive support from the maintainers as needed.
 
## Package Requirements
Packages hosted on PharmaForest must meet the following criteria:  

- [ ] The package name does not conflict with any already published package
- [ ] A license suitable for business use is declared  
  (MIT or Apache 2.0 is recommended)
- [ ] Not only the package file but also the source code is publicly available (the source code or a link to its location must be included in the repository)
- [ ] Help information necessary for understanding the package is provided
- [ ] Code is written with readability in mind and follows consistent coding practices
- [ ] Behavior can be easily verified through test code or notebook execution examples
- [ ] The package zip file must be less than 10MB, and files not directly required for operation (such as documentation) should not be included in the package
- [ ] All distributed content (code, data, images, etc.) must be original or properly licensed. 
- [ ] Content that undermines the safety and integrity of the community (e.g., unrelated advertisements/marketing, malicious code) is not included
- [ ] Proper version control is maintained, and previously published package versions are not modified after release
- [ ] Prioritize backward compatibility when adding or changing specifications; avoid altering existing behavior whenever possible. If changes are necessary, clearly document them in the README or changelog.
- [ ] A logo based on a regular hexagon has been created
- [ ] The README includes the required text

Please include the following in your README:
```
## What is SAS Packages?

The package is built on top of **SAS Packages Framework(SPF)** developed by Bartosz Jablonski.

For more information about the framework, see [SAS Packages Framework](https://github.com/yabwon/SAS_PACKAGES).

You can also find more SAS Packages (SASPacs) in the [SAS Packages Archive(SASPAC)](https://github.com/SASPAC).

## How to use SAS Packages? (quick start)

### 1. Set-up SAS Packages Framework

First, create a directory for your packages and assign a `packages` fileref to it.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~sas
filename packages "\path\to\your\packages";
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Secondly, enable the SAS Packages Framework.
(If you don't have SAS Packages Framework installed, follow the instruction in 
[SPF documentation](https://github.com/yabwon/SAS_PACKAGES/tree/main/SPF/Documentation) 
to install SAS Packages Framework.)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~sas
%include packages(SPFinit.sas)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


### 2. Install SAS package

Install SAS package you want to use with the SPF's `%installPackage()` macro.

- For packages located in **SAS Packages Archive(SASPAC)** run:
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~sas
  %installPackage(packageName)
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- For packages located in **PharmaForest** run:
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~sas
  %installPackage(packageName, mirror=PharmaForest)
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- For packages located at some network location run:
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~sas
  %installPackage(packageName, sourcePath=https://some/internet/location/for/packages)
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  (e.g. `%installPackage(ABC, sourcePath=https://github.com/SomeRepo/ABC/raw/main/)`)


### 3. Load SAS package

Load SAS package you want to use with the SPF's `%loadPackage()` macro.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~sas
%loadPackage(packageName)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


### Enjoy!
```

## Contact 
Contact us if you're interested!  
- Ryo Nakaya: `@Nakaya-Ryo`, [linkedin](https://www.linkedin.com/in/ryo-nakaya-a93292348/)   
- Yutaka Morioka: `@Morioka-Yutaka`, [linkedin](https://www.linkedin.com/in/morioka%E3%80%80%E6%A3%AE%E5%B2%A1-yutaka-%E8%A3%95-5ab910185/)  
- Hiroki Yamanobe: `@stainlessfish`, [linkedin](https://www.linkedin.com/in/hiroki-yamanobe-85a49a361/)
