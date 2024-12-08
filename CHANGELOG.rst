Changelog
=========

Alle nennenswerten Änderungen an diesem Projekt werden in dieser Datei
dokumentiert.

Das Format basiert auf `Keep a Changelog
<https://keepachangelog.com/en/1.0.0/>`_ und dieses Projekt hält sich an
`Calendar Versioning <https://calver.org>`_.

Die erste Zahl der Version ist das Jahr. Die zweite Zahl wird mit jeder Version
erhöht, beginnend bei 1 für jedes Jahr. Die dritte Zahl ist für Notfälle, wenn
wir Zweige für ältere Versionen starten müssen.

.. unreleased

`Unreleased <https://github.com/veit/python-basics-tutorial-de/compare/24.3.0...HEAD>`_
---------------------------------------------------------------------------------------

Added
~~~~~

* 📝 Add logging section from Python4DataScience
* 🔧 📝 Add social media support

  * Add OpenGraph tag for mastodon
  * Add social media links

* 📝 Add PyPI digital attestations
* 📝 Add tip for a stride of -1
* 📝 Add What’s new?
* 📝 Add conversion to reST

Changed
~~~~~~~

* 📝 Update GitLab package registry
* 📝 Expand the pytest plugins section
* 📝 Rearrange functions section
* 📝 Update installation of freethreaded Python
* 📝 Expand the contextmanager section
* 🎨 Restructure the documentation

  * Move packages outside libraries
  * Move apps in packages
  * Remove unittest2
  * Move doctests to Sphinx
  * Move the sqlite database test to unittest
  * Move Sphinx to a subchapter

`24.3.0 <https://github.com/veit/python-basics-tutorial-de/compare/24.2.0...24.3.0>`_
-------------------------------------------------------------------------------------

Added
~~~~~

* 📝 Add humanize
* 📝 Add testing code to documentation
* 📝 Add bump-my-version
* 📝 Add vale
* 📝 Add codespell
* 📝 Add checks
* 📝 Add LBYL and EAFP to exceptions
* 📝 Add the series of tutorials and trainings
* 📝 Adopt SOLID principles from Python4DataScience

Changed
~~~~~~~

* 📝 Update description for init files
* 📝 Update pytest plugins

  * Add pytest-freethreaded to plugins for modified test sequences
  * Add pytest-edit to modified output plugins
  * Add playwright and pyleniumio to web dev plugins
  * Add pytest-patterns to various plugins
  * Remove pytest-splinter, pytest-mimesis and pytest-freezegun

* 📝 Switch to uv for building envs, packaging etc.

  * Install different Python versions in parallel including PyPy and
    free-threaded Python 3.13.
  * Add tox-uv
  * Publishing packages
  * Update uv.lock file with a pre-commit hook

* 📝 Update cibuildwheel
* 👷 Switch to uv in ci
* 📝 Switch to .venv directory
* 📝 Update to Python 3.13
* 🔧 Switch to pyproject.toml
* 📝 Rearrange documentation tests
* 📝 Extend documentation of the string type
* 📝 Extend documentation of the tuple type
* 📝 Extend documentation of the list type
* 📝 Add sphinx-issues
* 📝 Add direnv tip
* 📝 Update instructions for installing packages
* 📝 Add proxy config for PyPI

Fixed
~~~~~

* 📝 Fix coverage pipeline
* ✏️ Workaround for pytest lexer warnings

`24.2.0 <https://github.com/veit/python-basics-tutorial-de/compare/24.1.0...24.2.0>`_
-------------------------------------------------------------------------------------

Added
~~~~~

* 📝 Add design patterns
* 📝 Add frozenset
* 📝 Add git filter for pytest
* 📝 Add interrogate for docstring coverage

Changed
~~~~~~~

* 📝 Update Python on mobile devices
* 📝 Update Tiobe Index
* 📝 Expand section for testing the documentation

`24.1.0 <https://github.com/veit/python-basics-tutorial-de/compare/v1.0.0...24.1.0>`_
-------------------------------------------------------------------------------------

Added
~~~~~

* 🌱 Add matplotlib for social cards
* 📝 Add links for strftime
* 📝 Add link to dataclasses
* 📝 Add exclude_also to coverage configs

Changed
~~~~~~~

* 🔧 Use git tag for versioning the docs
* 📝 Update None type
* 📝 Update the review of values and identity
* 📝 Update comparative expressions
* 📝 Update dataprep example
* 📝 Update publishining packages

  * Add trusted publisher

Fixed
~~~~~

* 🎨 pre-commit fixes

`v1.0.0 <https://github.com/veit/python-basics-tutorial-de/commit/550a5202cd0c3055dea6d027083f99678d149228>`_: 2023-11-28
-------------------------------------------------------------------------------------------------------------------------

Added
~~~~~

* 📝 Add dataclasses
* 📝 Add striding and link to slicing with pandas
* 📝 Add lambda functions

Changed
~~~~~~~

* 🔖 Update to 1.0.0, add changelog
* 💄 Switch to furo theme
* 📝 Switch to intersphinx links
* 📝 Add note to Unicode help
* 📝 Add link to pandas I/O tools and examples for serialisation files
* 📝 Update dicts type

  * Add setdefault
  * Add merging of dictionaries

* 📝 Update list type

  * Add loops with index
  * Add list comprehensions

* 📝 Update set type
* 📝 Extend the strings section
* 📝 Add link to bankers’ rounding
