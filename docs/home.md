---
title: "Agent Skills Overview"
sidebarTitle: "Overview"
description: "A standardized way to give AI agents new capabilities and expertise."
---

## history

* ORIGINALLY
  * developed -- by -- [Anthropic](https://www.anthropic.com/)
* AFTERWARDS,
  * released -- as -- an open standard
  * adopted -- by -- agent products

## How do Agent Skills work?

* Agents 
  * load -- , through **progressive disclosure**, -- skills
    1. **Discovery**
       * | startup,
         * agents load ONLY EACH AVAILABLE skill's name & description
           * Reason:🧠know when it might be relevant🧠
    2. **Activation**
       * if a task matches a skill's description -> the agent reads the full "SKILL.md" instructions
    3. **Execution**
       * == the agent follows the "SKILL.md"'s instructions / 
         * OPTIONALLY, 
           * execute bundled code
           * load referenced files
         * if a task calls the skill -> load FULL instructions
