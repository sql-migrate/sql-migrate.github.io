---
title: "PostgresCompare Release Notes — Latest Updates & Features"
description: "See what's new in PostgresCompare. Detailed changelog covering new features, improvements, and bug fixes for every release."
---

<section class="page-hero">
  <h1>Release <span>Notes</span></h1>
  <p>Track the evolution of PostgresCompare with detailed release notes and version history.</p>
</section>

<section class="releases-section" style="padding: 3rem 2rem; max-width: 900px; margin: 0 auto;">

<div class="release latest">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.2.2</h2>
      <p class="release-date">Released April 7th, 2026</p>
    </div>
    <span class="latest-badge">Latest</span>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">🤖</span> MCP Server for AI Agent Integration</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Model Context Protocol Support (`pgc mcp serve`)</strong> — PostgresCompare now integrates directly with AI coding assistants like Claude Desktop and Claude Code. Run <code>pgc mcp serve</code> to start a stdio JSON-RPC server that exposes your PostgreSQL environments to AI agents for schema exploration, comparison, and migration generation.</li>
      <li class="change-item"><strong>10 MCP Tools Available</strong> — AI agents can use <code>list_environments</code>, <code>compare_schemas</code>, <code>generate_migration</code>, <code>get_schema</code>, <code>health_check</code>, <code>validate_sql</code>, <code>apply_migration</code>, <code>create_snapshot</code>, <code>detect_drift</code>, and <code>explain_difference</code>.</li>
      <li class="change-item"><strong>Production Safety Controls</strong> — Use <code>--read-only</code> to prevent any write operations, or <code>--allowed-envs</code> to restrict which environments the AI can access. The <code>apply_migration</code> tool respects environment-level read-only settings and supports <code>--max-drops</code> / <code>--max-statements</code> guardrails.</li>
    </ul>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">📊</span> Data Comparison Enhancements</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Redesigned Data Comparison View</strong> — A new horizontal three-panel layout with a compact table sidebar, row grid with status filter badges, and a slide-up row detail drawer for side-by-side value comparison.</li>
      <li class="change-item"><strong>Overview Dashboard</strong> — A new Overview tab shows summary statistics with an interactive donut chart for row status distribution and a horizontal bar chart highlighting tables with the most differences.</li>
      <li class="change-item"><strong>Data Script Generation</strong> — Generate INSERT, UPDATE, and DELETE scripts directly from data comparison results. Select which row changes to include and export a ready-to-run SQL migration script.</li>
      <li class="change-item"><strong>CSV Per-Table Export</strong> — Export data comparison results to CSV on a per-table basis for further analysis or reporting.</li>
      <li class="change-item"><strong>Star / Favourite Tables</strong> — Mark frequently compared tables with a star for quick filtering.</li>
    </ul>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> CLI Improvements</h3>
    <ul class="change-list">
      <li class="change-item"><strong>License Activation Command</strong> — Use <code>pgc license activate &lt;key&gt;</code> to activate your license directly from the command line without needing the desktop app.</li>
      <li class="change-item"><strong>Configuration File Settings Apply Everywhere</strong> — Comparison settings defined in <code>pgc.yaml</code> now correctly apply to the <code>script</code> and <code>report</code> commands, not just <code>diff</code>.</li>
    </ul>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed partition detection to use <code>relispartition</code> instead of <code>pg_inherits</code>, eliminating false positives on inherited tables that are not partitions</li>
      <li class="change-item">Fixed the Update SQL tab not showing content in certain comparison scenarios</li>
      <li class="change-item">Fixed foreign key constraint scripting when adding new tables via the WebApplication script creator</li>
      <li class="change-item">Fixed CLI license file detection on Windows when the license was stored in a non-default location</li>
      <li class="change-item">Fixed data comparison row panel and drawer rendering issues</li>
      <li class="change-item">Fixed excessive Sentry error logging in both CLI and desktop builds</li>
    </ul>
  </div>
  <a href="/downloads" class="download-link">
    <svg width="18" height="18" viewBox="0 0 20 20" fill="none">
      <path d="M10 3v10m0 0l4-4m-4 4l-4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
      <path d="M3 13v3a2 2 0 002 2h10a2 2 0 002-2v-3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
    </svg>
    Download Version 1.2.2
  </a>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.2.1</h2>
      <p class="release-date">Released March 31st, 2026</p>
    </div>
    
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">🚀</span> New Command-Line Interface (CLI)</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Full-Featured CLI (`pgc`)</strong> — A powerful new CLI for schema comparison, script generation, and automation. All major features of the desktop app are now available in the terminal, designed for both interactive use and CI/CD integration.</li>
      <li class="change-item"><strong>Advanced Script Generation (`pgc script`)</strong> — Generate dependency-ordered, transactional deployment scripts directly from the command line. Supports pre/post deployment scripts, migration direction control, and dry-run checks.</li>
      <li class="change-item"><strong>New Data Sources (Git Refs & Folders)</strong> — Compare against schema files directly from any Git branch, tag, or commit (e.g., `git:main:schema.sql`). You can also now use a folder of `.sql` files as a single data source.</li>
      <li class="change-item"><strong>Interactive Mode (`pgc interactive`)</strong> — A new terminal UI for interactively exploring schema differences in a tree view, with keyboard shortcuts for easy navigation.</li>
      <li class="change-item"><strong>Schema Documentation (`pgc docs`)</strong> — Generate schema documentation in Markdown or HTML format from any data source.</li>
      <li class="change-item"><strong>Watch Mode (`pgc watch`)</strong> — Monitor a database for schema drift against a baseline, with the ability to trigger a command on change.</li>
      <li class="change-item"><strong>Configuration Files (`pgc.yaml`)</strong> — Manage project-level settings for environments, default comparison options, and script generation. Use `pgc config init` to get started.</li>
      <li class="change-item"><strong>Apply Scripts Safely (`pgc apply`)</strong> — Apply migration scripts to a database with built-in safety features: explicit <code>--confirm</code> flag required, <code>--dry-run</code> validation, <code>--max-drops</code> and <code>--max-statements</code> guardrails, and read-only environment protection.</li>
      <li class="change-item"><strong>Rich Comparison Reports (`pgc report`)</strong> — Generate standalone HTML reports (with dark mode support), Markdown summaries, or JSON reports. Optionally include the full migration script with <code>--include-script</code>.</li>
      <li class="change-item"><strong>Multi-Target Validation (`pgc validate`)</strong> — Validate SQL scripts, configuration files, database connections, or snapshot files with a single command. Checks for dangerous statements, unbalanced transactions, and connectivity issues.</li>
      <li class="change-item"><strong>Health Checks (`pgc health`)</strong> — Check database connectivity and CLI health. Use <code>--all-envs</code> to verify all configured environments at once — ideal for CI/CD pipeline pre-flight checks.</li>
    </ul>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> CLI Improvements</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Comprehensive Object Support</strong> — The CLI now supports comparison and scripting for over 40 PostgreSQL object types, including FDWs, Publications, Subscriptions, and more, achieving parity with the desktop app.</li>
      <li class="change-item"><strong>CI/CD Friendly Output Formats</strong> — New output formats like JUnit XML (`--format junit`) and GitHub Actions annotations (`--format github`) are available for seamless pipeline integration.</li>
      <li class="change-item"><strong>Advanced Filtering</strong> — Use wildcard patterns to include or exclude specific objects from a comparison (e.g., `--include 'users_*,orders_*'`).</li>
      <li class="change-item"><strong>Environment Management</strong> — `pgc profile` has been renamed to `pgc env` for consistency. The `@` prefix is now optional for better PowerShell compatibility.</li>
      <li class="change-item"><strong>Clean SQL Output</strong> — Using <code>--sql</code> or <code>--format sql</code> suppresses the license banner and progress indicators, so <code>pgc diff ... --sql > migration.sql</code> produces clean, pipeable output.</li>
    </ul>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed snapshot JSON serialization/deserialization losing constraints due to array handling with <code>PreserveReferencesHandling</code></li>
      <li class="change-item">Fixed <code>--only</code> type normalization (e.g. <code>tables</code> is now correctly normalized to <code>table</code>)</li>
      <li class="change-item">Fixed false positives when comparing databases with partitioned tables — constraints on partition child tables are now excluded from comparisons</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.2.0</h2>
      <p class="release-date">Released March 23rd, 2026</p>
    </div>
    
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item"><strong>pg_dump / SQL file import</strong> — Compare one or both sides of a comparison against a pg_dump file or folder instead of a live database connection. PostgresCompare accepts plain SQL pg_dump output and binary pg_dump format (automatically converted via <code>pg_restore</code> if it is on your PATH). You can also point to a folder containing multiple SQL files. This is useful for auditing schema definitions stored in version control, comparing a snapshot against a live database, or working in environments where a direct connection is not available</li>
      <li class="change-item"><strong>Comparison options saved per result</strong> — The ignore flags, object type filters, name filters, and pre/post deploy script settings that were active when a comparison ran are now stored with the result. Click the <strong>info icon</strong> next to the Re-run comparison button to see exactly which options produced a given result</li>
    </ul>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item"><strong>History viewer timeline</strong> — The schema history view has been redesigned as a vertical timeline with colour-coded indicator dots, relative timestamps (e.g. "2 hours ago"), and automatic selection of the most recent entry. The Monaco diff editor now shows the correct direction (older version on the left, newer on the right) and labels each side with its date. For objects that were created or deleted, a single editor with a contextual banner is shown instead of a blank diff panel</li>
      <li class="change-item"><strong>Changes tab in history viewer</strong> — A Changes tab alongside the diff panel gives a plain-English description of what changed for each object between the two selected history entries</li>
      <li class="change-item"><strong>Column reorder migration</strong> — PostgresCompare now detects when table columns have changed position and generates a safe migration using CREATE TABLE / INSERT INTO / DROP / RENAME with full constraint reconstruction (primary keys, unique constraints, check constraints, and foreign keys)</li>
      <li class="change-item"><strong>Column scale detection</strong> — Scale differences in numeric columns are now detected and generate <code>ALTER COLUMN TYPE</code> statements</li>
      <li class="change-item"><strong>Scrollable deployment progress log</strong> — The deployment progress log in the Run Script modal is now a scrollable, multi-line display that accumulates <code>RAISE NOTICE</code> output as each statement executes. The progress logging preference is saved per project in local storage</li>
    </ul>
  </div>
  <a href="/downloads" class="download-link">
    <svg width="18" height="18" viewBox="0 0 20 20" fill="none">
      <path d="M10 3v10m0 0l4-4m-4 4l-4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
      <path d="M3 13v3a2 2 0 002 2h10a2 2 0 002-2v-3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
    </svg>
    Download Version 1.2.0
  </a>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.107</h2>
      <p class="release-date">Released March 16th, 2026</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Changes tab</strong> — A new Changes tab sits alongside the SQL diff view and shows a human-readable summary of what changed: which columns were added or removed, which constraints were modified, and so on. Switch between the Changes tab and the SQL view depending on whether you want a plain-English summary or the full DDL comparison</li>
      <li class="change-item"><strong>Deployment progress logging</strong> — Generated scripts now include timestamped <code>RAISE NOTICE</code> statements after each DDL change (e.g. <code>[✓] 14:23:05 | public.users | alter table</code>) and a <code>[>>] Migration complete at …</code> notice at the end. Progress output is visible in any client — psql, DataGrip, or similar — and NOTICE output is captured and displayed in the in-app deployment modal. The toggle is on by default and can be disabled from the script toolbar; notices follow statement selection so deselecting a change also removes its notice</li>
      <li class="change-item"><strong>Name filters</strong> — Each object type in the comparison list now has its own name filter. Type to show only objects whose names match within that type — for example, showing only tables containing "order" — without affecting other object types in the list</li>
      <li class="change-item"><strong>Create database</strong> — PostgresCompare can now create a new PostgreSQL database directly from within the app. Open an environment and click <strong>Create database</strong> to create a database on that server without leaving the app</li>
    </ul>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Diff editor: navigator and word wrap</strong> — The SQL diff editor now includes a navigator panel listing each changed section for quick jumping, and a word wrap toggle in the toolbar for long lines such as function bodies</li>
      <li class="change-item"><strong>Cleaner object list</strong> — Value columns have been removed from the comparison list and the schema column is collapsed by default, giving more room for object names and making the list easier to scan</li>
    </ul>
  </div>
  <a href="/downloads" class="download-link">
    <svg width="18" height="18" viewBox="0 0 20 20" fill="none">
      <path d="M10 3v10m0 0l4-4m-4 4l-4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
      <path d="M3 13v3a2 2 0 002 2h10a2 2 0 002-2v-3" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
    </svg>
    Download Version 1.1.107
  </a>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.106</h2>
      <p class="release-date">Released March 8th, 2026</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Star / favourite comparisons</strong> — Mark comparisons as favourites by clicking the star icon on a comparison tile. Starred comparisons show an amber star and can be filtered to the top of the list with a single click on the toolbar toggle</li>
      <li class="change-item"><strong>Deploy confirmation dialog</strong> — Clicking "Run script…" now shows a confirmation modal displaying the target connection and database before executing. Destructive statements are highlighted in red and warnings in amber, and the Run button itself turns red when the script contains destructive changes</li>
      <li class="change-item"><strong>Export reports</strong> — Export comparison results in multiple formats: Excel (with a doughnut-chart summary sheet), HTML (with clickable status filter pills and sortable columns), PDF, JSON, Markdown, and CSV. Filenames default to the database names and date (e.g. <em>devDb_vs_prodDb_2026-03-08.xlsx</em>). A companion <code>.schema.json</code> is written alongside JSON exports for CI/CD validation. The export options modal defaults "Identical" objects to unchecked</li>
    </ul>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Pre/post deploy scripts in statement list</strong> — Pre- and post-deploy scripts now appear as entries in the deployment statement list with include/exclude checkboxes. Section headers and dependency comments are suppressed automatically when their associated change is excluded, and the deployment execution matches exactly what the list shows</li>
      <li class="change-item"><strong>Faster script generation</strong> — The dependency-ordering algorithm has been replaced with Kahn's O(V+E) topological sort, eliminating the previous O(n³) approach. The script page now shows shimmer skeleton loaders on both panels while the draft is being generated</li>
      <li class="change-item"><strong>Cleaner script section headers</strong> — Drop operations now use the same "Type name" heading format as creates. Indexes, triggers, and policies each get their own per-item section header rather than being grouped under a parent table heading</li>
    </ul>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed "Select all" / "Deselect all" not toggling pre/post deploy scripts</li>
      <li class="change-item">Fixed syntax errors when a column, index, or other identifier is named a PostgreSQL reserved keyword (e.g. <code>order</code>)</li>
      <li class="change-item">Fixed aggregate scripting to include the argument type in <code>CREATE AGGREGATE</code> and <code>ALTER AGGREGATE</code> statements</li>
      <li class="change-item">Fixed deployment script ordering for materialized views, corrected index expression parsing for multi-argument expressions such as <code>COALESCE(a, b)</code>, and fixed <code>character[]</code> function arguments being rendered as <code>haracter []</code></li>
      <li class="change-item">Fixed the Monaco diff editor failing to initialise in the comparison history viewer</li>
      <li class="change-item">Fixed a false "stale comparison" warning appearing when it should not</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.105</h2>
      <p class="release-date">Released March 2nd, 2026</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Re-run comparison</strong> — Run a fresh comparison directly from the comparison detail view without navigating away. Results stream in live, and the previous comparison is preserved in the history list</li>
      <li class="change-item"><strong>Comparison history swimlane view</strong> — Track how your schema has changed over time with a new grouped history view. Objects are organised by change category (Fixed, Regressed, New, Removed, Changed, Unchanged) with collapsible sections, count badges, and colour-coded borders</li>
      <li class="change-item"><strong>Related object navigation</strong> — When viewing a difference, clickable chips appear showing dependencies ("Depends on") and reverse references ("Referenced by") for the selected object, colour-coded by their comparison status. Clicking a chip jumps straight to that object in the list</li>
      <li class="change-item"><strong>SQL line highlighting</strong> — Clicking a child row (column, constraint, or property) in the differences list now scrolls the diff editor to and highlights the exact line of SQL for that sub-object</li>
      <li class="change-item"><strong>Auto-fetch databases and schemas</strong> — Databases load automatically when a connection is selected, and schemas load when a database is chosen. The environment dropdowns have been replaced with styled Bootstrap menus matching the rest of the app</li>
      <li class="change-item"><strong>Delete comparison confirmation</strong> — Deleting a comparison now shows a confirmation dialog. The tile fades out on confirm to give clear visual feedback</li>
    </ul>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Redesigned Overview tab</strong> — Replaced the text summary with five clickable stat cards (Total, Identical, Different, New, Dropped). Chart segments are now interactive — clicking navigates to the Objects tab with the relevant filter applied. The tab has been renamed from "Summary" to "Overview"</li>
      <li class="change-item"><strong>Save script respects your selection</strong> — The saved .sql file now matches exactly what would be deployed, correctly applying any statements you have deselected</li>
      <li class="change-item"><strong>Faster startup</strong> — The app loads noticeably faster through parallel API calls on launch and deferred loading of the Monaco editor (~2.5 MB removed from the critical path)</li>
    </ul>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed deployment executing the wrong statements when some script entries were deselected</li>
      <li class="change-item">Fixed save script failing silently on API error — an error dialog is now shown</li>
      <li class="change-item">Fixed the comparison toolbar scrolling out of view</li>
      <li class="change-item">Fixed the licence modal layout where the machine ID was overlapping the activation status indicator</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.104</h2>
      <p class="release-date">Released February 23rd, 2026</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Pre/post deploy scripts</strong> — Add custom SQL that runs before or after the generated deployment script, editable directly in the app using Monaco editor</li>
      <li class="change-item"><strong>Global search</strong> — Search across projects, environments, and comparison objects from anywhere in the app</li>
      <li class="change-item"><strong>Keyboard navigation in the difference list</strong> — Use arrow keys to move through differences without the mouse</li>
      <li class="change-item"><strong>Dependency cascade selection</strong> — Selecting a script statement automatically selects its dependencies</li>
    </ul>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item">Redesigned comparison card with progress bars and a breakdown of difference types</li>
    </ul>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed comparison panel overflowing the bottom of the screen</li>
      <li class="change-item">Fixed arrow key expand/collapse for tree rows</li>
      <li class="change-item">Fixed auto-updater crash on update errors</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.103</h2>
      <p class="release-date">Released February 18th, 2026</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item"><strong>Destructive change warnings</strong> — Deployment scripts now highlight dangerous statements before you run them. Statements are classified as Destructive (e.g. DROP TABLE, DROP COLUMN) or Warning (e.g. DROP FUNCTION, DROP VIEW), with colored row styling, warning icons, summary banners, and glyph margin indicators in the SQL editor</li>
      <li class="change-item"><strong>Hover to preview differences</strong> — Hovering over a row in the script statement list shows a floating diff popover so you can inspect the before/after SQL without leaving the screen</li>
      <li class="change-item"><strong>Deployment script section headers</strong> — Generated scripts now group statements by object type (Tables, Views, Types, Sequences, Column Changes, Constraints, Indexes, Functions, Materialized Views, Triggers, Policies, Privileges, Drop Objects). Section headers are clickable in the statement list, and a script header shows the version, source/target database names, and timestamps. Dependency comments explain ordering for dependency-sorted objects</li>
    </ul>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Select all now only toggles rows visible after filtering, and the header checkbox accurately reflects the current selection state</li>
      <li class="change-item">Filter dropdowns no longer lose their selected value on re-render</li>
      <li class="change-item">Filter counts exclude section header rows</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.102</h2>
      <p class="release-date">Released February 7th, 2026</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Added option to ignore column statistics differences - useful when source and target databases have different ANALYZE settings</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.101</h2>
      <p class="release-date">Released January 18th, 2026</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed PostgreSQL 18 compatibility by removing deprecated attcacheoff column from queries</li>
      <li class="change-item">Fixed compatibility issue with PostgreSQL 17's new MAINTAIN privilege</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.100</h2>
      <p class="release-date">Released July 1st, 2024</p>
    </div>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item">Removed heading from difference viewer for cleaner UI</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.99</h2>
      <p class="release-date">Released June 24th, 2024</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed bug where PostgresCompare was not ignoring tablespace differences in indexes</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.98</h2>
      <p class="release-date">Released May 8th, 2024</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed bug where PostgresCompare was failing to fetch the version number of PostgreSQL databases on Linux or MacOS</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.97</h2>
      <p class="release-date">Released May 6th, 2024</p>
    </div>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item">Updated .NET Core to v8</li>
      <li class="change-item">Refreshed the UI on the schema comparisons list</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.94</h2>
      <p class="release-date">Released April 1st, 2024</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed bug where PostgresCompare would attempt to read a now removed field from pg_database</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.93</h2>
      <p class="release-date">Released March 22nd, 2024</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Support variadic function arguments</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.91</h2>
      <p class="release-date">Released July 30th, 2022</p>
    </div>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item">Quote the object name fields when generating a CSV report</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.90</h2>
      <p class="release-date">Released July 28th, 2022</p>
    </div>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item">Add owner to CREATE sql for Materialized View</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.89</h2>
      <p class="release-date">Released July 7th, 2022</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fix blank window issue on macOS</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.88</h2>
      <p class="release-date">Released June 22nd, 2022</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fix escaping of CSV report</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.87</h2>
      <p class="release-date">Released November 15th, 2021</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed comparison of Casts</li>
      <li class="change-item">Added :: cast notation when changing data type of a column</li>
      <li class="change-item">Fixed filtering of tables to schema for data projects</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.86</h2>
      <p class="release-date">Released October 5th, 2021</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fixed navigation and display of data projects</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.85</h2>
      <p class="release-date">Released September 22nd, 2021</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Added comparison of Configuration Parameters</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.84</h2>
      <p class="release-date">Released September 13th, 2021</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Added history view</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.83</h2>
      <p class="release-date">Released August 5th, 2021</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fix comparison of partitions when comparing single schemas</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.82</h2>
      <p class="release-date">Released July 31st, 2021</p>
    </div>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item">Ignore differing schema names when comparing single schemas</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.80</h2>
      <p class="release-date">Released June 20th, 2021</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Compare and deploy table partitions</li>
    </ul>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fix bug deploying timestamp vs timestamp(0) differences</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.79</h2>
      <p class="release-date">Released June 13th, 2021</p>
    </div>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item">Ensure that adding a column comes before adding a foreign key</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.78</h2>
      <p class="release-date">Released June 4th, 2021</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Resolve issue comparing domains</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.76</h2>
      <p class="release-date">Released June 2nd, 2021</p>
    </div>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Resolve 'keynotfound' issue with data comparison</li>
      <li class="change-item">Resolve 'keynotfound' issue with schema comparison</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.75</h2>
      <p class="release-date">Released May 31st, 2021</p>
    </div>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item">Performance improvements, filtering by schema</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.74</h2>
      <p class="release-date">Released May 2nd, 2021</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Index include columns</li>
      <li class="change-item">Index column operator class names</li>
    </ul>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fix issue where an index implementing a fk constraint was being excluded</li>
      <li class="change-item">Fix ordering of deployment script when DROP/CREATE a view</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.71</h2>
      <p class="release-date">Released April 2nd, 2021</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Compare range types</li>
      <li class="change-item">Store and recover window state between launches</li>
      <li class="change-item">Include extensions when comparing single schemas</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.69</h2>
      <p class="release-date">Released March 22nd, 2021</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Add sample project option</li>
      <li class="change-item">Add ignore default arguments option</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.68</h2>
      <p class="release-date">Released March 15th, 2021</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Grant/Revoke privileges in deploy script</li>
      <li class="change-item">Show script folder progress</li>
      <li class="change-item">Add a description for a project</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.67</h2>
      <p class="release-date">Released March 8th, 2021</p>
    </div>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">⚡</span> Improvements</h3>
    <ul class="change-list">
      <li class="change-item">Sort differences with identical last</li>
      <li class="change-item">Added edit project button</li>
      <li class="change-item">Add view dependency planning</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.66</h2>
      <p class="release-date">Released March 1st, 2021</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Add remaining object types to scripts folder creation</li>
    </ul>
  </div>
  <div class="change-section bug-fixes">
    <h3><span class="section-icon">🔧</span> Bug Fixes</h3>
    <ul class="change-list">
      <li class="change-item">Fix issue comparing check constraint definitions between postgres versions</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Version 1.1.65</h2>
      <p class="release-date">Released February 22nd, 2021</p>
    </div>
  </div>
  <div class="change-section new-features">
    <h3><span class="section-icon">✨</span> New Features</h3>
    <ul class="change-list">
      <li class="change-item">Added first pass at scripts folder creation</li>
    </ul>
  </div>
</div>

<div class="release">
  <div class="release-header">
    <div class="version-info">
      <h2>Older Versions</h2>
      <p class="release-date">Prior releases</p>
    </div>
  </div>
  <div class="change-section improvements">
    <h3><span class="section-icon">📋</span> Historical Releases</h3>
    <ul class="change-list">
      <li class="change-item">v1.1.64 - Fix data compare issue, sort schemas alphabetically</li>
      <li class="change-item">v1.1.63 - Add scripting of owner of sequences, enums, domains and aggregates</li>
      <li class="change-item">v1.1.61 - Add comparison of Aggregates, new dependency calculation</li>
      <li class="change-item">v1.1.60 - Add comparison of privileges for functions, views, sequences, schemas</li>
      <li class="change-item">v1.1.59 - Fixed test connection feedback</li>
      <li class="change-item">v1.1.58 - Improved comparison performance, fixed v12 WHEN clause issue</li>
      <li class="change-item">v1.1.57 - Add comparison of privileges</li>
      <li class="change-item">v1.1.52 - Fix data compare 'Sequence contains no elements'</li>
      <li class="change-item">v1.1.51 - Fix deployment of CREATE POLICY, added altered primary key</li>
      <li class="change-item">v1.1.50 - Fix title bar on MacOS, improve query performance</li>
      <li class="change-item">v1.1.49 - Fix export report/save script, DROP POLICY, CREATE PUBLICATION</li>
      <li class="change-item">v1.1.48 - Fix frozen UI issue, add new onboarding</li>
      <li class="change-item">v1.1.47 - Detect differences in partition by clause, include CLI</li>
      <li class="change-item">v1.1.46 - Fix Citus tables being ignored, UI refactoring</li>
      <li class="change-item">v1.1.42 - Fix data compare issues, persist filter on projects list</li>
      <li class="change-item">v1.1.41 - Add environment variables, comments on Domain, Composite Type, MView</li>
      <li class="change-item">v1.1.40 - Add CSV report, improved keyboard navigation</li>
      <li class="change-item">v1.1.39 - Fix comparing views, procedure signature, NOT NULL scripting</li>
      <li class="change-item">v1.1.38 - Remove Easter Egg, fix OnUpdate, ReturnsSet properties</li>
      <li class="change-item">v1.1.35 - Improve performance, fix argument defaults, UI responsiveness</li>
      <li class="change-item">v1.1.34 - Add data compare beta</li>
      <li class="change-item">v1.1.33 - Fix deploying column comments, refactor options</li>
      <li class="change-item">v1.1.32 - Deploy precision differences</li>
      <li class="change-item">v1.1.31 - UI improvements, fix column name escaping, expression index parsing</li>
      <li class="change-item">v1.1.30 - UI improvements, add 'ignore column order' option</li>
      <li class="change-item">v1.1.28 - Deploy serial datatype changes, support v9.3</li>
      <li class="change-item">v1.1.27 - Deploy identity column differences</li>
      <li class="change-item">v1.1.16 - Add PostgreSQL 12 support</li>
      <li class="change-item">v1.0.45 - Added licensing</li>
      <li class="change-item">v1.0.0-alpha - Initial release (March 14th, 2018)</li>
    </ul>
  </div>
</div>

</section>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {"@type": "ListItem", "position": 1, "name": "Home", "item": "https://www.postgrescompare.com"},
    {"@type": "ListItem", "position": 2, "name": "Release Notes", "item": "https://www.postgrescompare.com/release-notes"}
  ]
}
</script>
