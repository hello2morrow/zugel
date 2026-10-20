---
name: zugel
description: Use for ANY question about a codebase's dependencies, dependency cycles or architecture rules ("who uses X", "what does X depend on", "what breaks if I change X", "are there cycles", "is this dependency allowed"), and before and after every change to Java, C# or Python source code in a project. Zügel answers these from a resolved model of the code; grep and import parsing give wrong answers here.
---

# Zügel: the architecture of the code you are changing

The `zugel` MCP server parses the project's sources, resolves every dependency, and computes the
architecture violations (against the project's `.arc` rules) and the dependency cycles. Its tools
are named `zugel` / `mcp__zugel__*`. Use this skill only when those tools are available.

## First: bind the project

Zügel answers for ONE repository at a time. Before anything else, call `switch_project` with the
absolute path of the repository you are working in, and again whenever you move to another one.
Every reply carries `projectRoot`: check it before relying on an answer. If the reply says the
project has no `zugel.json` yet, call `generate_config` to create it.

## Three rules

1. **Never grep for a dependency.** "Who uses / calls / constructs / subclasses X" and "what
   breaks if I change X": `query_dependencies` incoming. "What does X depend on": outgoing.
   "Why does A depend on B": `trace_dependency`. "Are there cycles": `list_cycles`. Grep and
   import parsing cannot do reverse or transitive lookups, miss usages, count comments and
   strings, and give no sign that they are wrong.
2. **A defined architecture is binding.** Before you write a new dependency, call
   `check_proposed_dependency`. On a `DENIED_*` verdict, do not write it: find a legal target
   with `describe_artifact` or `list_reachable_components`, or tell the user the design has to
   change. Never edit an `.arc` file to permit a violation.
3. **Violations and cyclicity only go down.** After you add, change, move or delete a source file,
   call `rescan_sources`. The diff must show `addedViolationsByArcFile` empty and no rise in
   `componentCyclicity` or `packageCyclicity`. A passing build and green tests do not establish
   this.

## The license

If a tool answers that Zügel has no valid license, tell the user what it says. When they give you
an activation code or the path of a license file, activate it with `activate_license`.

Each tool's description carries its full contract and reply shape; read it before relying on a
field.
