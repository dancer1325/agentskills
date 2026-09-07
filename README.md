# Agent Skills

* == standardized way / 
  * are
    * lightweight
    * open format 
    * version-controlled
    * reusable
  * provide -- , based on demand, -- to AI agents 
    * NEW capabilities
    * NEW expertise
  * 's structure

    ```
    my-skill/
    ├── SKILL.md          # Required: metadata + instructions(== how to perform a SPECIFIC task)
    ├── scripts/          # Optional: executable code
    ├── references/       # Optional: documentation
    ├── assets/           # Optional: templates, resources
    └── ..
    *               # Any additional files or directories
    ```

    * "SKILL.md"
      * metadata
        * `name`
        * `description`
  * use cases
    * agents miss a context
      * _Examples:_
        * company context
        * team context
        * user-specific context
  * [supported clients](docs/clients.md)

TODO: 

## Why Agent Skills?

* This gives agents:

- **Repeatable workflows**: Turn multi-step tasks into consistent, auditable procedures.
- **Cross-product reuse**: Build a skill once and use it across any skills-compatible agent.

## How do Agent Skills work?

Agents load skills through **progressive disclosure**, in three stages:

1. **Discovery**: At startup, agents load only the name and description of each available skill, just enough to know when it might be relevant.

2. **Activation**: When a task matches a skill's description, the agent reads the full `SKILL.md` instructions into context.

3. **Execution**: The agent follows the instructions, optionally executing bundled code or loading referenced files as needed.

Full instructions load only when a task calls for them, so agents can keep many skills on hand with only a small context footprint.

## Getting started

* [Documentation](docs)
* [_Examples:_](https://github.com/anthropics/skills)

## history

* ORIGINALLY
  * developed -- by -- [Anthropic](https://www.anthropic.com/)
* AFTERWARDS,
  * released -- as -- an open standard
  * adopted -- by -- agent products
