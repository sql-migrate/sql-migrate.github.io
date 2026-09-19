---
layout: post
title: "How to Detect PostgreSQL Schema Drift Across Environments"
author: "The PostgresCompare Team"
date: 2026-09-19
excerpt: "Dev matches staging. Staging matches production. Then a deploy fails on a column nobody remembers adding. Here's how to check every environment against production in one run."
---

Every team with more than one database has the same story. A production incident
at 2am needs an index, so someone adds it directly. The fix works, the incident
closes, and the index never makes it back into the migration. Three weeks later a
deployment fails because the migration tries to create an index that already
exists.

This is schema drift: environments that were identical slowly stop being
identical, and you find out at the worst possible moment.

## Why comparing two databases at a time misses it

The usual approach is to compare whatever you're about to deploy. Staging against
production, generate the script, ship it. That catches the differences between
*those two* databases, at *that* moment.

What it doesn't tell you is whether dev has drifted from staging, whether the
index someone added to production exists anywhere else, or whether last month's
rolled-back deploy left a column behind on test. Each of those is invisible until
it becomes the reason a deployment fails.

With four environments there are six possible pairs. Checking them by hand means
six comparisons, six sets of options to keep consistent, and six results to hold
in your head at once. Nobody does that regularly, which is exactly why drift
accumulates.

## Compare every environment against production in one run

A pipeline in PostgresCompare defines your environments once — dev, test,
staging, production — and then which pairs to compare. Run it, and you get every
comparison in a single pass.

The useful shape for drift detection is a hub: every environment compared against
production. One run tells you how far each environment has drifted from the thing
that matters, rather than how far each has drifted from its neighbour.

There's a preset for it. Add your stages, apply **Hub**, choose production as the
reference, and you have four comparisons instead of one. Comparisons aren't
limited to a chain, so you can point any environment at any other — a hub for
drift, a linear chain for a release path, or both in separate pipelines.

The [pipelines guide](/docs/guides/pipelines/) covers setting one up
step by step.

## Reading a drift run

Every comparison starts at once, so results appear as each finishes rather than
in order. When it's done you get a diagram: each environment as a node, each
comparison as an arrow carrying its own status and difference count.

What you're looking for is asymmetry. If dev, test and staging all show the same
three differences against production, that's a pending release — expected, and
everything is where it should be. If *one* environment shows a difference the
others don't, that's drift, and it's worth knowing where it came from before it
gets deployed somewhere else.

The run itself reports one of four states: **Identical** if nothing differs
anywhere, **Different** if at least one comparison found something, **Partial**
if some comparisons haven't been run yet, and **Error** if one failed — usually a
connection that has gone away.

## Acting on what you find

Click the arrow you care about and you get an ordinary comparison result:
the objects that differ, the SQL on both sides, and a deployment script that
brings one in line with the other. Deployment works per comparison, which is the
right granularity — you almost never want to push every environment into
alignment in one move, and you shouldn't.

For the index someone added at 2am, the usual answer isn't to deploy it outward.
It's to get it into a migration, deploy that migration through the normal path,
and let the next drift run come back clean.

## Making it a habit

Drift detection only works if it happens regularly. Two things help:

Run the pipeline before a release, not during one. Finding out that staging has
drifted while you're mid-deploy is how a fifteen-minute release becomes an
evening. Running it the day before turns the same discovery into a task.

For continuous checking, the `pgc` CLI compares schemas from CI and exits
non-zero when it finds differences, so a scheduled job can fail a build when
production stops matching your baseline. The pipeline is for the interactive
question — *where has everything drifted?* — and the CLI is for the automated
one: *has anything changed since we last looked?*

The two work well together. Use the pipeline when you want to see the whole
picture, and the CLI when you want to be told.

---

**Want to see where your environments have drifted?** [Download
PostgresCompare](/downloads) and start your 14-day trial — pipelines and the
`pgc` CLI are part of Pro, and the trial includes both. Comparing, scripting and
deploying stay free after it ends.
