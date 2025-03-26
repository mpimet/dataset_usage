---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# latitude-time diagram

```{code-cell} python
import xarray as xr
import easygems.healpix as egh
```

```{code-cell} python
#| label: lat_vs_time_timing
%%time
ngc4008 = xr.open_dataset("https://s3.eu-dkrz-1.dkrz.cloud/nextgems/rechunked_ngc4008/ngc4008_P1D_5.zarr",
                          engine="zarr").pipe(egh.attach_coords)
data = (ngc4008.prw
  .sel(time=slice("2021", "2025"))
  .where(lambda ds: (ds.lon > 300) & (ds.lon < 360), drop=True)
  .resample(time="3D")
  .mean(dim="time")
  .groupby("lat")
  .mean()
)
```

```{code-cell} python
#| label: lat_vs_time_plot
data.plot(x="time")
```