.. _lec_dood_s18_lec04_page:

Lecture 04 - Mysteries in Comp Arch and Basics
==============================================

.. warning::
    Incomplete. Stopped at 33:11


Info
----

* `Video Link <http://www.youtube.com/watch?v=WZeYoDkzAmc>`__
* `Lecture Slides <https://safari.ethz.ch/digitaltechnik/spring2018/lib/exe/fetch.php?media=onur-digitaldesign-2018-lecture4-mysteries-basics-afterlecture.pdf>`__

Reading
-------

* Chapter 2 in [harris2012digital]_
* Chapter 3 in [patt2005introduction]_
* [mutlu2007memory]_
* [rixner2000memory]_


Reading Notes
^^^^^^^^^^^^^

Lecture Minutes
---------------

* **04:30** Another mystery is **Memory Performance Attacks**
* **10:00** :term:`throughput computing` is mentioned.
* **10:55** We want N time the system performance with N times the cores.
  (:term:`super linear scaling`)
* **12:00** [mutlu2007memory]_ is mentioned. They run MATLAB and GCC on
  different cores and measured slow down comparing standalone runs. MATLAB
  slows down 7% if GCC is running. But GCC slows down 204% if MATLAB runs
  on the other core. MATLAB somewhat slows down GCC. Even if in OS MATLAB is
  set to low priority and GCC is set to high priority, results don't change.
  The problem is not related with OS priorities. MATLAB "hogs" memory. In a
  fair system, this isn't expected. This is neither fair nor controllable
  system.
* **17:00** If you don't "cross layers", you can't understand the issue.
* **18:30** This is important because multi-core computing is everywhere:
  cloud, mobile, etc. Also there are different applications with different
  QoS requirements: video processing, pedestrian detection, etc.
* **20:00** Although each core has different cache, DRAM memory controller is
  shared between cores. It looks like memory controller prioritize request
  of MATLAB.
* **21:30** Deeper DRAM structure. A DRAM bank consists of 2D array of cells.
  Dimensions: Columns and Rows. There are also other components. To access
  row, you need to bring it to the row buffer. When you need to bring a row
  to the row buffer, you activate that row by applying high voltage. After
  data is in the buffer, you select a column depending on your address input.
* **25:30** If you access Row:0, Column:0 and then access Row:0 Column:1, you
  don't need the activate Row:0 again because Column:1 is also brought to the
  buffer when Row:0 is transferred to the buffer for Column:0. This is true
  for all Columns with the same Row number. If you access Row:1 after some
  access to Row:0. This takes some time. Because memory first writes back
  (precharge)
  content of buffer to Row:0, then opens Row:1.
* **29:40** Memory controller may prioritize memory access with the same
  row number (row-hit first), then serve older request (oldest-first). This is
  a commonly used scheduling policy called FR-FCFS. See: [rixner2000memory]_
  The goal is to maximize DRAM throughput. But for multiple programs with
  different characteristics, this may be bad.
* **31:00** Row-hit first rule unfairly prioritizes apps with high row
  buffer locality.
  In other words, applications that keep on accessing the same row
  (i.e. :term:`streaming application`) have an
  advantage. Secondly, oldest-first unfairly prioritizes memory intensive
  applications. Memory intensive applications get higher chance to access
  memory. Oldest-first looks fair. But suppose that app A generates 1 million
  requests before app B's only one request. Now app B which is not a memory
  intensive application has to wait all request of app A. If applications are
  equal, it may be fair.
* **33:11** DRAM controller are vulnerable to denial of service (DoS) attacks.
  

Glossary
--------

.. glossary::

    streaming application
        Application that reaches memory regions with high locality. For example
        accessing x[i] while i is increasing like i++ is a streaming
        application.

    super linear scaling
        The case when N cores improves the performance N times

    throughput computing
        .. todo::

            I don't know yet.

.. index::
    FR-FCFS
    precharge


.. sectionauthor:: Alper Yazar
