# Parcel

A simple collection of scripts for rapidly computing air parcel backtrajectories using meteorlogical data from the ECMWF.


- [ECMWF Copernicus Data Store API Setup](https://cds.climate.copernicus.eu/api-how-to)
- [ERA Global Reanalysis](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-complete?tab=form)
- [ECMWF IFS Documentation](https://www.ecmwf.int/en/publications/ifs-documentation)
- [Global Data Assimilation System (NOAA)](https://www.ncei.noaa.gov/access/metadata/landing-page/bin/iso?id=gov.noaa.ncdc:C00379)
- [Prabuddha's Code for Meteorlogical Data Collection](https://github.com/mi3nts/pmModeling/blob/main/modelTraining/src/meteo_AOD_openAQ_down.py)
- [Full List of ECMWF ERA5 Variables](https://confluence.ecmwf.int/display/CKB/ERA5%3A+data+documentation#ERA5:datadocumentation-Parameterlistings)
- [Conversion of Model Levels to geopotential and geometric height](https://confluence.ecmwf.int/display/CKB/ERA5%3A+compute+pressure+and+geopotential+on+model+levels%2C+geopotential+height+and+geometric+height)

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
| [V component of wind](https://codes.ecmwf.int/grib/param-db/132) | $m \cdot s^{-1}$ | | v | 132 | ml |
| [Specific Humidity](https://codes.ecmwf.int/grib/param-db/133) | $\text{kg}\cdot\text{kg}^{-1}$ | | q | 133 | ml |
| [Vertical Velocity](https://codes.ecmwf.int/grib/param-db/135) | $\text{Pa}\cdot s^{-1}$ | | w | 135 | ml |
| [Logarithm of surface pressure](https://codes.ecmwf.int/grib/param-db/152) |  | | lnsp | 152 | ml |



Level Type:
 - sfc = surface
 - pl = pressure level
 - ml = model level




# Back-Trajectory Analysis
The concentration of species $i$ of an air parcel, $c_i$, is described by the [Reaction-Advection-Diffusion Equation](https://en.wikipedia.org/wiki/Convection%E2%80%93diffusion_equation) which fundamentally boils down to conservation of mass: 
$$ \frac{\partial c_i}{\partial t} = \nabla \cdot \left( D \nabla c_i - \mathbf{v}c_i \right) + R $$
where

- $D$ is the diffusivity (concentration moves from high to low according to [Ficks law](https://en.wikipedia.org/wiki/Fick%27s_laws_of_diffusion))
- $\mathbf{v}$ is the fluid velocity (i.e. $\mathbf{v}c_i$ is flux of species $i$ that is dragged along with the fluid)
- $R$ represent fixed sources and sinks (i.e. production/loss by chemical reactions)

There is a tradeoff between the strength of advection and diffusion depending on the relative magnitudes of $\mathbf{v}$ and $D$. For simple back trajectories, ignoring diffusion and assuming all species are in thermochemical equilibrium further simplifies the equations to 
$$  \frac{\partial c_i}{\partial t} + \nabla \cdot (\mathbf{v}c_i) = 0 \\
    \frac{\partial c_i}{\partial t} + \nalbda c_i \cdot \mathbf{v} + (\nabla \cdot v) c_i = 0 $$
treating the fluid as incompressible so that $\nabla \cdot \mathbf{v} = 0$ together with the assumption that the concentrations are steady so that $\partial_t c_i = $ reduces the entire system to
$$ \mathbf{v}\cdot\nabla c_i = 0 $$
