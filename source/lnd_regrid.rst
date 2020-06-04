.. _lnd_regrid:

***************
Land Regridding
***************

Regridding ELM/CLM land data files
----------------------------------

Low Complexity
^^^^^^^^^^^^^^

In this example, a directory of ne30 clm2.h0 files will be regridded. This will regrid all variables in the input files, using sub-grid-scale
regridding, which uses the land fraction around coastal areas to better represent the values around complex coastal geometry.

.. code-block:: bash

    #!/bin/bash
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc
    drc_in=./clm2.h0/
    drc_out=./180x360/
    land_file=<input file path> # this is the path to a single land file to pull the landfrac variable from

    ncremap -m ${mapfile} -I ${src_in} -O ${drc_out} --sgs_frac=${land_file}/landfrac


Medium Complexity
^^^^^^^^^^^^^^^^^

In this example, the same regridding operation as above is performed, but only on the DEADSTEMC and CDWC variables. 
Running with specific variables significantly speeds up the run and reduces output files size, as the rest of the variables are ignored.


.. code-block:: bash

    #!/bin/bash
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc
    drc_in=./clm2.h0/
    drc_out=./180x360/
    vars="DEADSTEMC,CDWC"               # the variables must be in a comma sepparated list with no spaces
    land_file=<input file path>         # this is the path to a single land file to pull the landfrac variable from
    
    ncremap -v ${vars} -m ${mapfile} -I ${src_in} -O ${drc_out} --sgs_frac=${land_file}/landfrac


High Complexity
^^^^^^^^^^^^^^^

In example, the regridding operation is performed and the selected variables are output in single-variable-per-file time-series files.
The output will be compressed and deflated. The model data starts at 1850-01 and ends at 2014-12.

.. code-block:: bash

    #!/bin/bash
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc
    drc_in=./clm2.h0/
    drc_out=./180x360/
    vars="DEADSTEMC,CDWC"               # the variables must be in a comma sepparated list with no spaces
    start=1850                          # the first year of model data
    end=2014                            # the last year of model data
    land_file=<input file path>         # this is the path to a single land file to pull the landfrac variable from
    flags="-7 --dfl_lvl=1"       # compression and deflation

    cd ${drc_in}
    ls | ncclimo \
      ${flags} \
      --var=${vars} \
      --yr_srt=${start} \
      --yr_end=${end} \
      -O=${dir_out} \
      --map=${mapfile} \
      --sgs_frac=${land_file}/landfrac

