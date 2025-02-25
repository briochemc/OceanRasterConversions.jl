# OceanRasterConversions.jl documentation

## Overview

This package converts ocean varaibles that are saved as `Raster` data structures using [GibbsSeaWater.jl](https://github.com/TEOS-10/GibbsSeaWater.jl).
[Rasters.jl](https://github.com/rafaqz/Rasters.jl) provides excellent reading, writing and manipulation of geospatial data.
Typically, the salt and temperature variables from ocean models or observational data are practical salinity and potential temperature so conversions must be to the TEOS-10 standard variables of absolute salinity and conservative temperature to accurately calculate further variables like seawater density.

Further conversions and other water mass transformation procedures will be added in the future.

## Package workings

This package will convert the variables practical salinity and potential temperature into absolute salinity and conservative temperature.
In doing so a pressure variable is needed, so this is created and returned in the `RasterStack`.
Lastly a density variable (either in-situ or potential referenced to a user input) is computed and added to the `RasterStack`.
See the example for how the package can be used.

### Variables

The variables are named using the symbols that represent them.
The symbols are unicode characters which can be generated in the julia repl by pressing tab after the varible

```julia
julia> \theta#press tab
```

will autocomplete to `θ`, the symbol for potential temperature.
The subscript letters that are used to distinguish between practical salinity, `Sₚ`, and absolute salinity, `Sₐ`, are also added in the julia repl

```julia
julia> S\_a#press tab
```

Currently the varabile symbols are:

- `θ` potential temperature
- `Θ` conservative temperature
- `Sₚ` practical salinity
- `Sₐ` absolute salinity
- `p` pressure
- `ρ` in-situ seawater density
- `σₚ` potential density at user defined reference pressure `ₚ`.

### Limitations

If the required dimensions for the conversions are not present an error will be thrown.
For example, trying to convert a `RasterStack` that has no depth dimension will not work as the `Z` dimension is not found and the pressure variable depends on depth.
There is a manual workaround for this.
When defining the `RasterStack` add the `Z` dimension as a single entry, rather than a `Vector`,

```julia
lons, lats, z = -180:180, -90:90, 0.0
stack = RasterStack(data, (X(lons), Y(lats), Z(z)))
```

This is equivalent to a two dimensional `RasterStack` at sea-surface height (z = 0).

Currently the only dimension names that are supported are `X`, `Y`, `Z`, and `Ti`.
Allowing for user specified dimensions has not yet been implemented.

## Functions exported from `OceanRasterConversions`

```@docs
convert_ocean_vars
depth_to_pressure
Sₚ_to_Sₐ
θ_to_Θ
```
