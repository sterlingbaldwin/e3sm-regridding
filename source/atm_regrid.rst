.. _atm_regrid:

**********************
Atmospheric Regridding
**********************

Regridding EAM/CAM atmospheric data files
-----------------------------------------

Simple example
^^^^^^^^^^^^^^

This example will regrid all files in a directory of ne30 cam.h0 files. This assumes that the mapfile is already in place in the current
directory and that a set of cam.h0 files are inplace under a directory named "cam.h0". All the variables inside the file will be regridded.


.. code-block:: bash

    #!/bin/bash
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc    # path to appropriate mapfile
    input_dir=cam.h0                                         # input directory path
    output_dir=180x360                                       # output directory path
    
    ncremap -m ${mapfile} -I ${input_dir} -O ${output_dir}


Medium Complexity
^^^^^^^^^^^^^^^^^

In this example, the same regridding operation as above is performed, but only on the CLD_CAL and FSDS variables. 
Running with specific variables selected significantly speeds up the regridding process, as the rest of the variables are ignored.
Output file size will also be reduced, as only the selected variables will be present.


.. code-block:: bash

    #!/bin/bash
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc   # path to mapfile
    input_dir=cam.h0                                        # input directory path
    output_dir=180x360                                      # output directory path
    vars=CLD_CAL,FSDS                                       # variables to regrid
    
    ncremap -v ${vars} -m ${mapfile} -I ${input_dir} -O ${output_dir}


Regridded Climatology
---------------------

In this example, the CLD_CAL and FSDS variables will be regridded and averaged over the seasons to create both seasonal and annual climatologies.
The output will be compressed and deflated. The model data starts at 1850-01 and ends at 2014-12. For high temporal frequency files 
(i.e. higher frequency then monthly), add the `--clm_md=hgh_frq` argument to the ncclimo command. The ncclimo tool needs the caseid of the
experiment data supplied to it with the "-c" flag.

.. code-block:: bash

    #!/bin/bash

    input_dir=cam.h0
    output_dir=native_ts                                    # path to output directory for native timeseries
    regrid_dir=180x360ts                                    # path to output directory for regridded timeseries
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc   # path to mapfile
    vars=CLD_CAL,FSDS                                       # comma separated variable list
    start=1850                                              # the first year of data
    end=2014                                                # the last year of data
    flags="-7 --dfl_lvl=1"                                  # format and deflation flags
    caseid=20180129.DECKv1b_piControl.ne30_oEC.edison       # the caseid for the data being processed

    ncclimo \
      ${flags} \
      --var=${vars} \
      -c ${caseid} \
      --yr_srt=${start} \
      --yr_end=${end} \
      --input=${input_dir} \
      --output=${output_dir} \
      --regrid=${regrid_dir}
      --map=${mapfile}

Regridded Timeseries
---------------------

In this example, the CLD_CAL and FSDS variables will be regridded and extracted into single variable per file time-series. The command is identical to the 
above climatology example, except that the additional year-per-file flag (--ypf) is added. 

When choosing variables to extract into time-series, its only possible to use variables with both a time and space dimension. In the E3SM model
the spacial dimension is denoted by the ncol axis (column number).

.. code-block:: bash

    #!/bin/bash

    input_dir=cam.h0
    output_dir=180x360
    mapfile=map_ne30np4_to_cmip6_180x360_aave.20181001.nc   # path to mapfile
    vars=CLD_CAL,FSDS                                       # comma separated variable list
    start=1850                                              # the first year of data
    end=2014                                                # the last year of data
    flags="-7 --dfl_lvl=1 --ypf=50"                         # format and deflation flags, create a new output file for each 50 year chunk
    caseid=20180129.DECKv1b_piControl.ne30_oEC.edison       # the caseid for the data being processed

    ncclimo \
      ${flags} \
      --var=${vars} \
      -c ${caseid} \
      --yr_srt=${start} \
      --yr_end=${end} \
      --input=${input_dir} \
      --output=${output_dir} \
      --map=${mapfile}

