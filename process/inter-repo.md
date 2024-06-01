# How to handle inter-repo dependencies

Taking into account that our repos have complex and sometimes nested
compile-time dependencies between repositories, we have to describe rules and
recommendations to reduce unexpected issues.

## Rule 1. Fix everything
As much as possible avoid referring to other repos using the branch name.
Use known-to-be-good commit hash or release tag.
When you use the branch name of the actively developing repo, you may meet
unexpected issues on any day. Your product may become not compilable, you may
get a few additional hours of build time, or the product may become broken at
the least expected moment.
Fix all external dependencies to avoid unexpected issues.

## Rule 2. Update dependencies reasonably
If you know that the external repo is actively developing, then update it
periodically.
This allows you to be aware of the possible issue early, and avoid complex
upgrades and rework.
This rule is especially important if you work in a team. It's always better
one team member will spend time to resolve upgrade related issues, and
provide known-to-be-good upgrade commits, than all team will stuck due to
unintended changes in the external repo.
