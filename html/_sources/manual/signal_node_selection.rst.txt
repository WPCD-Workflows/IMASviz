.. include:: ../icons.inc


.. _signal_node_selection:

Node selection features
=======================

:guilabel:`IMASViz` offers the user the ability to set or mark a
selection of plottable arrays (nodes) as once. This way plotting multiple
plots to the same :guilabel:`Figure` or to a :guilabel:`MultiPlot View`
is more convenient and faster, avoiding "one-by-one" plotting.

.. Note::
   How to plot selection is described later in section
   :ref:`plotting_1d_arrays`.

In the continuation of this section different methods of node selection are
described.

Select One-by-one
-----------------

To select nodes one by one, first, right-click on the wanted node.
From the shown pop-up menu, select the command
:guilabel:`Select <node name>` |icon_plus|.

.. figure:: images/DTV_popupmenu_select.png
   :align: center
   :scale: 75%

   Selecting a plottable node.

The selected node label gets colored red, indicating that it is added
to the selection.

.. figure:: images/DTV_node_red.png
   :align: center
   :scale: 75%

   Node colored red -> node is selected.

Repeat that procedure until all wanted nodes are selected.

.. figure:: images/DTV_node_selection.png
   :align: center
   :scale: 75%

   Example of multiple nodes selection.

.. Note::
   At the same time, nodes from other opened IDS databases too can be
   selected.

Select All Nodes of the same Structure (AOS)
--------------------------------------------

To select all nodes of the same structure (same node structure type),
right-click on one of the nodes and from the shown popup-menu select the option
:guilabel:`Select All Nodes From The Same AOS` |icon_plus3x|.

.. figure:: images/DTV_popupmenu_select_AOS.png
   :align: center
   :scale: 75%

   Selecting plottable nodes of the same structure/type.

All nodes of the same structure will be selected and their label will be
colored to red, indicating that they were added to the selection.

.. figure:: images/DTV_node_selection_AOS.png
   :align: center
   :scale: 75%

   Node colored red -> node is selected. All plottable nodes of the
   same structure/type are selected, in this case, 17 nodes.

Save Node Selection Configuration
---------------------------------

Any node selection can be saved to a configuration file and used later with any
opened IMAS database. To save a selection, follow the next steps:

1. In the main tree browser menu navigate to **Node Selection** ->
   **Save Node Selection**.

2. In opened GUI window type the name of the configuration.

   .. figure:: images/SavePlotSelection_dialog.png
      :align: center
      :scale: 75%

      Save Node Selection Dialog.

3. Press **OK** button.

.. Note::
   The configurations are saved to ``$HOME/.imasviz`` folder.

.. _apply_selection_from_configuration:

Apply Selection From Saved Configuration
----------------------------------------

Applying saved node selection can be performed using both
:guilabel:`Node Selection Configuration` and
:guilabel:`MultiPlot Configuration`.

Apply Selection From Saved Node Selection Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To apply selection from :guilabel:`Node Selection Configuration`, follow the
next steps:

1. In the main tree browser menu navigate to **Actions** ->
   **Apply Configuration**. In the shown window switch to
   :guilabel:`Available Node Selection Configurations` tab.

   .. figure:: images/ApplyPlotConfiguration_NodeSelection.png
     :align: center
     :scale: 75%

     :guilabel:`Apply Node Selection Configuration` tab.

2. Select the configuration from the list.

3. Press **Apply selection only**.

   The signal nodes, found in the configuration file, will then be selected.

Apply Selection From MultiPlot Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To apply selection from :guilabel:`MultiPlot Configuration`, follow the
next steps:

.. seealso::
   How the create MultiPlot Configuration is described in
   :ref:`TPV`.

1. In Main Tree View Window menu navigate to **Actions** ->
   **Apply Configuration**. In the shown window switch to
   :guilabel:`Apply Plot Configuration` tab.

   .. figure:: images/ApplyPlotConfiguration_window.png
     :align: center
     :scale: 75%

     :guilabel:`Apply Plot Configuration` tab.

2. Select the configuration from the list.

3. Press **Apply selection only**.

   The signal nodes, found in the configuration file, will then be selected.

Unselect selected Node Signals
------------------------------

There are few features that allow node signal unselection.

Unselect One-by-one
~~~~~~~~~~~~~~~~~~~

To unselect nodes one by one, first, right-click on the selected node.
From the shown pop-up menu, select the command
:guilabel:`Unselect <node name>` |icon_minus|.

.. figure:: images/DTV_popupmenu_unselect.png
   :align: center
   :scale: 75%

   Unselecting plottable node.

Unselect All
~~~~~~~~~~~~

To unselect all selected nodes, first, right-click on the selected node.
From the shown pop-up menu, select the command
:guilabel:`Unselect Nodes` |icon_minus3x| ->
:guilabel:`This IMAS Database` |icon_thisDTV| or
:guilabel:`All IMAS Databases` |icon_allDTV|.

.. figure:: images/DTV_popupmenu_unselect_all.png
   :align: center
   :scale: 75%

   Unselecting multiple plottable nodes at once.














