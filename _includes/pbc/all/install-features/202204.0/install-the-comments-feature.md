


This document describes how to integrate the [Comments](/docs/pbc/all/cart-and-checkout/{{site.version}}/comments-feature-overview.html) into a Spryker project.

## Install feature core

Follow the steps below to install the Comments feature core.

### Prerequisites

To start feature integration, overview and install the necessary features:

| NAME | VERSION | INTEGRATION GUIDE|
| --- | --- | --- |
| Spryker Core | {{site.version}} | [Spryker Core feature integration](/docs/scos/dev/feature-integration-guides/{{site.version}}/spryker-core-feature-integration.html)|
| Customer Account Management | {{site.version}} | [Customer Account Management feature integration](/docs/scos/dev/feature-integration-guides/{{site.version}}/customer-account-management-feature-integration.html) |

### 1) Install the required modules using Composer

Install the required modules:

```bash
composer require spryker-feature/comments: "{{site.version}}" --update-with-dependencies
```

{% info_block warningBox "Verification" %}

Make sure that the following modules were installed:

| MODULE | EXPECTED DIRECTORY |
| --- | --- |
| Comment | vendor/spryker/comment |
| CommentDataImport | vendor/spryker/comment-data-import |

{% endinfo_block %}

### 2) Set up configuration

Add the following configuration to your project:

| CONFIGURATION | SPECIFICATION | NAMESPACE |
| --- | --- | --- |
| CommentConfig::getAvailableCommentTags() | Used to allow saving comment tags to the database. | Pyz\Shared\Comment |

**src/Pyz/Zed/Comment/CommentConfig.php**

```php
<?php

namespace Pyz\Shared\Comment;

use Spryker\Shared\Comment\CommentConfig as SprykerCommentConfig;

class CommentConfig extends SprykerCommentConfig
{
	/**
	 * @return string[]
	 */
	public function getAvailableCommentTags(): array
	{
		return [
			'delivery',
			'important',
		];
	}
}
```

{% info_block warningBox "Verification" %}

Make sure that when you add or remove a comment tag listed in the config to comment is allowed.

{% endinfo_block %}

**config/Shared/config_default.php**

```php
<?php

$config[CustomerConstants::CUSTOMER_SECURED_PATTERN] = '(^(/en|/de)?/comment($|/))';

```

{% info_block warningBox "Verification" %}

Make sure that `mysprykershop.com/comment` with a guest user redirects to the login page.

{% endinfo_block %}

### 3) Set up database schema and transfer objects

Apply database changes and generate entity and transfer changes:

```bash
console propel:install
console transfer:generate
```

{% info_block warningBox "Verification" %}

Make sure that the following changes were applied by checking your database:

| DATABASE ENTITY | TYPE | EVENT |
| --- | --- | --- |
| spy_comment | table | created |
| spy_comment_tag | table | created |
| spy_comment_thread | table | created |
| spy_comment_to_comment_tag | table | created |

Make sure that the following changes in transfer objects:

| TRANSFER | TYPE | EVENT | PATH |
| --- | --- | --- | --- |
| SpyCommentEntityTransfer | class | created | src/Generated/Shared/Transfer/SpyCommentEntityTransfer |
| SpyCommentTagEntityTransfer | class | created | src/Generated/Shared/Transfer/SpyCommentTagEntityTransfer |
| SpyCommentThreadEntityTransfer | class | created | src/Generated/Shared/Transfer/SpyCommentThreadEntityTransfer |
| SpyCommentToCommentTagEntityTransfer | class | created | src/Generated/Shared/Transfer/SpyCommentToCommentTagEntityTransfer |
| Comment | class | created | src/Generated/Shared/Transfer/Comment |
| CommentThread | class | created | src/Generated/Shared/Transfer/CommentThread |
| CommentTag | class | created | src/Generated/Shared/Transfer/CommentTag |
| CommentFilter | class | created | src/Generated/Shared/Transfer/CommentFilter |
| CommentRequest | class | created | src/Generated/Shared/Transfer/CommentRequest |
| CommentTagRequest | class | created | src/Generated/Shared/Transfer/CommentTagRequest |
| CommentThreadResponse | class | created | src/Generated/Shared/Transfer/CommentThreadResponse |

{% endinfo_block %}

### 4) Add translations

Append glossary according to your configuration:

**src/data/import/glossary.csv**

```yaml
comment.validation.error.comment_not_found,Comment not found.,en_US
comment.validation.error.comment_not_found,Kommentar nicht gefunden.,de_DE
comment.validation.error.comment_thread_not_found,Comment Thread not found.,en_US
comment.validation.error.comment_thread_not_found,Kommentar Thread nicht gefunden.,de_DE
comment.validation.error.access_denied,Access denied.,en_US
comment.validation.error.access_denied,Zugriff verweigert.,en_US
comment.validation.error.comment_thread_already_exists,Comment Thread already exists.,de_DE
comment.validation.error.comment_thread_already_exists,Kommentar Thread existiert bereits.,de_DE
comment.validation.error.invalid_message_length,Invalid message length.,en_US
comment.validation.error.invalid_message_length,Ungültige Nachrichtenlänge.,de_DE
comment.validation.error.comment_tag_not_available,Comment tag not available.,en_US
comment.validation.error.comment_tag_not_available,Kommentar-Tag nicht verfügbar.,de_DE
```

Import data:

```bash
console data:import glossary
```

{% info_block warningBox "Verification" %}

Make sure that in the database the configured data are added to the `spy_glossary` table.

{% endinfo_block %}

### 5) Set up comment workflow

Enable the following behaviors by registering the plugins:

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
| --- | --- | --- | --- |
| CommentDataImportPlugin | Imports Comments data. | None | Spryker\Zed\CommentDataImport\Communication\Plugin |

**src/Pyz/Zed/DataImport/DataImportDependencyProvider.php**

```php
<?php

namespace Pyz\Zed\DataImport;

use Spryker\Zed\CommentDataImport\Communication\Plugin\CommentDataImportPlugin;
use Spryker\Zed\DataImport\DataImportDependencyProvider as SprykerDataImportDependencyProvider;

class DataImportDependencyProvider extends SprykerDataImportDependencyProvider
{
	/**
	 * @return array
	 */
	protected function getDataImporterPlugins(): array
	{
		return [
			new CommentDataImportPlugin(),
		];
	}
```

{% info_block warningBox "Verification" %}

Make sure that `spy_comment`, `spy_comment_thread`, and `spy_comment_to_comment_tag` are not empty.

{% endinfo_block %}

## Install feature frontend

Follow the steps below to install the Comments feature frontend.

### Prerequisites

To start feature integration, integrate the required features:

| NAME | VERSION | INTEGRATION GUIDE|
| --- | --- | --- |
| Spryker Core | {{site.version}} | [Spryker Core feature integration](/docs/scos/dev/feature-integration-guides/{{site.version}}/spryker-core-feature-integration.html)|
| Customer Account Management | {{site.version}} | [Customer Account Management feature integration](/docs/scos/dev/feature-integration-guides/{{site.version}}/customer-account-management-feature-integration.html) |

### 1) Install the required modules using Composer

Install the required modules:

```bash
composer require spryker-feature/comments: "^201907.0" --update-with-dependencies
```
{% info_block warningBox "Verification" %}

Make sure that the following modules were installed:

| MODULE | EXPECTED DIRECTORY |
| --- | --- |
| CommentWidget | vendor/spryker-shop/comment-widget |
| CommentWidgetExtension | vendor/spryker-shop/comment-widget-extension |

{% endinfo_block %}

### 2) Add translations

Append glossary according to your configuration:

**src/data/import/glossary.csv**

```yaml
comment_widget.comments_to_cart,Comments to Cart,en_US
comment_widget.comments_to_cart,Kommentare zum Warenkorb,de_DE
comment_widget.form.add_comment,Add,en_US
comment_widget.form.add_comment,Hinzufügen,de_DE
comment_widget.form.update_comment,Update,en_US
comment_widget.form.update_comment,Aktualisieren,de_DE
comment_widget.form.remove_comment,Remove,en_US
comment_widget.form.remove_comment,Entfernen,de_DE
comment_widget.form.you,You,en_US
comment_widget.form.you,Sie,de_DE
comment_widget.form.attach,Attach,en_US
comment_widget.form.attach,Anhängen,de_DE
comment_widget.form.unattach,Unattach,en_US
comment_widget.form.unattach, Anhang entfernen,de_DE
comment_widget.form.all,All,en_US
comment_widget.form.all,Alles,de_DE
comment_widget.form.attached,Attached,en_US
comment_widget.form.attached,Angehängt,de_DE
comment_widget.form.button_default,Button,en_US
comment_widget.form.button_default,Schaltfläche,de_DE
comment_widget.form.edited,edited,en_US
comment_widget.form.edited,bearbeitet,de_DE
comment_widget.form.tags,Tags,en_US
comment_widget.form.tags,Tags,de_DE
comment_widget.form.placeholder.add_comment,Add a comment,en_US
comment_widget.form.placeholder.add_comment,Kommentar hinzufügen,de_DE
comment_widget.tags.all,All,en_US
comment_widget.tags.all,Alles,de_DE
comment_widget.tags.delivery,Delivery,en_US
comment_widget.tags.delivery,Lieferung,de_DE
comment_widget.tags.important,Important,en_US
comment_widget.tags.important,Wichtig,de_DE
```

Import data:

```bash
console data:import glossary
```

{% info_block warningBox "Verification" %}

Make sure that in the database the configured data are added to the `spy_glossary` table.

{% endinfo_block %}

### 3) Enable controllers: Route List

Register the following route provider plugins:

| PROVIDER | NAMESPACE |
| --- | --- |
| CommentWidgetRouteProviderPlugin | SprykerShop\Yves\CommentWidget\Plugin\Router |

**src/Pyz/Yves/Router/RouterDependencyProvider.php**

```php
<?php

namespace Pyz\Yves\Router;

use Spryker\Yves\Router\RouterDependencyProvider as SprykerRouterDependencyProvider;
use SprykerShop\Yves\CommentWidget\Plugin\Router\CommentWidgetRouteProviderPlugin;

class RouterDependencyProvider extends SprykerRouterDependencyProvider
{
    /**
     * @return \Spryker\Yves\RouterExtension\Dependency\Plugin\RouteProviderPluginInterface[]
     */
    protected function getRouteProvider(): array
    {
        return [
            new CommentWidgetRouteProviderPlugin(),
        ];
    }
}
```

{% info_block warningBox "Verification" %}

Verify `CommentWidgetRouteProviderPlugin`:
1. Log in as a customer and open the link: `mysprykershop.com/comment/0adafdf4-cb26-477d-850d-b26412fbd382/tag/add?returnUrl=/cart`.
2. Make sure that the error flash message is shown.

{% endinfo_block %}

### 4) Set up widgets

Register the following plugins to enable widgets:

| PLUGIN | SPECIFICATION | PREREQUISITES | NAMESPACE |
| --- | --- | --- | --- |
| CommentThreadWidget | Displays comments. | None | SprykerShop\Yves\CommentWidget\Widget |

**src/Pyz/Yves/ShopApplication/ShopApplicationDependencyProvider.php**

```php
<?php

namespace Pyz\Yves\ShopApplication;

use SprykerShop\Yves\CommentWidget\Widget\CommentThreadWidget;
use SprykerShop\Yves\ShopApplication\ShopApplicationDependencyProvider as SprykerShopApplicationDependencyProvider;

class ShopApplicationDependencyProvider extends SprykerShopApplicationDependencyProvider
{
	/**
	 * @return string[]
	 */
	protected function getGlobalWidgets(): array
	{
		return [
			CommentThreadWidget::class,
		];
	}
}
```

Enable Javascript and CSS changes:

```bash
console frontend:yves:build
```

{% info_block warningBox "Verification" %}

Make sure that the following widget was registered:

| MODULE | TEST |
| --- | --- |
| CommentThreadWidget | You can check availability in twig `{% raw %}{%{% endraw %} widget 'CommentThreadWidget' args \[...\] only {% raw %}%}{% endraw %}{% raw %}{%{% endraw %} endwidget {% raw %}%}{% endraw %}` |

{% endinfo_block %}
