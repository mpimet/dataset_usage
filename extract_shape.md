---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

```{code-cell} python
import cartopy.crs as ccrs
import cartopy.feature as cf
import easygems.healpix as egh
import matplotlib.pyplot as plt
import numpy as np
import shapefile
import shapely
import xarray as xr
from cartopy.io import shapereader


def get_shapes(resolution="10m"):
    """Return a dictionary that maps ISO A3 country codes to shapely geometries."""
    shpfilename = shapereader.natural_earth(resolution, "cultural", "admin_0_countries")

    records = shapefile.Reader(shpfilename).records()
    shapes = shapefile.Reader(shpfilename).shapes()

    return {r["ISO_A3_EH"]: shapely.geometry.shape(s) for r, s in zip(records, shapes)}
```


```{code-cell} python
#| label: extract_country_timing
%%time
# Load ICON online catalog
ngc4008 = xr.open_dataset("https://s3.eu-dkrz-1.dkrz.cloud/nextgems/rechunked_ngc4008/ngc4008_P1D_9.zarr", chunks='auto', engine="zarr").pipe(egh.attach_coords)

# Select regional polygon (e.g. Portugal) from HEALPix grid
shapes = get_shapes()
polygon = shapes["PRT"]

icell = np.where(
    shapely.contains_xy(polygon, ((ngc4008.lon.values - 180) % 360) - 180, ngc4008.lat.values)
)[0]

ngc_tas = ngc4008["tas"].sel(cell=icell, time="2025").mean("time").load()
```

```{code-cell} python
#| label: extract_country_plot
# Plot regional cutout
fig, ax = plt.subplots(figsize=(4, 5), subplot_kw={"projection": ccrs.PlateCarree()})
ax.set_extent([-11, -5, 36, 43])
ax.coastlines()
ax.add_feature(cf.BORDERS)
egh.healpix_show(ngc_tas)
```
