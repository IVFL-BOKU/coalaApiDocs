# coalaApiDocs

## Postaman templates for COALA REST API endpoints

* [For the NNI and MZM indicators](https://raw.githubusercontent.com/IVFL-BOKU/coalaApiDocs/main/postman_template_mzm_nni.json)

## Recipes

### Debug intermidiate steps of the MZM indicator computation

* Prepare an ordinary mzm request specifying the time period (`dateFrom` and `dateTo` parameters) and area to be processed (e.g. with `geometry` and `srcSrid` parameters)
* Set output format to TIFF with the `format=image/tiff` parameter.
* Add the `debug={stage}` parameter where `{stage}` is one of:
  * `ndvi` to get the daily interpolated NDVI data
  * `et` to get the daily et0 values
  * `startend` to the get the computed vegetation period (according to `startValue`, `endValue` and `minPeriodLength` request parameters) for each pixel. The first band provides the number of days since `dateFrom` until the beginning of the vegetation period and the second band the number of days since `dateFrom` to the end of the vegetation perdiod. E.g. if `dateFrom=2020-10-01`, first band has a value of 10 and second band a value of 70, then the vegetation period of this pixel was between 2020-10-11 and 2020-12-20.
  * `raw` MZM values between standarization (before applying the `mzm = k * ((100 * mzm) / mean(mzm)) - 100` transformation) - a sum of `(slope * NDVI + const) * ET0` over the vegetation period.

### Debug intermidiate steps of the NNI indicator computation


* Prepare an ordinary mzm request specifying the time period (`dateFrom` and `dateTo` parameters) and area to be processed (e.g. with `geometry` and `srcSrid` parameters)
* Set output format to TIFF with the `format=image/tiff` parameter.
* Add the `debug={stage}` parameter where `{stage}` is one of:
  * `mtci` to get MTCI value
  * `mtci2` to get the nominator of the NNI fraction - `m1 * MTCI + b1`
  * `ndvi` to get the daily interpolated NDVI data for the `dateFrom` - `dateTo` period
  * `et` to get the daily et0 values for the `dateFrom` - `dateTo` period
  * `biomass` to get the biomass estimates computed as cumulated daily `a3 * (andvi * NDVI + bndvi) * ET0 + b3`
  * `biomass2` to get the denominator of the NNI fraction - `a2 * biomass^(1 - m2)`
