.. _mpas_regridding:


***************
MPAS Regridding
***************

Regridding MPAS ocean/sea-ice data files
----------------------------------------

Low Complexity
^^^^^^^^^^^^^^

In this example, a directory of mpaso.hist.am.timeSeriesStatsMonthly files will be regridded.
For mpassi files, add the flag "--sgs_frc=timeMonthly_avg_iceAreaCell" to turn on sub-grid-cell regridding, 
or use the --prc_typ=mpasseaice to change the procedure type to mpas-sea-ice.

.. code-block:: bash

    #!/bin/bash
    mapfile=map_oEC60to30v3_to_cmip6_180x360_aave.20181001.nc   # map from the MPAS 60km-to-30km mesh to the 1x1 degree grid
    input_dir=mpaso                                                # path to input directory
    output_dir=180x360                                             # path to output directory
    flags="--prc_typ=mpas --d2f"                                # This invokes the mpas regridder, and converts output from double precision to single
    
    ncremap -m ${mapfile} -I ${input_dir} -O ${output_dir} {flags}



High Complexity
^^^^^^^^^^^^^^^

In this example, two variables will be extracted into single-variable-per-file time-series files. 
These will be compressed and deflated, and the data will be converted from double to single precision.
For mpas sea-ice files, add the flag "--sgs_frc=timeMonthly_avg_iceAreaCell" to turn on sub-grid-cell regridding, 
or use the --prc_typ=mpasseaice to change the procedure type to mpas-sea-ice

.. code-block:: bash

    #!/bin/bash
    mapfile=map_oEC60to30v3_to_cmip6_180x360_aave.20181001.nc                       # map from the MPAS 60km-to-30km mesh to the 1x1 degree grid
    input_dir=mpaso                                                                    # path to input directory
    output_dir=180x360                                                                 # path to output directory
    vars=timeMonthly_avg_seaSurfaceSalinity,timeMonthly_avg_seaSurfaceTemperature   # variables to extract in a comma sepperated list
    start=1850                                                                      # first year of model data
    end=2014                                                                        # last year of model data
    flags="-7 --dfl_lvl=1 -m mpas --d2f"                                            # format and deflation flags

    ncclimo \
      ${flags} \
      --var=${vars} \
      --yr_srt=${start} \
      --yr_end=${end} \
      --input=${input_dir} \
      --output=${dir_out} \
      --map=${mapfile}
