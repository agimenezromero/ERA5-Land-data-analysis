# ERA5-Land data analysis
A simple julia library built on top of [GRIB.jl](https://github.com/weech/GRIB.jl) package to analyse ERA5-Land reanalysis dataset (ECMWF)

# Overview
The aim of this library is to analyse data from the ERA5-Land dataset with a specific purpose, for instance, obtain the monthly minimum average temperature of the coldest month in a given period of time for all the pixels provided by the input data. Other implemented functionality is related to the computation of specific metrics used to asses the risk of *Xylella fastidiosa* (*Xf*) outbreaks in vineyards. All the code in this repository was used in [paper](link)

Table of contents
=================

<!--ts-->
   * [Overview](#overview)
   * [Table of contents](#table-of-contents)
   * [Requeriments](#requeriments)
   * [Documentation](#documentation)
   * [Examples](#examples)
   * [Authors](#authors)
   * [License](#license)
<!--te-->

# Requeriments
Julia 1.5 or higher installed with the following libraries
- [GRIB.jl](https://github.com/weech/GRIB.jl)
- [DataFrames.jl](https://dataframes.juliadata.org/stable/)
- [Feather.jl](https://github.com/JuliaData/Feather.jl)
- [Dates.jl](https://docs.julialang.org/en/v1/stdlib/Dates/)

# Documentation
* `grib_to_feather(infilename, outfilename)` 

  Creates a feather file for simpler use with DataFrames.jl or Pandas in Python. Not recommended unless necessary, as files can be big.

  - `ìnfilename` (string) - Input filename.
  - `outfilename` (string) - Output filename.
  
  - `output file format` - Each column contains a date as header and the temperature of each pixel value, leading to table of dimensions (N_hours, N_pixels)
  
* `compute_MGDD_HN(infilename, outfilename; threshold_1=285.15, threshold_2=291.15, threshold_3=301.15, threshold_4=305.15, last_T=308.15)`

  Computes the *Modified Growing Degree Days* for the North Hemisphere over given input data (which must be in GRIB format).
  
  - `infilename` (string) - Input filename
  - `outfilename` (string) - Output filename.
  
  - `threshold_i` (float, optional) - Threshold temperatures in Kelvin (See supl. material of [paper](link) for more information).
  - `last_T` (float, optional) - Last temperature point that accounts for MGDD (See supl. material of [paper](link) for more information).
  
  - `output file format` - "MGDD \t Lon \t Lat"
  
* `compute_CDD_HS(infilename, outfilename; T_base=279.15)`

  Computes the *Cold Degree Days* for the South Hemisphere over the given input data (which must be in GRIB format).
  
  - `infilename` (string) - Input filename
  - `outfilename` (string) - Output filename.
  
  - `T_base` (float, optional) - Base temperature for the CDD computation (See supl. material of [paper](link) for more information).
  
  - `output file format` - "CDD \t Lon \t Lat"
  
* `compute_CDD_HN(infilename_1, infilename_2, outfilename; T_base=279.15)`
  
  Computes the *Cold Degree Days* for the North Hemisphere over the given input data (which must be in GRIB format). As winter periods comprehend two years there are two input filenames.
  
  - `infilename_2` (string) - First input filename (Nov and Dec of first year)
  - `infilename_2` (string) - Second input filename (Jan, Feb and Mar of second year)
  - `outfilename` (string) - Output filename.
  
  - `T_base` (float, optional) - Base temperature for the CDD computation (See supl. material of [paper](link) for more information).
  
  - `output file format` - "CDD \t Lon \t Lat"
  
* `compute_avg_GDD(infilenames, outfilename)`
  
  Computes the averaged GDD (CDD or MGDD) over the period given as input filenames.
  
  - `infilenames` (array) - Array of input filenames which contain the data to average.
  - `outfilename` (string) - Output filename.
  
  - `output file format` - "Avg_GDD \t Lon \t Lat"
  
* `compute_min_avg_T_min(infilename, outfilename)`

  Computes the average minimum temperature of the coldest month for the given input period.
   
   - `infilename` (string) - Input filename
   - `outfilename` (string) - Output filename
   
   - `output file format` - "Min_avg_T_min \t Lon \t Lat"
   
* `compute_avg_T(infilename, outfilename)`

  Computes the average temperature of the given imput period
  
  - `infilename` (string) - Input filename
  - `outfilename`(string) - Output filename
  
  - `output file format` - "Avg_T \t Lon \t Lat"
  
# Examples

# Authors
* **Alex Giménez-Romero**

# License
This project is licensed under the GNU General Public License v3.0 - see the [LICENSE.md](https://github.com/agimenezromero/ERA5-Land-data-analysis/blob/main/LICENSE) file for details

