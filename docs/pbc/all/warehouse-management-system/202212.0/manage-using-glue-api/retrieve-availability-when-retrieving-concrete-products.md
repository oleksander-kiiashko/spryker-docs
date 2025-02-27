---
title: Retrieve availability when retrieving concrete products
description: Retrieve general information about concrete products.
last_updated: Aug 22, 2022
template: glue-api-storefront-guide-template
---

This endpoint allows retrieving general information about concrete products.

## Installation

For detailed information on the modules that provide the API functionality and related installation instructions, see the docs:
* [Glue API: Products Feature Integration](/docs/scos/dev/feature-integration-guides/{{site.version}}/glue-api/glue-api-product-feature-integration.html)
* [Install the Inventory Management Glue API](/docs/pbc/all/warehouse-management-system/{{site.version}}/install-and-upgrade/install-features/install-the-inventory-management-glue-api.html)


## Retrieve a concrete product

To retrieve general information about a concrete product, send the request:

---
`GET` **/concrete-products/*{% raw %}{{{% endraw %}concrete_product_sku{% raw %}}}{% endraw %}***

---

| PATH PARAMETER | DESCRIPTION |
| --- | --- |
| ***{% raw %}{{{% endraw %}concrete_product_sku{% raw %}}}{% endraw %}*** | SKU of a concrete product to get information for. |

### Request

| STRING PARAMETER | DESCRIPTION | EXEMPLARY VALUES |
| --- | --- | --- |
| include | Adds resource relationships to the request. | concrete-product-availabilities |

`GET https://glue.mysprykershop.com/concrete-products/001_25904006?include=concrete-product-availabilities`: Get information about the `001_25904006` product with its availability.

### Response



<details>
<summary markdown='span'>Response sample: retrieve information about a concrete product with the details on product availability</summary>

```json
{
    "data": {
        "type": "concrete-products",
        "id": "001_25904006",
        "attributes": {
            "sku": "001_25904006",
            "isDiscontinued": false,
            "discontinuedNote": null,
            "averageRating": null,
            "reviewCount": 0,
            "name": "Canon IXUS 160",
            "description": "Add a personal touch Make shots your own with quick and easy control over picture settings such as brightness and colour intensity. Preview the results while framing using Live View Control and enjoy sharing them with friends using the 6.8 cm (2.7”) LCD screen. Combine with a Canon Connect Station and you can easily share your photos and movies with the world on social media sites and online albums like irista, plus enjoy watching them with family and friends on an HD TV. Effortlessly enjoy great shots of friends thanks to Face Detection technology. It detects multiple faces in a single frame making sure they remain in focus and with optimum brightness. Face Detection also ensures natural skin tones even in unusual lighting conditions.",
            "attributes": {
                "megapixel": "20 MP",
                "flash_range_tele": "4.2-4.9 ft",
                "memory_slots": "1",
                "usb_version": "2",
                "brand": "Canon",
                "color": "Red"
            },
            "superAttributesDefinition": [
                "color"
            ],
            "metaTitle": "Canon IXUS 160",
            "metaKeywords": "Canon,Entertainment Electronics",
            "metaDescription": "Add a personal touch Make shots your own with quick and easy control over picture settings such as brightness and colour intensity. Preview the results whi",
            "attributeNames": {
                "megapixel": "Megapixel",
                "flash_range_tele": "Flash range (tele)",
                "memory_slots": "Memory slots",
                "usb_version": "USB version",
                "brand": "Brand",
                "color": "Color"
            }
        },
        "links": {
            "self": "https://glue.mysprykershop.com/concrete-products/001_25904006?include=concrete-product-availabilities"
        },
        "relationships": {
            "concrete-product-availabilities": {
                "data": [
                    {
                        "type": "concrete-product-availabilities",
                        "id": "001_25904006"
                    }
                ]
            }
        }
    },
    "included": [
        {
            "type": "concrete-product-availabilities",
            "id": "001_25904006",
            "attributes": {
                "isNeverOutOfStock": false,
                "availability": true,
                "quantity": "10.0000000000"
            },
            "links": {
                "self": "https://glue.mysprykershop.com/concrete-products/001_25904006/concrete-product-availabilities"
            }
        }
    ]
}
```
</details>



<a name="retrieve-a-concrete-product-response-attributes"></a>

{% include pbc/all/glue-api-guides/retrieve-a-concrete-product-response-attributes.md %} <!-- To edit, see /_includes/pbc/all/glue-api-guides/retrieve-a-concrete-product-response-attributes.md -->

For the attributes of concrete product availability, see [Retrieve availability of a concrete product](/docs/pbc/all/warehouse-management-system/{{site.version}}/manage-using-glue-api/retrieve-concrete-product-availability.html#concrete-product-availability-response-attributes).


## Possible errors

| CODE | REASON |
| --- | --- |
| 302 | Concrete product is not found. |
| 312 | Concrete product is not specified.  |

To view generic errors that originate from the Glue Application, see [Reference information: GlueApplication errors](/docs/scos/dev/glue-api-guides/{{site.version}}/reference-information-glueapplication-errors.html).
