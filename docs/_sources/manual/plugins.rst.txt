.. _IMASViz_plugins:

Plugins
=======

IMASViz allows the use of custom-made plugins which further extend the IMASViz
functionality. The plugins currently available are listed here together with a
manual section on how to use them.

Equilibrium overview plugin
---------------------------

This subsection describes and demonstrates the use of IMASViz
**equilibrium overview plugin**, a simple utility for loading and displaying
multiple data from **equilibrium IDS** at once.

Executing the equilibrium overview plugin
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The procedure of executing the IMASViz
**Equilibrium overview plugin** is as follows:

1. In opened IMAS database navigate to the **equilibrium** IDS.

2. While holding the **shift keyboard button**, **right-click** on the
   **equilibirum** node. In a pop up menu a list of available plugins for the
   equilibrium IDS will be displayed.

3. Click the :guilabel:`Equilibrium overview` option.

   .. figure:: images/equilibrium_plugin_popupmenu.png
     :align: center
     :scale: 80%
     :alt: Equilibrium overview plugin popup menu option.

     Equilibrium overview plugin popup menu option.

The plugin will then first grab the data from the Equilibrium IDS which
takes a few seconds. After that the plugin window will appear.

   .. figure:: images/equilibrium_plugin_window_start.png
     :align: center
     :scale: 60%
     :alt: Equilibrium overview plugin main window.

     Equilibrium overview plugin main window.

Equilibrium overview plugin GUI features
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Equilibrium plugin allows some interaction with the plots that are described
below.

Plot at time slice using slider
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The plotting time value can be easily set by **holding** and **moving** the
slider. By doing that the **red dashed line** on the left plot will move at the
same time, indicating the selected time value (position).

On **slider release** all plots are updated for the selected time.

Plot at time slide using time value
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The time can be specified directly by typing the value to the **time value box**.

   .. figure:: images/equilibrium_plugin_window_time_value_textbox.png
     :align: center
     :scale: 80%
     :alt: Time value box.

     Time value box.

By pressing :guilabel:`enter` keyboard button, all plots are be updated for the
specified time.

.. note::
   Check the left plot for the time values range.

Run through time slices
^^^^^^^^^^^^^^^^^^^^^^^

This feature allows the plugin to go through all time values and at the same
time update all plots, giving a good overall overview.

This is done by pressing the :guilabel:`Run` button.

Enable plot grid
^^^^^^^^^^^^^^^^

To enable grid on all plots check the :guilabel:`Enable Grid` option.

SOLPS overview plugin
---------------------

This subsection describes and demonstrates the use of IMASViz
**SOLPS overview plugin**, a simple utility for loading and displaying
grid geometry (including grid subsets), and physics quantities (such as
electron density, ion temperature etc.) stored within
**edge_profiles IDS GGD (General Grid Description)**.

.. note::
   The **SOLPS overview plugin** can be run also as standalone
   (outside IMASViz) by navigating to ``$VIZ_HOME/imasviz/VizPlugins/viz_solps``
   and running the command ``python3 SOLPSPlugin.py``.

Executing the SOLPS overview plugin
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The procedure of executing the IMASViz **SOLPS overview plugin** is as follows:

1. In opened IMAS database navigate to the **edge_profiles** IDS.

2. While holding the **shift keyboard button**, **right-click** on the
   **edge_profiles** node. In a pop up menu a list of available plugins for the
   edge_profiles IDS will be displayed.

3. Click the :guilabel:`SOLPS overview` option.

   .. figure:: images/SOLPS_plugin_popupmenu.png
     :align: center
     :scale: 80%
     :alt: SOLPS overview plugin popup menu option

     SOLPS overview plugin popup menu option

   After that the main plugin window will appear, containing an empty plot
   widget and a few buttons.

   .. figure:: images/SOLPS_plugin_window_start.png
     :align: center
     :scale: 60%
     :alt: SOLPS overview plugin main window.

     SOLPS overview plugin main window.

4. Click the :guilabel:`Set IDS` button. The plugin will then first read the
   available data from the Edge Profiles IDS (provided by IMASViz) and build
   the tree view which takes a few seconds.

5. Click the :guilabel:`Set Data` button. After that a dialog window will
   appear, requesting:

   - :guilabel:`GGD Grid (Slice)`, specifying a grid geometry time slice.
     In most cases the grid geometry does not change with time so in such cases
     is obsolete to 're-write' it (that is also the reason why the
     **GGD Grid (grid_ggd)** and **GGD Quantities (ggd)** structures are
     separated).
   - :guilabel:`GGD Quantities (Slice)`, specifying the time slice for physics
     quantities,
   - :guilabel:`Grid Subset`, listing all available 2D grid subsets for specified
     **GGD Grid** and **GGD Quantities** slice.
   - :guilabel:`Grid Subset Quantity`, listing all available quantities for
     grid subset specified by :guilabel:`Grid Subset` drop down list.

   .. figure:: images/SOLPS_plugin_dialog_set_data.png
     :align: center
     :scale: 80%
     :alt: SOLPS overview plugin dialog for setting data (basic example values
           are set).

     SOLPS overview plugin dialog for setting data (basic example values
     are set).

   .. figure:: images/SOLPS_plugin_dialog_list_grid_subset.png
     :align: center
     :scale: 80%
     :alt: List of available (2D) grid subsets for current IDS.

     List of available (2D) grid subsets for current IDS.

   .. figure:: images/SOLPS_plugin_dialog_list_quantities.png
     :align: center
     :scale: 80%
     :alt: List of available physics quantities for current IDS.

     List of available physics quantities for current IDS.

   After the requested parameters are set, press the :guilabel:`OK` button.

6. Click the :guilabel:`Plot Data` button. After pressing the button the plot
   widget will be populated with plot created using the specified data.

   .. figure:: images/SOLPS_plugin_plot_te.png
     :align: center
     :scale: 80%
     :alt: SOLPS overview plugin plot - **Cells** grid subset (all 2D quad
           elements in the domain) + electron temperature quantity values.

     SOLPS overview plugin plot - **Cells** grid subset (all 2D quad
     elements in the domain) + electron temperature quantity values.
