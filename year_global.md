---
kernelspec:
  name: python3
  display_name: 'Python 3'
---

# Yearly mean, global

**Goal:** Show the yearly averaged global surface temperature in 2021.

```{code-cell} python
import numpy as np
import xarray as xr
import easygems.healpix as egh
import cmocean
```

```{code-cell} python
tstart = "20210101"
tend = "20220101"
```

```{code-cell} python
#| label: year_global_timing
%%time
ngc4008 = xr.open_dataset("https://s3.eu-dkrz-1.dkrz.cloud/nextgems/rechunked_ngc4008/ngc4008_P1D_7.zarr", chunks='auto', engine="zarr")
ngc_tas = ngc4008.tas.sel(time=slice(tstart, tend)).mean("time").load()
```

```{code-cell} python
#| label: year_global_plot
egh.healpix_show(ngc_tas, cmap=cmocean.cm.thermal);
```
