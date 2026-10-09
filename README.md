# ds9cli

Command-line helpers for DS9

I've had trouble installing the latest versions of the [XPA tools](https://ds9.si.edu/doc/ref/xpa.html) for interacting with open DS9 sessions via the command line.  Much of that functionality has been ported using the [SAMP (Simple Application Messaging Protocol)] by the [ds9samp](https://github.com/cxcsds/ds9samp) package.  The scripts provided here uses ``ds9samp`` to implement some simple command-line actions useful for loading files and easily manipulating the DS9 display.

## Installation

```bash
$ pip install git+https://https://github.com/gbrammer/ds9cli.git

$ which dload
[/python/environment/]/bin/dload

