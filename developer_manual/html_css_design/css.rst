.. sectionauthor:: John Molakvoæ <skjnldsv@protonmail.com>
.. codeauthor:: John Molakvoæ <skjnldsv@protonmail.com>
..  _css:

===
CSS
===

While the recommended way to develop the user interface is using Vue with the Nextcloud provided components,
Nextcloud also provides CSS variables and classes to style components to have a consistent look.

..  _cssvars:


CSS variables
=============

It is strongly recommended to use the Nextcloud provided CSS variables for styling components.
This way you can be sure that the theming and accessibility app can dynamically adjust the values.
Doing so also allows users to enforce dark or light theme, high contrast or other theming related options.


Primary color variables
-----------------------

The primary color is the main accent color that can be configured by the administrator or the user (if user theming is enabled).

+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| Variable                                     | Example          | Usage                                                                                     |
+==============================================+==================+===========================================================================================+
| ``--color-primary``                          | ``#00679e``      | Primary color configured by user                                                          |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-text``                     | ``#ffffff``      | Text color to be used on ``--color-primary``                                              |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-hover``                    | ``#3285b1``      | Variant of ``--color-primary`` for hover effects                                          |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-light``                    | ``#e5eff5``      | Light variant of ``--color-primary`` used for secondary actions                           |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-light-text``               | ``#00293f``      | Text color to be used on ``--color-primary-light``                                        |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-light-hover``              | ``#dbe4ea``      | Variant of ``--color-primary-light`` for hover effects                                    |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-element``                  | ``#00679e``      | Accessibility adjusted variant of ``--color-primary`` to be used on interactive elements  |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-element-text``             | ``#ffffff``      | Text color to be used on ``--color-primary-element``                                      |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-element-text-dark``        | ``#f5f5f5``      | Less contrast version of ``--color-primary-element-text``                                 |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-element-hover``            | ``#005a8a``      | Variant of ``--color-primary-element`` for hover effects                                  |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-element-light``            | ``#e5eff5``      | Light variant of ``--color-primary-element`` used for secondary actions                   |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-element-light-text``       | ``#00293f``      | Text color to be used on ``--color-primary-element-light``                                |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--color-primary-element-light-hover``      | ``#dbe4ea``      | Variant of ``--color-primary-element-light`` for hover effects                            |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--primary-invert-if-bright``               | ``invert(100%)`` | Filter to invert icons placed on primary color backgrounds if the primary color is bright |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+
| ``--primary-invert-if-dark``                 | ``invert(100%)`` | Filter to invert icons placed on primary color backgrounds if the primary color is dark   |
+----------------------------------------------+------------------+-------------------------------------------------------------------------------------------+

Background variables
--------------------

+----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------+
| Variable                                     | Example               | Usage                                                                               |
+==============================================+=======================+=====================================================================================+
| ``--color-background-plain``                 | ``#00679e``           | The background color of the ``body`` element                                        |
+----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------+
| ``--color-background-plain-text``            | ``#ffffff``           | Text color to be used directly on the background (e.g. header menu)                 |
+----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------+
| ``--image-background``                       | ``url('clouds.jpg')`` | Background image used on the ``body`` element (optional)                            |
+----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------+
| ``--background-image-invert-if-bright``      | ``invert(100%)``      | Filter to invert (bright) icons placed directly on the body background (app menu)   |
+----------------------------------------------+-----------------------+-------------------------------------------------------------------------------------+

General color variables
-----------------------

+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| Variable                                     | Example                                        | Usage                                                                                                     |
+==============================================+================================================+===========================================================================================================+
| ``--color-main-text``                        | ``#222222``                                    | Main text color                                                                                           |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-text-maxcontrast``                 | ``#6b6b6b``                                    | Brighter text color that still fulfills accessibility requirements                                        |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-text-maxcontrast-background-blur`` | ``#595959``                                    | ``--color-text-maxcontrast`` for blurred background, see ``--color-main-background-blur``                 |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-main-background``                  | ``#fffff``                                     | Main background color                                                                                     |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-main-background-rgb``              | ``255,255,255``                                | RGB variant of ``--color-main-background``                                                                |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-main-background-blur``             | ``rgba(var(--color-main-background-rgb), .8)`` | Background color used for blurred background, see ``--filter-background-blur``                            |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-background-hover``                 | ``#f5f5f5``                                    | Background color for hover effects                                                                        |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-background-dark``                  | ``#ededed``                                    | Can be used e.g. to colorize selected table rows                                                          |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-background-darker``                | ``#dbdbdb``                                    | Should only be used for elements, not as a text background! Otherwise it will not work for accessibility. |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-border``                           | ``#ededed``                                    | Default element border color                                                                              |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-border-dark``                      | ``#dbdbdb``                                    | Darker version of the border color                                                                        |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-border-maxcontrast``               | ``#7d7d7d``                                    | Brightest possible border color for accessibility                                                         |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-placeholder-light``                | ``#e6e6e6``                                    | Color for input placeholders                                                                              |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-placeholder-dark``                 | ``#cccccc``                                    | Darker version of ``--color-placeholder-light``                                                           |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-scrollbar``                        | ``#33333344``                                  | Color for scrollbars                                                                                      |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-loading-light``                    | ``#cccccc``                                    | Color for the loading spinner (the bright part of it)                                                     |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-loading-dark``                     | ``#444444``                                    | Color for the loading spinner (the dark part of it)                                                       |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-box-shadow-rgb``                   | ``77,77,77``                                   | Color for box shadow effects in RGB notation                                                              |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--color-box-shadow``                       | ``rgba(var(--color-box-shadow-rgb), 0.5)``     | Color for box shadow effects                                                                              |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--background-invert-if-dark``              | ``invert(100%)``                               | Filter to invert (dark) icons (e.g. set if the color theme is dark)                                       |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--background-invert-if-bright``            | ``invert(100%)``                               | Filter to invert (bright) icons (e.g. set if the color theme is bright)                                   |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+

State colors variables
----------------------

+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| Variable                                     | Example         | Usage                                                                                     |
+==============================================+=================+===========================================================================================+
| ``--color-favorite``                         | ``#a37200``     | Color to mark favorites, can be used to color e.g. a star icon for favorites              |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-error``                            | ``#db0606``     | Element color to show error state, this should not be used for text                       |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-error-rgb``                        | ``219,6,6``     | RGB variant of ``--color-error``                                                          |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-error-hover``                      | ``#df2525``     | Element color for hover effects of ``--color-error``                                      |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-error-text``                       | ``#c20505``     | Text color to show error state                                                            |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-warning``                          | ``#a37200``     | Element color to show warning state, this should not be used for text                     |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-warning-rgb``                      | ``163,114,0``   | RGB variant of ``--color-warning``                                                        |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-warning-hover``                    | ``#8a6000``     | Element color for hover effects of ``--color-warning``                                    |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-warning-text``                     | ``#7f5900``     | Text color to show warning state                                                          |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-success``                          | ``#2d7b41``     | Element color to show success state, this should not be used for text                     |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-success-rgb``                      | ``45,123,65``   | RGB variant of ``--color-success``                                                        |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-success-hover``                    | ``#428854``     | Element color for hover effects of ``--color-success``                                    |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-success-text``                     | ``#286c39``     | Text color to show success state                                                          |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-info``                             | ``#0071ad``     | Element color to show info state, this should not be used for text                        |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-info-rgb``                         | ``0,113,173``   | RGB variant of ``--color-info``                                                           |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-info-hover``                       | ``#197fb5``     | Element color for hover effects of ``--color-info``                                       |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+
| ``--color-info-text``                        | ``#006499``     | Text color to show info state                                                             |
+----------------------------------------------+-----------------+-------------------------------------------------------------------------------------------+

Element structure variables
---------------------------

+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| Variable                                     | Example                                        | Usage                                                                                                     |
+==============================================+================================================+===========================================================================================================+
| ``--animation-quick``                        | ``100ms``                                      | Animation time for snappy CSS transitions                                                                 |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--animation-slow``                         | ``300ms``                                      | Animation time for more complex transitions                                                               |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--breakpoint-mobile``                      | ``1024px``                                     | Breakpoint for mobile responsive layout (e.g. if app navigation should be always visible)                 |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--filter-background-blur``                 | ``blur(25px)``                                 | Filter to be used on elements with background blur (e.g. app navigation)                                  |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--font-face``                              | ``system-ui, 'Segoe UI', Roboto, Oxygen-Sans`` | Font used for Nextcloud user interface                                                                    |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--default-font-size``                      | ``15px``                                       | Font size for normal text                                                                                 |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--default-line-height``                    | ``1.5``                                        | Line height for normal text                                                                               |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--default-grid-baseline``                  | ``4px``                                        | Foundation of all spacing sizes used on Nextcloud which are multiples of the baseline size                |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--border-width-input``                     | ``1px``                                        | Border width for interactive elements such as text fields and selects                                     |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--border-width-input-focused``             | ``2px``                                        | Border width for interactive elements when focussed (adjusted for accessibility)                          |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--border-radius-small``                    | ``4px``                                        | Border radius used for smaller elements                                                                   |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--border-radius-element``                  | ``8px``                                        | Border radius of interactive elements such as buttons, input, navigation and list items.                  |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--border-radius-container``                | ``12px``                                       | For smaller containers like action menus.                                                                 |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--border-radius-container-large``          | ``16px``                                       | For larger containers like body or modals.                                                                |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--default-clickable-area``                 | ``34px``                                       | Default size (width and height) for interactive elements like buttons                                     |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--clickable-area-large``                   | ``48px``                                       | Larger size for the main UI elements                                                                      |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--clickable-area-small``                   | ``24px``                                       | Smallest possible size of interactive elements, used by tertiary actions like filter chips                |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--body-container-radius``                  | ``calc(var(--default-grid-baseline) * 3)``     | Border radius of the body container                                                                       |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--body-container-margin``                  | ``calc(var(--default-grid-baseline) * 2)``     | Margin of the body container                                                                              |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--header-height``                          | ``50px``                                       | Height of the main app navigation bar                                                                     |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--navigation-width``                       | ``300px``                                      | Width of the in-app navigation sidebar                                                                    |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--sidebar-min-width``                      | ``200px``                                      | Minimum width of the app sidebar on small screens                                                         |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+
| ``--sidebar-max-width``                      | ``500px``                                      | Maximum width of the app sidebar on wide screens                                                          |
+----------------------------------------------+------------------------------------------------+-----------------------------------------------------------------------------------------------------------+

..  _cssclasses:

CSS classes
===========

There are some predefined classes for public use to ease developing an application for Nextcloud.

+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| CSS class                                    | Usage                                                                                                                                          |
+==============================================+================================================================================================================================================+
| ``.hidden-visually``                         | Hides an element visually from the page but keeps it in the accessibility tree                                                                 |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| ``.hidden``                                  | Hides an element completely from the page (also removed from the accessibility tree)                                                           |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| ``.bold``                                    | Make content of the element bold emphasize                                                                                                     |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| ``.center``                                  | Center align the elements text                                                                                                                 |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| ``.clear-left``                              | Clear float left                                                                                                                               |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| ``.clear-right``                             | Clear float right                                                                                                                              |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| ``.clear-both``                              | Clear float on both                                                                                                                            |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| ``.pull-left``                               | Float left                                                                                                                                     |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| ``.pull-right``                              | Float right                                                                                                                                    |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
| ``.inlineblock``                             | Make an element an inline block                                                                                                                |
+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+
