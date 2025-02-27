

## Install Feature API

### Prerequisites

To start feature integration, overview and install the necessary features:

| NAME | VERSION | REQUIRED SUB-FEATURE |
| --- | --- | --- |
| Spryker Core | {{site.version}} | [Glue Application feature integration](/docs/scos/dev/feature-integration-guides/{{site.version}}/glue-api/glue-api-glue-application-feature-integration.html) |
| Alternative Products | {{site.version}} | |
| Products | {{site.version}} | [Product API feature integration](/docs/scos/dev/feature-integration-guides/{{site.version}}/glue-api/glue-api-product-feature-integration.html) |

## 1) Install the required modules using Composer

Run the following command to install the required modules:

```bash
composer require spryker/alternative-products-rest-api:"^1.0.0" --update-with-dependencies
```

{% info_block warningBox “Verification” %}

Make sure that the following module is installed:

| MODULE | EXPECTED DIRECTORY |
| --- | --- |
| AlternativeProductsRestApi | vendor/spryker/alternative-products-rest-api |

{% endinfo_block %}


## 2) Set up behavior

Activate the following plugins:

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
| --- | --- | --- | --- |
| AbstractAlternativeProductsResourceRoutePlugin | Registers the abstract alternative products resource. | None | Spryker\Glue\AlternativeProductsRestApi\Plugin\GlueApplication |
| ConcreteAlternativeProductsResourceRoutePlugin | Registers the concrete alternative products resource. | None | Spryker\Glue\AlternativeProductsRestApi\Plugin\GlueApplication |

**src/Pyz/Glue/GlueApplication/GlueApplicationDependencyProvider.php**

```php
<?php

namespace Pyz\Glue\GlueApplication;

use Spryker\Glue\GlueApplication\GlueApplicationDependencyProvider as SprykerGlueApplicationDependencyProvider;
use Spryker\Glue\AlternativeProductsRestApi\Plugin\GlueApplication\AbstractAlternativeProductsResourceRoutePlugin;
use Spryker\Glue\AlternativeProductsRestApi\Plugin\GlueApplication\ConcreteAlternativeProductsResourceRoutePlugin

class GlueApplicationDependencyProvider extends SprykerGlueApplicationDependencyProvider
{
    /**
     * @return \Spryker\Glue\GlueApplicationExtension\Dependency\Plugin\ResourceRoutePluginInterface[]
     */
    protected function getResourceRoutePlugins(): array
    {
        return [
            new AbstractAlternativeProductsResourceRoutePlugin(),
            new ConcreteAlternativeProductsResourceRoutePlugin(),
        ];
    }
}
```

{% info_block warningBox “Verification” %}

Make sure that the following endpoints are available:

* `http://mysprykershop.com/concrete-products/{% raw %}{{{% endraw %}concrete_sku{% raw %}}}{% endraw %}/abstract-alternative-products`
* `http://mysprykershop.com/concrete-products/{% raw %}{{{% endraw %}concrete_sku{% raw %}}}{% endraw %}/abstract-alternative-products`

{% endinfo_block %}
