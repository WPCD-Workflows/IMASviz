.. include:: ../icons.inc


.. _plotting_1d_arrays:

Plotting 1D arrays
==================

The plotting of 1D arrays option and plot handling is the main feature and
purpose of the :guilabel:`IMASViz`.

This section describes the basics of plotting a 1D array, stored in
the IDS, and how to handle the existing plots.

.. _plotting_single_1d_array:

Plotting a single 1D array to plot figure
-----------------------------------------

The procedure to plot 1D array is as follows:

1. Navigate through the **magnetics IDS** and search for the node containing
   **FLT_1D** data, for example **magnetics.flux_loop[0].flux.data**.
   Plottable FLT_1D nodes are colored **blue** (array length > 0).

   .. figure:: images/DTV_magnetics_IDS_contents_FLT_1D.png
     :align: center
     :scale: 80%
     :alt: FLT_1D plot

     Example of plottable FLT_1D node.

   By clicking on the node the preview plot will be displayed in the
   :guilabel:`Preview Plot`, located in the main window. This
   feature helps to quickly check how the data, stored in the FLT_1D, looks
   when plotted.

   .. figure:: images/DTV_preview_plot.png
     :align: center
     :width: 550px
     :alt:   Preview Plot

     Preview Plot

2. Right-click on the **magnetics.flux_loop[0].flux.data (FLT_1D)** node.

3. From the pop-up menu, select the command
   :guilabel:`Plot ids.magnetics.flux_loop[0].flux.data to` |icon_plotSingle| ->
   :guilabel:`figure` |icon_Figure| -> :guilabel:`New` |icon_new|.

   .. figure:: images/DTV_popupmenu_plotting_single_plot.png
     :align: center
     :scale: 80%

     Navigating through right-click menu to plot data to plot figure.

   The plot should display in plot figure as shown in the image below.

   .. figure:: images/plotWidget_basic.png
     :align: center
     :width: 550px

     Basic plot figure display.


Basic plot display features
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The below features are available for any :guilabel:`plot display`. Most of them
are available in the right-click menu.

.. Note::
   Term :guilabel:`Plot Display` is used for any base subwindow for displaying
   plots. Following that the :guilabel:`Plot Figure` contains a single
   :guilabel:`Plot Display`, while :guilabel:`Table Plot View`
   and :guilabel:`Stacked Plot View` consist of multiple
   :guilabel:`Plot Displays`.

.. figure:: images/plotDisplay_popupmenu.png
  :align: center
  :width: 450px

  Plot display window right-click menu.

.. Disable/Enable Mouse
.. ^^^^^^^^^^^^^^^^^^^^

.. Allows enabling or disabling mouse.
.. This feature can ticked on/off on bottom left corner

View All
^^^^^^^^

Zoom to view whole plot area.

   .. figure:: images/plotDisplay_popupmenu_viewAll.png
     :align: center
     :scale: 75%

     :guilabel:`View All` feature in the right-click menu.

Auto Range
^^^^^^^^^^

Similar to :guilabel:`View All` feature with the difference that it shows
plot area between values ``X_min`` -> ``X_max`` and ``Y_min`` -> ``Y_max``,
without additional "plot margins" on the sides.

   .. figure:: images/plotDisplay_popupmenu_autoRange.png
     :align: center
     :scale: 75%

     :guilabel:`Auto Range` feature in the right-click menu.

Left Mouse Button Mode Change
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Change between :guilabel:`Pan Mode` (move plot around) and
:guilabel:`Area Zoom Mode` (choose selectable area to zoom into).

   .. figure:: images/plotDisplay_popupmenu_mouseMode.png
     :align: center
     :scale: 75%

     :guilabel:`Mouse Mode` feature in the right-click menu.

   .. figure:: images/plotDisplay_area_zoom.png
     :align: center
     :width: 300px

     :guilabel:`Area Zoom` example: Marking zoom area using.


   .. figure:: images/plotDisplay_area_zoom_result.png
     :align: center
     :width: 300px

     :guilabel:`Area Zoom` example: Result.

Axis options
^^^^^^^^^^^^

X and Y axis range, inverse, mouse enable/disable options and more.

   .. figure:: images/plotDisplay_popupmenu_axisOptions.png
     :align: center
     :scale: 75%

     :guilabel:`Axis Options` feature in the right-click menu.

Plot Configuration and Customization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Setting color and line properties of plots shown in the Plot Display.

   .. figure:: images/plotDisplay_popupmenu_configurePlot.png
     :align: center
     :scale: 75%

     :guilabel:`Configure Plot` feature in the right-click menu.

Each plot can be customized. By selecting this feature a separate GUI window
will open, listing all plots within the plot display window and their
properties that can be customized.

   .. figure:: images/plotDisplay_configurePlot_window.png
     :align: center

     :guilabel:`Configure Plot` GUI.

   .. figure:: images/plot_configuration_example.png
     :align: center

     Plot configuration example for single plot.

Plot options
^^^^^^^^^^^^

Enable/Disable grid, log scale and more.

   .. figure:: images/plotDisplay_popupmenu_plotOptions.png
     :align: center
     :scale: 75%

     :guilabel:`Plot Options` feature in the right-click menu.

Export feature
^^^^^^^^^^^^^^

The :guilabel:`Plot Display` scene can be exported to:
   - image file (PNG, JPG, ...). A total of **16** image formats are supported.
   - scalable vector graphics (SVG) file
   - matplotlib window
   - CSV file
   - HDF5 file

   .. figure:: images/plotDisplay_popupmenu_export.png
     :align: center
     :scale: 75%

     :guilabel:`Export` feature in the right-click menu.

   .. figure:: images/plotDisplay_export_window.png
     :align: center
     :scale: 75%

     Export GUI window.

   .. figure:: images/plotDisplay_export_matplotlib.png
     :align: center
     :width: 550px

     Comparison of IMASViz :guilabel:`Plot Figure` and
     :guilabel:`matplotlib window`

.. _add_plot_to_existing_figure:

Adding a plot to existing figure
--------------------------------

The procedure of adding a plot to an already existing figure is as follows:

1. From the previous navigation tree, navigate to the wanted node, for example
   **ids.magnetics.flux_loop[16].flux.data**

2. Right-click on the node.

3. From the pop-up menu, navigate and select
   :guilabel:`Plot <node name> to` |icon_plotSingle| ->
   :guilabel:`Figure` |icon_Figure| -> :guilabel:`Figure:0`

   .. figure:: images/DTV_popupmenu_plot_to_existing_figure.png
     :align: center
     :scale: 75%

     Plotting to existing figure.

The plot will be added to the selected existing plot as shown in the image
below.

   .. figure:: images/DTV_popupmenu_plot_to_existing_figure_result.png
     :align: center
     :width: 550px

     Plotting to existing figure - result.


Comparing plots between two IDS databases
-----------------------------------------

:guilabel:`IMASViz` allows comparing of FLT_1D arrays between two different
IDS databases (different shots too). The procedure is very similar to the one
presented in the section :ref:`add_plot_to_existing_figure`:

1. Open another IMAS database, same as shown in section :ref:`loading_IDS`.
   In this manual this will be demonstrated using IDS with :guilabel:`shot`
   **52682** and :guilabel:`run` **0** parameters.

   +-------------------------+-----+
   | **Manual IDS case**           |
   +--------------------+----------+
   | parameters         | values   |
   +--------------------+----------+
   | User name          | g2penkod |
   +--------------------+----------+
   | IMAS database name | viztest  |
   +--------------------+----------+
   | Shot number        | 52682    |
   +--------------------+----------+
   | Run number         | 0        |
   +--------------------+----------+


2. Load **occurrence 0** of **magnetics** IDS

3. Navigate through the IDS search for the wanted node, for example
   **ids.magnetics.flux_loop[0].flux.data**.

4. Right-click on the node.

5. From the pop-up menu, navigate and select
   :guilabel:`Plot <node name> to` |icon_plotSingle| ->
   :guilabel:`Figure` |icon_Figure| -> :guilabel:`Figure:0`

The plot will be added to the existing plot as shown in the image below.

   .. figure:: images/DTV2_popupmenu_plot_to_existing_figure_result.png
     :align: center
     :width: 550px

     Plotting from other IDS to existing figure - result.

.. _plotting_a_selection_to_figure:

Plotting a selection of 1D arrays to figure
-------------------------------------------

The procedure of 1D arrays selection and plotting to figure is as
follows:

1. In main tree view window set a selection of nodes holding 1D arrays.

   .. note::
      How to create a selection of arrays is described in section
      :ref:`signal_node_selection`.

2. When finished with node selection, either:
   - right-click on any FLT_1D node, or
   - click :guilabel:`Node Selection` menu on menubar of the main tree view
   window.

3. From the pop-up menu, navigate and select
   :guilabel:`Plot selected nodes to` |icon_plotMultiple| ->
   :guilabel:`Figure` |icon_Figure| -> :guilabel:`New` |icon_new|->
   :guilabel:`This IMAS database` |icon_thisDTV|.

   .. Note::
      The same procedure applies plotting the selection to an existing figure.

   .. figure:: images/DTV_popupmenu_plot_selected_nodes_to_figure_thisDTV.png
     :align: center
     :scale: 75%

     Plotting selection to a new figure using selection from the currently
     opened IDS database.

   .. figure:: images/DTV_plot_selected_nodes_to_figure_thisDTV_result.png
     :align: center
     :width: 550px

     Example of plot figure, created by plotting data from node selection.

Plotting 1D array as a function of coordinate1 along the time axis
------------------------------------------------------------------

One of the IMASViz features is plotting coordinate along the time axis.
This is allowed for IDS nodes, located within **time_slice[:]** structure, and
it is already set as a default plotting feature for such arrays.

The procedure to plot such 1D array is quite identical as in section
:ref:`plotting_single_1d_array`. The procedure is described and demonstrated
on **equilibrium.time_slice[0].profiles_1d.phi** (Torodial Flux) array.

1. Navigate through the **equilibrium IDS** and search for the time slice node
   containing
   **FLT_1D** data, for example **equilibrium.time_slice[0].profiles_1d.phi**.

   .. figure:: images/DTV_plot_as_a_function_of_coordinate1.png
     :align: center
     :scale: 80%

     Example of plottable FLT_1D time slice node.

2. Right-click on the **equilibrium.time_slice[0].profiles_1d.phi (FLT_1D)**
   node.

3. From the pop-up menu, select the command
   :guilabel:`Plot equilibrium.time_slice[0].profiles_1d.phi to`
   |icon_plotSingle| -> :guilabel:`figure` |icon_Figure| ->
   :guilabel:`New` |icon_new|.

   .. figure:: images/DTV_popupmenu_plotting_as_a_function_of_coordinate1.png
     :align: center
     :scale: 80%

     Navigating through right-click menu to plot data to plot figure.

   The plot should display in plot figure as shown in the image below.
   Note that **coordinate1 = ids.equilibrium.time_slice[0].profiles_1d.psi**
   for this FLT_1D data array.

   .. figure:: images/plotWidget_function_of_coordinate1.png
     :align: center
     :width: 550px

     Time slice plot figure display. The data are represented as a function of
     coordinate1 (**ids.equilibrium.time_slice[0].profiles_1d.psi**) for the
     first **phi** time slice
     (**ids.equilibrium.time_slice[0].profiles_1d.phi**).

   The time slider allows you to move along the time axis and the plot will
   change accordingly.

   .. figure:: images/plotWidget_function_of_coordinate1_2.png
     :align: center
     :width: 550px

     Time slice plot figure display for
     **ids.equilibrium.time_slice[48].profiles_1d.phi**. The data are
     represented as a function of coordinate1
     (**ids.equilibrium.time_slice[48].profiles_1d.psi**).

.. Note:: Adding a plot (presented in :ref:`add_plot_to_existing_figure`) to
          such existing plot might not work as expected, as the sliding through
          indexes works directly only the last added plot.

Plotting 1D arrays at index as a function of the time
-----------------------------------------------------

One of the IMASViz features is plotting array values at certain index as a
function of time. This is allowed for IDS nodes, located within
**time_slice[:]** structure, and it is already set as a default plotting
feature for such arrays.

The procedure is described and demonstrated
on **equilibrium.time_slice[0].profiles_1d.f** (Torodial Flux) array.

1. Navigate through the **equilibrium IDS** and search for the time slice node
   containing
   **FLT_1D** data, for example **equilibrium.time_slice[0].profiles_1d.f**.

   .. figure:: images/DTV_plot_as_a_function_of_coordinate1.png
     :align: center
     :scale: 80%

     Example of plottable FLT_1D time slice node.

2. Right-click on the **equilibrium.time_slice[0].profiles_1d.f (FLT_1D)**
   node.

3. From the pop-up menu, select the command
   :guilabel:`Plot equilibrium.time_slice[0].profiles_1d.f to`
   |icon_plotSingle| -> :guilabel:`figure` |icon_Figure| ->
   :guilabel:`New` |icon_new|.

   .. figure:: images/DTV_popupmenu_plotting_as_a_function_of_time.png
     :align: center
     :scale: 80%

     Navigating through right-click menu to plot data to plot figure.

   The plot should display in plot figure as shown in the image below.
   Note that Y-axis values are an array of
   **equilibrium.time_slice[:].profiles_1d.f[0]** values through all time slices
   (marked by **[:]**) and X-axis values are time values found in
   **equilibrium.time**.

   .. figure:: images/plotWidget_function_of_time_1.png
     :align: center
     :width: 550px

     Plot as a function of time. Y-axis values are an array of
     **equilibrium.time_slice[:].profiles_1d.f[0]** values through all time
     slices (marked by **[:]**) and X-axis values are time values found in
     **equilibrium.time**.

   The time slider allows you to move along the array index and the plot will
   change accordingly.

   .. figure:: images/plotWidget_function_of_time_2.png
     :align: center
     :width: 550px

     Plot as a function of time. Y-axis values are an array of
     **equilibrium.time_slice[:].profiles_1d.f[23]** values through all time
     slices (marked by **[:]**) and X-axis values are time values found in
     **equilibrium.time** node (array of FLT_1D values).

.. Note:: Adding a plot (presented in :ref:`add_plot_to_existing_figure`) to
          such existing plot might not work as expected, as the sliding through
          indexes works directly only the last added plot.

.. _multiplot_features:

MultiPlot features
------------------

IMASViz provides few features that allow plotting a selection of plottable
arrays to a single plot view window.

Currently there are two such features available:
   - :guilabel:`Table Plot View` and
   - :guilabel:`Stacked Plot View`.

Each of those Plot Views feature its own plot display layout and plot display
window interaction features.

.. Note::
   In the old IMASViz, the :guilabel:`Table Plot View` is known as
   :guilabel:`MultiPlot` and the :guilabel:`Stacked Plot View` is known as
   :guilabel:`SubPlot`. The decision to rename those features was made due to
   the previous names not properly describing the feature itself and both of
   those features being a form of 'MultiPlot' - a window consisting of multiple
   plot displays.

.. _TPV:

Table Plot View
~~~~~~~~~~~~~~~

:guilabel:`Table Plot View` allows the user to create a multiplot window by
plotting every array from selection to its own plot display.
The plot displays are arranged to resemble a table layout, as shown in figure
below.

.. figure:: images/TablePlotView_example.png
    :align: center
    :scale: 50%

    MultiPlot - :guilabel:`Table Plot View` Example.

Creating New View
^^^^^^^^^^^^^^^^^

To create a new :guilabel:`Table Plot View`, follow the next steps:

1. Create a selection of nodes, as described in section
   :ref:`plotting_a_selection_to_figure`.

2. When finished with node selection, either:
      - right-click on any **FLT_1D** node or
      - click :guilabel:`Node Selection` menu on menubar of the main tree view
        window.

3. From the pop-up menu, navigate and select
   :guilabel:`Plot selected nodes to` |icon_plotMultiple| ->
   :guilabel:`TablePlotView` |icon_TablePlotView| -> :guilabel:`New` |icon_new|->
   :guilabel:`This IMAS database` |icon_thisDTV| or
   :guilabel:`All IMAS databases` |icon_allDTV| or.

   .. figure:: images/DTV_plot_selected_nodes_to_TPV_thisDTV.png
     :align: center
     :scale: 75%

     Plotting selection to a new figure using selection from the currently
     opened IDS database.

   The :guilabel:`Table Plot View` window will then be created and shown.

.. note::
   Each plot can be customized individually by right-clicking to the plot d
   display and selecting option :guilabel:`Configure Plot`.

.. note::
   Scrolling down the :guilabel:`Table Plot View` window using the middle mouse
   button is disabled as the same button is used to interact with the plot
   display (zoom in and out). Scrolling can be done by clicking the scroll bar
   on the right and dragging it up and down.

.. _save_multiplot_configuration:

Save MultiPlot Configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

MultiPlot configuration (currently available only for
:guilabel:`Table Plot View` feature) allows the user to save the MultiPlot
session and load it later.

To create MultiPlot configuration, follow the next steps:

1. Create a selection of nodes, as described in section
   :ref:`plotting_a_selection_to_figure`.

2. Create a :guilabel:`Table Plot View`, as described in :ref:`TPV`.

3. In :guilabel:`Table Plot View` menubar navigate to **Options** ->
   **Save Plot Configuration**

   .. figure:: images/SavePlotConfiguration_dialog.png
     :align: center
     :scale: 75%

     Save Plot Configuration Dialog Window.

4. Type configuration name in the text area.

5. Press **OK**.

.. Note::
   The configurations files are saved to :file:`$HOME/.imasviz` folder.

.. _apply_multiplot_configuration:

Applying MultiPlot configuration to other IMAS database
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To apply MultiPlot configuration to any IMAS database, follow the next steps:

1. Open IMAS database.

   .. See also::
      How to open IMAS database is described in :ref:`loading_IDS`.

2. In Main Tree View Window menu navigate to **Actions** ->
   **Apply Configuration**. In the shown window switch to
   :guilabel:`Apply Plot Configuration` tab.

   .. figure:: images/ApplyPlotConfiguration_window.png
     :align: center
     :scale: 75%

     Apply Plot Configuration GUI Window.

3. Select the configuration from the list.

4. Press **Apply selection and plot selected data**.

   The :guilabel:`Table Plot View` will be created using the data stored in the
   configuration file.

.. Note::
   Currently this feature will take all plot data from single (currently)
   opened IMAS database, event though MultiPlot configuration was made using
   plots from multiple IMAS databases at once. This feature is to be improved
   in the future.

.. warning::
   The plots order depends on the order in which the data selection has
   been performed. First selected data will be the first plots in the
   :guilabel:`Table Plot View` window.

Stacked Plot View
~~~~~~~~~~~~~~~~~

:guilabel:`Stacked Plot View` allows the user to create a multiplot window by
plotting every array from selection to its own plot display.
The plot displays are arranged to resemble a stack layout, as shown in figure
below. All plots displays always share the same X and Y range, even when
using plot interaction features such as :guilabel:`Zoom in/out`,
:guilabel:`Pan Mode`, :guilabel:`Area Zoom Mode` etc.

.. figure:: images/StackedPlotView_example.png
    :align: center
    :scale: 50%

    MultiPlot - :guilabel:`Stacked Plot View` Example.

Creating New View
^^^^^^^^^^^^^^^^^

To create a new :guilabel:`Stacked Plot View`, follow the next steps:

1. Create a selection of nodes, as described in section
   :ref:`plotting_a_selection_to_figure`.

2. When finished with node selection, either:
      - right-click on any FLT_1D node or
      - click :guilabel:`Node Selection` menu on menubar of the main tree view
        window.

3. From the pop-up menu, navigate and select
   :guilabel:`Plot selected nodes to` |icon_plotMultiple| ->
   :guilabel:`StackedPlotView` |icon_StackedPlotView| ->
   :guilabel:`New` |icon_new|->
   :guilabel:`This IMAS database` |icon_thisDTV| or
   :guilabel:`All IMAS databases` |icon_allDTV| or.

   .. figure:: images/DTV_plot_selected_nodes_to_SPV_thisDTV.png
     :align: center
     :scale: 75%

     Plotting selection to a new figure using selection from the currently
     opened IDS database.

   The :guilabel:`Stacked Plot View` window will then be shown.

.. note::
   Each plot can be customized individually; right click on a node and
   select 'Configure Plot'.

