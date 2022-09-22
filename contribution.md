You can contribute to Xen-Troops-related projects in various ways: for example, by creating an issue, if you encountered a problem, or providing pull request with your patches.

As we strive to have well-organized codebase, there are some rules, that will help you pass review process faster.

## Commits submission

All code is stored in git repositories.
Basically, all development is done in master branch:
> We do not keep master branch as “always production stable”, we use release tags/branches for that. Instead, we tend to keep master for the latest and greatest developments
Every commit needs to be reviewed prior to merging to master branch. Reviewers express consent with commit explicitly, by adding Reviewed-by or Acked-by tag. More on this later.

### Commits formatting
We trying to follow basic git rules and Linux kernel patch submission rules (https://www.kernel.org/doc/html/v4.15/process/submitting-patches.html) where it is applicable.

#### Commits should be as small as possible. One commit - one logical change
> Separate each logical change into a separate patch.
> For example, if your changes include both bug fixes and performance enhancements for a single driver, separate those changes into two or more patches. If your changes include an API update, and a new driver which uses that new API, separate those into two patches.
> On the other hand, if you make a single change to numerous files, group those changes into a single patch. Thus a single logical change is contained within a single patch.
> The point to remember is that each patch should make an easily understood change that can be verified by reviewers. Each patch should be justifiable on its own merits.
> If one patch depends on another patch in order for a change to be complete, that is OK. Simply note “this patch depends on patch X” in your patch description.

Again, it is perfectly fine to have a multiple commits to solve one particular task.

#### Any single commit should not break project
E.g. you can check out to any given commit in master branch and you should to get working project. It is okay to have unintended bugs, but commit should not deliberately introduce breaking changes:
> When dividing your change into a series of patches, take special care to ensure that the kernel builds and runs properly after each patch in the series. Developers using git bisect to track down a problem can end up splitting your patch series at any point; they will not thank you if you introduce bugs in the middle.

#### Commits with refactoring
It is okay to refactor related things, when working on a task. But to simplify things, refactoring should be done in a separate commit. Whitespace changes (adding or removing tabs, spaces, new lines) should be in separate commit, when they aren't changing logic (e.g. moving code to inner block in python).

#### Commit messages
Describe your problem. Whether your patch is a one-line bug fix or 5000 lines of a new feature, there must be an underlying problem that motivated you to do this work. Convince the reviewer that there is a problem worth fixing and that it makes sense for them to read past the first paragraph.
Describe user-visible impact. Straight up crashes and lockups are pretty convincing, but not all bugs are that blatant. Even if the problem was spotted during code review, describe the impact you think it can have on users.
Once the problem is established, describe what you are actually doing about it in technical detail. It’s important to describe the change in plain English for the reviewer to verify that the code is behaving as you intend it to.
Solve only one problem per patch. If your description starts to get long, that’s a sign that you probably need to split up your patch. See 2.1.1
Describe your changes in imperative mood, e.g. “make xyzzy do frotz” instead of “[This patch] makes xyzzy do frotz” or “[I] changed xyzzy to do frotz”, as if you are giving orders to the codebase to change its behaviour.
If the patch fixes a logged bug entry, refer to that bug entry by number and URL.
However, try to make your explanation understandable without external resources. In addition to giving a URL to a mailing list archive or bug, summarize the relevant points of the discussion that led to the patch as submitted.
If you want to refer to a specific commit, don’t just refer to the SHA-1 ID of the commit. Please also include the oneline summary of the commit, to make it easier for reviewers to know what it is about. Example:
```
Commit e21d2170f36602ae2708 ("video: remove unnecessary
platform_set_drvdata()") removed the unnecessary
platform_set_drvdata(), but left the variable "dev" unused,
delete it.
```
You should also be sure to use at least the first twelve characters of the SHA-1 ID. The project repository holds a lot of objects, making collisions with shorter IDs a real possibility. Bear in mind that, even if there is no collision with your six-character ID now, that condition may change five years from now.
If your patch fixes a bug in a specific commit, e.g. you found an issue using git bisect , please use the ‘Fixes:’ tag with the first 12 characters of the SHA-1 ID, and the one line summary. For example:
```
Fixes: e21d2170f366 ("video: remove unnecessary platform_set_drvdata()")
```
The following git config settings can be used to add a pretty format for outputting the above style in the git log or git show commands:
```
[core]
abbrev = 12
[pretty]
fixes = Fixes: %h (\"%s\")
```

#### The canonical commit message format
- Summary phrase (or header) of commit.
- An empty line.
- The body of the explanation, line wrapped at 75 column.
- An empty line.
- The Signed-off-by: lines, described above, which will also go in the changelog.

Bear in mind that the summary phrase of your commit becomes a globally-unique identifier for that patch. It propagates all the way into the git changelog. The summary phrase may later be used in developer discussions which refer to the patch. People will want to google for the summary phrase to read discussion regarding that patch. It will also be the only thing that people may quickly see when, two or three months later, they are going through perhaps thousands of patches using tools such as gitk or git log --oneline .

For these reasons, the summary must be no more than 70-75 characters, and it must describe both what the patch changes, as well as why the patch might be necessary. It is challenging to be both succinct and descriptive, but that is what a well-written summary should do.
Use imperative mood in the summary phrase. E.g. insead of "added new cool feature" write "add new cool feature".

A couple of example headers:
```
ext2: improve scalability of bitmap searching
x86: fix eflags tracking
```
The explanation body will be committed to the permanent source changelog, so should make sense to a competent reader who has long since forgotten the immediate details of the discussion that might have led to this patch. Including symptoms of the failure which the patch addresses (kernel log messages, oops messages, etc.) is especially useful for people who might be searching the commit logs looking for the applicable patch. If a patch fixes a compile failure, it may not be necessary to include _all_ of the compile failures; just enough that it is likely that someone searching for the patch can find it.
As in the summary phrase , it is important to be both succinct as well as descriptive.

#### Review process
You need to create Pull Request on GitHub with your changes and ask appropriate persons to review your commits.

If you are not sure who this 'appropriate person' is, look into the [list of reviewers](reviewers).

When all comments are addressed, reviewers need to explicitly approve pull request by providing `Reviewed-by` or `Acked-by` tag.

Reviewer’s statement of oversight
By offering my Reviewed-by: tag, I state that:
1. I have carried out a technical review of this patch to evaluate its appropriateness and readiness for inclusion into the project.
2. Any problems, concerns, or questions relating to the patch have been communicated back to the submitter. I am satisfied with the submitter’s response to my comments.
3. While there may be things that could be improved with this submission, I believe that it is, at this time, (1) a worthwhile modification to the codebase, and (2) free of known issues which would argue against its inclusion.
4. While I have reviewed the patch and believe it to be sound, I do not (unless explicitly stated elsewhere) make any warranties or guarantees that it will achieve its stated purpose or function properly in any given situation.

After collecting all needed R-b tags, commiter should include them into appropriate commit messages, rebase changes onto current master branch and force push changes.

#### Merge process
PR is ready to be merged when:
1. It is on top of the current master branch (or at least does not have any conflicts with it)
2. Has all needed tags (Signed-off-by, Reviewed-by:, etc)
3. All pipelines are finished with success

If you meet all of those requirements, you can as integrator to merge your changes into master branch.


This work is licensed under the Creative Commons Attribution 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/.
