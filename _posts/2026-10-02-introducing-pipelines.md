---
layout: post
title: "Introducing Pipelines: One Run Across Every Environment"
author: "The PostgresCompare Team"
date: 2026-10-02
excerpt: "A release touches four databases but you compare them two at a time. Pipelines let you define your environments once and compare them all in a single run."
---

Most teams run the same database change through the same sequence: dev, then
test, then staging, then production. PostgresCompare has always handled any one
of those steps well. What it didn't handle was the sequence.

So you made a project for dev against test. Another for test against staging. A
third for staging against production. Three projects to open one at a time, three
sets of comparison options to keep identical by hand, and no single view of where
the change had actually reached.

Pipelines fix that. You define the environments once, say which pairs to compare,
and run the whole thing in one pass.

## Stages and comparisons

A pipeline is two things. **Stages** are your environments — each one a label, a
connection, a database, and optionally a single schema. **Comparisons** say which
stages to compare against which.

That second part matters more than it sounds. Comparisons aren't locked to a
chain, so a stage can be compared against any other, and take part in as many
comparisons as you like. The shape is yours to choose, and there are three
presets to start from:

**Linear** compares each stage with the next: dev → test → staging → production.
This is the release path, and it answers the question you ask during a
deployment — how far has this change actually got?

**Hub** compares every stage against one reference. Point it at production and
each arrow tells you how far that environment has drifted from the thing that
matters. We wrote about [using this to find schema
drift](/2026/09/19/postgresql-schema-drift-across-environments.html) a couple of
weeks ago.

**Matrix** compares every possible pair. Thorough, and the most work: five stages
make ten comparisons, and each one reads both of its sides independently.

If none of those is quite right, switch the editor to its graph view and draw the
comparisons you want — drag from one stage to another to compare them, click an
arrow to remove it.

## Running one

Hit **Run pipeline** and every comparison starts at once rather than queuing
behind each other, so results arrive as they finish rather than in stage order.
Each step raises a toast as it completes, and you get a summary when the run
finishes.

The result is a diagram: your environments as nodes, each comparison as an arrow
carrying its own status and difference count. For a release path, that's the
picture you wanted — the change present in dev and test, absent from staging and
production, and a count on each arrow telling you how much is outstanding.

A run reports **Identical** when nothing differs anywhere, **Different** when
something does, **Partial** when some comparisons haven't run yet, and **Error**
when one failed.

## Acting on a step

Click an arrow and you get an ordinary comparison result: the objects that
differ, the SQL side by side, and a deployment script that brings one side in
line with the other.

Deployment is per comparison, deliberately. There's no "deploy the whole
pipeline" button, because pushing four environments into alignment in one
unreviewed action is not something a schema tool should make easy. You promote
one step at a time, review the script, and run it — the same flow as any other
comparison.

If a single step needs re-running — someone deployed to staging while you were
looking — hover its arrow and run just that one. It replaces that step's result
inside the latest run rather than starting a new one, so the rest of the picture
stays as it was.

## What it keeps

Every run is kept under **Run history**, so you can look back at what the
environments looked like before a release rather than reconstructing it
afterwards. A run can be exported as CSV, HTML, Markdown, JSON or PDF, and so can
any individual comparison within it.

## Two things worth knowing

Comparison options belong to the pipeline, not to each comparison. Every pair
runs with the same ignore rules and the same object types. That's usually what
you want across environments of the same application — and if you genuinely need
different rules for different pairs, use two pipelines.

And pipelines don't run themselves. There's no scheduler: a pipeline runs when
you run it. For continuous checking, the `pgc` CLI belongs in CI, where it exits
non-zero on differences and can fail a build. The pipeline is the interactive
view; the CLI is the automated one.

## Getting started

Pipelines are part of Pro, and your 14-day trial includes them. Click
**Pipelines** in the sidebar, then **New pipeline**, add a stage per environment
and apply a preset. The [pipelines guide](/docs/guides/pipelines/) covers the
whole thing in detail, including what the free tier can still do with a pipeline
once a trial ends — you keep read access to pipelines and their past runs.

---

**Want to see your whole release path in one run?** [Download
PostgresCompare](/downloads) and start your 14-day trial — pipelines are part of
Pro, and the trial includes them. Comparing, scripting and deploying stay free
after it ends.
