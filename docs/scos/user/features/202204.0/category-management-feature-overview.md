---
title: Category Management feature overview
description: The feature lets you keep your product catalog organized and comprehensible for the customers who can easily navigate the storefront and search products quicker
last_updated: Oct 20, 2021
template: concept-topic-template
originalLink: https://documentation.spryker.com/2021080/docs/category-management-feature-overview
originalArticleId: 33b65d5e-fd6c-4017-92ed-7229883beeb0
redirect_from:
  - /2021080/docs/category-management-feature-overview
  - /2021080/docs/en/category-management-feature-overview
  - /docs/category-management-feature-overview
  - /docs/en/category-management-feature-overview
  - /docs/scos/user/features/202200.0/category-management-feature-overview.html
---

A category is a set of products that share a common attribute and, therefore, can be united logically. The *Category Management* feature lets Back Office users structure products into a logical system where each product belongs to a category or set of interrelated categories. You can [assign categories to all or individual stores](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/assigning-products-to-categories.html) from the Back Office or [import stores for categories](/docs/scos/dev/data-import/{{page.version}}/data-import-categories/catalog-setup/categories/file-details-category-store.csv.html). For details about how a Back Office user can group products under categories, see [Assigning products to categories](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/assigning-products-to-categories.html).

## Root, parent, and child categories

The product catalog is structured in a category tree, which consists of root, parent, and child categories.

A *root category* is a base category that stands on top of the category hierarchy. Product and child categories are assigned to root categories, but root categories can not be assigned to anything. Also, a store can only be linked to one root category tree, while a root category tree can be linked to multiple stores. Root categories are added through [data import](/docs/scos/dev/data-import/{{page.version}}/data-import-categories/catalog-setup/categories/file-details-category.csv.html) and cannot be created in the Back Office, unlike other categories, which can be added in both ways.

A parent category is a category that has products and other categories assigned to it. Categories assigned to parent categories are referred to as *child categories*. Products belonging to a child category that is assigned to a parent one belong to the parent category too. You can assign child categories to parent ones by editing categories in the Back Office or importing the categories. For information about how to assign child categories to parent categories, see [Create categories](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/create-categories.html) and [Edit categories](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/edit-categories.html). For information about importing child categories, see [File details: category.csv](/docs/scos/dev/data-import/{{page.version}}/data-import-categories/catalog-setup/categories/file-details-category.csv.html).

On the Storefront, in comparison to parent category names, child category names are indented so that they can be identified.

In the image below, the Cameras & Camcorders parent category has the Digital Cameras, Camcorders, Actioncams, and Dashcams child categories. Cameras & Camcorders is a root category, so it cannot have a parent one.

![parent-category.png](https://spryker.s3.eu-central-1.amazonaws.com/docs/Features/Catalog+Management/Category+Management/Category+Management+Feature+Overview/parent-category.png)


## Category templates

A Back Office user defines the visual representation of a category on the Storefront by assigning a template to the category. For information about how to do that, see [Create categories](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/create-categories.html).

For more information about types of category templates, see [Category page template types](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/create-categories.html#reference-information-template).

## Category filters

Through category filters, you can locale items better and add customizable category filters to the catalog pages on the Storefront. They also let you add, rearrange, and define filters for any given parameter in the category tree, such as price or brand. For details about how a Back Office user can add and manage category filters, see Managing category filters.

### Filter preferences

The default filter functionality includes *standard filters* and *dynamic filters and facets*.

#### Standard filters

On the Storefront, the standard filters let buyers narrow down the search results by filtering products according to the specified price range, product ratings, product labels, color, material, and brand. For more details about standard filters, see [Standard filters](/docs/scos/user/features/{{page.version}}/search-feature-overview/standard-filters-overview.html).

#### Dynamic filters and facets

Compared to standard filters, dynamic filters and facets allow the creation of more advanced filter options. For example, you can customize facet filters for any product attribute: the design, quantity of filters, or order criteria by which filters are displayed on the Storefront. For more details about how to configure the filter preferences in the Back Office, see [Managing filter preferences](/docs/scos/user/back-office-user-guides/{{page.version}}/merchandising/filter-preferences/define-filter-preferences.html).

## Video tutorial
Check out this video on managing categories:

{% wistia g3l3c7xt93 960 720 %}

## Current constraints

The Category Management feature has the following functional constraints, which are going to be resolved in the future:

* A category page is accessible by the same URL across all the stores. If the category is not available in the given store, the URL will lead to a 404.
* Root categories cannot be created in the Back Office. They are imported through CSV files.
* A store can only be linked to one root category tree, but in the future, a store can have multiple root category trees.
* Category URLs are locale-dependent.
* Category URLs are available in all the stores that share the same locales.
* Price Range Filter is not supported with merchant relations. That is why this filter is not included in the [B2B demo shop](/docs/scos/user/intro-to-spryker/b2b-suite.html). However, in the [B2C demo shop](/docs/scos/user/intro-to-spryker/b2c-suite.html), you can still filter the products using the price range filter.

## Related Business User articles

|BACK OFFICE USER GUIDES|
|---|
| [Create a category](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/create-categories.html) |
| [Assign products to categories](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/assign-products-to-categories.html) |
| [Edit categories](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/edit-categories.html) |
| [Order products in categories](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/order-products-in-categories.html) |
| [Delete categories](/docs/scos/user/back-office-user-guides/{{page.version}}/catalog/category/delete-categories.html) |



{% info_block warningBox "Developer guides" %}

Are you a developer? See [Category management feature walkthrough](/docs/scos/dev/feature-walkthroughs/{{page.version}}/category-management-feature-walkthrough.html) for developers.

{% endinfo_block %}
