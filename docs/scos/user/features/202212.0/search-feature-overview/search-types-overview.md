---
title: Search types overview
last_updated: Jul 8, 2021
template: concept-topic-template
originalLink: https://documentation.spryker.com/2021080/docs/search-types-overview
originalArticleId: 39ea357f-d75f-47e9-ac77-0c4073b7d0ad
redirect_from:
  - /2021080/docs/search-types-overview
  - /2021080/docs/en/search-types-overview
  - /docs/search-types-overview
  - /docs/en/search-types-overview
---

This document describes the default search types shipped with the *Search* feature.


## Full-site search
The default full-site search has the following functionality:

* *Fuzzy search*: Suggests search results that do not exactly match the search request.
* *Auto-completion*: Helps customers by predicting the rest of a search string and offers a list of matching options.
* *Search suggestions*: Proposes on-the-fly page suggestions for products, categories, or CMS Pages.
* *Did-you-mean*: Offers typo corrections for the search string.
* *Mimic a dynamic category* by saving a search result and embedding it like a category page in your navigation.

## Multi-language search
If you set up a multi-language store, the search function automatically checks and adjusts the language your customer has selected. All search functions, such as auto-complete or auto-suggest, are then applied to the selected language.

## Textual search
By default, all content on CMS and Product Pages, such as product name, description text, or allocated attributes, is searchable. Additionally, Product attributes can be boosted in the search results. You can easily define which products or content to include in or exclude from the full-text search.

For details about how you can manage the search preferences, see [Managing search preferences](/docs/scos/user/back-office-user-guides/{{page.version}}/merchandising/search-preferences/define-search-preferences.html).

## Current constraints

The feature has the following functional constraints:

* Search preference attributes are shared across all the stores in a project.
* You cannot define a search preference for a single store.

## Related Business User articles

|BACK OFFICE USER GUIDES|
|---|
| [Manage search preferences](/docs/scos/user/back-office-user-guides/{{page.version}}/merchandising/search-preferences/define-search-preferences.html)  |

{% info_block warningBox "Developer guides" %}

Are you a developer? See [Search feature walkthrough](/docs/scos/dev/feature-walkthroughs/{{page.version}}/search-feature-walkthrough.html) for developers.

{% endinfo_block %}
