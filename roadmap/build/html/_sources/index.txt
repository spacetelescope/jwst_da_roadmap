.. JWST Astronomy Data Analysis Tools Roadmap documentation master file, created by
   sphinx-quickstart on Mon Jun 24 17:48:14 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

JWST Astronomy Data Analysis Tools Roadmap
==========================================

:abstract:
   :author: Henry Ferguson, Perry Greenfield, and Alberto Conti
   :date: 30 June 2013

   The purpose of this document is to provide a roadmap for developing
   the software tools that astronomers need for going from pipeline-reduced
   data to scientific publications. The document looks at the process of
   analyzing data for several different science cases, and looks at existing
   tools in IRAF to identify the highest priorities for new tool development
   or for porting of existing algorithms to a modern python environment. 
   For the most part, the numerical computations will be coded in Python and 
   incorporated into astropy. Many of the tools will have broad applicability
   beyond JWST, so it makes sense for the code to be open source and the
   development to be a broad community effort. This roadmap, while geared toward
   JWST, is intended to foster a community dialog for the evolution of 
   these software tools.

Contents:

.. toctree::
   :maxdepth: 2
   :numbered:

   exec_summary
   intro
   use_cases
   infrastructure
   architecture
   comp_toolbox
   graphics
   timeline
   


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

