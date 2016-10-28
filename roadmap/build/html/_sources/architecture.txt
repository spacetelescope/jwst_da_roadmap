############
Architecture
############

The :ref:`figure below <fig_architecture>` provides a high-level overview of the
architecture of the data-analysis tools.
The goal is to keep the computational tools lightweight and modular,
and insulated from details of the data structure on disk and the
details of the user interfaces. The tools will primarily work on 
data arrays stored in memory, with parameters passed as arguments. 
There will be a separate layer to read data in from disk and present
the data to the tools via standard data models (which may in some cases 
be `objects <http://docs.python.org/2/tutorial/classes.html#class-objects>`_ 
with `methods <http://docs.python.org/2/tutorial/classes.html#method-objects>`_).
Reading and writing of data is accomplished in a separate layer, so that
data can generally be passed in memory from task to task. This is 
different from IRAF, which tends to work on files, which must be read
and written for each separate operation. Similarly, interaction with 
user-interfaces will be in a separate layer, which can be very lightweight:
all it has to do is gather the parameters from the user-interface,
call the routine with those parameters, and return the results. There 
will also be a separate event-driven interface to the same tools, which
can be triggered by events such as mouse clicks in tools that are
connected via some form of inter-process communication (IPC). To support 
multiple IPC protocols, there will need to be an IPC abstraction layer
that makes these all look the same to the event-driven interface layer.
Sometimes the tools will interact with image displays and graphics 
through IPC channels with two-way communication. Other times, the tools
will send their output without setting up an event-driven interaction.


.. _fig_architecture:
.. figure:: flow_diagrams/strawman_architecture.png
   :alt: Analysis Tools Architecture
   :scale: 100%
   :figwidth: 100%
   :width: 100%

   A schematic view of the architecture of the data analysis tools. 

There are a few things to note about this architecture. 

* It is not compatible with *client-side* computing in a web browser. 

* The software is nevertheless compatible with *server-side* computing,
  where the computations are done either on the user's machine or on a
  remote server but the user interface is a web browser.

* The software is compatible with user interfaces that are not web 
  browsers, which includes other types of graphical-user interfaces, 
  along with editing of human-readable parameter files and invoking 
  tools from a Unix command line or from within python.

* It is a goal to make it straightforward for a user to fire off a wide
  variety of computations via data selections in an image display or plot.
  This is handled by the event-driven interfaces which will then present these selections
  to the tools in the same way that a user would present them if the user 
  had instead defined the selection mathematically in a script. 
  (Indeed, one of the options in this layer should be to save the selections
  so that they can be edited and turned into scripts.)

