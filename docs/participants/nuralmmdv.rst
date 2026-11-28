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

**Warnings:** ``rm -rf _build && bazel run //:docs`` on ``main`` reports
0 warnings: the build ends with ``build succeeded.`` and the schema validation
with ``0 warning(s)``. The list would be written to ``_build/warnings.txt``.
The build runs Sphinx with ``-W``, so any warning would fail it. The message
"the root module requires module version aspect_rules_lint@2.3.0, but got
2.5.0" comes from Bazel's dependency resolution, not from the documentation
build.

**Why delete _build:** Sphinx builds incrementally and only re-reads changed
files, so warnings from unchanged files are not reported again. A clean build
reports the complete list and no stale output remains.

**pre-commit hooks:** ``pre-commit run --all-files`` runs: fix end of files,
trim trailing whitespace, check that scripts with shebangs are executable,
check that executables have shebangs, check for added large files, copyright
headers (cr_checker), reuse lint-file, yamlfmt, bazel mod tidy and bazel mod
deps lockfile check. A violation of any of them would not make
``bazel run //:docs`` fail: Sphinx does not check file endings, whitespace,
permissions, licence headers, YAML formatting or the lockfile (``.bazelrc``
does not set ``--lockfile_mode=error``, so Bazel updates the lockfile
silently). Only pre-commit finds them. The two checks complement each other,
and CI runs both.
