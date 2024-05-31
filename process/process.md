# Preface

This document defines processes regarding making code changes adopted
by Xen-Troops project. It should be followed by all Xen-Troops
members. Parts of the document that describe commits, pull requests
and such should be followed by external contributors as well.

# Definition of Done

Every task is considered completed when resulting changes are fully
integrated into the codebase. This means that you have done your task
only **after** your Pull Request(s) was/were merged. Before that
moment your task is **not** done.

In some cases "Done" means that code is taken into upstream. You need
to try to upstream any changes that are upstreamable. Ideally, we want
to have as little non-upstreamed (and thus non-supported) code as
possible.

Not all tasks end as pull requests. For example, there can be research
tasks where you research some topic or experiment with different
approaches. Those still require some tangible results, like Wiki page
for example.

# Submitting changes

We adhere to processes developed by Linux kernel community. Please
read ["Submitting
Patches"](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#separate-your-changes),
["Patch
preparation"](https://www.kernel.org/doc/html/latest/process/5.Posting.html#patch-preparation)
and ["Patch formatting and
changelogs"](https://www.kernel.org/doc/html/latest/process/5.Posting.html#patch-formatting-and-changelogs)

Also you can check our [contributing
guide](https://xen-troops.github.io/contribution)

Here we will re-iterate most important points and describe how it
applies to GitHub, as we don't use mailing-list based review.

## Coding style

Every project has own coding style so we can't provide here any
specific requirements. But you should always adhere to the coding
style of a project where you making commit. When in doubt - look at
surrounding code and follow its style.

When starting a new project is a good idea to adopt coding style used
in large open source projects line Linux Kernel. This will allow us to
reuse existing tools (like `checkpatch.pl`). Please refrain from using
CamelCase and Wintel Hungarian Notation.

Most modern languages provide own canonical coding style guides:

 * Python - [PEP8](https://peps.python.org/pep-0008/)
 * Golang - [Go Style Guide](https://google.github.io/styleguide/go/guide)
 * Rust - [Rust Style Guide](https://doc.rust-lang.org/nightly/style-guide/)

And so on.

## Make your commits atomic

Each commit should be atomic. This means that each logical change
should be in a separate commit. Commit can be small (like "fix an
error message") or large (like "rework locking model") but it should
be conceptually small: it should do one task and you must be able to
describe this task with one line.

Do not mix different types of changes in one commit. If the same
commit fixes issue, rearranges some fields in a structure and
reformats the code, it probably should be split into three different
commits. Or there should be really good reason why it can't be
done. "It is hard to split" is not a good reason. There are plenty
of tools, ranging from `git add -p` to "Magit" that will enable you to
do this.

When dividing your change into a series of patches, take special care
to ensure that the code builds and runs properly after each patch in
the series. Developers using `git bisect` to track down a problem can
end up splitting your patch series at any point; they will not thank
you if you introduce bugs in the middle.

## Include changes to documentation

This includes both comments in code and separate documentation files.

If project has a separate documentation files - check if your changes
affect documented parts. Alter the documentation so it always
up-to-date with the code. Do this in the same commit that changes the
code. This is crucial.

If you are adding public interfaces - document them in the same way,
as other parts are documented or better. Always make sure that
comment reflect the code.

Doxygen is established standard in many projects. Use it in new
projects. Existing projects may use own formats, like
[kernel-doc](https://www.kernel.org/doc/html/v5.0/doc-guide/kernel-doc.html).

## Write tests

If project includes test framework and code you are changing is
already covered by tests - you need to update tests as well. If you
are adding a new feature - cover it with tests. Tests should be added
by the same commit as a feature.

If you are adding a new module - cover it with tests as well.

## Commit authorship

If you are EPAM employee - you need to use your EPAM email in
commits. This includes commit authorship and `Signed-off-by` tag. It
may seem that this will cause issues when you'll try to mail patches
to upstream mailing lists, but `git send-email` will cover you.

## Describe your changes

You must describe **what** your commit does and **why** it does
this. In most cases every patch solves some problem. Describe this
problem, in non-obvious cases describe why you chose this solution
over another. Description "this patch adds meta-virtualization layer"
is bad, because anyone can see that from diff. On other hand "add
meta-virtualization layer, because it provides recipes for building
Xen" is good, because it clearly depicts what problem you are solving
with this change.

On other hand, you can omit the obvious parts, like "I am fixing this
bug because this is bug and it should be fixed". Better describe how
exactly you are fixing it.

Don't be afraid to add any relevant information, like links to other
commits, logs, crash dumps, diagrams and such.

### Commit message format

Commit message consist of one-line description (aka header), main
description and tag lines. They are divided by empty lines:

```
subsystem: add possibility to grok baz

When working on foo, we noticed that blah-blah-blah.
This lead to issues with blah-blah-blah.

We can fix this by blah-blah-blah and blah-blah-blah.

Signed-off-by: Your Friendly Developer <nobody@example.com>
```

### One-line description AKA header

Header should be short but comprehensive. Its length is limited to 72
characters in most cases (size of one line in github). But this is more
than enough. Do not use overly generic descriptions.
"Add a new feature" is a bad description because it tells almost
nothing about a commit. "libxl: add "grant_usage" parameter for
virtio disk devices" is the great description because it clearly states
what is goes in the patch.

Use imperative mood when describing the change. Like you are
commanding codebase to change. When in doubt, do `git log --oneline`
and see if your commits stand out in the log:

```
1234567890 I just added a new feature, yay!            <=== Agree, something is wrong here.
5e600b4fb9 NUMA: limit first_valid_mfn exposure
137c08aa07 xen/riscv: introduce system.h
c8bb7553f2 x86emul: support AVX-VNNI-INT16
1ec3fe1f66 xen/arm32: head: Improve logging in head.S
...
```

### Main body

After commit description goes the main part, where you state problem
you trying to solve and describe how you are solving it. Add any
relevant information. Refer to previous section "Describe your
changes" for more information.

Line length in main body is limited to 75 characters.

### Tags section

After main body add any relevant tags. You should always include
`Signed-off-by` tag (use `git commit -s'). It basically states that
you are author of this change or have a legal right to submit
it. Refer to [Developer's Certificate of
Origin](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#sign-your-work-the-developer-s-certificate-of-origin)
for more information.

After `Signed-off-by:` tag you can add another tags. Most common are
`Tested-by:`, `Suggested-by:`, `Reported-by:`, `Reviewed-by:` and `Acked-by:`. Here is a short description for each of them:

* `Suggested-by:` - refer to person who suggested this commit.

* `Reported-by:` - refer to person who reported a issue, which you are
  fixing with this commit.

* `Tested-by:` - "I successfully tested this commit". If you are the
  author of the commit, don't bother adding this tag for yourself. It
  is expected that you tested in any case.

* `Acked-by:` - "I didn't make a full deep review, but I am okay with
  the change". It is mostly used by code maintainers

* `Reviewed-by:` - "I carried technical review, didn't found any flaws
  and I believe that this commit is ready to be merged". Please read
  [Reviewer's statement of
  oversight](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#reviewer-s-statement-of-oversight)
  for more detailed description.

Most of this tags you will collect during the review process.

## Verify that your commit includes only relevant changes

Use `git log -p` or GUI tool to check that you are not committing
extra changes and that you didn't forget to include something.

## Style-check and spell-check your changes

Some projects provide built-in checkers, like `checkpatch` tools,
which is used in Linux and Zephyr. Use this tool before creating PR.

Also, it is very good idea to configure spell checker in your editor,
so reviewers will be not busy pointing spelling errors in your
documentation, comments and commit message.

## Create Pull Request

When your commits are ready, you can create a pull requests to
appropriate repositories. We are hosting all our code on GitHub, so
you should be familiar with the process. But, in short:

1. Fork the repository into your account
2. Clone it locally
3. Make changes and create commits as described above
4. Push changes back into your GitHub repository, preferably in a
   separate branch
5. Use this branch to create Pull Request into corresponding
   Xen-Troops repository

### Describe your Pull Request

If your PR consists of one commit, GitHub will do this for you. But if
there are more commits, you need to provide comprehensive description
for the PR as a whole. The same rules apply: short and descriptive PR
name, detailed description of a problem solved. Do not try to squash
your commit messages, write description for the PR as one entity instead.

If appropriate, write how you tested it. Wait for automated checks to run.

### Check-list

* Code conforms coding style
* Documentation is up-to date
* Tests are up-to date
* Commits are atomic
* Commits have good description
* Code checker / style checker is happy
* PR description is in good shape
* Issue is created if this is a "temporary" fix (see below)

## Review

When you satisfied with your PR, as someone to review it. You can find
list of maintainers and reviewers at
[reviewers](https://xen-troops.github.io/reviewers) page. In most
cases GitHub will assign reviewers immediately, but it is a good idea
to ping reviewers to tell them that PR is really ready for review.

Not every comment from reviewer require changes in code. But many of
them are. If reviewer asks you a question, do not haste to change your
code. Answer the question first.

Try to avoid unsolicited changes during review phase. Do not add new
features or change existing code unless asked by reviewer. This will
increase review time drastically, as reviewers will need to review the
PR after any such change.

Every project has maintainer(s). Maintainers have full vision of
project and its architecture. In most cases maintainers will support
code you contributed for rest of its life. So maintainers can ask you
to make changes into your PR so it better fit into project's
architecture. Even if your code is 100% correct and bug-free, it does
not mean that it will be accepted as is.

When all comments are addressed, reviewers need to explicitly approve
pull request by providing Reviewed-by or Acked-by tag.

After collecting all needed R-b tags, you should include them into
appropriate commit messages, rebase changes onto current upstream
branch and force push changes into your local branch. Refer to
[this](https://stackoverflow.com/questions/9790448/how-to-update-a-pull-request-from-forked-repo)
SO question and
[this](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
GitHub manual entry if you have any problems with this.

## If you are reviewer

Follow [Reviewer's statement of
  oversight](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#reviewer-s-statement-of-oversight
  ).

Try to not delay review. We all have own tasks to do. But code review
is one of such tasks.

Please remember that scope of you review is not only diff. Commit's
author might forget to include documentation changes or tests or
something else. It is your duty to remind them of missed changes.

If you are maintainer - it is your duty to ensure that patches conform
all the requirements. In the end, it is you who will support this code
for a long-long time.

Try to avoid [bike
shedding](https://en.wikipedia.org/wiki/Law_of_triviality). There are
multiple ways to achieve the same result. Evaluate changes only from
technical standpoint. Always support your claims with arguments. For
example: "this approach is incorrect because under circumstances A and
B it will lead to deadlock". Try to provide alternative solution in
non-obvious cases. Continuing the example: "I believe it is better to
take lock X earlier to avoid this situation"

## Merge

PR is ready to be merged when:

1. It is on top of the current upstream branch (or at least does not
   have any conflicts with it)
2. Has all needed tags (Signed-off-by, Reviewed-by:, etc)
3. All pipelines are finished with success

If you meet all of those requirements, you can ask integrator or
maintainer to merge your PR.

Please note, that we do not use merge commits. We follow rebase
strategy.

# Licensing

All new projects should be licensed under Apache-2.0 license by default.

New code in existing projects (i.e. new Linux kernel driver or Xen
feature) shall follow the licensing of existing projects.

Use [SPDX
tags](https://spdx.github.io/spdx-spec/v3.0/annexes/using-SPDX-short-identifiers-in-source-files/)
where possible.

For new files and significantly changed (new major features, major
refactoring, etc.) add EPAM's copyright line in a suitable format.  If
usure on licensing/copyright, ask Artem Mygaiev aka
[@klogg](https://github.com/klogg).

# "Temporary" changes

In some cases it is tempting to make temporary fix to get project
going forward. But practice shows that this temporary fixes tend to
stick forever. So we are trying to avoid them at all costs. But
sometimes it is only way forward. For such cases we have developed the
following procedure:

1. Describe the temporary fix in the code with `TODO` or `HACK`
   comment. Describe why this is not a final solution. Describe how
   proper fix may look like.

2. Create a commit. In the commit message you need clearly state that the
   change is temporary.

3. Make PR with the fix. In any case it should conform your
   requirements for commits and PRs described above. Again, clearly
   indicate that this is a temporary fix, provide information on who
   are responsible on providing proper solution + time frame when it
   will be done.

4. Open issue for this change in the corresponding repository. Issue
   should look like "Rework temporary fix introduced by PR
   #NNN". Assign yourself to it. Set resolution date. Make sure that
   PR page has reference to this issue.
