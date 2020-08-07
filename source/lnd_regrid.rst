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
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc   # path to mapfile
    input_dir=clm2.h0                                       # path to input directory
    output_dir=180x360                                      # path to output directory
    land_file=<input file path>                             # this is the path to a single land file to pull the landfrac variable from

    ncremap -m ${mapfile} -I ${input_dir} -O ${output_dir} --sgs_frc=${land_file}/landfrac


Medium Complexity
^^^^^^^^^^^^^^^^^

In this example, the same regridding operation as above is performed, but only on the DEADSTEMC and CDWC variables. 
Running with specific variables significantly speeds up the run and reduces output files size, as the rest of the variables are ignored.


.. code-block:: bash

    #!/bin/bash
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc   # path to mapfile
    input_dir=clm2.h0                                       # path to input directory
    output_dir=180x360                                      # path to output directory
    vars=DEADSTEMC,CDWC                                     # the variables must be in a comma separated list with no spaces
    land_file=<input file path>                             # this is the path to a single land file to pull the landfrac variable from
    
    ncremap -v ${vars} -m ${mapfile} -I ${input_dir} -O ${output_dir} --sgs_frc=${land_file}/landfrac


High Complexity
^^^^^^^^^^^^^^^

In example, the regridding operation is performed and the selected variables are output in single-variable-per-file time-series files.
The output will be compressed and deflated. The model data starts at 1850-01 and ends at 2014-12.

.. code-block:: bash

    #!/bin/bash
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc  # path to mapfile 
    input_dir=clm2.h0                                      # path to input directory
    output_dir=180x360                                     # path to output directory
    vars=DEADSTEMC,CDWC                                    # the variables must be in a comma separated list with no spaces
    start=1850                                             # the first year of model data
    end=2014                                               # the last year of model data
    land_file=<input file path>                            # this is the path to a single land file to pull the landfrac variable from
    flags="-7 --dfl_lvl=1"                                 # format and deflation

    ncclimo \
      ${flags} \
      --var=${vars} \
      --yr_srt=${start} \
      --yr_end=${end} \
      --ypf=50 \
      --input=${input_dir} \
      --output=${output_dir} \
      --map=${mapfile} \
      --sgs_frc=${land_file}/landfrac

