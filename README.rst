Django LESS
===================

Django LESS provides template tags to compile LESS into CSS from templates.
It works with both inline code and extenal files.

Installation
************

1. Add ``"less"`` to ``INSTALLED_APPS`` setting.
2. Make sure that you have ``lessc`` executable installed. See
   `LESS official site <http://lesscss.org>`_ for details.
3. Optionally, you can specify the full path to ``lessc`` executable with ``LESS_EXECUTABLE`` setting.
   By default it's set to ``lessc``.

Example Usage
*************

Inline
------

::

    {% load less %}
    
    <style>
      {% inlineless %}
        #header {
          h1 {
            font-size: 26px;
            font-weight: bold;
          }
          p { font-size: 12px;
            a { text-decoration: none;
              &:hover { border-width: 1px }
            }
          }
        }
      {% endless %}
    </style>

renders to

:: 

      <style>
        #header h1 {
          font-size: 26px;
          font-weight: bold;
        }
        #header p {
          font-size: 12px;
        }
        #header p a {
          text-decoration: none;
        }
        #header p a:hover {
          border-width: 1px;
        }
      </style>


External file
-------------

::

    {% load less %}
    
    <link rel="stylesheet" href="{{ STATIC_URL}}{% less "path/to/styles.less" %}" />
    
renders to

::

    <link rel="stylesheet" href="/media/LESS_CACHE/path/to/styles-91ce1f66f583.css" />

Note that by default compiled files are saved into ``LESS_CACHE`` folder under your ``STATIC_ROOT`` (or ``MEDIA_ROOT`` if you have no ``STATIC_ROOT`` in your settings).
You can change this folder name with ``LESS_OUTPUT_DIR`` setting.

Note that all relative URLs in your stylesheet are converted to absolute URLs using your ``STATIC_URL`` setting.


Settings
********

``LESS_EXECUTABLE``
    Path to LESS compiler executable. Default: ``"lessc"``.

``LESS_OUTPUT_DIR``
    Output directory for compiled external stylesheets. It's relative to ``STATIC_ROOT``. Default: ``"LESS_CACHE"``.
    
``LESS_USE_CACHE``
    Whether to use cache for inline styles. Default: ``True``.
    
``LESS_CACHE_TIMEOUT``
    Cache timeout for inline styles (in seconds). Default: 30 days.
    
``LESS_MTIME_DELAY``
    Cache timeout for reading the modification time of external stylesheets (in seconds). Default: 10 seconds.

``LESS_INCLUDE_APP_STATIC_DIRS``
    Include the static dirs from all of the apps in INSTALLED_APPS in the lessc import search path. Default: ``False``
