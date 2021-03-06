.. _status:

==============
Project Status
==============

There is support for producing 64-bit CPython distributions for Windows,
macOS, and Linux. All distributions are highly self-contained and have
limited shared library dependencies.

Planned and features include:

* Static/dynamic linking toggles for dependencies
* Support for configuring which toolchain/version to use
* Support for BSDs
* Support for iOS and/or Android
* Support for Python distributions that aren't CPython

Test Failures
=============

This repository contains ``test-distribution.py`` script that can be
used to run the Python test harness from a distribution archive.

Here, we track the various known failures when running
``test-distribution.py /path/to/distribution.tar.zst -u all``.

``test_ssl``
------------

Known failing on: Linux

When building against LibreSSL instead of OpenSSL, ``test_ssl`` fails
in the following manner::

    ======================================================================
    FAIL: test_openssl_version (test.test_ssl.BasicSocketTests)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "/tmp/tmpinhxfy8t/python/install/lib/python3.7/test/test_ssl.py", line 573, in test_openssl_version
        (s, t, hex(n)))
    AssertionError: False is not true : ('LibreSSL 3.0.2', (2, 0, 0, 0, 0), '0x20000000')

    ----------------------------------------------------------------------

This seems to be the test assuming the existence of OpenSSL and not
coping with LibreSSl. This failure seems pretty benign.

``test_subprocess``
-------------------

Known Failing on: Linux

This fails in the following manner::

    test_executable_without_cwd (test.test_subprocess.ProcessTestCaseNoPoll) ... Could not find platform independent libraries <prefix>
    Could not find platform dependent libraries <exec_prefix>
    Consider setting $PYTHONHOME to <prefix>[:<exec_prefix>]
    Fatal Python error: initfsencoding: Unable to get the locale encoding
    ModuleNotFoundError: No module named 'encodings'

    Current thread 0x00007fd77c231740 (most recent call first):
    FAIL

    ======================================================================
    FAIL: test_executable_without_cwd (test.test_subprocess.ProcessTestCaseNoPoll)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "/tmp/tmp8hef0kr4/python/install/lib/python3.7/test/test_subprocess.py", line 436, in test_executable_without_cwd
        executable=sys.executable)
      File "/tmp/tmp8hef0kr4/python/install/lib/python3.7/test/test_subprocess.py", line 355, in _assert_cwd
        self.assertEqual(47, p.returncode)
    AssertionError: 47 != -6

We're unsure what is going on here. The error from ``initfsencoding``
is what happens when the first ``import`` during ``Py_Initialize()``
fails. So it appears the test somehow can't locate the Python
standard library.

``test_tk``
-----------

Known Failing on: Linux

This fails in the following manner::

    ======================================================================
    FAIL: test_from (tkinter.test.test_tkinter.test_widgets.ScaleTest)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "/tmp/tmpoqqjd5gi/python/install/lib/python3.7/tkinter/test/test_tkinter/test_widgets.py", line 867, in test_from
        self.checkFloatParam(widget, 'from', 100, 14.9, 15.1, conv=float_round)
      File "/tmp/tmpoqqjd5gi/python/install/lib/python3.7/tkinter/test/widget_tests.py", line 106, in checkFloatParam
        self.checkParam(widget, name, value, conv=conv, **kwargs)
      File "/tmp/tmpoqqjd5gi/python/install/lib/python3.7/tkinter/test/widget_tests.py", line 63, in checkParam
        self.assertEqual2(widget[name], expected, eq=eq)
      File "/tmp/tmpoqqjd5gi/python/install/lib/python3.7/tkinter/test/widget_tests.py", line 47, in assertEqual2
        self.assertEqual(actual, expected, msg)
    AssertionError: 14.9 != 15.0

This seems like a minor issue and might be a bug in the test itself.

Test Skips
==========

Linux
-----

The following tests are skipped on Linux:

test_asdl_parser
   test irrelevant for an installed Python
test_clinic
   install/lib/Tools/clinic' path does not exist
test_dbm_gnu
   No module named '_gdbm'
test_devpoll
   test works only on Solaris OS family
test_gdb
   test_gdb only works on source builds at the moment.
test_kqueue
   test works only on BSD
test_msilib
   No module named 'msilib'
test_ossaudiodev
   [Errno 2] No such file or directory: '/dev/dsp'
test_startfile
   object <module 'os' from '.../install/lib/python3.7/os.py'> has no attribute 'startfile'
test_winconsoleio
   test only relevant on win32
test_winreg
   No module named 'winreg'
test_winsound
   No module named 'winsound'
test_zipfile64
   test requires loads of disk-space bytes and a long time to run

macOS
-----

The following tests are skipped on macOS:

test_asdl_parser
   test irrelevant for an installed Python
test_clinic
   python/install/lib/Tools/clinic' path does not exist
test_dbm_gnu
   No module named '_gdbm'
test_devpoll
   test works only on Solaris OS family
test_epoll
   test works only on Linux 2.6
test_gdb
   Couldn't find gdb on the path
test_msilib
   No module named 'msilib'
test_multiprocessing_fork
   test may crash on macOS (bpo-33725)
test_nis
   No module named 'nis'
test_ossaudiodev
   No module named 'ossaudiodev'
test_spwd
   No module named 'spwd'
test_startfile
   object <module 'os' from '.../install/lib/python3.7/os.py'> has no attribute 'startfile'
test_tix
   cannot run without OS X gui process
test_tk
   cannot run without OS X gui process
test_ttk_guionly
   cannot run without OS X gui process
test_winconsoleio
   test only relevant on win32
test_winreg
   No module named 'winreg'
test_winsound
   No module named 'winsound'
test_zipfile64
   test requires loads of disk-space bytes and a long time to run
