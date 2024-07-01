# Naming conventions

- [Branches](#branches)
- [Release tags](#release-tags)

This document describes the approach to name branches and releases on repos
maintained by the Xen-Troops team.

---
## Branches
We have two types of repos:
- our repos, i.e. created and developed by Xen-Troops team
- forks from the open-source

### Our repos
For our repos we maintain the `main`/`master` branch as the working,
compilable, and more or less tested. This branch is used for releases.
Development branches should be created on the developer's fork of the main
repo and should be merged into the main branch after the review.
In some cases, we may create a permanent branch but this has to have good
reasoning.
For example, repo upgrades to a new version of the yocto, but we still have
to maintain the previous version due to the needs of other repos.
In such case, we may create a side branch to keep some fixes for the old
yocto, but the main branch upgrades and development of the repo is going
on the main branch.
E.g.: `meta-xt-common` is going to switch from the `dunfell` to `kirkstone`.
So we create the branch `dunfell` and upgrade the master to the `kirkstone`.
And for some time we maintain the `dunfell` branch as well as the main one.

### Forks from the open source
We keep the `main`/`master` branch as the mirror of the original repo only.
In most cases, it is outdated and updated occasionally.
All development is going on the our branches for specific needs. The branch
name should contain:
- platform-specific prefix, if needed
- clearly specified base release of the upstream
- `-xt` suffix, so we can distinguish our branches
Something like `<platform>-<upstream release>-xt`. Dash `-` or slash `/`
should be used as the separator.

Examples of acceptable names for branches:
- xen-troops/uboot: rpi5-2024.04-xt
- xen-troops/zephyr: zephyr/v3.6.0/xt
- xen-troops/linux: v5.10.41/rcar-5.1.7.rc11.2-xt
- xen-troops/xen: xen-4.19-xt0.1

We have the same requirement for those branches as for the main branch on
our repos - they are working, compilable, and more or less tested.
Suitable for the release.

---
## Release tags
Main rule - release tags can't be moved after creation. If fixes/updates
are made to the release, then new release tag should be created according
to release rules and SemVer requirements.

We have slightly different approaches for our repos and our forks
of the open-source repos.

### Our repos
We use SymVer with a `v` prefix and three numbers separated by the dot `.`.
So, proper release tags for our repos: `v1.0.0`, `v0.1.0`, `v2.0.3`.
That simple.

In some rare cases, we may need to create platform-specific branches on our
repo. In this case we should follow bellow rule "Forks from the open source"
and use branch-specific prefix.
But in general, we should avoid these cases, and reconsider approach that
leads to the creation of release tags on the non-main branch on our repo.

### Forks from the open source
This one is more complex, taking into account that we may have branches for
different platforms, different upstream releases on the same platform, and
our releases above upstream releases. So we should take the name of our
branch and append `-vX.Y.Z`.
Examples of release tags:
- xen-troops/uboot: rpi5-2024.04-xt-v1.0.0
- xen-troops/zephyr: zephyr/v3.6.0/xt-v1.0.0

It's quite easy to understand which branch was used for the release and
to find all releases for your branch.
