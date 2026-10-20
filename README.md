# Zügel

Zügel keeps AI coding agents inside your software architecture. It is an MCP server that parses
your project's source code, builds a resolved model of its dependencies, and checks them against
the architecture rules in your `.arc` files. Before the agent writes a new dependency it can ask
whether that dependency is legal; after every change it sees which violations and dependency
cycles were added or removed. Violations and cycles become ratchets the agent may only drive
down. Zügel supports Java, C# and Python, and works with or without
[Sonargraph](https://www.hello2morrow.com/products/sonargraph).

Zügel is commercial software from [hello2morrow](https://www.hello2morrow.com). You need a
license key; [request a free evaluation license](https://www.hello2morrow.com/products/zugel).

## Requirements

- **Java 21 or newer** on your `PATH`. Zügel must run on at least the Java version your project
  uses: it cannot analyse a Java 25 project on a Java 21 runtime.
- For C# projects, the .NET 10 SDK.
- For Python projects, a Python 3 interpreter.

## Install

```
claude plugin install zugel --marketplace hello2morrow/zugel
```

or, in two steps:

```
claude plugin marketplace add hello2morrow/zugel
claude plugin install zugel@hello2morrow
```

When you enable the plugin, Claude Code asks for your license. Fill in one of the two fields:

- **License file**: the `.license` file you received, for example with an evaluation license.
- **Activation code**: Claude Code keeps it in your system's secure credential store.

Entering either means you accept the [Zügel EULA](https://www.hello2morrow.com/products/zugel/eula).
You can change both later in `/config`.

Zügel binds to the project you start Claude Code in. If the project has no `zugel.json` yet,
Zügel starts in bootstrap mode and the agent offers to generate one. Commit it to your
repository. [First steps with Zügel](https://blog.hello2morrow.com/2026/09/first-steps-with-zugel/)
describes the configuration per language, and the
[manual](https://blog.hello2morrow.com/2026/07/sonargraph-mcp-2/) covers everything else.

The plugin also lets you choose the server version in `/config`: `latest` (the default), a line
such as `26.6.x`, or an exact version such as `26.6.2`. The plugin needs server 26.6.2 or newer,
so do not pin an older one.

## What it runs, downloads and sends

This plugin contains one file that runs: `zugel-launcher.jar`, a small launcher signed by
hello2morrow GmbH. Claude Code starts it as a local MCP server with `java -jar`.

- **Downloads.** Each time it starts, the launcher reads the version index at
  `https://eclipse.hello2morrow.com/jenkins/architectureMcp/` and, when a newer server version
  is available that your license covers, downloads it from there. Server versions are cached in
  Zügel's per-user product directory, beside your license file, and the launcher loads the
  server into its own Java process.
- **License check.** Zügel validates your license with `https://www.hello2morrow.com`,
  or with your organisation's on-premises license server if you configure one. The check sends
  the license key, the product version and build date, the operating system name and version,
  your operating-system user name, and the MAC addresses of your computer.
- **Your code stays on your machine.** Zügel parses and analyses source code locally. It never
  sends source code, architecture models or analysis results anywhere.

How hello2morrow processes the license data and how long it keeps it is described in the
[privacy policy](https://www.hello2morrow.com/privacy).

## Support

[hello2morrow support](https://www.hello2morrow.com/support), or email info@hello2morrow.com.

## License

Proprietary. Use is governed by the [Zügel EULA](https://www.hello2morrow.com/products/zugel/eula);
see [LICENSE](LICENSE).
