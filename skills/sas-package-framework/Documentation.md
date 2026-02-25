# Guide for Advanced Documentation

SPF documentation generation produces a single markdown file, which becomes lengthy and functionally limited for large projects.  
For creating richer package documentation, consider converting to HTML using existing documentation libraries.  
Especially when providing execution examples, using Jupyter Notebook with the [SAS kernel](https://sassoftware.github.io/sas_kernel/) is effective.  
While placing them as reference materials is sufficient, depending on the library used, they can be utilized as sources during HTML conversion.

## Repository Structure and Target Files
For repository structure, documentation files and folders may violate package naming conventions, so it's recommended to separate the package root from the repository root and place it in a source folder.

Files included in the package such as macros and functions basically require help comments. However, for internally used ones, if configuration is difficult, adding the exclusion comment `/*##ExcludeFromDocumentation##*/` at the beginning will exclude them from the target.
However, it's preferable to include them when possible; if excluding, it's recommended to make it clear that they're for internal use, such as starting the filename with `_` or separate folder by adding suffix number.

## Using the Package
Using SASDoGs, a SAS package distributed through SASPAC|PharmaForest, you can create appropriately divided markdown files and configuration files for Jupyter Book (MyST) or Sphinx.
MyST is generally recommended as it's easy to add files and supports .ipynb files.  
Sphinx is better if you have experience or want to use extensions/themes and perform detailed customization.

To add your own markdown or .ipynb files, add the referenced files to myst.yml for MyST or the toc section of index.md for Sphinx.
SASDoGs only generates source text; for detailed settings and usage methods, refer to the official pages of each tool.

Using the exclusion comment `/*##ExcludeFromMarkdownDoc##*/` excludes content from package markdown but allows output in Sphinx or MyST.
Similarly, selective exclusion using `/*##ExcludeFromMySTDoc##*/` and `/*##ExcludeFromSphinxDoc##*/` is also available.

Help comment sections are used as-is, so if you want to use features unavailable in plain Markdown, consider programmatically reading and adjusting the generated markdown files.

## Build and Deploy
MyST requires a server, so use Sphinx if you want to build as HTML locally.  
When publishing packages, using GitHub Pages eliminates the need to prepare separate servers, etc.  
The SASDoGs repository includes GitHub Actions files; using these allows automatic building and deployment without preparing Python environments.

The procedure in that case is as follows.
Modify the configuration file according to branch name and source folder location.

1. Enable GitHub Pages
  - Go to your repository Settings → Pages
  - Under "Build and deployment", set Source to GitHub Actions

2. Push Your Documentation
  - Place documentation source files in the docs folder and push to remote

3. Automatic Deployment
  - The workflow triggers on push to the `main` branch
  - It automatically detects whether you're using Jupyter Book (myst.yml) or Sphinx (conf.py)
  - Builds the HTML documentation
  - Deploys to GitHub Pages

4. Access Your Documentation
  - After deployment completes, your documentation will be available at:
  - `https://<username>.github.io/<repository-name>/`

The `%packagedoc` macro includes functionality to generate this workflow file. Set `ghActions=1` to enable generation.  
To output to an arbitrary location instead of `.github/workflows`, specify `wfLocation=`.
When using SAS OnDemand, the `.github` directory cannot be accessed from the file system, so you need to output to a different folder.