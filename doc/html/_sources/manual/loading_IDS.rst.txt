.. |icon_arrowIDSroot| image:: images/DTV_IDS_root_arrow.png
   :scale: 75%

.. |button_undock| image:: images/button_undock.png

.. _loading_IDS:

Loading IDS from IMAS local data source
=======================================

This section describes and demonstrates how to load the **IMAS IDS case**
within **IMASViz** and open one of the **IDS nodes**.

.. Note:: The procedure below is executed on the **GateWay HPC** and thus the
          **IMAS IDS cases** available on the GateWay are used.

Loading IMAS IDS
----------------

The procedure to load the IDS is as follows:

- In the main :guilabel:`IMASViz GUI`, select the first
  tab - :guilabel:`Local data source`.
- Enter the following parameters, listed below, to the appropriate text fields.

+-------------------------------+
| **IMAS IDS case**             |
+--------------------+----------+
| Parameters         | Values   |
+====================+==========+
| User name          | g2penkod |
+--------------------+----------+
| IMAS database name | viztest  |
+--------------------+----------+
| Shot number        | 52344    |
+--------------------+----------+
| Run number         | 0        |
+--------------------+----------+

By default, the data source is a pulse file located in
:file:`$HOME/public/imasdb/<IMAS database name>/3/0/` directory. In this
case, the :file:`~public/imasdb/viztest/3/0/` directory of user ``g2penkod``.

The filled GUI should then look as shown in the next figure:

.. image:: images/startup_window_filled.png
   :align: center
   :scale: 80%

Open IDS
--------

The procedure to open any IDS is the same. In this manual,
the procedure will be shown on **magnetics IDS**.


1. Click :guilabel:`Open` button to open the IDS.

   A navigation tree window will open, as shown in the figure below.

   .. image:: images/DTVFrame_empty.png
      :align: center
      :width: 550px

2. Press the **arrow button** |icon_arrowIDSroot|  on the left side of the
   **IDS root node**.

   This will expand the navigation :guilabel:`tree window` and display a
   list of all IDSs.
   The tree will allow browsing data for the specific shot number which is
   displayed by the root node ( ``IDSs(52344)`` ).

   .. image:: images/DTV_IDS_root_open.png
      :align: center
      :width: 550px

   When IDS or node label is selected the :guilabel:`Node documentation`
   widget will display the basic information (name and documentation) of
   the node, as shown below.

   .. image:: images/DTVFrame_node_doc.png
      :align: center
      :width: 550px

   The :guilabel:`Node Documentation` widget can be freely taken out from the
   main window by clicking :guilabel:`undock` button |button_undock| the and positioned anywhere on the screen. The same thing goes for the
   :guilabel:`Preview Plot` and :guilabel:`Log` widget.

   .. image:: images/DTVFrame_undock_example.png
      :align: center
      :width: 550px

3. Open **magnetics IDS** by right-clicking on the **magnetics** node
   and selecting the command :guilabel:`Get magnetics data` (occurrence 0)
   as shown in the figure below.

   .. image:: images/DTV_open_magnetics_IDS.png
      :align: center
      :width: 400px

   .. Note:: Alternative: Double-clicking on the **IDS node label** ->
             **occurrence 0** (default) of the selected IDS will load.

   The magnetics IDS nodes are displayed as new nodes in the tree, as shown in
   the figure below. Nodes of an IDS are organized according to the
   **IMAS data dictionary**. Inside the **magnetics** tree, plottable
   **FLT_1D** nodes are colored blue (array length > 0).

    .. image:: images/DTV_magnetics_IDS_contents_FLT_1D.png
      :align: center
      :scale: 80%

