# Best practices

The following is a list of general advice on how to help your module interact seamlessly with others.

## Configuration files

1. These files should always be validated to detect invalid user settings following the latest [JSON-schema specification](https://json-schema.org/).
In some cases, it might be necessary to write additional tests to catch problems that regular JSON-schemas cannot guard against.
1. Configuration settings should state units explicitly in their naming.

    ???+ example "Naming configuration variables clearly"

        ```yaml
        maximum_installable_mw_per_km2: # (1)!
            pv_tilted: 160
            pv_flat: 80
        maximum_roof_ratio: 0.80 # (2)!
        ```

        1. Sections should state the unit of their subitems.
        2. Use clear names even for unitless cases like ratios.

### Common configuration structures

We recommend the following structures for commonly seen configuration settings.

1. Request users to specify their coordinate reference systems of choice, and group these under the `crs` key.

    ```yaml
    crs:
      geographic: "EPSG:4326" # (1)!
      projected: "EPSG:3035" # (2)!
    ```

    1. Geographic operations where preserving position matters.
    1. Projected operations where preserving distance or area matters.

1. If your module supports a flexible temporal scope, define the date range using an inclusive start date and an exclusive end date.

    The range below includes data from `2017-01-01` up to, but not including, `2018-01-01`.

    ```yaml
    temporal_scope:
      start: "2017-01-01" # (1)!
      end: "2018-01-01" # (2)!
    ```

    1. Will be included in data.
    2. Won't be included in the data.

## Tabular and Vector GIS data

1. We _highly_ recommend the use of [Apache Parquet](https://parquet.apache.org/) (`.parquet`) over other formats (such as `.csv`).
    - Very good read/write performance
    - High storage efficiency and [metadata compatibility](https://parquet.apache.org/docs/file-format/metadata/).
    - Supports geospatial data thanks to the [Geoparquet standard](https://geoparquet.org/).
1. Follow [tidy data](https://vita.had.co.nz/papers/tidy-data.pdf) principles to make data machine-readable (prefer "long" tables over "wide" ones when applicable).

    ???+ example "Example of a tidy table"

        | year          | country_id       | shape_id         | demand_mwh   |
        |---------------|------------------|------------------|--------------|
        | 2020          | ITA              | North            | 4500         |
        | 2020          | ITA              | East             | 4800         |
        | 2020          | ITA              | South            | 3000         |

1. Embed important metadata within the file instead of saving a separate file.
The following metadata values are often useful:
    - `units`: a dictionary specifying per-column units, using `no_unit` for unitless cases.
    - `source`: a string specifying the source and/or author of a dataset.
    - `license`: a string specifying the license of the dataset.

    ??? example "Embedding metadata in `pandas`"

        `pandas` will automatically convert data in `df.attrs` into [file-level metadata](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.attrs.html#pandas.DataFrame.attrs) when saving to `.parquet`:

        ```python
        dataframe.attrs["units"] = {
            "year": "yr",
            "country_id": "no_unit",
            "shape_id": "no_unit",
            "demand": "mwh"
            }
        dataframe.attrs["source"] = "github.com/modelblocks-org/docs"
        dataframe.attrs["license"] = "CC-BY-4.0"
        dataframe.to_parquet('my_data.parquet')
        ```

## Other GIS data formats

1. **Raster data**: we prefer to use GeoTIFF (`.tiff`, `.tif`) files.
1. **Gridded data**: we prefer to use [netCDF](https://www.unidata.ucar.edu/software/netcdf/) (`.nc`) files.

## Metadata recommendations

1. Use snake case (`foo_bar`) for headers, keys, indexes, variables, etc. Avoid hyphens (`foo-bar`) and camel case (`FooBar`).
1. For timeseries data, follow [ISO 8601 UTC](https://en.wikipedia.org/wiki/ISO_8601) (e.g., 2024-08-01T15:00:00Z) specifications to avoid date time problems.
1. For national / subnational data:
    1. Country IDs should always be under the `country_id` naming and follow [ISO 3166-1 alpha-3](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) (e.g., CHE, CHN, GBR, MEX, etc).
    1. Unique identifiers related to specific polygons should be under the `shape_id` naming.
    This applies even in cases where national resolution is requested (i.e., `country_id`  and `shape_id` should match).

    ???+ example "Example of tabular subnational data"

        | country_id | shape_id | demand_mwh   |
        |------------|----------|--------------|
        | DEU        | DE13     | 4500         |
        | DEU        | DE14     | 4800         |
        | ITA        | ITA0     | 20000        |

1. For spatial data:
    1. Use `longitude` | `latitude` to express position and avoid ambiguous values like `x` | `y`.
    1. Make sure to save the CRS with the spatial data.
    This is guaranteed with the [recommended file types][file-specific-recommendations] for GIS data.
1. For currency data: currency codes must follow [ISO 4217 alpha-3](https://en.wikipedia.org/wiki/ISO_4217) codes in combination with the year of the currency (e.g., CHF2024, EUR2015, USD2020) to allow for inflation adjustments.
