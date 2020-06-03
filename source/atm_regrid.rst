.. _atm_regrid:

**********************
Atmospheric Regridding
**********************

Regridding EAM/CAM atmospheric data diles
-----------------------------------------

A simple example, regridding a directory of ne30 cam.h0 files. This assumes that the mapfile is already in place
and that a set of cam.h0 files are inplace under a directory named "cam.h0". All the variables inside the file will be regridded.

.. code-block:: bash

    #!/bin/bash
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc
    drc_in=./cam.h0/
    drc_out=./180x360/
    
    ncremap -m ${mapfile} -I ${src_in} -O ${drc_out}


In this medium complexity example, the same regridding operation as above is performed, but only on the PRECT and FSDS variables. 
Running with specific variables selected significantly speeds up the run, as the rest of the variables are ignored.


.. code-block:: bash

    #!/bin/bash
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc
    drc_in=./cam.h0/
    drc_out=./180x360/
    vars="PRECT,FSDS"
    
    ncremap -v ${vars} -m ${mapfile} -I ${src_in} -O ${drc_out}


In this example, the PRECT and FSDS variables will be regridded and extracted into single-variable-per-file time-series files.
The output will be compressed and deflated. The model data starts at 1850-01 and ends at 2014-12.

.. code-block:: bash

    #!/bin/bash

    drc_in=./cam.h0/
    drc_out=./180x360/
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc
    vars="PRECT,FSDS"              # comma separated variable list
    start=1850                     # the first year of data
    end=2014                       # the last year of data
    format_flags="-7 --dfl_lvl=1"  # compression and deflation flags

    cd ${drc_in};
    ls *.nc | ncclimo --dbg=1 --var=${vars} {format_flags} --yr_srt=${start} --yr_end=${end} -O=${dir_out} --map=${mapfile}

For high temporal frequency files (i.e. higher frequency then monthly), add the `--clm_md=hgh_frq` argument to the ncclimo command 