Getting started
===============

..  include:: _robot.rst

Installation
------------

**Plone Mosaic** is installed just like any other Plone add-on by activating it at the Add-on control panel.

..  figure:: _screenshots/mosaic-product-activated.png
..  code:: robotframework

    Show Plone Mosaic activation
        Go to  ${PLONE_URL}/prefs_install_products_form

        Page should contain element  id=plone.app.mosaic
        Assign id to element
        ...  xpath=//*[@id='plone.app.mosaic']/parent::*
        ...  addons-plone-app-mosaic
        Assign id to element
        ...  xpath=//*[@id='plone.app.mosaic']/ancestor::form
        ...  addons-enabled

        Highlight  addons-plone-app-mosaic
        Capture and crop page screenshot
        ...  _screenshots/mosaic-product-activated.png
        ...  id=addons-enabled

After **Plone Mosaic** has been installed, everything should look normal. Yet, each page is now being rendered through the Plone Mosaic composition chain.

..  figure:: _screenshots/mosaic-document-layout-default.png
..  code:: robotframework

    Show document rendered with the default layout
        Create content  type=Document
        ...  id=example-document
        ...  title=Example Document
        ...  description=This is an example document
        ...  text=<p>This document will soon have a custom layout.</p>
        Go to  ${PLONE_URL}/example-document/++layout++default
        Capture and crop page screenshot
        ...  _screenshots/mosaic-document-layout-default.png
        ...  css=html

If something breaks just by installing **Plone Mosaic**, it's probably a bug and it should be reported_ as such.

.. _reported: https://github.com/plone/plone.app.mosaic/issues


Custom layout
-------------

The most prominent feature provided by **Plone Mosaic** is the new **Layout-behavior**, which appears as new **Custom layout** option in the familiar display menu.

..  figure:: _screenshots/mosaic-custom-layout-enable.png
..  code:: robotframework

    Show how to select the custom layout option
        Element should be visible  id=plone-contentmenu-layout
        Click element  css=#plone-contentmenu-layout dt a
        Element should be visible  id=plone-contentmenu-display-view

        Update element style  css=.managePortletsFallback  display  none
        Highlight  id=plone-contentmenu-display-view
        Capture and crop page screenshot
        ...  _screenshots/mosaic-custom-layout-enable.png
        ...  id=edit-bar  id=plone-contentmenu-layout
        ...  css=#plone-contentmenu-layout dd

        Mouse over  id=plone-contentmenu-display-view
        Click element  id=plone-contentmenu-display-view
        Page should contain  View changed.

How the current content looks after the first time the **Custom layout** is activated, depends on the configured defaults for its portal type. Still, at least the title and the description should be always displayed.

..  figure:: _screenshots/mosaic-custom-layout-enable-done.png
..  code:: robotframework

    Show how to custom layout view
       Update element style  css=.managePortletsFallback  display  none
       Capture and crop page screenshot
        ...  _screenshots/mosaic-custom-layout-enable-done.png
        ...  id=edit-bar  id=plone-contentmenu-layout  id=content
        ...  jquery=.portalMessage:visible
        ...  jquery=#content > div:last


Mosaic editor
-------------

When the **Custom layout** has been enabled, the **Mosaic editor** is opened by clicking the **Edit** tab.

..  figure:: _screenshots/mosaic-editor-open.png
..  code:: robotframework

    Show how Mosaic editor is opened
        Go to  ${PLONE_URL}/example-document
        Highlight  id=contentview-edit
        Capture and crop page screenshot
        ...  _screenshots/mosaic-editor-open.png
        ...  id=edit-bar  id=plone-contentmenu-layout

..  figure:: _screenshots/mosaic-editor-overview.png
..  code:: robotframework

    Show the Mosaic editor
        Go to  ${PLONE_URL}/example-document/edit
        Element should be visible  css=.mosaic-toolbar
        Capture and crop page screenshot
        ...  _screenshots/mosaic-editor-overview.png
        ...  css=html

To add a new tile in the **Mosaic editor**, select the tile from the rightmost menu

..  figure:: _screenshots/mosaic-editor-select-field-text-tile.png
..  figure:: _screenshots/mosaic-editor-menu-insert.png
    :align: right
..  code:: robotframework

    Show how to select a new tile from menu
        Element should be visible  css=.mosaic-toolbar

        # XXX: Because Selenium cannot capture open dropdown menu from Firefox,
        # we need to hack a bit to get a shot. Later, change to Select2 widget
        # or something else instead of native dropdown will fix that.
        Execute javascript  $('.mosaic-menu-insert').attr('size', 20)
        Mouse down  css=.mosaic-menu-insert
        Mouse over  css=.mosaic-option-IRichText-text
        Capture and crop page screenshot
        ...  _screenshots/mosaic-editor-menu-insert.png
        ...  css=.mosaic-menu-insert
        Mouse up  css=.mosaic-toolbar
        Execute javascript  $('.mosaic-menu-insert').attr('size', 1)

        Highlight  css=.mosaic-menu-insert
        Mouse down  css=.mosaic-menu-insert
        Mouse over  css=.mosaic-option-IRichText-text

        Capture and crop page screenshot
        ...  _screenshots/mosaic-editor-select-field-text-tile.png
        ...  css=.mosaic-toolbar

        Clear highlight  css=.mosaic-menu-insert

and drag the appearing tile into the desired position.

..  figure:: _screenshots/mosaic-editor-drag-field-text-tile.png
..  code:: robotframework

    Show how to drag a new tile into its initial position

        Mouse up  css=.mosaic-option-IRichText-text
        Mouse over  css=.mosaic-tile.removable
        Capture and crop page screenshot
        ...  _screenshots/mosaic-editor-drag-field-text-tile.png
        ...  css=html

Finally, a mouse click drops the tile into selected position and the page can be saved.

..  figure:: _screenshots/mosaic-editor-drop-field-text-tile.png
..  code:: robotframework

    Show how to drop a new tile into its initial position

        Click element at coordinates  css=.mosaic-tile.removable  0  25
        Highlight  css=.mosaic-button-save
        Capture and crop page screenshot
        ...  _screenshots/mosaic-editor-drop-field-text-tile.png
        ...  css=html

That's how we can build custom content layouts using Plone Mosaic.

..  figure:: _screenshots/mosaic-page-saved.png
..  code:: robotframework

    Show how the custom layout looks after saving

        Choose ok on next confirmation
        Click button  css=.mosaic-button-save
        Page should contain  Changes saved
        Capture and crop page screenshot
        ...  _screenshots/mosaic-page-saved.png
        ...  css=html
