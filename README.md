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
Download the repository as a zip file, extract it, and place the `skills/sas-packages-framework` folder in the appropriate location for your tool.

If you can use Github CLI (v2.90.0 or later), you can add it as follows:
```bash
gh skill install sas-packages-framework
```

If you can use [Vercel Labs Skills](https://github.com/vercel-labs/skills), you can add it as follows:

```bash
npx skills add PharmaForest/spf-skills/skills/sas-packages-framework
```

2. Add instructions
If you clearly state that your instructions or questions are related to SPF, the information included in this skill will be referenced.

If your entire project is related to SPF, you can add this information to AGENTS.md or your custom instructions so you don’t need to mention it in every message.

## Navigator as an Agent (Experimental)
In addition to using the skills directly, you can configure Navigators of the PharmaForest provided as a GPTs as a custom agent in supported coding-agent environments.  
This feature is currently experimental.

Agent definitions are available in the following directories:  
- `agents/`: Markdown definitions for Claude Code, GitHub Copilot, OpenCode, Antigravity, and other compatible coding-agent environments
- `agents_toml/`: TOML definitions for Codex

Place the required files in the custom-agent directory used by your tool:

- **Codex:** `agents_toml/` → `.codex/agents/`
- **Claude Code:** `agents/` → `.claude/agents/`
- **GitHub Copilot:** `agents/` → `.github/agents/`
- **Other tools:** `agents/` → the tool's supported agent directory, such as `.agents/agents/`

After configuration, select or invoke the appropriate Navigator agent using the method supported by your coding tool.  
Depending on the tool, this may involve an agent selector, an `@` mention, a command-line option, or an explicit request to the parent agent, such as: 
```text
Use the Navigator agent to find a suitable PharmaForest package for this task.
```
Compatible tools may also delegate relevant questions to a Navigator agent automatically based on its description.

The agents use `sas-packages-framework` as their operational guidance and return information to the parent coding agent.  
As this integration is experimental, supported features and behavior may vary between coding-agent environments.