# Requirements for releases

Each repo has its release schedule. Release may be created for different
reasons, like:
- obligations before the customer
- significant change in the API provided by the project
- the need to change the build base, like upgrade the yocto version
- the periodic milestone of the actively developing project
- ...

Despite the reasons, the release has the following requirements:
- all external dependencies have to be fixed using the commit hash or
release tag only
- the usage of the developer's forks of the xen-troops' repos is not acceptable
- the head of the branch of the external repo can't be used in the release

To be more specific:
- for yocto recipes, if SRC_URI has any internet link, there has to be SRCREV
with the commit hash or release tag (AUTOREV can't be used)
- any component mentioned in the `sources:` block of the moulin configuration
(.yaml) has to have `rev:` set to the commit hash or release tag (branch name
can't be used)
- android manifest has to have fixed commits

Before the release, we need to take into account the impact on other dependable
repos. This means, that release notes have to describe:
- changes in the provided API
- changes in build requirements
- expected build issues and possible solutions
- known issues with workaround
