---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

```{code-cell} python
import numpy as np
import xarray as xr
import easygems.healpix as egh
import matplotlib.pylab as plt
import cmocean
import cartopy.crs as ccrs
```

```{code-cell} python
lon1 = -10
lon2 = 30
lat1 = 30
lat2 = 60
tstart = "20210101"
tend = "20220101"
```

```{code-cell} python
#| label: year_over_europe_timing 
%%time
ngc4008 = xr.open_dataset("https://s3.eu-dkrz-1.dkrz.cloud/nextgems/rechunked_ngc4008/ngc4008_P1D_9.zarr", chunks='auto', engine="zarr").pipe(egh.attach_coords)


cells = np.where(
    (ngc4008.lat >= lat1) &
    (ngc4008.lat <= lat2) &
    ((((ngc4008.lon + 180) % 360) - 180) >= lon1) &
    ((((ngc4008.lon + 180) % 360) - 180) <= lon2))[0]
ngc_tas = ngc4008.tas.sel(time=slice(tstart, tend), cell=cells).mean("time").load()
```

```{code-cell} python
#| label: year_over_europe_plot
projection = ccrs.Robinson(central_longitude=10)
fig, ax = plt.subplots(
    figsize=(8, 4), subplot_kw={"projection": projection}, constrained_layout=True
)
ax.set_extent([lon1, lat1, lon2, lat2], crs=ccrs.PlateCarree())

egh.healpix_show(ngc_tas, ax=ax, cmap=cmocean.cm.thermal)
```
