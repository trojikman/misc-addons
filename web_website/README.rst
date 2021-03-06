.. image:: https://itpp.dev/images/infinity-readme.png
   :alt: Tested and maintained by IT Projects Labs
   :target: https://itpp.dev

.. image:: https://img.shields.io/badge/license-LGPL--3-blue.svg
   :target: http://www.gnu.org/licenses/lgpl
   :alt: License: LGPL-3.0

=====================
 Multi-Brand Backend
=====================

Technical module to properly handle multi-website setup.

The modules sets context variable **allowed_website_ids**:

* in backend: selected websites
* in frontend: current website (as a list)

The module adds ``env`` properties:

* ``env.website`` -- first website from the list: ``browse(context["allowed_website_ids"][0])``
* ``env.websites`` -- all websites: ``browse(context["allowed_website_ids"])``

website_dependent
=================

The module adds new field attribute ``website_dependent``, which is analog of ``company_dependent``, but for websites.

See `<models/test_website.py>`_ and `<tests/test_website_dependent.py>`_ to understand how it works.

If you need to convert existing field to a website-dependent field it's not
enough just to add the attributes. You need additional stuff to make your module
safely installable and uninstallable. See module
``ir_config_parameter_multi_company`` as an example. Things to do:

* extend ``ir.property``'s ``write`` to call ``_update_db_value_website_dependent``
* Add to the field both ``company_dependent=True`` and ``website_dependent=True``
* In the field's module extend following methods:

  * ``create`` -- call ``_force_default``
  * ``write`` -- call ``_update_properties_label``
  * ``_auto_init`` -- call ``_auto_init_website_dependent``

* In the field's module add ``uninstall_hook``:

  * remove field's properties

Roadmap
=======

* TODO: Since odoo 12, there is another switcher at ``[[ Website ]] >> Dashboard`` menu. It has to be syncronized with the switcher of this module, i.e. hide default one and use value of this module switcher.

Questions?
==========

To get an assistance on this module contact us by email :arrow_right: help@itpp.dev

Contributors
============
* `Ivan Yelizariev <https://it-projects.info/team/yelizariev>`__


Further information
===================

Odoo Apps Store: https://apps.odoo.com/apps/modules/13.0/web_website/


Notifications on updates: `via Atom <https://github.com/it-projects-llc/misc-addons/commits/13.0/web_website.atom>`_, `by Email <https://blogtrottr.com/?subscribe=https://github.com/it-projects-llc/misc-addons/commits/13.0/web_website.atom>`_

Tested on `Odoo 13.0 <https://github.com/odoo/odoo/commit/8ebb5bdb4b63927a302f0d057b2f4db535d93829>`_
