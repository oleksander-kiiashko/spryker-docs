---
title: Float Stock for Products
last_updated: Jun 16, 2021
template: howto-guide-template
originalLink: https://documentation.spryker.com/2021080/docs/float-stock-for-products
originalArticleId: 741857cb-b4b4-4132-afbf-1aa5171cf35d
redirect_from:
  - /2021080/docs/float-stock-for-products
  - /2021080/docs/en/float-stock-for-products
  - /docs/float-stock-for-products
  - /docs/en/float-stock-for-products
  - /v6/docs/float-stock-for-products
  - /v6/docs/en/float-stock-for-products
  - /v5/docs/float-stock-for-products
  - /v5/docs/en/float-stock-for-products
  - /v4/docs/float-stock-for-products
  - /v4/docs/en/float-stock-for-products
  - /v3/docs/float-stock-for-products
  - /v3/docs/en/float-stock-for-products
  - /v2/docs/float-stock-for-products
  - /v2/docs/en/float-stock-for-products
  - /v1/docs/float-stock-for-products
  - /v1/docs/en/float-stock-for-products
related:
  - title: CRUD Scheduled Prices migration concept
    link: docs/pbc/all/price-management/page.version/install-and-upgrade/upgrade-modules/upgrade-to-crud-scheduled-prices.html
  - title: Decimal Stock migration concept
    link: docs/scos/dev/migration-concepts/decimal-stock-migration-concept.html
  - title: Migrating from Twig v1 to Twig v3
    link: docs/scos/dev/migration-concepts/migrating-from-twig-v1-to-twig-v3.html
  - title: Split Delivery migration concept
    link: docs/scos/dev/migration-concepts/split-delivery-migration-concept.html
  - title: Silex Replacement migration concept
    link: docs/scos/dev/migration-concepts/silex-replacement/silex-replacement.html
---

## Float stock migration

We have changed the type of stock and quantity fields from int to float. With this change, we allow to manage fractions of items in the system.

As stock and quantity are very basic concepts of any commerce system, changing their type is a horizontal barrier for the modules. This means that, if a module is upgraded to a version that uses float stock, all the other modules in the project which are involved in the float stock barrier must be upgraded to avoid accidental type incompatibilities. For example, if ModuleX uses float stock and ModuleY uses int stock and there is no hard dependency between them, it’s technically possible to upgrade only one of them, however, during runtime, both modules may happen to process the same request data and, as a result, the response will either be calculated incorrectly or a fatal error will be thrown due to a data type mismatch.

In case of upgrading one of the modules involved into the float stock concept, we strongly recommend upgrading all the modules in your project from the list below. Mostly, the migrations have very low or even zero effort, because they only break type of a transfer object or an internal function’s signature. Some modules changed database field types to DOUBLE which is automatically upgradable by running the necessary console commands.

{% info_block errorBox %}

If you upgrade to the float stock concept, you should also review your project code and make sure to adapt all stock and quantity related customizations and implementations to use float type accordingly. If you have any partner or ECO integrations, they might have to be adjusted too.

{% endinfo_block %}

Here are some typical occurrences of working with stocks and quantities and tips to make them compatible with the float type:

* Product stock and availability management - use `UtilQuantityService` to handle arithmetic operations with float quantity values.
* Cart item management - use `UtilQuantityService` to handle arithmetic operations with float quantity values.
* Price calculations - use `UtilPriceService` to handle rounding of price values in a centralized way and avoid handling money fractions.

## Migration Process

You can try to upgrade all affected modules together by following the steps below.

1. You can try to upgrade all affected modules together by following the steps below.

```bash
composer update "spryker/*" "spryker-shop/*"
composer require spryker/availability:"^7.0.0" spryker/availability-cart-connector:"^5.0.0" spryker/availability-gui:"^4.0.0" spryker/availability-offer-connector:"^2.0.0" spryker/cart:"^6.0.0" spryker/cart-extension:"^3.0.0" spryker/carts-rest-api:"^4.0.0" spryker/checkout:"^5.0.0" spryker/discount:"^8.0.0" spryker/discount-promotion:"^2.0.0" spryker/manual-order-entry-gui:"^0.6.0" spryker/offer:"^0.2.0" spryker/offer-gui:"^0.2.0" spryker/oms:"^9.0.0" spryker/orders-rest-api:"^2.0.0" spryker/persistent-cart:"^2.0.0" spryker/price-cart-connector:"^5.0.0" spryker/price-product:"^3.0.0" spryker/price-product-storage:"^3.0.0" spryker/price-product-volume:"^2.0.0" spryker/price-product-volume-gui:"^2.0.0" spryker/product-availabilities-rest-api:"^2.0.0" spryker/product-bundle:"^5.0.0" spryker/product-discount-connector:"^4.0.0" spryker/product-label-discount-connector:"^2.0.0" spryker/product-management:"^0.17.0" spryker/product-measurement-unit:"^3.0.0" spryker/product-option:"^7.0.0" spryker/product-option-cart-connector:"^6.0.0" spryker/product-packaging-unit:"^2.0.0" spryker/product-packaging-unit-storage:"^3.0.0" spryker/product-quantity:"^2.0.0" spryker/product-quantity-data-import:"^2.0.0" spryker/product-quantity-storage:"^2.0.0" spryker/quick-order:"^2.0.0" spryker/sales:"^9.0.0" spryker/sales-quantity:"^2.0.0" spryker/sales-split:"^4.0.0" spryker/shipment-discount-connector:"^2.0.0" spryker/shopping-list:"^3.0.0" spryker/stock:"^6.0.0" spryker/stock-sales-connector:"^4.0.0" spryker/util-price:"^1.0.0" spryker/util-quantity:"^1.0.0" spryker/wishlist:"^7.0.0" spryker-shop/cart-page:"^2.0.0" spryker-shop/customer-reorder-widget:"^5.0.0" spryker-shop/discount-promotion-widget:"^2.0.0" spryker-shop/product-detail-page:"^2.0.0" spryker-shop/product-measurement-unit-widget:"^0.7.0" spryker-shop/product-packaging-unit-widget:"^0.3.0" spryker-shop/product-search-widget:"^2.0.0" spryker-shop/quick-order-page:"^3.0.0" spryker-shop/shopping-list-page:"^0.7.0" spryker-shop/shopping-list-widget:"^0.5.0" --update-with-dependencies
```

2. Run database migration:

```bash
console propel:install
console transfer:generate
```

3. Manually upgrade the following modules:

* `ProductQuantityStorage`
* `ShoppingListWidget`

You can find the affected modules of the float stock update in the following list.

<details open><summary markdown='span'>Affected modules</summary>


| OPERATOR | OPERATOR FOR PLAIN QUERY | MIGRATION GUIDE |
| --- | --- | --- |
| spryker/availability | 7.0.0 | [Upgrade the Availability module](/docs/pbc/all/warehouse-management-system/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-availability-module.html) |
| spryker/availability-cart-connector | 5.0.0 | [Upgrade the AvailabilityCartConnector module](/docs/pbc/all/warehouse-management-system/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-availabilitycartconnector-module.html) |
| spryker/availability-gui | 4.0.0 | [Upgrade the AvailabilityGui module](/docs/pbc/all/warehouse-management-system/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-availabilitygui-module.html) |
| spryker/availability-offer-connector | 2.0.0 | [Upgrade the AvailabilityOfferConnector module](/docs/pbc/all/warehouse-management-system/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-availabilityofferconnector-module.html) |
| spryker/cart | 6.0.0 | [Upgrade the Cart module](/docs/pbc/all/cart-and-checkout/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-cart-module.html) |
| spryker/cart-extension | 3.0.0 | [Upgrade the CartExtension module](/docs/pbc/all/cart-and-checkout/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-cartextension-module.html) |
| spryker/carts-rest-api | 4.0.0 | [Upgrade the CartsRestApi module](/docs/scos/dev/module-migration-guides/glue-api/cartsrestapi-migration-guide.html) |
| spryker/checkout | 5.0.0 | [Upgrade the Checkout module](/docs/pbc/all/cart-and-checkout/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-checkout-module.html) |
| spryker/discount | 8.0.0 | [Migration Guide - Discount](/docs/pbc/all/discount-management/{{site.version}}/install-and-upgrade/upgrade-the-discount-module.html) |
| spryker/discount-promotion | 2.0.0 | [Migration Guide - DiscountPromotion](/docs/pbc/all/discount-management/{{site.version}}/install-and-upgrade/upgrade-the-discountpromotion-module.html) |
| spryker/manual-order-entry-gui | 0.6.0 | [Migration Guide - ManualOrderEntryGui](/docs/scos/dev/module-migration-guides/migration-guide-manualorderentrygui.html) |
| spryker/offer | 0.2.0 | [Migration Guide - Offer](/docs/scos/dev/module-migration-guides/migration-guide-offer.html) |
| spryker/offer-gui | 0.2.0 | [Migration Guide - OfferGui](/docs/scos/dev/module-migration-guides/migration-guide-offergui.html) |
| spryker/oms | 9.0.0 | [Migration Guide - Oms](/docs/scos/dev/module-migration-guides/migration-guide-oms.html) |
| spryker/orders-rest-api | 2.0.0 | [Migration Guide - OrdersRestApi](/docs/scos/dev/module-migration-guides/glue-api/migration-guide-ordersrestapi.html) |
| spryker/persistent-cart | 2.0.0 | [Migration Guide - PersistentCart](/docs/scos/dev/module-migration-guides/migration-guide-persistentcart.html) |
| spryker/price-cart-connector | 5.0.0 | [Migration Guide - PriceCartConnector](/docs/pbc/all/price-management/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-pricecartconnector-module.html) |
| spryker/price-product | 3.0.0 | [Migration Guide - PriceProduct](/docs/pbc/all/price-management/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-priceproduct-module.html) |
| spryker/price-product-storage | 3.0.0 | [Migration Guide - PriceProductStorage](/docs/pbc/all/price-management/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-priceproductstorage-module.html) |
| spryker/price-product-volume | 2.0.0 | [Migration Guide - PriceProductVolume](/docs/pbc/all/price-management/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-priceproductvolume-module.html) |
| spryker/price-product-volume-gui | 2.0.0 | [Migration Guide - PriceProductVolumeGui](/docs/pbc/all/price-management/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-priceproductvolumegui-module.html) |
| spryker/product-availabilities-rest-api | 2.0.0 | [Migration Guide - ProductAvailabilitiesRestApi](/docs/scos/dev/module-migration-guides/glue-api/migration-guide-productavailabilitiesrestapi.html) |
| spryker/product-bundle | 5.0.0 | [Migration Guide - ProductBundle](/docs/scos/dev/module-migration-guides/migration-guide-productbundle.html) |
| spryker/product-discount-connector | 4.0.0 | [Migration Guide - ProductDiscountConnector](/docs/scos/dev/module-migration-guides/migration-guide-productdiscountconnector.html) |
| spryker/product-label-discount-connector | 2.0.0 | [Migration Guide - ProductLabelDiscountConnector](/docs/scos/dev/module-migration-guides/migration-guide-productlabeldiscountconnector.html) |
| spryker/product-management | 0.17.0 | [Migration Guide - ProductManagement](/docs/scos/dev/module-migration-guides/migration-guide-productmanagement.html) |
| spryker/product-measurement-unit | 3.0.0 | [Migration Guide - ProductMeasurementUnit](/docs/scos/dev/module-migration-guides/migration-guide-productmeasurementunit.html) |
| spryker/product-option | 7.0.0 | [Migration Guide - ProductOption](/docs/scos/dev/module-migration-guides/migration-guide-productoption.html) |
| spryker/product-option-cart-connector | 6.0.0 | [Migration Guide - ProductOptionCartConnector](/docs/scos/dev/module-migration-guides/migration-guide-productoptioncartconnector.html) |
| spryker/product-packaging-unit | 2.0.0 | [Migration Guide - ProductPackagingUnit](/docs/scos/dev/module-migration-guides/migration-guide-productpackagingunit.html) |
| spryker/product-packaging-unit-storage | 3.0.0 | [Migration Guide - ProductPackagingUnitStorage](/docs/scos/dev/module-migration-guides/migration-guide-productpackagingunitstorage.html) |
| spryker/product-quantity | 2.0.0 | [Migration Guide - ProductQuantity](/docs/scos/dev/module-migration-guides/migration-guide-productquantity.html) |
| spryker/product-quantity-data-import | 2.0.0 | [Migration Guide - ProductQuantityDataImport](/docs/scos/dev/module-migration-guides/migration-guide-productquantitydataimport.html) |
| spryker/product-quantity-storage | 2.0.0 | [Migration Guide - ProductQuantityStorage](/docs/scos/dev/module-migration-guides/migration-guide-productquantitystorage.html) |
| spryker/quick-order | 2.0.0 | [Migration Guide - QuickOrder](/docs/scos/dev/module-migration-guides/migration-guide-quickorder.html) |
| spryker/sales | 9.0.0 | [Migration Guide - Sales](/docs/scos/dev/module-migration-guides/migration-guide-sales.html) |
| spryker/sales-quantity | 2.0.0 | [Migration Guide - SalesQuantity](/docs/scos/dev/module-migration-guides/migration-guide-salesquantity.html) |
| spryker/sales-split | 4.0.0 | [Migration Guide - SalesSplit](/docs/scos/dev/module-migration-guides/migration-guide-salessplit.html) |
| spryker/shipment-discount-connector | 2.0.0 | [Migration Guide - ShipmentDiscountConnector](/docs/pbc/all/carrier-management/{{site.version}}/install-and-upgrade/upgrade-the-shipmentdiscountconnector-module.html) |
| spryker/shopping-list | 3.0.0 | [Migration Guide - ShoppingList](/docs/pbc/all/shopping-list-and-wishlist/{{site.version}}/install-and-upgrade/upgrade-the-shoppinglistwidget-module.html) |
| spryker/stock | 6.0.0 | [Migration Guide - Stock](/docs/scos/dev/module-migration-guides/migration-guide-stock.html) |
| spryker/stock-sales-connector | 4.0.0 | [Migration Guide - StockSalesConnector](/docs/scos/dev/module-migration-guides/migration-guide-stocksalesconnector.html) |
| spryker/wishlist | 7.0.0 | [Migration Guide - WishList](/docs/pbc/all/shopping-list-and-wishlist/{{site.version}}/install-and-upgrade/upgrade-the-wishlist-module.html) |
| spryker-shop/cart-page | 2.0.0 | [Upgrade the CartPage module](/docs/pbc/all/cart-and-checkout/{{site.version}}/install-and-upgrade/upgrade-modules/upgrade-the-cartpage-module.html) |
| spryker-shop/customer-reorder-widget | 5.0.0 | [Migration Guide - CustomerReorderWidget](/docs/scos/dev/module-migration-guides/migration-guide-customerreorderwidget.html) |
| spryker-shop/discount-promotion-widget | 2.0.0 | [Migration Guide - DiscountPromotionWidget](/docs/pbc/all/discount-management/{{site.version}}/install-and-upgrade/upgrade-the-discountpromotionwidget-module.html) |
| spryker-shop/product-detail-page | 2.0.0 | [Migration Guide - ProductDetailPage](/docs/scos/dev/module-migration-guides/migration-guide-productdetailpage.html) |
| spryker-shop/product-measurement-unit-widget | 0.7.0 | [Migration Guide - ProductMeasurementUnitWidget](/docs/scos/dev/module-migration-guides/migration-guide-productmeasurementunitwidget.html) |
| spryker-shop/product-packaging-unit-widget | 0.3.0 | [Migration Guide - ProductPackagingUnitWidget](/docs/scos/dev/module-migration-guides/migration-guide-productpackagingunitwidget.html) |
| spryker-shop/product-search-widget | 2.0.0 | [Migration Guide - ProductSearchWidget](/docs/scos/dev/module-migration-guides/migration-guide-productsearchwidget.html) |
| spryker-shop/quick-order-page | 3.0.0 | [Migration Guide - QuickOrderPage](/docs/scos/dev/module-migration-guides/migration-guide-quickorderpage.html) |
| spryker-shop/shopping-list-page | 0.7.0 | [Migration Guide - ShoppingListPage](/docs/pbc/all/shopping-list-and-wishlist/{{site.version}}/install-and-upgrade/upgrade-the-shoppinglistpage-module.html) |
| spryker-shop/shopping-list-widget | 0.5.0 | [Migration Guide - ShoppingListWidget](/docs/pbc/all/shopping-list-and-wishlist/{{site.version}}/install-and-upgrade/upgrade-the-shoppinglistwidget-module.html) |

</details>   
