.. _mpas_regridding:


***************
MPAS Regridding
***************

Regridding MPAS ocean/sea-ice data diles
----------------------------------------

A simple example, regridding a directory of mpaso.hist.am.timeSeriesStatsMonthly files.
For mpassi files, add the flag "--sgs_frc=timeMonthly_avg_iceAreaCell" to turn on sub-grid-cell regridding.

.. code-block:: bash

    #!/bin/bash
    mapfile=map_oEC60to30v3_to_cmip6_180x360_aave.20181001.nc
    drc_in=./mpaso/
    drc_out=./180x360/
    flags="-P mpas --d2f"
    
    ncremap -m ${mapfile} -I ${drc_in} -O ${drc_out} {flags}


A more complex example, extracting two variables into single-variable-per-file time-series files. These will be compressed and deflated,
and the data will be converted from double to single precision.
For mpassi files, add the flag "--sgs_frc=timeMonthly_avg_iceAreaCell" to turn on sub-grid-cell regridding.

.. code-block:: bash

    #!/bin/bash
    mapfile=map_oEC60to30v3_to_cmip6_180x360_aave.20181001.nc
    drc_in=./mpaso/
    drc_out=./180x360/
    vars="timeMonthly_avg_seaSurfaceSalinity,timeMonthly_avg_seaSurfaceTemperature"
    start=1850
    end=2014
    flags="-7 --dfl_lvl=1 -m mpas --d2f"

    cd ${drc_in};
    ls *.nc | ncclimo --var=${vars} {flags} --yr_srt=${start} --yr_end=${end} -O=${dir_out} --map=${mapfile}
