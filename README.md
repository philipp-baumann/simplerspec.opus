
<!-- README.md is generated from README.Rmd. Please edit that file -->
simplerspec.opus
================

The goal of simplerspec.opus is to facilitate spectral measurement workflows for Bruker spectrometers. It is tailored to file outputs and directory structures produced by the Bruker OPUS and OPUS Lab software.

Installation
------------

You can install the most recent version of simplerspec.opus using

``` r
devtools::install_github("philipp-baumann/simplerspec.opus")
```

Perform folder, file, and data and metadata integrity tests
-----------------------------------------------------------

This test verifies whether both metadata and data are complete, for each of the "date" folders separately. The binary Bruker OPUS files, that contain all spectral measurements and aquisition parameters, are located in folders named by date. The names are unluckily in the format `"YYYY_M_D"`. That does not follow
[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

``` r
# Load packages
pkgs <- c("simplerspec.opus", "here")
lapply(pkgs, library, character.only = TRUE)
```

You can do the test using the function below:

``` r
struct_data_metadata(data_root = here("data", "spectra", "2018-BDM"))
```

This final check ensures that there is a minimum chance that samples were wrongly labelled or measurement positions were swapped.

### Test category 1:

-   **Expectation:** Metadata are Open Office spreadsheets with extension `.ods`. The file name has a date prefix of format `YYYY-MM-DD`

-   **Test:** Test whether every measurement folder has a corresponding metadata file. This is done by extracting `"YYYY_M_D"` from the metadata file names, and converting to `"YYYY-MM-DD"`, and check if metadata file name information and and folder names are all identical.

### Test category 2:

-   **Expectation:** Every metadata spreadsheet has a `date` column with header named identically, where entries represent dates in format `YYYY-MM-DD`.

-   **Test:** Test whether the `date` column entries are matching the expanded measurement folders for each date.

### Tests category 3: All OPUS files have the correct complete metadata and vice versa

-   **Expectations:**: The metadata spreadsheets contain a column `pos` that specifies the well plate positions, and also a column `sample_id`. The `sample_id` of a sample needs to be unique. The OPUS files that are generated after measurements have a `_<plate-position>` suffix after a `sample_id` string.

-   **Test:**: In order to check that there is a metadata entry for each measured file, file names are reconstructed from `sample_id` and `pos` information. Further, repetition numbers starting from 0 are sequentially numbered by an integer increasing by 1, are reconstructed as file extension
    `.<repetition_number>`. This is what is expected to be generated from the measurements managed by the Bruker OPUS lab interface.
