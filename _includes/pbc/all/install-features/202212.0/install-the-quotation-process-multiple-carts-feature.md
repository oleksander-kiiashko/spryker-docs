

## Install feature core

### Prerequisites
To start feature integration, overview and install the necessary features:

| NAME | VERSION |
| --- | --- |
| Quotation Process | {{site.version}} |
| Multiple Carts | {{site.version}} |

### 1) Set up behavior

Register the following plugins:

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
| --- | --- | --- | --- |
| MultiCartQuotePersistPlugin | Creates a new active customer cart. | None | Spryker\Client\MultiCart\Plugin\PersistentCart |

**src/Pyz/Client/PersistentCart/PersistentCartDependencyProvider.php**

```php
<?php

namespace Pyz\Client\PersistentCart;

use Spryker\Client\PersistentCart\PersistentCartDependencyProvider as SprykerPersistentCartDependencyProvider;
use Spryker\Client\PersistentCartExtension\Dependency\Plugin\QuotePersistPluginInterface;
use Spryker\Client\MultiCart\Plugin\PersistentCart\MultiCartQuotePersistPlugin;

class PersistentCartDependencyProvider extends SprykerPersistentCartDependencyProvider
{
    /**
     * @return \Spryker\Client\PersistentCartExtension\Dependency\Plugin\QuotePersistPluginInterface
     */
    protected function getQuotePersistPlugin(): QuotePersistPluginInterface
    {
        return new MultiCartQuotePersistPlugin();
    }
}
```

{% info_block warningBox "Verification" %}

Make sure that when you converting quote request with status "Ready" to cart, new cart created instead of replacing the existing one.

{% endinfo_block %}
