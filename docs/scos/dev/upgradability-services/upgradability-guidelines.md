---
title: Upgradability guidelines
description: Reference information for evaluator and upgrader tools.
last_updated: Nov 25, 2021
template: concept-topic-template
---

## Entity name is not unique

The names of the following entities should be unique:

* Transfers
* Transfer properties
* Database tables
* Database columns
* Methods
* Constants

If a minor or major release introduces an entity with the same name, the entity in your project might change behavior or cause issues.

### Making entity names unique

To avoid unexpected issues and achieve the same result, make the names of entities unique. This section contains examples of transfer, database table, and method names. You can apply the same solution to any other entity.

#### Examples of code and related errors

`ProductAbstractStore` transfer name is not unique:

```xml
<?xml version="1.0"?>
<transfers xmlns="spryker:transfer-01" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="spryker:transfer-01 http://static.spryker.com/transfer-01.xsd">
    <transfer name="ProductAbstractStore">
        <property name="productAbstractSku" type="string"/>
        <property name="storeName" type="string"/>
    </transfer>
</transfers>
```

Related error in the Evaluator output:
```bash
************************************************************************************************************************
Evaluator\Business\Check\IsNotUnique\TransferShouldHavePrefixCheck
You should use Pyz prefix for unique transfer objects on the project level
************************************************************************************************************************
------------------------------------------------------------------------------------------------------------------------
ProductAbstractStore
"\/src\/Pyz\/Shared\/Product\/Transfer\/product.transfer.xml"
["productAbstractSku","storeName"]
************************************************************************************************************************
```

`evaluator_spryker` table name is not unique:

```xml
<?xml version="1.0"?>
<database xmlns="spryker:schema-01" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="spryker:schema-01 https://static.spryker.com/schema-01.xsd" name="zed" namespace="Orm\Zed\EvaluatorSpryker\Persistence" package="src.Orm.Zed.EvaluatorSpryker.Persistence">
    <table name="evaluator_spryker" idMethod="native">
        <column name="id_evaluator_spryker" required="true" type="INTEGER" autoIncrement="true" primaryKey="true"/>
        <column name="reversed_string" required="true" size="128" type="VARCHAR"/>

        <id-method-parameter value="evaluator_spryker_pk_seq"/>
    </table>
</database>
```
Related error in the Evaluator output:

```bash
************************************************************************************************************************
Evaluator\Business\Check\IsNotUnique\DbTableCheck
You should use project specific prefix "pyz" for the table
************************************************************************************************************************
------------------------------------------------------------------------------------------------------------------------
evaluator_spryker
"\/src\/Pyz\/Zed\/EvaluatorSpryker\/Persistence\/Propel\/Schema\/evaluator_spryker.schema.xml"
["id_evaluator_spryker","reversed_string"]
************************************************************************************************************************
```

`getPublishQueueConfiguration` method name is not unique:

```php
/**
 * @SuppressWarnings(PHPMD.CouplingBetweenObjects)
 */
class RabbitMqConfig extends SprykerRabbitMqConfig
{
    ...
    /**
     * @return array
     */
    protected function getPublishQueueConfiguration(): array
    {
        ...
    }
    ...
}
```


Related error in the Evaluator output:

```bash
************************************************************************************************************************
Evaluator\Business\Check\IsNotUnique\MethodCheck
Use project specific prefix, e.g Pyz
************************************************************************************************************************
------------------------------------------------------------------------------------------------------------------------
Pyz\Client\RabbitMq\RabbitMqConfig
{"name":"getPublishQueueConfiguration","class":"Pyz\\Client\\RabbitMq\\RabbitMqConfig"}
{"parentClass":"Spryker\\Client\\RabbitMq\\RabbitMqConfig","methods":["getQueueConnections","getMessageConfig","getDefaultQueueConnectionConfig","isRuntimeSettingUpEnabled","getQueueConnectionConfigs","getQueueOptions","getQueueConfiguration","getDefaultBoundQueueNamePrefix","createExchangeOptionTransfer","createQueueOptionTransfer","get","getConfig","setSharedConfig","getSharedConfig","resolveSharedConfig"]}
```




#### Examples of making entity names unique

To resolve the errors provided in the preceding examples, rename the entities. For example, add the project name as a prefix.

{% info_block infoBox "Future-proof names" %}

The names should be unique to the extent of making it impossible to accidentally match the name of a core entity introduced in future.

{% endinfo_block %}

Renamed transfer name:

```xml
<?xml version="1.0"?>
<transfers xmlns="spryker:transfer-01" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="spryker:transfer-01 http://static.spryker.com/transfer-01.xsd">
    <transfer name="PyzProductAbstractStore">
        <property name="productAbstractSku" type="string"/>
        <property name="storeName" type="string"/>
    </transfer>
</transfers>
```
---


Renamed table name:

```xml
<?xml version="1.0"?>
<database xmlns="spryker:schema-01" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="spryker:schema-01 https://static.spryker.com/schema-01.xsd" name="zed" namespace="Orm\Zed\EvaluatorSpryker\Persistence" package="src.Orm.Zed.EvaluatorSpryker.Persistence">
    <table name="pyz_evaluator_spryker" idMethod="native">
        <column name="id_evaluator_spryker" required="true" type="INTEGER" autoIncrement="true" primaryKey="true"/>
        <column name="reversed_string" required="true" size="128" type="VARCHAR"/>

        <id-method-parameter value="evaluator_spryker_pk_seq"/>
    </table>
</database>
```

Renamed method name:

```php
/**
 * @SuppressWarnings(PHPMD.CouplingBetweenObjects)
 */
class RabbitMqConfig extends SprykerRabbitMqConfig
{
    ...
    /**
     * @return array
     */
    protected function getPyzPublishQueueConfiguration(): array
    {
        ...
    }
    ...
}
```

After renaming the entity, re-evaluate the code. The same error shouldn't be returned.

## A core method is used on the project level

Modules have public and private APIs. The public API includes the entities like facade, plugin stack, dependency list, etc. This check covers private API entities, including Business model, Factory, Dependency provider, Repository, and Entity manager.

While public API updates always support backward compatibility, private API updates can break backward compatibility. So, backward compatibility in minor releases is not guaranteed in the private API. For example, if you use a core method on the project level, and it is updated or removed, it can cause unexpected issues during minor updates.

### Moving core methods to the project level

To avoid unexpected issues and achieve the same result, replace the core methods with their copies on the project level.

#### Example of the code that causes the upgradability error

`CustomerAccessUpdater` uses the `setContentTypesToInaccessible` method from the core level.

```php
<?php
namespace Pyz\Zed\CustomerAccess\Business\CustomerAccess;
...
class CustomerAccessUpdater extends SprykerCustomerAccessUpdater
{
    ...
    /**
     * @param \Generated\Shared\Transfer\CustomerAccessTransfer $customerAccessTransfer
     *
     * @return \Generated\Shared\Transfer\CustomerAccessTransfer
     */
    public function updateUnauthenticatedCustomerAccess(CustomerAccessTransfer $customerAccessTransfer): CustomerAccessTransfer
    {
        return $this->getTransactionHandler()->handleTransaction(function () use ($customerAccessTransfer) {
            ...
            return $this->customerAccessEntityManager->setContentTypesToInaccessible($customerAccessTransfer);
        });
    }
}
```

Related error in the Evaluator output:
```text
------------------------------------------------------------------------------------------------------------------------
Pyz\Zed\CustomerAccess\Business\CustomerAccess\CustomerAccessUpdater
"Please avoid Spryker dependency: customerAccessEntityManager->setContentTypesToInaccessible(...)"
************************************************************************************************************************
```

#### Example of moving a core method to the project level

To resolve the error provided in the example, do the following:

1. Copy the method from the core to the project level and rename it, for example, by adding a prefix.

2. Add the method to the class and the interface:

```php
src/Pyz/Zed/CustomerAccess/Persistence/CustomerAccessEntityManager.php
/**
 * @param \Generated\Shared\Transfer\CustomerAccessTransfer $customerAccessTransfer
 *
 * @return \Generated\Shared\Transfer\CustomerAccessTransfer
 */
public function setContentTypesToInaccessible(CustomerAccessTransfer $customerAccessTransfer): CustomerAccessTransfer
{
   ...
}
```

```php
src/Pyz/Zed/CustomerAccess/Persistence/CustomerAccessEntityManagerInterface.php

/**
 * @param \Generated\Shared\Transfer\CustomerAccessTransfer $customerAccessTransfer
 *
 * @return \Generated\Shared\Transfer\CustomerAccessTransfer
 */
public function setContentTypesToInaccessible(CustomerAccessTransfer $customerAccessTransfer): CustomerAccessTransfer;
```

After replacing the core method with its project-level copy, re-evaluate the code. The same error shouldn't be returned.