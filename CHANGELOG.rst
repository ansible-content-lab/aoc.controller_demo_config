========================================
Aoc.Controller_Demo_Config Release Notes
========================================

.. contents:: Topics

This changelog describes changes after version 3.0.0.

v6.2.0
======

Release Summary
---------------

Remove python deps because they broke cloud EE build.

Major Changes
-------------

- Added `requirements.yml`
- Removed `requirements.txt`

v6.1.0
======

Release Summary
---------------

Small tweaks and bug fixes

Minor Changes
-------------

- Added "vanilla" EE that contains the latest version of ansible core with no collections for light weight testing.
- Added example linux job templates and project.
- Moved multiple settings for AD auth into a single task.

v6.0.0
======

Release Summary
---------------

Added AWS, Google Cloud, and Azure validated content examples.

Major Changes
-------------

- Adding AWS CoP playbooks as job templates
- Adding AWS CoP project
- Adding Google Cloud CoP playbooks as job templates
- Adding Google Cloud CoP project
- Adding new Azure CoP playbooks as job templates

Minor Changes
-------------

- Begin variable migration to ansible-lint suggested `role_` syntax with new variables
- Bumped pre-commit dependencies
- Updated URL links on README files to correct broken links.

Breaking Changes / Porting Guide
--------------------------------

- Renamed nested variables to snake case to align with Ansible lint

Bugfixes
--------

- Credential no_log default when running in EEs
- Use name parameter for projects since IDs returned from operations are not guaranteed

v5.1.2
======

Release Summary
---------------

Minor changes and bug fixes.

Minor Changes
-------------

- Do not delete demo job by default.
- Upgraded pre-commit checks.

v5.1.1
======

Release Summary
---------------

Updated AWS deployment template variables.

Minor Changes
-------------

- Updated AWS deployment variables based on changes to playbook.

v5.1.0
======

Release Summary
---------------

Added AWS Deployment collection and improvements.

Major Changes
-------------

- Added the AWS deployment job template that will deploy a self-managed AAP infrastructure on AWS.
- Reverted the templatization of variables that was redundant.  If you used these variables, then they will simply be ignored now.

Minor Changes
-------------

- Added pre-commit.
- Added yamlfmt.
- Updated readme with better instructions and layout.

v5.0.0
======

Release Summary
---------------

Version 5.0.0 release.

Major Changes
-------------

- Changed paths to account for lab.azure and lab.aws collections.
- Updated pre-commit dependencies.

Breaking Changes / Porting Guide
--------------------------------

- Changed namespace to "lab".
- Removed references to awx collection.
- Updated role syntax to use fully qualified collection names.

v4.2.0
======

Release Summary
---------------

Updates to dependent collections.

Major Changes
-------------

- Moved playbook to playbooks folder.

Minor Changes
-------------

- Updated for changes to dependent collections.

v4.1.1
======

Release Summary
---------------

Added azure tags to job templates.

Minor Changes
-------------

- Added azure tag to job templates.
- Bumped pre-commit tool versions.

v4.1.0
======

Release Summary
---------------

Adding AWS content to the cloud content lab seeded content.

Major Changes
-------------

- Added AWS job templates.
- Added GitHub Workflows tests.

Minor Changes
-------------

- Readme updates to account for the new content added.

Breaking Changes / Porting Guide
--------------------------------

- Changed variables that start with ``azure_`` to ``azure.``.

v4.0.3
======

Release Summary
---------------

Fixes to ensure that seeded content would deploy properly on AAP.

Minor Changes
-------------

- Fixes to ensure that seeded content would deploy properly on AAP.

v4.0.2
======

Release Summary
---------------

Removed assertions prior to playbook run that were no longer needed.

Minor Changes
-------------

- Removed assertions prior to playbook run.

v4.0.1
======

Release Summary
---------------

Removed requirements.yml to push dependency to EE.

Minor Changes
-------------

- Removed requirements.yml

v4.0.0
======

Release Summary
---------------

Refactored the collection with the intent of having a general use as a PoC for seeding content into automation controller.

Major Changes
-------------

- Flags to deploy validated content, content lab content, or both.
- Separated the ability to deploy validated content and content lab content.

Minor Changes
-------------

- Introduced change log.

Breaking Changes / Porting Guide
--------------------------------

- All variable names have been edited and refactored. See ``roles/controller/defaults/main.yml`` for new variables and structure.
