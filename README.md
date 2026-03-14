# SAS Packages Framework Skills

Agent skills for the SAS Packages Framework (SPF).

[Agent Skills](https://agentskills.io/home) is a simple open format for giving AI agents new capabilities and expertise.  
It is supported by many coding tools such as Claude Code, VS Code extensions, and other AI development environments.

With this skill, you can:
- Get accurate answers to questions about the framework
- Generate code for typical operations such as obtaining and loading packages
- Get help with tasks such as placing files in the correct folders or formatting comments when creating packages

Even if you don't use agents, the content is written in Markdown, so you can read it directly and use it as a cookbook.

## How to Use

The location where the files should be placed depends on the tool you are using.  
First, check the Agent Skills section in your tool's documentation.

1. Place the files  
Download the repository as a zip file, extract it, and place the `skills/sas-package-framework` folder in the appropriate location for your tool.

If you can use [Vercel Labs Skills](https://github.com/vercel-labs/skills), you can add it as follows:

```bash
npx skills add PharmaForest/spf-skills/skills/sas-package-framework
```

2. Add instructions
If you clearly state that your instructions or questions are related to SPF, the information included in this skill will be referenced.

If your entire project is related to SPF, you can add this information to AGENTS.md or your custom instructions so you don’t need to mention it in every message.