.. _guide:


****************
Regridding Guide
****************

This guide will cover regridding native E3SM model output from each of its components native grids, to
any regular grid. These regridding examples are specific for the E3SMv1.x model, and may not work with later versions. For detailed up-to-date information on the 
E3SM model activities, refer to the `E3SM organizational website <https://e3sm.org/>`_.


Environment Setup
-----------------

You will need to first create an anaconda environment with the depedencies and install the netCDF Operators (NCO). 
Follow `this link <http://nco.sourceforge.net/>`_ for the full set of NCO documentation. 
Although there are several ways to intall nco, the recommended method is to use the `conda <https://www.anaconda.com/products/individual#linux>`_ package manager.

Conda
^^^^^

::

    conda install -c conda-forge nco

To create a new conda environment named "regrid" with just nco use: 

::

    conda create --name regrid -c conda-forge nco
   

Pre-build Executible
^^^^^^^^^^^^^^^^^^^^

`Pre-build executibles are available on a variety of platforms. <http://nco.sourceforge.net/#Executables>`_


Map files
---------

All of these methods assume you have a mapping file on hand to convert between the relevant grids. This is a partial list
of mapping files for commonly used E3SM resolutions.

* `High resolution MPAS ocean (18km to 6km) to 1/4 degree lon lat <https://aims3.llnl.gov/thredds/fileServer/user_pub_work/E3SM/1_0/1950-Control/0_25deg_atm_18-6km_ocean/misc/native/mapping/fixed/ens1/v1/map_oRRS18to6v3_to_0.25x0.25degree_bilinear.nc>`_
* `High resolution atmos/land (ne120) to 1/4 degree lon lat <http://esgf-data2.llnl.gov/thredds/fileServer/user_pub_work/E3SM/1_0/1950-Control/0_25deg_atm_18-6km_ocean/misc/native/mapping/fixed/ens1/v2/map_ne120np4_to_cmip6_720x1440_aave.20181001.nc>`_
* `Standard resolution MPAS ocean (60km to 30km) to 1x1 degree lon lat <https://aims3.llnl.gov/thredds/fileServer/user_pub_work/E3SM/1_0/1950-Control/1deg_atm_60-30km_ocean/misc/native/mapping/fixed/ens1/v1/map_oEC60to30v3_to_cmip6_180x360_aave.20181001.nc>`_
* `Standard resolution atmos/land (ne30) to 1x1 degree lon lat <http://esgf-data2.llnl.gov/thredds/fileServer/user_pub_work/E3SM/1_0/piControl/1deg_atm_60-30km_ocean/misc/native/mapping/fixed/ens1/v1/map_ne30np4_to_cmip6_180x360_aave.20181001.nc>`_

`A set of commonly used mapfiles can be found here. <https://web.lcrc.anl.gov/public/e3sm/mapping/maps/>`_ The mapfiles follow the naming convention of "map_<source-grid>_to_<destination-grid>_<regridding-algorithm>". This allows you to mix-and-match
whichever raw grid was used to whichever destination grid is desired. The "ne30" raw grid corresponds to roughly 1 by 1 degree longitude/latitude, and is the "standard" resolution for the E3SM model. Higher
resolutions are ne120 which corresponds to roughtly 0.25 degree by 0.25 degree lon/lat. 

The two most common regridding algorithns are bi-linear ("bilin"/"blin") and Area Averaged ("aave"). For bi-linear each of the grid points in the dest grid contains the 
average of the nearest four grid points in the source grid, weighted by their distance from the destination grid point. 
If any of the four surrounding input grid points contain missing data, the interpolated value will be flagged as missing. Bilin is a good default regridding method. 

The aave method is the area average with latitude weighting, and gives better results when going from a high resolution grid to a lower resolution. 
A spatial average of the source data is calculated in the area outlined by each grid box in the destination grid. 
If a grid box in the source grid partially overlaps the area of a destination grid cell, its contribution to the spatial average is weighted by the fraction of area within the destintion grid cell domain. 
See `here <http://cola.gmu.edu/grads/gadoc/gradfunclterp.html>`_ for additional information.


Data files
----------

EAM/CAM
^^^^^^^

`Raw E3SM model output can be obtained from ESGF here. <https://esgf-node.llnl.gov/search/e3sm/>`_ The ESGF search interface allows faceted search of the published E3SM model data.
On the left side of the interface, each of the E3SM search facets is represented by a drop down menu. For example, the "Campaign" facet allows for the selection of data from a specific
simulation campaign. Selecting the "DECK-v1" option and pressing "search" will narrow down the datasets displayed to only those experiments that participated in the `DECK experimental campaign. <https://www.wcrp-climate.org/wgcm-cmip/wgcm-cmip6>`_
Similarly, the "experiment" facet can be used to display all datasets belonging to a single simulation. The facets can be added together in an AND relationship, for example selecting the `experiment=1pctCO2 AND realm=atmos <https://esgf-node.llnl.gov/search/e3sm/?experiment=1pctCO2&realm=atmos>`_
will display all datasets that have both belong to the 1pctCO2 experiment, and are atmospheric data files.

The atmospheric files take the form of <experiment_case_name>.cam.h0.<year>-<month>.nc, for example 20180215.DECKv1b_1pctCO2.ne30_oEC.edison.cam.h0.0001-01.nc. The E3SM atmospheric model is a heavily modified decendent of the 
UCAR `Community Atmospheric Model <http://www.cesm.ucar.edu/models/atm-cam/>`_, and so include "cam" in their name. Note that future versions of the E3SM model will include a name change to the "E3SM Atmosphere Model" or eam. The EAM/CAM
data files come in 4 temporal frequencies, monthly (cam.h0), daily (cam.h1), 6 hourly (cam.h3), and 3hr (cam.h4). 

ELM/CLM
^^^^^^^

Similarly, the E3SM Land Model decends from the `Community Land Model <http://www.cgd.ucar.edu/tss/clm/distribution/index.html>`_, and so contain the substring "clm2". Future versions of the E3SM output will replace this with
"elm" for the E3SM Land Model. All land model data are monthly averages.

MPAS
^^^^

The Ocean/Ice components of E3SM come from the `Model for Prediction Across Scales (MPAS) <https://mpas-dev.github.io/>`_, a modern state-of-the-art ocean model developed specifically for the E3SM project. All MPAS
data are monthly averages.

