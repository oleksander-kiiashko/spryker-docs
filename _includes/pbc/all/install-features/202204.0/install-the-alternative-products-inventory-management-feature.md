


This document describes how to integrate Alternative Products + Inventory Management into a Spryker project.

## Install feature core

Follow the steps below to install the Alternative Products + Inventory Management feature core.

### Prerequisites

To start feature integration, review and install the necessary features:

| NAME | VERSION | INTEGRATION GUIDE |
|---|---|---|
|Alternative Products|{{site.version}}| [Alternative Products feature integration](/docs/scos/dev/feature-integration-guides/{{site.version}}/alternative-products-feature-integration.html)|
|Inventory Management|{{site.version}}| [Inventory Management feature integration](/docs/pbc/all/warehouse-management-system/{{site.version}}/install-and-upgrade/install-features/install-the-inventory-management-feature.html) |

### 1) Set up behavior

Enable the following behaviors by registering the plugins:

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
|---|---|---|---|
|AvailabilityCheckAlternativeProductApplicablePlugin|Checks whether product alternatives are to be shown for the product.|None|`Spryker\Zed\Availability\Communication\Plugin\ProductAlternative|
|AvailabilityCheckAlternativeProductApplicablePlugin|Checks whether product alternatives are to be shown for the product.|Expects SKU and `IdProductAbstract` to be set for `ProductViewTransfer`.|Spryker\Client\AvailabilityStorage\Plugin\ProductAlternativeStorage|

**src/Pyz/Client/ProductAlternativeStorage/ProductAlternativeStorageDependencyProvider.php**

```php
<?php

namespace Pyz\Client\ProductAlternativeStorage;

use Spryker\Client\AvailabilityStorage\Plugin\ProductAlternativeStorage\AvailabilityCheckAlternativeProductApplicablePlugin;
use Spryker\Client\ProductAlternativeStorage\ProductAlternativeStorageDependencyProvider as SprykerProductAlternativeStorageDependencyProvider;

class ProductAlternativeStorageDependencyProvider extends SprykerProductAlternativeStorageDependencyProvider
{
	/**
	 * @return \Spryker\Client\ProductAlternativeStorageExtension\Dependency\Plugin\AlternativeProductApplicablePluginInterface[]
	 */
	protected function getAlternativeProductApplicableCheckPlugins(): array
	{
		return [
			new AvailabilityCheckAlternativeProductApplicablePlugin(),
		];
	}
}
```

**src/Pyz/Zed/ProductAlternative/ProductAlternativeDependencyProvider.php**

```php
<?php

namespace Pyz\Zed\ProductAlternative;

use Spryker\Zed\Availability\Communication\Plugin\ProductAlternative\AvailabilityCheckAlternativeProductApplicablePlugin;
use Spryker\Zed\ProductAlternative\ProductAlternativeDependencyProvider as SprykerProductAlternativeDependencyProvider;

class ProductAlternativeDependencyProvider extends SprykerProductAlternativeDependencyProvider
{
	/**
	 * @return \Spryker\Zed\ProductAlternativeExtension\Dependency\Plugin\AlternativeProductApplicablePluginInterface[]
	 */
	protected function getAlternativeProductApplicablePlugins(): array
	{
		return [
			new AvailabilityCheckAlternativeProductApplicablePlugin(),
		];
	}
}
```

{% info_block warningBox “Verification” %}

Make sure you can see alternatives for unavailable products on the product detail page.

{% endinfo_block %}
