======================
Firefox Configurations
======================

.. role:: js(code)
  :language: js
  
These are some common useful configs that can be set in Firefox's ``about:config``.

.. list-table::
  :widths: auto
  :header-rows: 1

  * - Name
    - Description
  * - :js:`browser.urlbar.clickSelectsAll`
    - When true, single-clicking on the URL bar selects all text instead of placing cursor
  * - :js:`browser.urlbar.doubleClickSelectsAll`
    - When false, double-clicking on the URL bar selects a single word below the cursor instead of all text
  * - :js:`browser.urlbar.suggest.{bookmark,history,openpage,searches}`
    - When true, bookmarked sites/sites you've visited/open pages/past searches will show up as you type in the URL bar
  * - :js:`browser.urlbar.ctrlCanonizesURLs`
    - When false, URLs are copied in percent-encoding
  * - :js:`middlemouse.paste`
    - When false, on systems where the middle button pastes (e.g. most Linux), clicking the middle button will not paste within Firefox.
