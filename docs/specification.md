---
title: "Specification"
description: "The complete format specification for Agent Skills."
---

## Directory structure

* 's structure

    ```
    skill-name/
    ├── SKILL.md          # Required: metadata + instructions(== how to perform a SPECIFIC task)
    ├── scripts/          # Optional: executable code
    ├── references/       # Optional: documentation
    ├── assets/           # Optional: templates, resources
    └── ..                # Any additional files or directories
    ```

## "SKILL.md" format

* == YAML frontmatter + Markdown content

### Frontmatter

| Field           | Required  | Constraints                                                                                                                                                                                                                          |
|-----------------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`          | Yes       | \<= 64 characters <br/> ONLY ALLOWED: unicode lowercase alphanumeric characters (`a-z`, `0-9`) OR hyphens (`-`) <br/> ❌NOT start OR end with a hyphen (`-`) ❌ <br/> ❌NOT consecutive hyphens (`--`) ❌ <br/> == parent directory name |
| `description`   | Yes       | \<= 1024 characters <br/> NON-empty <br/> == WHAT the skill does + WHEN to use the skill <br/> recommendations: contain specific keywords / agents can identify relevant tasks                                                       |
| `license`       | No        | == license name OR reference -- to -- a bundled license file                                                                                                                                                                                 |
| `compatibility` | No        | <= 500 characters <br/> == environment requirements (intended product, system packages, network access, etc.).                                                                                                                    |
| `metadata`      | No        | == key(string)/value(string)pairs <br/> uses: ADDITIONAL properties / NOT defined -- by -- Agent Skills spec                                                                                                                                      |
| `allowed-tools` | No        | == pre-approved tools / <br/> skill may use <br/> Space-separated string                                                                                                                                                       |

#### `name`

* _Examples valid:_

  ```yaml
  name: pdf-processing
  ```

  ```yaml
  name: data-analysis
  ```

  ```yaml
  name: code-review
  ```

* _Examples NOT valid:_

  ```yaml
  name: PDF-Processing  # uppercase not allowed
  ```

  ```yaml
  name: -pdf  # cannot start with hyphen
  ```

  ```yaml
  name: pdf--processing  # consecutive hyphens not allowed
  ```

#### `description`

* _Examples valid:_
  ```yaml
  description: Extracts text and tables from PDF files, fills PDF forms, and merges multiple PDFs. Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction.
  ```

* _Examples poor:_
  ```yaml
  description: Helps with PDFs.
  ```

#### `license`

* _Examples valid:_

  ```yaml
  license: Proprietary. LICENSE.txt has complete terms
  ```

#### `compatibility`

* _Examples valid:_

  ```yaml
  compatibility: Designed for Claude Code (or similar products)
  ```
  
  ```yaml
  compatibility: Requires git, docker, jq, and access to the internet
  ```

  ```yaml
  compatibility: Requires Python 3.14+ and uv
  ```

#### `metadata`

* _Examples valid:_

  ```yaml
  metadata:
    author: example-org
    version: "1.0"
  ```

#### `allowed-tools` field

* _Examples valid:_

  ```yaml
  allowed-tools: Bash(git:*) Bash(jq:*) Read
  ```

### Body content

The Markdown body after the frontmatter contains the skill instructions. There are no format restrictions. Write whatever helps agents perform the task effectively.

Recommended sections:
- Step-by-step instructions
- Examples of inputs and outputs
- Common edge cases

Note that the agent will load this entire file once it's decided to activate a skill. Consider splitting longer `SKILL.md` content into referenced files.

## Optional directories

A skill directory may contain any files and directories beyond the required `SKILL.md`. The conventions below are recommendations for organizing common types of content.

### `scripts/`

Contains executable code that agents can run. Scripts should:
- Be self-contained or clearly document dependencies
- Include helpful error messages
- Handle edge cases gracefully

Supported languages depend on the agent implementation. Common options include Python, Bash, and JavaScript.

### `references/`

Contains additional documentation that agents can read when needed:
- `REFERENCE.md` - Detailed technical reference
- `FORMS.md` - Form templates or structured data formats
- Domain-specific files (`finance.md`, `legal.md`, etc.)

Keep individual [reference files](#file-references) focused. Agents load these on demand, so smaller files mean less use of context.

### `assets/`

Contains static resources:
- Templates (document templates, configuration templates)
- Images (diagrams, examples)
- Data files (lookup tables, schemas)

## Progressive disclosure

Agents load skills *progressively*, pulling in more detail only as a task calls for it. Skills should be structured to take advantage of this:

1. **Metadata** (~100 tokens): The `name` and `description` fields are loaded at startup for all skills
2. **Instructions** (< 5000 tokens recommended): The full `SKILL.md` body is loaded when the skill is activated
3. **Resources** (as needed): Files (e.g. those in `scripts/`, `references/`, or `assets/`) are loaded only when required

Keep your main `SKILL.md` under 500 lines. Move detailed reference material to separate files.

## File references

When referencing other files in your skill, use relative paths from the skill root:

```markdown SKILL.md
See [the reference guide](references/REFERENCE.md) for details.

Run the extraction script:
scripts/extract.py
```

Keep file references one level deep from `SKILL.md`. Avoid deeply nested reference chains.

## Validation

Use the [skills-ref](https://github.com/agentskills/agentskills/tree/main/skills-ref) reference library to validate your skills:

```bash
skills-ref validate ./my-skill
```

This checks that your `SKILL.md` frontmatter is valid and follows all naming conventions.
