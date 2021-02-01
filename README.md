# Coding Guidelines for NBIS Developers

This document is is currently a work in progress, and presents practical
guidelines for developers and others at NBIS who write or contribute to
software. The guidelines cover:

* coding style and documentation,
* licensing, packaging and distribution of software,
* the use of Git and GitHub in large and small projects, and
* code reviews.

This is a reference document, and as such you are expected to dip into
it to read the parts that you need to know something about.

* If you only occasionally write code, or if you write code outside
of the NBIS system developers' team, you should read the full text of
this document at least once, but there is no expectation that you will
remember it all.
* If you are a member of the NBIS system developers' team, you should
make yourself familiar with these guidelines.
* In any case, these are *guidelines*, not hard rules. Apart from the
restrictions placed upon NBIS by our funding agencies, we do not force
these guidelines on you or on ourselves. We do however think that they
would help to streamline our workflow, improve teamwork and benefit both
ourselves individually as well as the "products" that we produce as
developers.

The NBIS team of system developers are more than happy to try to answer
any questions you may have about the contents of this document. We are
available on the NBIS Slack. The `#code-review-forum` channel is a good
place to ask.

Remember, these guidelines are supposed to make everything we produce
better, so that we can say *"I was a part of that!"*. It is important
that we don't feel that we lose too much velocity trying to adhere to
these guidelines. If you feel that they slow things down too much, we
need to review them. These things are not set in stone.

Table of Contents
=================

  * [Coding Guidelines for NBIS Developers](#coding-guidelines-for-nbis-developers)
    * [Things to be aware of when writing code](#things-to-be-aware-of-when-writing-code)
      * [Writing secure software](#writing-secure-software)
      * [Intent](#intent)
      * [Comments in code](#comments-in-code)
      * [Readability](#readability)
      * [Best programming practices](#best-programming-practices)
      * [What programming language to use](#what-programming-language-to-use)
      * [Documentation, licensing and packaging](#documentation-licensing-and-packaging)
        * [Licensing](#licensing)
        * [Files bundled with a piece of software](#files-bundled-with-a-piece-of-software)
      * [Sensitive data](#sensitive-data)
      * [Testing](#testing)
    * [How we use GitHub](#how-we-use-github)
    * [How we use Git](#how-we-use-git)
    * [General stuff about working with Git](#general-stuff-about-working-with-git)
      * [Helpful commit messages](#helpful-commit-messages)
    * [How we do code reviews](#how-we-do-code-reviews)
      * [Code reviewing steps](#code-reviewing-steps)
      * [General stuff about code reviews](#general-stuff-about-code-reviews)

## Things to be aware of when writing code

### Writing secure software
While not always the most exciting part of writing code, it is important
to consider the security risks with a software project to avoid costly
patches and perhaps more importantly, reputation damage. Security risks
analysis should be employed at the early stage of a project (it is
usually way more expensive and complicated at a late stage) and
continuously carried out until the end. 

The security risks will differ vastly between project, but general
guidelines are:

* Educate yourself, research on what security risks are related to
the different components of your project. Some example resources are:
    * [Linux Foundation (LF) Core Infrastructure Initiative (CII) Best
    Practices](https://bestpractices.coreinfrastructure.org/en)
    * [OWASP Top 10 Web Application Security Risks](https://owasp.org/www-project-top-ten/)
* Don’t blindly trust out-of-the-box software and default
configurations, e.g. popular Docker images, aws services, etc. 
Malicious software can be present and default configurations usually
have simplicity as the primary goal, not security.
* Incorporate security in the entire Software Development Lifecycle:
    * Include security considerations when gathering requirements
    * Threat model when designing
    * Implement tests and build processes that evaluate security
  
    Some examples:
    * During the design phase:
        * Explicitly map out attack surfaces.
	        * Discuss what security means for your project - you may
	        have different parts where different aspects are
	        important (e.g. an information site where the main risk
	        is lack of availability or integrity and a user part
	        where confidentiality is the major concern).
        * Discuss risks and consequences (using a risk analysis
        framework may be beneficial).
	        * Do not forget to document - identified risks, risk
	        mitigations and accepted risks may need to be revisited.
    * Enable services like [Dependabot](https://dependabot.com/) or
    [Snyk](https://snyk.io/) to enable alerts for dependices for your
    project (this can easily be done at GitHub).
    * Fuzzing can often help discovering bugs, both security related
    and others.
  
  [More information on Secure Software Development Lifecycle](https://owasp.org/www-project-integration-standards/writeups/owasp_in_sdlc/) 

### Speaking about security….
To be safe while navigating the wild west that modern computing can
be, make sure to:
* Keep software up-to-date (Turn on automatic updates for your
operating system and programs).
* Use a password manager (third-party or browser’s built-in depending
on your needs) to facilitate strong unique passwords. Do not reuse
passwords.
* Use two-factor authentication, e.g. Authy or Google Authenticator.
This is supported by many services such as GitHub, Slack, Gmail etc.
In the not so unlikely event that someone does obtain your password,
two-factor authentication can still protect you and sensitive
information.
* Always lock your computer when left unattended (even if you are
just going for a quick coffee!)
* Configure so your device is automatically locked after 1-5 minutes.
* Remove unnecessary programs from your computer to reduce attack
surface.
* Encrypt your drives, especially external. Be careful about what you
insert.
* Be careful regarding what you links you click and what you download.
Avoid visiting unknown websites, especially if they stem from a
suspicious email, and do not download software from untrusted sources.

### Intent

Names of variable, functions, methods etc. should be clear and
descriptive, not cryptic. For example, function names might be a
verb or a question: `get_gene_name()`, `find_downstream_feature()`,
`is_circular()`, or `has_multiple_flurbs()`.

It is common practice to name simple loop variables `i`, `j`, and `k`,
so there's no need to give them silly names like `the_index` unless
it's necessary for some reason or other.  Variables with longer scope
should have slightly more informative names, like `filename_map`,
`common_prefix`, `current_gene`, `feature_length` etc. (basically a
similar rule as for functions).  Avoid calling arrays `array` and hashes
`hash`, let the reader know a bit more than what's immediately obvious
from the code.

Whether you use CamelCase or any other standard for naming variables
and functions is less important, as long as it adheres to the naming
conventions of the language, and is consistent within the project.

Avoid global variables if the language allows you to do so. Global
variables should otherwise be documented in the code and ideally "stand
out" (using upper-case variable names is a common way to do this).

### Comments in code

Comments should explain *why* the code does what it does. *What* it
does should ideally already be evident from the code itself. If the code
is cryptic and can't easily be simplified, explanations might well be
needed.

A good comment clarifies intent.

### Readability

Use consistent indentation.

Make use of horizontal whitespace (code paragraphs/blocks). There is
no benefit of compacting code into as few lines as possible (unless
you're doing assembly programming for some custom chip, which we don't
do).

Avoid long lines (>75-80 characters) if possible.

* Benefits co-developers editing code in Emacs/Vim over SSH
and/or in narrower windows (commonly around 80 characters wide).
* Often makes pull requests smaller.
* Makes the code more readable.

Use a tool for automatic indentation if the editor you're using does
not do it for you, e.g. `clang-format` or `indent` for C or C++ code,
`perltidy` for Perl code, (insert others here, please).

If you have to choose between a efficient but cryptic or non-intuitive
way of doing something and a less efficient or more verbose way of
solving the same problem, either provide ample documentation to the
non-intuitive approach or do it less efficiently and leave a comment
pointing this out.

The bottlenecks in the type of programs we're writing are *usually*
network access or disk access, not memory access or CPU. To aid in
making the code readable, avoid complicated optimizations. This is
not an invitation to write slow code but to write code that is easily
understood and therefore maintainable.

### Best programming practices

Acquaintance yourself with, and follow, the best practices for the
programming language(s) that you are using.

* Google has [a good set of best practices](https://github.com/google/styleguide)
for different languages which can be a good jump-off point.
* For Perl: [Perl Best Practices](http://shop.oreilly.com/product/9780596001735.do)
(O'Reilly book).
* (Further references here, please)

If the project has any kind of best practices (explicit or implicit),
follow these.

Follow the best practices agreed upon within the organisation
(NBIS/ELIXIR).

* [ELIXIR best practices](https://github.com/SoftDev4Research)
    * [F1000 paper](https://f1000research.com/articles/6-876/v1)
* [Good Enough Practices for Scientific Computing](http://swcarpentry.github.io/good-enough-practices-in-scientific-computing/)


### What programming language to use

We don't have any _specific_ guideline for what programming languages
NBIS software should be written in.  However, keep in mind that any
software that you produce will need to be maintained by yourself and by
your colleagues (current and future), and that we do not want to end up
with unmaintainable software.

Therefore:  For a brand new project, use a major programming language
that we have expertise for within NBIS.

At the time of writing, GitHub tells us that our "top languages" in our
NBISweden repositories are

* Python
* Shell
* Perl

Based on this we can say that there exists expertise in NBIS for writing
and maintaining software written in these languages.  We also have
people with good knowledge of R, Ruby, JavaScript and C, although these
languages are not currently well represented in our GitHub repositories.

You should obviously also consider the availability of utility libraries
and the like for the language that you use, as well as your own
proficiency in the language and the proficiency of any team members.

### Documentation, licensing and packaging

Public interfaces should be documented.

Public interfaces include any method, function, subroutine or any
similar interface that a user's code may call. It also includes command
line flags and other command line arguments that are available for the
user to use.

* If a standard way of documenting an interface is given by the language
(e.g. JavaDoc in Java, or POD for use with `perldoc` in Perl), use that.
* If the language does not support generation of interface documentation
from structured inline comments or similar, you may choose to use
something like [Doxygen](http://www.doxygen.org/), or some other
tool/framework that is popular for documentation of that specific
language or type of code. REST APIs, for example, may be documented
using [Swagger](http://swagger.io/).
* At the very least, the code itself should provide comments that
explains the function and calling sequence of each public interface.

The documentation, in whatever form it exists, should be publicly
available, packaged together with the software, and easy (trivial) for a
developer to get to.

It is better to write the documentation in close proximity of the code
than to separate it out into a separate file, or worse, a wiki.

#### Licensing

When at all possible, NBIS software will be licensed under an Open
Source license. If this, for whatever reason, is not possible, you
need to consult (insert name/group here) before making the code public
on GitHub or elsewhere. See also the text in the introduction to the
section "[How we use GitHub](#how-we-use-github)" regarding Open Source
and our code as Public Record.

The preferred Open Source license that we promote is the
[*GNU General Public License version 3* (GPLv3)](https://opensource.org/licenses/GPL-3.0).
Use this license unless there is a reason to do otherwise.  Examples of
other Open Source licenses includes the
[MIT license](https://opensource.org/licenses/MIT),
[the "new"/"revised" 3-clause BSD license](https://opensource.org/licenses/BSD-3-Clause),
and
[the "simplified" 2-clause BSD license](https://opensource.org/licenses/BSD-2-Clause).

See also [http://choosealicense.com](http://choosealicense.com)

#### Files bundled with a piece of software

* **README** (plain text or Markdown-formatted). Every project should
have a `README` or `README.md` file that contains at least
    * "What this is".
    * How to run/invoke the software.
    * Short example(s) (may be included in a separate "examples"
    subdirectory).
    * A reference to the `INSTALL` and `LICENSE`/`COPYING` files.

* **INSTALL** (plain text or Markdown-formatted). Unless the software
is trivial to install (e.g. just copy one file), then an `INSTALL` or
`INSTALL.md` document should be added in which a user may find the
following:
    * External dependencies (including specific versions, where
    applicable).
    * Step-by-step instructions for how to install.
    * If appropriate, how to test the installation to make sure it works.

* **LICENSE** or **COPYING** (plain text).  This is simply a plain text
file containing the license text.

### Sensitive data

Sensitive data includes things like passwords, usernames, server names,
and data protected by law.

* Do not ever put sensitive data in files that are pushed to GitHub
or made public in any other way.
* Do not *ever* put sensitive data in files that are pushed to
GitHub or made public in any other way.
* Some types of data may even only exist in certain folders or on
certain machines. Do not proliferate this kind of data, *not even
internally*. Also avoid putting this type of data in Dropbox, Google
Drive or in similar cloud storage or shared network drive.
* If a password is published by mistake, *you need to change the
password* (with everything that this entails). It is *not* enough to
remove/reverse the commit or submit a new commit with the password
removed.
* Code should use placeholders that point to:
    * Local read-protected files, possibly located outside of the
    Git repository file structure to avoid accidental inclusion as
    part of the repository,
    * Environment variables,
    * or some sort of secured (possibly remote) storage.
* The documentation (`README`/`INSTALL`, whichever is most appropriate)
should mention how to instantiate those variables/files, etc.

### Testing

(TODO: Write me)

## How we use GitHub

In order to maximize exposure, and to facilitate collaborations with
users and other organizations, we have opted to use GitHub and the
infrastructure that GitHub provides for publishing all our publicly
accessible software. This also means that we will be using Git (rather
than Subversion, Mercurial, CVS or any other code revision system) for
keeping track of source code and documents relating to software that we
make available on GitHub.

GitHub provides excellent support for doing code reviews (more on that
below), collecting issues (bug reports) connected with a project, and
for organising work tasks ("projects" in GitHub speak).

On GitHub, we have an Educational Account called
"[NBISweden](https://github.com/NBISweden)" ("NBIS" was taken)
which acts like an umbrella for all our various repositories. Code
repositories are public (available to the world), but we have the
ability to create private repositories that are only available to
members of NBISweden, *but only if there are very special circumstances
that require this*. The ELIXIR Open Source Principles say "Start a
project in the open from the very first day, in a publicly accessible,
version controlled repository [...] The longer a project is run in a
closed manner, the harder it is to open source it later".

The source code that we produce are Public Records, and as such should
be made publicly available as Open Source. This is a requirement
within ELIXIR and for projects funded by the Swedish government.

To contribute to NBISweden repositories, or to create repositories
there, you will need to set up a GitHub account for yourself and let the
admins of NBISweden know. Current admins include *Jonas Hagberg* and
*Johan Viklund*.

## How we use Git

When appropriate, we use the *Git-Flow branching model*. This is a way
of using Git branches as a help in the development cycle.

With Git-Flow, branches are categorised into

* A **master** branch.
* A main **development** branch.
* One or several **feature** branches.
* One or several **hotfix** branches.

The code on the **master** branch (often called `master` or `release`) is
stable, properly tested and is the version of the code that a typical
user should pick. No changes are made directly on the master branch
(but see below).

Strictly speaking, Git-Flow makes a distinction between a "master"
and a "release" branch where the release branch contains the next
release-in-making, branched off from the development branch. Bugfixes
(only) are made to the release branch which is then reviewed and merged
into the master *and* development branches, creating a new release of
the software on the master branch. We do not follow this, but you are
free to do so if you think it make more sense, for example in a highly
distributed project with many active users/developers.

The code on the **development** branch (often called `develop`) should
be working, but without guarantees. For small projects, development
might well happen directly on the development branch and the code here
may therefore sometimes be broken (this should ideally never happen
though). When the development branch is deemed "done" and has undergone
testing and review, it is merged into the master branch. The release is
then tagged with an appropriate release version.

A **feature** branch (often called `feature/some_name` where `some_name` is
a very short descriptive name of the feature) is branched off from the
main development branch when a new "feature" is being implemented. A
new feature is any logically connected set of changes to the code base
regardless of how many files are being changed.

Examples of features may be implementing command line parsing, adding
support for a new type of input data format, fixing a non-critical bug
for the next release, or updating the documentation (because you forgot
to do that when you changed the code, didn't you?).

Once the feature is finished, it is merged back into the main
development branch, and its feature branch is deleted. In larger project
(more than a single developer), new features should be implemented in
feature branches and undergo review before merging (see below). This may
be highly beneficial for small projects too, obviously (do this!).

A **hotfix** branch (often called `hotfix/some_name`) is a essentially
a branch that implements a bugfix to a release. In terms of branching,
it is thus very similar to a feature branch but for the master branch
rather than for the development branch. A hotfix should fix critical
errors that were not caught in testing before the release was made.
Hotfixes should not implement new behaviour, unless this is needed to
fix a critical bug. Hotfixes need to undergo review before they are
merged back into the master *and* development branches.

The master and development branches are never deleted, while the others
are transient (temporary, for the duration of the development and review
of the feature or hotfix).

The benefits of this type of branching model in development are

* Co-developers work on separate branches, and do not "step on each
other's toes" during the development process, even if they push their
work back to GitHub.
* Co-developers and users have a stable master branch to use (for doing
work in the case of the user, and as reference to their own coding in
the case of the developer).
* Features in "feature" branches are independent of each other. Any
conflicts are resolved when merging.

There are a host of different graphical user interfaces that
helps keeping track of Git and the various branches in a
project. [SourceTree](https://www.sourcetreeapp.com) is a good free one
for macOS and Windows, for example.

The GitHub user Vincent Driessen ("nvie" on GitHub), who actually came
up with Git-Flow in the first place, has a set of Git extensions that
makes it easy to work with Git-Flow from the command line.  See his
[nvie/gitflow](https://github.com/nvie/gitflow) repository.

For more in-depth descriptions of Git-Flow, see

* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* [Git-Flow Cheatsheet](http://danielkummer.github.io/git-flow-cheatsheet/)
* [Using git-flow to automate your git branching workflow](http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/)

## General stuff about working with Git

Commit often, possibly several times a day. It's easier to roll back
a small commit than to roll back large commits. This also makes the
code easier to review (see below) as each commit carries its own commit
message. Remember to push the commits to GitHub every once in awhile
too.

Write a [helpful commit message](#helpful-commit-messages) with each
commit that describes what the changes are and possibly even why they
were necessary.

Each commit should ideally contain changes that are functionally
connected/related.  For example, changes to the command line parsing
code, changes to the documentation, and changes to some unrelated
comments may be split into three commits.

Learn how to select chunks of changed files to do multiple
separate commits of unrelated things. This is done using `git commit -p
...`.

Avoid "force push" unless it makes everyone's life easier.

If a "live" checkout of the repository needs to exist somewhere, for
example to run a public web service, then

* Don't do development in the live checkout.
* Do development and testing in a private checkout.
* Only ever do "git pull" in the live checkout.
* A live service with active users should run a stable release from
the master branch.

### Helpful commit messages

The commit messages may be seen as meta-comments on the code that are
incredibly helpful for anyone who wants to know how this piece of
software is working, including colleagues (current and future) and
external users.

Some tips about writing helpful commit messages:

1. Separate subject (the first line of the message) from body with a blank line.
2. Limit the subject line to 50 characters.
3. Capitalize the subject line.
4. Do not end the subject line with a period.
5. Use the [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood)
in the subject line.
6. Wrap the body at 72 characters.
7. Use the body to explain what and why vs. how.

For an in-depth explanation of the above points, please see [How to
Write a Git Commit Message](http://chris.beams.io/posts/git-commit/).

## How we do code reviews

Through reviewing each other's code, we believe that we will produce
better code, that we will learn more about programming, that we will
learn more about what our colleagues are actually doing, and that
teamwork across NBIS is improved.

A code review may be an iterative process in which a piece of code
is commented upon or discussed, changed by the original author, and
reviewed again before being approved.

Reviews can be conducted at any stage in development (just let someone
look at the code), but we'd like code to be more formally reviewed at
least

* before a feature branch is merged to the main development branch, or
* when a bug is fixed on the master branch before its hotfix/bugfix
branch is merged, or
* when a release is made by merging the current state of the development
branch (or release branch, if such a branch is used) to the master
branch.

To be able to use GitHub or a code review, both the author and the
reviewer should have their own personal GitHub accounts.

### Code reviewing steps

1. The code is written on a separate branch, for example on a
`feature/some_name` branch off of the main development branch.

2. The author feels that the code is correct and finished and pushes the
branch to GitHub one last time before the actual review.

3. The author creates a "pull request" for the branch by switching to
the branch on the GitHub web pages and clicking the button labelled "New
pull request".

4. The author finds one or several reviewers for the pull request and
assigns them to it.  A reviewer may be found
    * By asking one of the already designated reviewers connected to the
    project, if such a group of people has been created.
    * By asking in the "code-review-forum" in the NBIS Slack.
    * By meeting up with or contacting any other colleague that is not
    directly involved with the code that is being reviewed.

5. If needed, the author gives the reviewer(s) some background on the
project, and what the code under review is supposed to do etc. Having a
fixed group of reviewers for a project would minimize the need for this
step.
This may be done in a face-to-face meeting, on Slack, or in any other
way that is convenient. It's also possible for the author to leave
comments on GitHub (in addition to the meta-comments that the commit
messages themselves already provide)
    * With the pull request.
    * On separate commits (?).
    * On separate lines in the commits.

6. If there's more than one reviewer, one of the reviewers is designated
as the "main" reviewer. This reviewer will later do one extra thing (see
below).

7. The reviewer(s) looks at the code, specifically at the bits of code
that the specified pull request is about.

8. The reviewer(s) leaves comments and/or questions in the code by
clicking individual lines in the web viewer. Note that these comments
are public.

9. The reviewer(s) leaves a summary of their review by clicking "Review
changes" and submits it as feedback
    * without explicitly approving the pull request (it's just feedback)
    * explicitly approving the pull request (it all look good), or
    * explicitly rejecting the pull request (there's something that
    needs to be discussed and/or fixed).

10. If no reviews are rejecting the pull request, the designated
"main" reviewer will merge the pull request and delete the feature (or
whatever) branch. Note: this is the reviewer's job, not the author's
job. *The code review process ends here*.

11. If there are things that need to be modified, further commits to the
same feature branch may be necessary. These commits are automatically
added to the existing pull request.

12. The author asks the reviewer(s) to have a further look at the new
changes. The process continues from step 7.

### General stuff about code reviews

Just as with making commits often, it is better to review often in small
chunks.

It's a good idea to allocate code reviewers for a project, so that the
same people (or person) can do all the reviewing. This probably works
best if the authors and the reviewer(s) work in the same organisational
team.

Do not ask to have more than 400 lines of code reviewed in one
go. Smaller chunks are better.

A review doesn't need to take much time. In some cases 5-10 minutes (or
even less!) will be enough if the pull request is of reasonably small
size.

The reviewer is not expected to check out the code for testing, only
to read it on the GitHub website. Testing is something that the author
and/or a designated test user should do.

The reviewer reviews the code from his/her own understanding of
it. There is actually no requirement that the reviewer knows the ins
and outs of the specific programming language used. The purpose of the
review from such a reviewer is to make sure that the logic of the code
(with its comments!) is intelligible enough to be able to say "that'll
probably work" (this is not a useless review!).

The following is adapted from
[thoughtbot/guides/code-review](https://github.com/thoughtbot/guides/tree/master/code-review)

About communication (both author and reviewer):

* Ask questions and ask for clarifications. Do not make demands.
* Many development decisions are based upon personal opinions. Discuss
trade-offs.
* Avoid selective ownership of code. The code should not be referred to
as "mine", "yours" or "not yours".
* Assume good intent and well-meaning.
* Be humble. Everyone can be wrong, even both of you at once. Do not try
to "show off".
* Be explicit. Make sure that both of you know what thing you're talking
about.
* Do not use sarcasm. Keep a good and friendly tone.
* Keep an alive discussion (on Slack or in person, for example) if
something needs to be discussed. Do not lock yourselves away.

About communication (author):

* The review is adding something to your project. Acknowledge this.
* The review is all about the code, not at all about you as a person.
* Try to respond to every comment.
* Seek to understand the perspective of the reviewer(s).

About communication (reviewer):

* Do the review promptly.
* Communicate what ideas (proposed changes) you feel strongly about, and
which ones you don't.
* Identify ways in which a simpler solution to the problem may be had.
* Let the author have the last call on the final implementation,
and move philosophical, academic or otherwise unrelated technical
discussions to an alternate forum.
* Seek to understand the perspective of the author.
* Sign off the final review with a thumbs up or some other positive remark.
