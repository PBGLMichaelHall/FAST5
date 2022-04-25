.. FAST5 documentation master file, created by
   sphinx-quickstart on Mon Apr 25 13:19:00 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

======================================================
Processing and Analyzing FAST5 files from a minION Run
======================================================

:Author: Michael Hall
:Date:   4/25/2022

FAST5 Files
===========

   FAST5 files are raw data produced from a MinION Run. 
   In this tutorial I show two major data processing steps after DNA has been extracted from a model organism. 
   An experiment was done on a Coffee Plant in this case and two fastq files were produced, one failed, the other succeeded.
   First I would like to determine the quality of the data using Qualimap.
   Second I would like to run FAST5 files through an R Script pipleine that also assess its quality. 



-------------------------------------------------------------------------------------------------------------------------



Installation
------------

   It is necessary to clone a github repository to do the analysis.
   First clone https://github.com/PBGLMichaelHall/FAST5.git.
  
Data Tree
~~~~~~~~~

.. code:: r 

.. figure:: ../images/1.png

::


===========================
Failed Minion Run QC Report
===========================

.. code:: r

   setwd("~/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91/fast5/guppy_out")
   library(fastqcr)
   fastqc_install()
   fastqc(fq.dir = "/home/michael/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91/fast5/guppy_out/fail",threads = 4)

.. figure:: ../images/2.png

::
   
FASTQC Directory and HTML OUTPUT
--------------------------------


Summary
~~~~~~~

.. code:: r

.. figure:: ../images/summary.png

::

Basic Statistics
~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/3.png

::

Per base sequence quality
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/4.png

::

Per sequence quality scores
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/5.png
  
::

Per base sequence content
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/6.png

::

Per sequence GC content
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/7.png

::

Per base N content
~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/8.png

::

Sequence Length Distribution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/9.png

::

Sequence Duplication Levels
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/10.png

::

Overrepresented sequences
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/11.png

::

Adpater Content
~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/12.png

::

===============================
Successful Minion Run QC Report
===============================

.. code:: r

   # Set the working directory
   setwd("~/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91/fast5/guppy_out")

   # Confirm you have fastqcr loaded in your namespace and attached
   library(fastqcr)

   # If not install it
   fastqc_install()
   
   # Call the fastqc function from the package provided passed fastq directory as input argument and specify thread count
   fastqc(fq.dir = "/home/michael/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91/fast5/guppy_out/pass",threads = 4)

.. figure:: ../images/13.png

::

FASTQC Directory and HTML OUTPUT
--------------------------------


Summary
~~~~~~~

.. code:: r

.. figure:: ../images/summary2.png

::

Basic Statistics
~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/14.png

::

Per base sequence quality
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/15.png

::

Per sequence quality scores
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/16.png
  
::

Per base sequence content
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/17.png

::

Per sequence GC content
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/18.png

::

Per base N content
~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/19.png

::

Sequence Length Distribution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/20.png

::

Sequence Duplication Levels
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/21.png

::

Overrepresented sequences
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/22.png

::

Adpater Content
~~~~~~~~~~~~~~~

.. code:: r

.. figure:: ../images/23.png

::

==========
MinIONQC.R
==========

   First we want to reorgainze the data structure so the RScript from https://github.com/roblanf/minion_qc can plot,interpret and analyze the MinION reads. 
   Create a summary directory and place final_summary.txt and sequencing_summary.txt. Copy the MinIONQC.R Script into main directory and run the following command line. 
   It will produce a standard output if successful

.. code:: r

.. figure:: ../images/24.png

::

Channel Summary
---------------

.. code:: r

.. figure:: ../images/25.png

::

Flowcell Overview
-----------------

.. code:: r

.. figure:: ../images/26.png

::

GB Per Channel Overview
-----------------------

.. code:: r

.. figure:: ../images/27.png

::

Length Per Hour
---------------

.. code:: r

.. figure:: ../images/28.png

::

Length Histogram
----------------

.. code:: r

.. figure:: ../images/29.png

::

Length vs. Quality
------------------

.. code:: r

.. figure:: ../images/30.png

::

Quality by Hour
---------------

.. code:: r

.. figure:: ../images/31.png

::

Quality Histogram
-----------------

.. code:: r

.. figure:: ../images/32.png

::

Reads Per Hour
--------------

.. code:: r

.. figure:: ../images/33.png

::

Yield by Length
---------------

.. code:: r

.. figure:: ../images/34.png

::

Yield Over Time
---------------

.. code:: r

.. figure:: ../images/35.png

::








