========================================
Aoc.Controller_Demo_Config Release Notes
========================================

.. contents:: Topics

This changelog describes changes after version 3.0.0.

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
