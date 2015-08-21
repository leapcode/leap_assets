Icons and Images for LEAP
====================================

Here lies all the assets for the various LEAP clients. All sources are in SVG format, and rendered to PNG.

All icons derived from either Gnome (GPL) or Font Awesome (Open Font License, GPL compatible).

Generating PNG images
======================================

Requirements:

    apt-get install ruby inkscape optipng icnsutils graphicsmagick

Usage:

    cd leap_assets
    rake

Android Icons
=====================================

Android Launcher Icons
-------------------------------------

See https://developer.android.com/guide/practices/ui_guidelines/icon_design_launcher.html

Pixels height and width for launcher icons:

36x36   -- ldpi, 120 dpi, Low density screen
48x48   -- hdpi, 160 dpi, Medium density screen
72x72   -- mdpi, 240 dpi, High density screen
96x96   -- xhdpi, 320 dpi, Extra-high density screen
512x512 -- displayed in Google Play website.

Links:

* Android Launcher Icon Guidelines -- https://developer.android.com/guide/practices/ui_guidelines/icon_design_launcher.html

Android Action Bar Icons
-----------------------------------

See https://developer.android.com/guide/practices/ui_guidelines/icon_design_action_bar.html

18x18   -- ldpi, 120 dpi, Low density screen
24x24   -- hdpi, 160 dpi, Medium density screen
36x36   -- mdpi, 240 dpi, High density screen
48x48   -- xhdpi, 320 dpi, Extra-high density screen

Mac OS Icons
====================================

NSStatusItem Icons
------------------------------------

See: http://alastairs-place.net/blog/2013/07/23/nsstatusitem-what-size-should-your-icon-be/

Normal displays:
  21px height, but should include 3px space at the top and bottom of the icon
  in order to look in line with the system icons.
  The icon can be up to 22px wide (no space is needed).

Retina displays:
  41px height, but should include a 6px space at the top and a 5px space at
  the bottom in order to look in line with the system icons.
  The icon can be up to 44px wide (no space is needed).

The empty space on the top and the bottom is not required, and is flexible. It
still looks fine if a few pixels poke into this space, and even some system
icons do this.

Unlike in the past, the Mac menu bar can now be light or dark colored, so the
icon should look good on both backgrounds.

Application Icons
------------------------------------

App icons must be 1024×1024 for the app to be accepted in the Mac App Store.

Resources
=====================

Android Design Downloads
http://developer.android.com/design/downloads/index.html

Launcher Icon Generator
https://android-ui-utils.googlecode.com/hg/asset-studio/dist/icons-launcher.html

Nice Symbolic Icons

  * /usr/share/icons/gnome/scalable/**-symbolic.svg
  * http://fortawesome.github.io/Font-Awesome/

