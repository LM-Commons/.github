# Contributing

Contributions to this project are more than welcomed.

In order to have consistency in the quality of the code base, there are rules to follow.

### Coding Standards

We follow the same [Coding Standards](https://github.com/laminas/laminas-coding-standard) that the Laminas Project
is using (CodeSniffer).

### Static Analysis

We use Psalm to perform static analysis on the code.

### Testing

We use PhpUnit to perform tests.  We strive to achieve 100% code coverage in testing.

### Markdown Linting

We use [Markdownlint](https://www.npmjs.com/package/markdownlint) for Markdown Linting, primarily of the `README.md` file.

### Continuous Integration Testing and Quality Control

We use GitHub actions to perform Continuous Integration to validate that commits comply with requirements.
We use the [Laminas Continuous Integration Github Action](https://github.com/laminas/laminas-continuous-integration-action)
to perform CI on Pull Requests.

The Laminas CI action will perform the following actions:

- Markdown Linting of the `README.md` file
- Coding Standard checks
- Static Analysis checks
- Unit Testing against all PHP version that the packages supports for both lastest and lowest versions of the dependencies. 

## New Features or Refactoring

If you are working on new features or refactoring for an existing project item, create a Request For Comments (RFC) in the corresponding repository via a new issue report.
The objective is to generate discussion and comments before initiating work.

If you want to propose a new component/package, create a RFC via a new issue report in the [LM-Commons Github repository](https://github.com/LM-Commons/.github)

## Release Branches

Each repository uses _release branches_ named after the minor release series they represent; e.g., "2.11.x", "3.1.x", "0.2.x", etc.
Each branch is under a "security window", and can and will receive security updates as long as the minimum supported version of PHP in that branch is still supported by php.net.
Generally speaking, the default branch is targeting the next minor or major version, and does not yet have releases against it; it represents new features or changes to the package.
The release branch with the next most recent release series is the one for the currently active release, and will receive bugfixes and security fixes, but no new features.
Since each component has its own lifecycle, the number and names of releases branches will vary from component to component.

For example, assuming the latest release of a component is 1.1.0, then:

- Branch 2.0.x corresponds to the next major release, if any.
- Branch 1.2.x, typically the default branch, corresponds to the next minor release with new features
- Branch 1.1.x corresponds to the branch receiving bugfixes and patches for release 1.1.0 which will be eventually released as 1.1.1

## Getting Started

If you want to contribute to an existing repository, here is the recommended workflow:

1. Your first step is to establish a public repository from which we can pull your work into the canonical repository.
We recommend using [GitHub](https://github.com), as that is where the component is already hosted.
2. Setup a [GitHub account](https://github.com/join), if you dont' have one.
2. Fork the repository using the "Fork" button at the top right of the repository landing page. It is recommended that you include all branches in your forked repository
3. Clone the forked repository into your local development environment.
   Use the "Clone or download" button above the code listing on the repository landing pages to obtain the URL and instructions.
4. Navigate to the directory where you have cloned the repository.
5. Add a remote to your fork; substitute your GitHub username and the repository name in the commands below.

   ```console
   $ git remote add fork git@github.com:{username}/{repository}.git
   $ git fetch fork
   ```

Alternately, you can use the [GitHub CLI tool](https://cli.github.com) to accomplish these steps:

```console
$ gh repo clone {org}/{repo}
$ cd {repo}
$ gh repo fork
```

### Keeping Up-to-Date

Periodically, you should update your fork or personal repository to match the canonical repository.
Assuming you have setup your local repository per the instructions above, you can do the following:


```console
$ git fetch origin
$ git switch {branch to update}
$ git pull --rebase --autostash
# OPTIONALLY, to keep your remote up-to-date -
$ git push fork {branch}:{branch}
```

If you're tracking other release branches, you'll want to do the same operations for each branch.

### Working on a patch

We recommend you do each new feature or bugfix in a new branch.
This simplifies the task of code review as well as the task of merging your changes into the canonical repository.

A typical workflow will then consist of the following:

1. Create a new local branch based off the appropriate release branch.
   For example, `hotfix/9295` from branch 1.1.x for a fix on release 1.1.0
2. Switch to your new local branch.
   (This step can be combined with the previous step with the use of `git switch -c {new branch} {original branch}`, or, if the original branch is the current one, `git switch -c {new branch}`.)
3. Do some work, commit, repeat as necessary.
4. Push the local branch to your remote repository.
5. Send a pull request.

The mechanics of this process are actually quite trivial. Below, we will
create a branch for fixing an issue in the tracker.

```console
$ git switch -c hotfix/9295
Switched to a new branch 'hotfix/9295'
```

... do some work ...

### Run the QA tools

To ensure that your work will pass the CI tests, run the QA tools:

```console
$ composer test
$ composer cs-check
$ composer static-analysis

# if you modified the README.md file
$ npx markdownlint-cli2 ./README.md
```

Once there are no more errors reported, you are ready to commit you changes.

#### Resolving Psalm static analysis issues

Psalm will perform static analysis and signal potential issues that must be resolved or baselined in order to
pass CI when commits are pushed to GitHub.

Ideally, you should fix the code such where Psalm raises an issue. However, this is
not always possible for many reasons.  Issues that cannot be resolved will need
to be either baselined or suppressed.

First, when running Psalm, ensure that the existing baseline, if any, is updated
following your changes:

```console
$ composer static-analysis -- --update-baseline
  or
$ composer static-analysis-update-baseline
```

To baseline remaining issues, which basically tells Psalm to ignore, for now,
the errors it reports without modifying the code:

```console
$ composer static-analysis -- --set-baseline=psalm.baseline.xml
```
To suppress an error, which basically tells Psalm to ignore an error permanently, you can add
instructions in the code for Psalm to ignore specific errors:

```php
  /** @psalm-suppress MixedArgument */
  return new MyService($container->get('Some dependency'));
```

**Use `@psalm-suppress` carefully and scarcely to avoid suppressing errors that should be caught.**

### Commit your changes

```console
$ git commit --signoff
```

> ### About the --signoff flag
>
> See the [section on commit signoffs](#commit-signoffs) below for more details on the `--signoff` option to `git commit` and why we require it.

... write your log message ...

```console
$ git push fork hotfix/9295:hotfix/9295
Counting objects: 38, done.
Delta compression using up to 2 threads.
Compression objects: 100% (18/18), done.
Writing objects: 100% (20/20), 8.19KiB, done.
Total 20 (delta 12), reused 0 (delta 0)
To ssh://git@github.com/{username}/{lmc-package-name}.git
   b5583aa..4f51698  HEAD -> hotfix/9295
```

To send a pull request, you have several options.

If using GitHub, you can do the pull request from there.
Navigate to your repository, select the branch you just created, and then select the "Pull Request" button in the upper right.
Select the user/organization "lm-commons" as the recipient.

You can also perform the same steps via the [GitHub CLI tool](https://cli.github.com).
Execute `gh pr create`, and step through the dialog to create the pull request.
If the branch you will submit against is not the default branch, use the `-B {branch}` option to specify the branch to create the patch against.

Make sure that you submit your pull request against the appropriate branch in the upstream repository.

#### Performing assertions on the code

We distinguish assertions in two categories:

1. Expectations of something that is always supposed to be true
1. Expectations of some input to follow some rule

The former is a development-only concern, aiming to aid tools (e.g. IDEs, static analysis) to perform a more accurate type detection.
In this scenario, you should use the [`assert()`](https://www.php.net/manual/en/function.assert.php) function, which executes the expression when [`zend.assertions`](https://www.php.net/manual/en/ini.core.php#ini.zend.assertions) is enabled.
That will make sure that the condition is valid, while easing future refactoring.

The latter is something that must be executed regardless of configuration or environment.
These expectations should be configured using the class [`Webmozart\Assert\Assert`](https://github.com/webmozarts/assert), which provides an extensive list of helpful methods for input validation.
Please make sure to add a helpful message to be used when the assertion fails.

Example:

```php
<?php
declare(strict_types=true);

namespace Lmc\Examples;

use Psr\Container\ContainerInterface;
use Webmozart\Assert\Assert;

$container = require __DIR__ . '/../config/container.php';
// we know that the file above ALWAYS returns an instance of this type
\assert($container instanceof ContainerInterface);

// now we can call `ContainerInterface#get()`, which will be understood by psalm and even autocompleted by PHPStorm
$app = $container->get(MyApp::class);

// we cannot always rely that the `MyApp::class` service is indeed an instance of that class, hence an assertion
// that guards it despite the environment
Assert::isInstanceOf(
    $app,
    MyApp::class,
    'It looks like the DI container is misconfigured, please verify the service ' . MyApp::class
);

$app->run();
```

### What branch to issue the pull request against?

Which branch should you issue a pull request against?

- For fixes against the stable release, issue the pull request against the release branch matching the minor release you want to fix.
- For new features, or fixes that introduce new elements to the public API (such as new public methods or properties), issue the pull request against the default branch.

### Branch Cleanup

As you might imagine, if you are a frequent contributor, you'll start to get a ton of branches both locally and on your remote.

Once you know that your changes have been accepted to the canonical repository, we suggest doing some cleanup of these branches.

- Local branch cleanup

  ```console
  $ git branch -d <branchname>
  ```

- Remote branch removal

  ```console
  $ git push fork :<branchname>
  ```

## Contributing Documentation

The documentation for each repository is available in the "**docs/**" directory of that repository, is built using [Docusaurus](https://docusaurus.io/).
To learn more about how to contribute to the documentation for a repository, including how to setup the documentation locally, please refer to https://github.com/LM-Commons/documentation-theme.

## Commit Signoffs

In order for us to accept your patches, you must provide a signoff in all commits; this is done by using the `-s` or `--signoff` option when calling `git commit`.
The signoff is used to certify that you have rights to submit your patch under the project's license, and that you agree to the [Developer Certificate of Origin](https://developercertificate.org).

### Automating signoffs

You can automate adding your signoff by using a commit template that includes the line:

```text
Signed-off-by: Your Name <your.email@example.com>
```

Put that line into a file, and then run the command:

```console
git config commit.template {path to file}
```

If you want to use this template everywhere, pass the `--global` option to `git config`.

Many IDEs, such as PhpStorm, supports signoff with their GIT commands.

### Adding signoffs when committing from the GitHub website

You can add a signoff manually when committing from the GitHub website by adding the line `Signed-off-by: Your Name <your.email@example.org>` by itself in the commit message.

### Adding signoffs after-the-fact

If you have forgotten to signoff your commits, you have two options.
For both, the first step is determining the number of commits you have made in your patch.
On Unix-like systems (Linux, BSD, OSX, WSL, etc.), this is generally accomplished via:

```console
git log --oneline {original branch}..HEAD | wc -l
```

From there, choose either to only signoff, or perform an interactive rebase:

- Signoff only: run `git rebase --signoff HEAD~{number you discovered}`

- Full interactive rebase: run `git rebase -i HEAD~{number you discovered} -x "git commit --amend --signoff --no-edit"`.
  This will present the standard interactive rebase screen, but include the line `exec git commit --amend --signoff --no-edit` between each commit.
  Leave those lines after each commit you will be keeping.

If you run into issues during a rebase operation, you can generally execute `git rebase --abort` to return to the original state.

When done, execute `git push --force-with-lease`, specifying the correct remote and branch, in order to force-push your amendments.
