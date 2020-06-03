.. e3sm-regridding documentation master file, created by
   sphinx-quickstart on Wed Jun  3 10:11:38 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to e3sm-regridding's documentation!
===========================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:


.. _regridding_guide:

****************
Regridding Guide
****************

This guide will cover regridding native E3SM model output from each of its components native grids, to
any regular grid.

Environment Setup
-----------------

You will need to first create an anaconda environment with the depedencies
and install NCO. Although there are `several ways to intall nco <http://nco.sourceforge.net/>`_, the 
recommended method is to `use the "conda" package manager <https://www.anaconda.com/products/individual#linux>`_ and simply
activate your environment and then use: 

::

    conda install -c conda-forge nco

To create a new conda environment named "regrid" with just nco use: 

::

    conda create --name regrid -c conda-forge nco
   

Map files
---------

All of these methods assume you have a mapping file on hand to convert between the relevant grids. This is a partial list
of mapping files for commonly used E3SM resolutions.

* `High resolution MPAS ocean (18km to 6km) to 1/4 degree lon lat <https://aims3.llnl.gov/thredds/fileServer/user_pub_work/E3SM/1_0/1950-Control/0_25deg_atm_18-6km_ocean/misc/native/mapping/fixed/ens1/v1/map_oRRS18to6v3_to_0.25x0.25degree_bilinear.nc>`_
* `High resolution EAM/CAM atmospheric data (ne120) to 1/4 degree lon lat <http://esgf-data2.llnl.gov/thredds/fileServer/user_pub_work/E3SM/1_0/1950-Control/0_25deg_atm_18-6km_ocean/misc/native/mapping/fixed/ens1/v2/map_ne120np4_to_cmip6_720x1440_aave.20181001.nc>`_
* `Standard resolution MPAS ocean (60km to 30km) to 1x1 degree lon lat <https://aims3.llnl.gov/thredds/fileServer/user_pub_work/E3SM/1_0/1950-Control/1deg_atm_60-30km_ocean/misc/native/mapping/fixed/ens1/v1/map_oEC60to30v3_to_cmip6_180x360_aave.20181001.nc>`_
* `Standard resolution EAM/CAM atmospheric data (ne30) to 1x1 degree lon lat <http://esgf-data2.llnl.gov/thredds/fileServer/user_pub_work/E3SM/1_0/piControl/1deg_atm_60-30km_ocean/misc/native/mapping/fixed/ens1/v1/map_ne30np4_to_cmip6_180x360_aave.20181001.nc>`_
* `Standard resolution ELM/CLM atmospheric data to 1x1 degree lon lat <http://esgf-data2.llnl.gov/thredds/fileServer/user_pub_work/E3SM/1_0/piControl/1deg_atm_60-30km_ocean/misc/native/mapping/fixed/ens1/v1/map_ne30np4_to_cmip6_180x360_sgs_elm.20190301.nc>`_


Data files
----------

Raw E3SM model output can be obtained from ESGF here: https://esgf-node.llnl.gov/search/e3sm/

.. toctree::
   :maxdepth: 1
   :caption: Contents:

   atm_regrid
   lnd_regrid
   mpas_regrid




