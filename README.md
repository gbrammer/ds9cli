# ds9cli: Command-line helpers for DS9

I've had trouble installing the latest versions of the [XPA
tools](https://ds9.si.edu/doc/ref/xpa.html) for interacting with open DS9
sessions via the command line. Much of that functionality has been ported using
the PyVO Simple Application Messaging
Protocol ([SAMP](https://pyvo.readthedocs.io/en/stable/samp/index.html)) by the
[ds9samp](https://github.com/cxcsds/ds9samp) package. The scripts provided here
use ``ds9samp`` to implement some simple command-line actions useful for
loading files and easily manipulating the DS9 display.

## Installation

```bash
$ pip install git+https://github.com/gbrammer/ds9cli.git
```

Verify that the shell scripts were installed:

```bash
$ which dset
[/python/environment/]/bin/dset

$ dset
dget: ds9.set()

Usage:  dset [xpa set commands]
```

## Scripts

### ``dget``, ``dset``

Wrappers around the low-level ``get`` and ``set`` commands:

```bash
$ dget

dget: ds9.get()

Usage:  dget [xpa get commands]

$ dset

dget: ds9.set()

Usage:  dset [xpa set commands]

```

### ``dload``

Load FITS files **and** *Roman* ASDF files into a DS9 frame.

Note: The
[roman_datamodels](https://github.com/spacetelescope/roman_datamodels) package
is required for displaying ASDF files.

```bash
$ dload -h

Display normal FITS and Roman ASDF files in DS9.

Notes:
  - For Roman ASDF files, the file is displayed with a header generated from `img.meta.wcs.to_fits_sip()`

Usage:
  $ dload r0003201001001001004_0004_wfi16_f106_cal.asdf  [--info]  # print asdf info
  
  # display a specific image attribute (e.g., data, dq, err, var_poisson, chisq, dumo)
  
  $ dload r0003201001001001004_0004_wfi16_f106_cal.asdf[data]
  $ dload r0003201001001001004_0004_wfi16_f106_cal.asdf --ext=data

  $ dload {file} --frame                # load into new frame
  $ dload {file} --frame=1              # frame 1
  $ dload {file} [-rgb] [-r] [-g] [-b]  # rgb channel, -rgb creates new RGB frame

```

### ``dreg``

```bash
$ dreg -h

Work with DS9 regions

Usage:

 # Load a region file, optionally delete existing before loading new
    
    $ dreg ds9.reg [-d/--delete]
    
 # Run a selection (before loading a file, if specified)
 # --select options: all, none, invert, front, back
 #
 # With --color[=magenta] change the color of everything selected
    
    $ dreg [file] --select[=all] [--color=magenta]

 # Groups
    
    $ dreg --group[=group]   #  Update "group" based on selection, create if new
    $ dreg -gi               #  Print group indices and names (NB: ordering can change after adding / updating groups!)
    $ dreg -gs[=0,1]         #  Select entries from groups, all or a comma-separated list of group indices
    $ dreg -ug[=0]           #  Update group index (or all groups) with current selection.  With nothing selected -ug removes all groups

 # List [-l/--list] / save [--save] regions (-s shows only selected)
    
    $ dreg [--save=/tmp/ds9.reg] [-l/--list] [-s] [-wcs/-image] [--sky=icrs]

```

### ``dmark``

```bash
Mark a circular region at the central DS9 window position

Usage options:

    $ dmark [-r=10] [-c=cyan] [-s] [Label text]
    $ dmark [--radius=10] [--color=cyan] [--select] [Label text]
```

### ``dscale``

Quick default colorbar scaling.  Defaults work well for pixel values order ~unity [i.e., HST, JWST].

```bash
$ dscale -h

Autoscale a DS9 window

Usage options:

    $ dscale scale
    $ dscale min max bkg
    $ dscale min max bkg scale
    $ dscale [...] [-q] [--quiet]  #  suppress messages
    $ dscale [...] --auto[=20]     #  compute median, NMAD scale (default: NMAD x 20)
```

### ``dmatch``

```bash
$ dmatch -h

Wrapper around locking DS9 frame parameters

Usage:  dmatch [wcs]

 - frame lock [wcs, image, etc]
 - lock colorbar
 - match scale
 - frame single
```