

## Install Feature API

### Prerequisites

To start feature integration, overview and install the necessary features:

| NAME | VERSION | INTEGRATION GUIDE |
| --- | --- | --- |
| Spryker Core | {{site.version}} | [Feature API](/docs/scos/dev/feature-integration-guides/{{site.version}}/spryker-core-feature-integration.html) |
| Return Management | {{site.version}} | [Return Management feature integration](/docs/pbc/all/return-management/{{site.version}}/install-and-upgrade/install-the-return-management-feature.html) |

### 1) Install the required modules using Composer

Run the following command(s) to install the required modules:

```bash
composer require spryker/sales-returns-rest-api:"{{site.version}}" --update-with-dependencies
```

{% info_block warningBox "Verification" %}

Make sure that the following modules have been installed:

| MODULE | EXPECTED DIRECTORY |
| --- | --- |
| SalesReturnsRestApi | vendor/spryker/sales-returns-rest-api |

{% endinfo_block %}

### 2) Set up transfer objects

Run the following commands to generate transfer changes:

```bash
console transfer:generate
```

{% info_block warningBox "Verification" %}

Make sure that the configured data are added to the `spy_glossary` table  in the database.

| TRANSFER | TYPE | EVENT | PATH |
| --- | --- | --- | --- |
| RestReturnsAttributes | class | created | src/Generated/Shared/Transfer/RestReturnsAttributesTransfer |
| RestReturnReasonsAttributes | class | created | src/Generated/Shared/Transfer/RestReturnReasonsAttributesTransfer |
| RestReturnRequestAttributes | class | created | src/Generated/Shared/Transfer/RestReturnRequestAttributesTransfer |
| RestReturnItemRequestAttributes | class | created | src/Generated/Shared/Transfer/RestReturnItemRequestAttributesTransfer |
| RestReturnTotalsAttributes | class | created | src/Generated/Shared/Transfer/RestReturnTotalsAttributesTransfer |
| RestReturnItemsAttributes | class | created | src/Generated/Shared/Transfer/RestReturnItemsAttributesTransfer |

{% endinfo_block %}

### 3) Set up behavior

Enable resources and relationships:

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
| --- | --- | --- | --- |
| OrderItemByResourceIdResourceRelationshipPlugin | Adds order-items resource as relationship by order item uuid. | None | Spryker\Glue\OrdersRestApi\Plugin |
| ReturnItemByReturnResourceRelationshipPlugin | Adds return-items resource as relationship by return. | None | `Spryker\Glue\SalesReturnsRestApi\Plugin |
| ReturnsResourceRoutePlugin | Registers `/returns` route. | None | Spryker\Glue\SalesReturnsRestApi\Plugin |
| ReturnReasonsResourceRoutePlugin | Registers `/return-reasons` route. | None | Spryker\Glue\SalesReturnsRestApi\Plugin |

**src/Pyz/Glue/GlueApplication/GlueApplicationDependencyProvider.php**

```php
<?php

namespace Pyz\Glue\GlueApplication;

use Spryker\Glue\GlueApplication\GlueApplicationDependencyProvider as SprykerGlueApplicationDependencyProvider;
use Spryker\Glue\GlueApplicationExtension\Dependency\Plugin\ResourceRelationshipCollectionInterface;
use Spryker\Glue\SalesReturnsRestApi\Plugin\ReturnItemByReturnResourceRelationshipPlugin;
use Spryker\Glue\SalesReturnsRestApi\Plugin\ReturnReasonsResourceRoutePlugin;
use Spryker\Glue\SalesReturnsRestApi\Plugin\ReturnsResourceRoutePlugin;
use Spryker\Glue\SalesReturnsRestApi\SalesReturnsRestApiConfig;

class GlueApplicationDependencyProvider extends SprykerGlueApplicationDependencyProvider
{
    /**
     * @return \Spryker\Glue\GlueApplicationExtension\Dependency\Plugin\ResourceRoutePluginInterface[]
     */
    protected function getResourceRoutePlugins(): array
    {
        return [
            new ReturnReasonsResourceRoutePlugin(),
            new ReturnsResourceRoutePlugin(),
        ];
    }

    /**
     * @param \Spryker\Glue\GlueApplicationExtension\Dependency\Plugin\ResourceRelationshipCollectionInterface $resourceRelationshipCollection
     *
     * @return \Spryker\Glue\GlueApplicationExtension\Dependency\Plugin\ResourceRelationshipCollectionInterface
     */
    protected function getResourceRelationshipPlugins(
        ResourceRelationshipCollectionInterface $resourceRelationshipCollection
    ): ResourceRelationshipCollectionInterface {
        $resourceRelationshipCollection->addRelationship(
            SalesReturnsRestApiConfig::RESOURCE_RETURNS,
            new ReturnItemByReturnResourceRelationshipPlugin()
        );
        $resourceRelationshipCollection->addRelationship(
            SalesReturnsRestApiConfig::RESOURCE_RETURN_ITEMS,
            new OrderItemByResourceIdResourceRelationshipPlugin()
        );

        return $resourceRelationshipCollection;
    }
}
```

{% info_block warningBox "Verification" %}

Make sure that following endpoints are available now:

- `https://glue.mysprykershop.com/returns`
- `https://glue.mysprykershop.com/return-reasons`

Make sure that items from `https://glue.mysprykershop.com/orders` endpoint contain `isReturnable` and uuid properties.

{% endinfo_block %}
