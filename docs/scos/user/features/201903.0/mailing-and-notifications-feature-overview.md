---
title: Mailing and Notifications feature overview
last_updated: Jul 31, 2020
template: concept-topic-template
originalLink: https://documentation.spryker.com/v2/docs/mailing-communication
originalArticleId: 633b156b-4a0c-4e3c-bda3-ea8d014d5eea
redirect_from:
  - /v2/docs/mailing-communication
  - /v2/docs/en/mailing-communication
  - /v2/docs/newsletter-subscription
  - /v2/docs/en/newsletter-subscription
  - /v2/docs/transactional-email-management
  - /v2/docs/en/transactional-email-management
---

The *Mailing & Notifications* feature allows you to manage newsletters and notifications.

## Newsletter subscriptions

Offer Newsletter Subscriptions to your customers to increase loyalty. Send updates on product related news, special offers or any other update you wish to share. The Spryker Commerce OS offers opt-in and opt-out options.

The newsletter subscription locates in the **Newsletter** section of the user profile.

All your customers need to do is to agree for the newsletter subscription. Once they submit the agreement form, an email is triggered to confirm that the request is received and the sign-up is successful.

![Newsletter](https://spryker.s3.eu-central-1.amazonaws.com/docs/Features/Mailing+%26+Communication/Newsletter+Subscription/subscribe-to-the-newsletter.gif)

## Transactional email management

Keep your customers updated with a variety of emails you can either send via the internal SMTP system or an external email provider of your choice.

Automated Emails regarding order status, shipping or transactions are just a few examples of how you can support the purchase process and increase brand loyalty.

The following links provide additional information about the Mail module, plugins, and procedures:

*  `MailTypeBuilderPlugin` creation and  registration—[HowTo - Create and Register a MailTypeBuilderPlugin](/docs/scos/dev/tutorials-and-howtos/howtos/howto-create-and-register-a-mail-type-builder-plugin.html)
*  `MailProviderPlugin` general overview and the registration procedure—[HowTo - Create and Register a Mail Provider](/docs/scos/dev/tutorials-and-howtos/howtos/howto-create-and-register-a-mail-provider.html)
*  Tutorial that helps to understand the procedure for sending an email—[Tutorial - Sending a Mail](/docs/scos/dev/tutorials-and-howtos/introduction-tutorials/tutorial-sending-an-email.html).
* The following guide describes how to install and set up `SymfonyMailer` [Mailing and Notifications integration guide](docs/scos/dev/feature-integration-guides/{{site.version}}/mailing-and-notifications-feature-integration.html)

{% info_block warningBox "Developer guides" %}

Are you a developer? See [Mailing & Notification feature walkthrough](/docs/scos/dev/feature-walkthroughs/{{page.version}}/mailing-and-notifications-feature-walkthrough.html) for developers.

{% endinfo_block %}
