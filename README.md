# ESM dataset usage examples

This project aims at collecting usage examples for earth system model output.
While the examples are somewhat artificial, they are oriented along real scientific analysis use cases for earth system output.

Currently, the examples mainly focus on Hierarchical HEALPix output [as described on easy.gems](https://easy.gems.dkrz.de/Processing/healpix/index.html).
The examples are given as executable code and use data which is publicly available.
Most examples will use the `ngc4008` dataset from nextGEMS ([](doi:10.5194/egusphere-2025-509)), hosted on [DKRZ's S3 Storage](https://docs.dkrz.de/doc/datastorage/minio/index.html), which provides some large storage capacity based on repurposed harddrives from their previous supercomputer [Mistral](https://www.dkrz.de/en/communication/news-archive/new-supercomputer-at-dkrz-starts-operation-mistral).

We provide some timing information, which can be used as a rough estimate for the time required to retrieve the data for a given use case.
On the published website, the timing is taken from executing the code in a Github Action, without any specialized runner.
In particular, this runner is not located on DKRZ premises, so data will be accessed over the internet.

## running

```
uv run myst start --execute
```

## timing

::::{grid} 1 2 2 2

```{card} Timeseries at single point
:link: timeseries_at_point.md
:footer: ![](#timeseries_point_timing)

![](#timeseries_point_plot)
```

```{card} latitude-time diagram
:link: lat_vs_time.md
:footer: ![](#lat_vs_time_timing)

![](#lat_vs_time_plot)
```

```{card} Yearly mean over Europe
:link: year_over_europe.md
:footer: ![](#year_over_europe_timing)

![](#year_over_europe_plot)
```

```{card} year_global.md
:link: year_global.md
:footer: ![](#year_global_timing)

![](#year_global_plot)
```

```{card} Extract data based on shapefiles
:link: extract_country.md
:footer: ![](#extract_country_timing)

![](#extract_country_plot)
```
::::
