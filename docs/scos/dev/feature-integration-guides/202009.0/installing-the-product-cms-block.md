---
title: Installing the product CMS block
description: The guide describes the process of installing the Product CMS Block into your project.
last_updated: Aug 27, 2020
template: feature-integration-guide-template
originalLink: https://documentation.spryker.com/v6/docs/product-block
originalArticleId: 9232edfa-9d16-4c79-92b6-ee8463d3094f
redirect_from:
  - /v6/docs/product-block
  - /v6/docs/en/product-block
related:
  - title: CMS Block
    link: docs/scos/user/features/page.version/cms-feature-overview/cms-blocks-overview.html
  - title: Creating CMS Blocks
    link: docs/scos/user/back-office-user-guides/page.version/content/blocks/creating-cms-blocks.html
  - title: Managing CMS Blocks
    link: docs/scos/user/back-office-user-guides/page.version/content/blocks/managing-cms-blocks.html
  - title: Migration Guide - CMS Block
    link: docs/pbc/all/content-management-system/page.version/install-and-upgrade/upgrade-modules/upgrade-the-cmsblock-module.html
  - title: Migration Guide - CMS Block GUI
    link: docs/pbc/all/content-management-system/{{page.version}}/install-and-upgrade/upgrade-modules/upgrade-the-cmsblockgui-module.html
---

Product blocks are blocks that can be embedded in the product template, for which we can specify on which specific product we want them to be rendered.

To install the Product CMS Block in your project, follow the steps described in this procedure:

1. Install the CMS Block Product Connector module with composer: 

```bash
"spryker/cms-block-product-connector": "^1.0.0"
```

2. Register the form plugin by adding `CmsBlockProductFormPlugin` to the CMS Block GUI dependency provider `\Pyz\Zed\CmsBlockGui\CmsBlockGuiDependencyProvider`.

```php
<?php
namespace Pyz\Zed\CmsBlockGui;

use Spryker\Zed\CmsBlockProductConnector\Communication\Plugin\CmsBlockProductFormPlugin;

class CmsBlockGuiDependencyProvider extends CmsBlockGuiCmsBlockGuiDependencyProvider
{
    /**
    * @return array
    */
    protected function getCmsBlockFormPlugins()
    {
        return = array_merge(parent::getCmsBlockFormPlugins(), [
            new CmsBlockProductFormPlugin(),
        ]);
    }
}
```

3. Register the form handler plugin by adding  `CmsBlockProductConnectorUpdatePlugin` to the CMS Block dependency provider `Pyz\Zed\CmsBlock\CmsBlockDependencyProvider`.

```php
<?php
namespace Pyz\Zed\CmsBlock;

use Spryker\Zed\CmsBlockProductConnector\Communication\Plugin\CmsBlockProductConnectorUpdatePlugin;


class CmsBlockDependencyProvider extends CmsBlockCmsBlockDependencyProvider
{
    protected function getCmsBlockUpdatePlugins()
    {
        return array_merge(parent::getCmsBlockUpdatePlugins(), [
            new CmsBlockProductConnectorUpdatePlugin()
        ]);
    }
}
```

4. Register the collector plugin by adding  `CmsBlockProductConnectorCollectorPlugin` to the Collector dependency provider `\Pyz\Zed\Collector\CollectorDependencyProvider`.

```php
<?php
namespace Pyz\Zed\Collector;

...

class CollectorDependencyProvider extends SprykerCollectorDependencyProvider
{
    public function provideBusinessLayerDependencies(Container $container)
    {
        ...
        $container[self::STORAGE_PLUGINS] = function (Container $container) {
            return [
                ...
                CmsBlockProductConnectorConstants::RESOURCE_TYPE_CMS_BLOCK_PRODUCT_CONNECTOR => new CmsBlockProductConnectorCollectorPlugin(),
            ];
        };
    }
}
```

5. Register the product list plugin (optional).
To show which product abstracts are assigned to a block on a block view page, add `CmsBlockProductAbstractListViewPlugin` to the CMS Block GUI dependency provider.

```php
<?php
namespace Pyz\Zed\CmsBlockGui;

...

class CmsBlockGuiDependencyProvider extends CmsBlockGuiCmsBlockGuiDependencyProvider
{
    ...

    /**
    * @return array
    */
    protected function getCmsBlockViewPlugins()
    {
       return array_merge(parent::getCmsBlockViewPlugins(), [
           new CmsBlockProductAbstractListViewPlugin(),
       ]);
    }
}
```

6. Register the block list to the product abstract view (optional).
To show which blocks are assigned to a product abstract on a product abstract view page, add `CmsBlockProductAbstractBlockListViewPlugin` to the Product Management dependency provider.

```php
<?php
namespace Pyz\Zed\ProductManagement;

...

class ProductManagementDependencyProvider extends SprykerProductManagementDependencyProvider
{
    protected function getProductAbstractViewPlugins()
    {
        return array_merge(parent::getProductAbstractViewPlugins(), [
            new CmsBlockProductAbstractBlockListViewPlugin()
        ]);
    }
}
```