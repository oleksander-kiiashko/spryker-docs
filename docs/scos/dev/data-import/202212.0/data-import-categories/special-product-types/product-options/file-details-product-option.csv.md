---
title: File details- product_option.csv
last_updated: Jun 23, 2021
template: data-import-template
originalLink: https://documentation.spryker.com/2021080/docs/file-details-product-optioncsv
originalArticleId: baab3597-94b0-47b1-825c-65c0d369f529
redirect_from:
  - /2021080/docs/file-details-product-optioncsv
  - /2021080/docs/en/file-details-product-optioncsv
  - /docs/file-details-product-optioncsv
  - /docs/en/file-details-product-optioncsv
  - /docs/scos/dev/data-import/201811.0/data-import-categories/special-product-types/product-options/file-details-product-option.csv.html
  - /docs/scos/dev/data-import/201903.0/data-import-categories/special-product-types/product-options/file-details-product-option.csv.html
  - /docs/scos/dev/data-import/201907.0/data-import-categories/special-product-types/product-options/file-details-product-option.csv.html
---

This document describes the `product_option.csv` file to configure [product option](/docs/scos/user/features/{{page.version}}/product-options-feature-overview.html) information in your Spryker Demo Shop.

To import the file, run

```bash
data:import:product-option
```

## Import file parameters

The file should have the following parameters:

| PARAMETER | REQUIRED | TYPE | REQUIREMENTS OR COMMENTS | DESCRIPTION |
| --- | --- | --- | --- | --- |
| abstract_product_skus |  | String | | List of Abstract Product SKUs separated by comma. |
| option_group_id | &check; | String |If doesn't exist then it will be automatically created.  | Identifier of the Product Option Group. |
| tax_set_name |  | String || Name of the tax set. |
| group_name_translation_key|  | String | |  Translation key of the name of the group in different locales.|
| group_name.{ANY_LOCALE_NAME}<br>Example value: *group_name.en_US* | No | String |Locale data is dynamic in data importers. It means that ANY_LOCALE_NAME postfix can be changed, removed, and any number of columns with different locales can be added to the CSV files. | Name of the group in the specified locale (US for our example). |
| option_name_translation_key |  | String | | Translation key of the name of the option in different locales. |
| option_name.{ANY_LOCALE_NAME}<br>Example value: *option_name.en_US |  | String || Name of the option in the specified locale (US for our example).  |
| sku | | String | | SKU identifier of the product option. |
| avalara_tax_code |  | String | | [Avalara tax code](/docs/pbc/all/tax-management/{{page.version}}/tax-management.html#avalara-system-for-automated-tax-compliance) for automated tax calculation. |


## Dependencies

This file has the following dependencies: [product_abstract.csv](/docs/scos/dev/data-import/{{page.version}}/data-import-categories/catalog-setup/products/file-details-product-abstract.csv.html)

## Import template file and content example

Find the template and an example of the file below:

| FILE | DESCRIPTION |
| --- | --- |
| [template_product_option.csv](https://spryker.s3.eu-central-1.amazonaws.com/docs/Developer+Guide/Back-End/Data+Manipulation/Data+Ingestion/Data+Import/Data+Import+Categories/Special+Product+Types/202109.0/Template_product_option.csv) | Exemplary import file with headers only. |
| [product_option.csv](https://spryker.s3.eu-central-1.amazonaws.com/docs/Developer+Guide/Back-End/Data+Manipulation/Data+Ingestion/Data+Import/Data+Import+Categories/Special+Product+Types/202109.0/product_option.csv) | Exemplary import file with the Demo Shop data. |
