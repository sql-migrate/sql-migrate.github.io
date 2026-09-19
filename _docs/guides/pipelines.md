---
title: Pipelines
description: Compare several environments in one run and deploy the differences between them
category: guides
order: 7
permalink: /docs/guides/pipelines/
---

A pipeline compares several environments in one run. Instead of opening a project
for dev against test, another for test against staging, and a third for staging
against production, you define the environments once and tell PostgresCompare
which pairs to compare.

<div class="note">
<strong>Pro feature.</strong> Pipelines require a Pro subscription and are included in your 14-day trial. On the free tier you can still open a pipeline and read its past runs, but not create or run one. <a href="/purchase">See pricing</a>.
</div>

## Stages and Comparisons

A pipeline is built from two things:

- **Stages** — the environments. Each stage has a label, a connection, a
  database, and optionally a single schema. Leave the schema as **All schemas**
  to compare everything the connection can see.
- **Comparisons** — which stages get compared against which. Each comparison has
  a source and a target, and produces one result, exactly like a project's
  comparison does.

A pipeline needs at least two stages and at least one comparison.

Comparisons are not limited to a chain. Any stage can be compared against any
other, and a stage can take part in as many comparisons as you like — so
"every environment against production" is as valid as "each environment against
the next".

## Creating a Pipeline

1. Click **Pipelines** in the sidebar
2. Click **New pipeline**
3. Give it a **Pipeline name**, and a **Description** if you want one
4. Under **Stages**, add one stage per environment: a **Stage label** such as
   `dev`, `test` or `staging`, then its **Connection**, **Database** and
   **Schema**. Use **Add stage** for each one
5. Under **Comparisons**, choose which stages to compare — see below
6. Click **Save**, or **Save & Run** to start comparing straight away

If something is missing, the dialog tells you which: a pipeline needs a name, at
least two stages, a connection on every stage, and at least one comparison.

### Choosing which stages to compare

Under **Quick presets** there are three starting points. Each has an **Apply**
button, and you can adjust the result afterwards:

| Preset | What it creates |
|--------|-----------------|
| **Linear** | Compares each stage with the next one — dev → test → staging → production |
| **Hub** | Compares every stage against one reference stage, which you pick |
| **Matrix** | Compares every possible pair of stages |

To adjust the comparisons by hand, switch the editor from **List** to **Graph**.
There you can drag a stage to move it, drag from a stage's blue handle to another
stage to compare them, and click an arrow to remove that comparison.

Hub is the usual choice for drift detection — every environment against
production tells you how far each one has drifted. Matrix grows quickly: five
stages produce ten comparisons.

## Comparison Options

The **Comparison options** section holds the same ignore rules and object type
selection as a project, including which differences to ignore and which object
types to compare.

These options belong to the pipeline as a whole. Every comparison in the pipeline
runs with the same settings — you cannot ignore column order on one comparison
but not another. If you need different rules for different pairs, use separate
pipelines.

## Running a Pipeline

Click **Run pipeline** on the pipeline, or **Run** from the list. **Run all** on
the list page runs every pipeline you have.

Every comparison starts at once rather than one after another, so results arrive
as each finishes rather than in stage order. A toast appears per step as it
completes, and a summary toast when the whole run finishes.

Because each comparison reads both of its sides independently, a stage that
appears in several comparisons is read once per comparison. A Matrix pipeline
across five large databases will therefore do considerably more work than a
Linear one across the same five.

### Run status

| Status | Meaning |
|--------|---------|
| **Identical** | Every comparison finished and found no differences |
| **Different** | Every comparison finished and at least one found differences |
| **Partial** | Some comparisons have not been run yet |
| **Error** | At least one comparison failed |

## Reading the Results

The pipeline view has a **Diagram** and a **List** view. The diagram opens by
default when the comparisons are not a simple chain, or when you have arranged
the stages yourself.

On the diagram, each arrow is a comparison and carries its own status and a
summary of what it found. Click an arrow to select that comparison and see its
differences below, then use **Previous** and **Next** to step through the rest.
Dragging a stage on the pipeline view only rearranges the diagram — it does not
change which stages are compared.

### Re-running a single comparison

Hover an arrow and a small run button appears, labelled **Run this step** if it
has never run, or **Re-run this step** if it has.

This replaces that comparison's result inside the most recent run rather than
starting a new one. The other comparisons keep the results they already had, so
a run where you have only re-run one step reports **Partial** until the rest are
run too.

## Deploying a Change

Deployment happens per comparison, not for the pipeline as a whole. Select the
comparison you want to deploy, open the **Export** menu, and choose **Script...**
to generate a deployment script from it. The button is unavailable while that
comparison has **No changes to deploy**.

From there it is the ordinary deployment flow — review the script, choose which
statements to include, then **Run script...** and confirm the target. See
[Deployment Scripts](/docs/guides/deployment-scripts/) for the detail, and
[Safe Deployments](/docs/guides/safe-deployments/) before running anything
against production.

## Run History

Past runs are listed under **Run history** on the pipeline. **Export report**
saves a run in CSV, HTML, Markdown, JSON or PDF, and an individual comparison can
be exported the same way.

**Clear run history** removes the stored runs for that pipeline. The pipeline
itself, its stages and its comparisons are untouched.

## Editing and Deleting

Editing a pipeline rebuilds its stages and comparisons, so reordering stages or
removing one clears the run history for any comparison it affected.

Deleting a stage is refused if it would leave fewer than two, and removes every
comparison that touched it.

Connections and pipelines are linked: **deleting a connection also deletes any
pipeline stage that used it**. PostgresCompare warns you first and lists the
pipelines involved.

If a stage's connection has been removed or has stopped responding, the pipeline
view flags it when the page loads, and running a comparison that uses it fails
with that step marked as an error.

## On the Free Tier

Pipelines are readable but not runnable without a subscription. You can open the
Pipelines list, open a pipeline, look at its stages and comparisons, and read the
results of runs from when you had a licence.

Creating, editing, running, re-running a single comparison, rearranging the
diagram and exporting a report all need an active subscription. The app opens the
licence dialog when you try.

## Next Steps

- [Comparing Databases](/docs/guides/comparing-databases/) — comparison options, filtering and results in detail
- [Deployment Scripts](/docs/guides/deployment-scripts/) — what the generated SQL contains and how to control it
- [Safe Deployments](/docs/guides/safe-deployments/) — checks to run before deploying to production
