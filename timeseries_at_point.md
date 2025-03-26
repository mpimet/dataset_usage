---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# 5 year timeseries at point

**Goal:** Show a 5 year long timeseries of a single location (Hamburg) based on the highest spatial and temporal resolution of the dataset.
In this case, it's healpix zoom 9 (≈ 10km) spatial and 15 minutes temporal resolution.

```{code-cell} python
import xarray as xr
import healpix as hp
```

```{code-cell} python
hh = [9.993333, 53.550556]
tstart = "20210101"
tend = "20260101"
```

```{code-cell} python
#| label: timeseries_point_timing
%%time
ngc4008 = xr.open_dataset("https://s3.eu-dkrz-1.dkrz.cloud/nextgems/rechunked_ngc4008/ngc4008_PT15M_9.zarr",
                          chunks={"time":19200},
                          engine="zarr")
ngc_tas = ngc4008.tas.sel(
    time=slice(tstart, tend),
    cell=hp.ang2pix(hp.order2nside(9),
                    *hh[::-1],
                    lonlat=True,
                    nest=True)).load()
```

The retrieved timeseries contains {eval}`len(ngc_tas)` data points.

```{code-cell} python
#| label: timeseries_point_plot
ngc_tas.plot()
```
