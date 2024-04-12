# Parcel

A simple collection of scripts for rapidly computing air parcel backtrajectories using meteorlogical data from the ECMWF.


- [ECMWF Copernicus Data Store API Setup](https://cds.climate.copernicus.eu/api-how-to)
- [ERA Global Reanalysis](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-complete?tab=form)
- [ECMWF IFS Documentation](https://www.ecmwf.int/en/publications/ifs-documentation)
- [Global Data Assimilation System (NOAA)](https://www.ncei.noaa.gov/access/metadata/landing-page/bin/iso?id=gov.noaa.ncdc:C00379)
- [Prabuddha's Code for Meteorlogical Data Collection](https://github.com/mi3nts/pmModeling/blob/main/modelTraining/src/meteo_AOD_openAQ_down.py)
- [Full List of ECMWF ERA5 Variables](https://confluence.ecmwf.int/display/CKB/ERA5%3A+data+documentation#ERA5:datadocumentation-Parameterlistings)
- [Conversion of Model Levels to Altitude]()

Relevant [variables](https://confluence.ecmwf.int/display/CKB/ERA5%3A+data+documentation#ERA5:datadocumentation-Parameterlistings) to download:

| Name | Units | CDS Name | short name | param id | level type |
| ------- | -- | ----- | --- | - | - |
| [Geopotential](https://apps.ecmwf.int/codes/grib/param-db/129) | $m^2 \cdot s^{-2}$ | geopotential | z | 129 | sfc |
| [Surface pressure](https://apps.ecmwf.int/codes/grib/param-db/134) | $\text{Pa}$ | surface_pressure | sp | 134 | sfc |
| [Boundary Layer Height](https://apps.ecmwf.int/codes/grib/param-db/159) | $m$ | boundary_layer_height | blh | 159 | sfc |
| [10 meter U wind component](https://apps.ecmwf.int/codes/grib/param-db/165) | $m \cdot s^{-1}$ | 10m_u_component_of_wind | 10u | 165 | sfc |
| [10 meter V wind component](https://apps.ecmwf.int/codes/grib/param-db/166) | $m \cdot s^{-1}$ | 10m_v_component_of_wind | 10v | 166 | sfc |
| [2 meter temperature](https://apps.ecmwf.int/codes/grib/param-db/167) | $K$ | 2m_temperature | 2t | 167 | sfc |
| [2 meter dewpoint temperature](https://apps.ecmwf.int/codes/grib/param-db/168) | $K$ | 2m_dewpoint_temperature | 2d | 168 | sfc |
| [100 meter U wind component](https://apps.ecmwf.int/codes/grib/param-db/228246) | $m \cdot s^{-1}$ | 100m_u_component_of_wind | 100u | 228246 | sfc |
| [100 meter V wind component](https://apps.ecmwf.int/codes/grib/param-db/228247) | $m \cdot s^{-1}$ | 100m_v_component_of_wind | 100v | 228247 | sfc |
| [Geopotential](https://codes.ecmwf.int/grib/param-db/129) | $m^{2} s^{-2}$ | geopotential | z | 129 | pl |
| [Temperature](https://codes.ecmwf.int/grib/param-db/130) | $K$ | temperature | t | 130 | pl |
| [U component of wind](https://codes.ecmwf.int/grib/param-db/131) | $m \cdot s^{-1}$ | u_component_of_wind | u | 131 | pl |
| [V component of wind](https://codes.ecmwf.int/grib/param-db/132) | $m \cdot s^{-1}$ | v_component_of_wind | v | 132 | pl |
| [Specific Humidity](https://codes.ecmwf.int/grib/param-db/133) | $\text{kg}\cdot\text{kg}^{-1}$ | specific_humidity | q | 133 | pl |
| [Vertical Velocity](https://codes.ecmwf.int/grib/param-db/135) | $\text{Pa}\cdot s^{-1}$ | vertical_velocity | w | 135 | pl |
| [Relative Humidity](https://codes.ecmwf.int/grib/param-db/157) | % | relative_humidity | r | 157 | pl |
| [Eta-coordinate vertical velocity](https://codes.ecmwf.int/grib/param-db/77) | $s^{-1}$ |  | etadot | 77 | ml |
| [Geopotential (at level 1)](https://codes.ecmwf.int/grib/param-db/129) | $m^2 \cdot s^{-2}$ | | z | 129 | ml |
| [Temperature](https://codes.ecmwf.int/grib/param-db/130) | $K$ | | t | 130 | ml |
| [U component of wind](https://codes.ecmwf.int/grib/param-db/131) | $m \cdot s^{-1}$ | | u | 131 | ml |
| [V component of wind](https://codes.ecmwf.int/grib/param-db/132) | $m \cdot s^{-1}$ | v | | 132 | ml |
| [Specific Humidity](https://codes.ecmwf.int/grib/param-db/133) | $\text{kg}\cdot\text{kg}^{-1}$ | | q | 133 | ml |
| [Vertical Velocity](https://codes.ecmwf.int/grib/param-db/135) | $\text{Pa}\cdot s^{-1}$ | | w | 135 | ml |
| [Logarithm of surface pressure](https://codes.ecmwf.int/grib/param-db/152) |  | | lnsp | 152 | ml |



Level Type:
 - sfc = surface
 - pl = pressure level
 - ml = model level



