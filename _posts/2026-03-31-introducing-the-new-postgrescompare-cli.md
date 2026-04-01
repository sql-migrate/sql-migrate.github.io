---
layout: post
title: "Introducing the New PostgresCompare CLI: Power and Automation for Your Database Pipelines"
author: "The PostgresCompare Team"
date: 2026-03-31
excerpt: "We’ve completely rewritten the PostgresCompare CLI. With support for over 40 object types, Git-integrated comparisons, and CI/CD-ready output formats, it's the ultimate tool for automating your PostgreSQL schema workflows."
---

Today, we are thrilled to announce the release of **PostgresCompare CLI v1.2.1**. 

While the PostgresCompare desktop application has long been the gold standard for visual schema comparison and deployment, we know that modern development doesn't just happen in a GUI. It happens in the terminal, in shell scripts, and in CI/CD pipelines.

The new CLI, `pgc`, isn't just a minor update—it’s a complete rewrite from the ground up, designed to bring the full power of our comparison engine to your automated workflows.

## Why a New CLI?

In the age of "Everything as Code," your database schema shouldn't be an outlier. Whether you're validating that a developer’s local changes match the canonical schema in Git, or you're automatically generating a migration script as part of a deployment pipeline, you need a tool that is fast, reliable, and easy to automate.

The new `pgc` CLI addresses these needs with three core principles: **Parity**, **Integration**, and **Safety**.

### 1. Full Parity with the Desktop App
The CLI now supports comparison and scripting for over **40 PostgreSQL object types**. From tables and views to more complex objects like Foreign Data Wrappers, Publications, Subscriptions, and advanced Text Search configurations—if you can compare it in the desktop app, you can automate it with the CLI.

### 2. Built for CI/CD Integration
Automation is only useful if the tool "plays nice" with your existing stack. The new CLI includes:
*   **Structured Output Formats:** Beyond human-readable text, you can output results as **JSON** for custom scripting, **JUnit XML** for test reporters, or **GitHub Actions annotations** to see schema drift directly on your Pull Requests.
*   **Predictable Exit Codes:** Seamlessly fail your build pipeline if drift is detected (Exit Code 1) or if an error occurs (Exit Code 2).
*   **Git Integration:** Compare your live database directly against a schema file at any Git reference (e.g., `pgc diff @prod git:main:schema.sql`). No need to check out the code first.

### 3. Safety as a First-Class Citizen
Applying database changes via the command line can be nerve-wracking. We’ve added several features to make this process safer:
*   **`pgc apply` with Guardrails:** The new `apply` command requires an explicit `--confirm` flag and allows you to set limits like `--max-drops` or `--max-statements` to prevent accidental data loss.
*   **Read-Only Environments:** You can mark production environments as `readonly` in your configuration, and the CLI will refuse to run any destructive commands against them.
*   **Dry-Run Validation:** Validate your migration scripts before they ever touch a live database.

## A Better Developer Experience

We didn’t just build the CLI for robots; we built it for developers, too.

*   **Interactive Mode:** Sometimes you want the speed of the terminal but the context of a GUI. Run `pgc interactive` to explore differences in a rich terminal UI (TUI) with keyboard shortcuts.
*   **Watch Mode:** Developing a feature that involves schema changes? Use `pgc watch` to monitor your local database for drift against your baseline and trigger a command (like running your tests) every time a change is detected.
*   **Project-Level Config (`pgc.yaml`):** Stop typing long connection strings. Define your environments and default ignore options in a `pgc.yaml` file at the root of your project.

## How to Get Started

The new CLI is available now for Windows, macOS (including Apple Silicon), and Linux.

1.  **Download:** Head over to our [Downloads page](/downloads) to get the binary for your platform.
2.  **Install:** It's a single, self-contained executable. Just drop it in your `PATH`.
3.  **Initialize:** Run `pgc config init` to create a starter configuration file.
4.  **Compare:** Try your first comparison: `pgc diff @dev @staging`.

We believe the new PostgresCompare CLI will fundamentally change how you manage your PostgreSQL schemas. By bringing industrial-grade comparison and deployment tools to the command line, we're helping you move faster and with more confidence.

Check out the [full CLI documentation](/docs/cli/installation/) to see everything `pgc` can do. Happy diffing!

---

**Ready to automate your schema workflows?** [Sign up for a free 30-day trial](/downloads) and get the latest CLI today.
