..
   # *******************************************************************************
   # Copyright (c) 2026 Contributors to the Eclipse Foundation
   #
   # See the NOTICE file(s) distributed with this work for additional
   # information regarding copyright ownership.
   #
   # This program and the accompanying materials are made available under the
   # terms of the Apache License Version 2.0 which is available at
   # https://www.apache.org/licenses/LICENSE-2.0
   #
   # SPDX-License-Identifier: Apache-2.0
   # *******************************************************************************

Nural Mammadov
==============

Track: quality

Environment
-----------

.. list-table::
   :header-rows: 1

   * - Tool
     - Version
   * - OS
     - Ubuntu 24.04.4 LTS (WSL 2 on Windows 11)
   * - Bazelisk
     - 1.25.0
   * - Bazel
     - 8.8.2
   * - gh
     - 2.45.0
   * - VS Code
     - 1.119.0
   * - Python
     - 3.12.12 (``.venv_docs``)
   * - pre-commit
     - 3.6.2

Eclipse Contributor Agreement
-----------------------------

Already in place: the ECA is signed, and my GitHub username ``nuralmmdv`` is
entered in my Eclipse account profile.

Notes
-----

- ``gh auth login`` and an SSH key registered on GitHub: done.
- ``bazel run //:ide_support``: done, ``.venv_docs/bin/python`` selected as
  interpreter in VS Code.
- ``bazel run //:live_preview``: works.
- Issues are enabled in this repository.

Task 1.1: Following an upstream
-------------------------------

**Compared commits:** ``main`` (``c62896d6``, merge of pull request #21, based
on the initial commit ``bde880bb``) with ``upstream/main`` (``14888d15``,
build(bazel): align SCORE sanitizer configurations (#264)) of
``eclipse-score/module_template``.

``git log --oneline main..upstream/main`` lists 200 commits, and
``git diff --stat main upstream/main`` reports 96 changed files. The numbers are
this large because this copy was made with "Use this template": it starts from
a new single commit and shares no history with the template
(``git merge-base main upstream/main`` returns nothing, exit code 1). The lists
therefore contain the whole template history and the training's own files, not
only what is new since the copy was made.

**Fork and Sync fork:** I forked ``eclipse-score/module_template`` to
``nuralmmdv/module_template``. "Sync fork" reported that the fork is not behind
upstream. In a clone of the fork, ``git merge-base main upstream/main`` returns
``14888d15`` and ``git log --oneline main..upstream/main`` lists 0 commits.

**Difference:** A template copy is unrelated to its origin: new upstream
commits cannot be listed with ``git log``, and changes have to be carried over
by hand. A fork shares the history: new upstream commits can be listed, taken
over with "Sync fork" or ``git merge upstream/main``, and pull requests can be
opened against the upstream repository.

**For S-CORE:** I will use a fork. Contributions reach an S-CORE repository as
pull requests from a branch in the contributor's fork; this is how I opened
pull requests on ``eclipse-score/logging``.

Task 2.1: Clean documentation build
-----------------------------------

**Warnings:** None. The clean build on ``main`` ended with
``build succeeded.``, and the schema validation also reported 0 warnings.
If there were any, they would end up in ``_build/warnings.txt``. Since the
build runs with ``-W``, a single warning would already make it fail. The
``aspect_rules_lint`` version message at the top is a Bazel warning about
dependencies, not a documentation warning.

**Why delete _build first:** Sphinx only rebuilds files that changed, so
warnings from untouched files don't show up again on the next run. Starting
from an empty ``_build`` is the only way to see the full picture.

**pre-commit:** All ten hooks passed (end of files, trailing whitespace, two
shebang checks, large files, copyright headers, reuse, yamlfmt, bazel mod tidy
and the lockfile check). None of them would break ``bazel run //:docs``:
Sphinx doesn't care about whitespace, file permissions, licence headers or the
lockfile. So these problems are only caught by pre-commit, and that's why CI
runs both.

fix end of files.........................................................Passed
trim trailing whitespace.................................................Passed
check that scripts with shebangs are executable..........................Passed
check that executables have shebangs.....................................Passed
check for added large files..............................................Passed
Check and fix copyright headers with cr_checker..........................Passed
reuse lint-file..........................................................Passed
yamlfmt..................................................................Passed
bazel mod tidy...........................................................Passed
bazel mod deps lockfile check............................................Passed

Task 2.4: defect/build-3
------------------------

``defect/build-3`` renames ``user_manual.rst`` to ``user_guide.rst``. I built
``main`` first and then ``defect/build-3`` without deleting ``_build``. The
build was green, but ``_build/module/manuals/`` had both ``user_guide.html``
(new, 23:06) and ``user_manual.html`` (still from the ``main`` build, 22:58).

The docs build is incremental: it writes pages for the sources that exist, but
it never removes the output of a source that was deleted or renamed. So the old
page stays behind and can still be opened, even though nothing in the sources
points to it anymore. After ``rm -rf _build && bazel run //:docs`` on the same
branch, ``user_manual.html`` was gone.

That's the practical side of task 2.1: only a clean build shows what the
sources really produce. The branch is not merged.
