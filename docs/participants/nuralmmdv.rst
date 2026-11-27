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
