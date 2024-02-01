# SaaS Subscription

## Listing Process

### 1. Collect Assets
Before submitting a product, you will need to provide:
- Product logo URL
- End User License Agreement (EULA) URL
- SaaS fulfillment URL (redirect page customers will be sent to after subscribing)
- Metadata
- Support information

### 2. Submit via Management Portal
Create a product page using a seller account that has access to the AWS Marketplace Management Portal (AMMP).

### 3. Product Page Published to Limited
The AWS MP Ops team will publish your submission as a limited product page visible to you and any AWS accounts you have requested to be whitelisted. Prices will be temporarily reduced to enable you to test the purchase flow without incurring high charges. The Ops team will send you the following via email to enable this testing:
- Product code
- SNS topic(s)
- Product page URL

## Integration Requirements

### 4. Validate New Customers
After a customer subscribes to your product, they will be redirected to the fulfillment URL. The redirect is a POST request & includes a temporary token. Your app then needs to:
- Exchange the token for a customerID by calling ResolveCustomer in the AWS Marketplace Metering Service.
- After obtaining a customerID, persist it in your application for future calls.

### 5. Onboard New Customers
After successfully verifying a customer, onboard them onto your application. For example, have them fill out a form to create a new user account. Or, provide them with next steps to get access to the application.

### 6. Sending Metering Records
You use the BatchMeterUsage operation in the AWS Marketplace Metering Service to deliver metering records to AWS on behalf of your customers. We recommend using CloudTrail to monitor activity to ensure that billing information is being sent to AWS Marketplace. Keep in mind when sending metering records:
- Marketplace de-duplicates metering requests on the hour.
- Records sent every hour are cumulative.
- Best practice is to send records every hour, even if quantity is 0.

### 7. Monitor for Changes
Setup an SQS queue and subscribe to your product's SNS topic. This topic provides notifications about changes to customers' subscription. This enables you to know when to provide and revoke access for specific customers. Possible scenarios include: unsubscribes, successful subscription, & failed subscription.

### 8. Verify Successful Subscription
After you receive a subscription notification with subscribe-success, the customer account is ready for metering. Records that you send before this notification aren't metered. Additionally, we recommend waiting for this message before launching resources on behalf of a customer.

## Listing Process (continued after completion of integration)

### 9. End-to-end testing with AWS Marketplace
After you have completed all the integration requirements and tested the solution, notify the AWS Marketplace Ops team. They will then test the solution by verifying you have successfully sent metered records via BatchMeterUsage and sufficiently onboard new customers. After end-to-end testing is complete, you will have the chance to review the product page with the original prices. After giving approval, the AWS Marketplace Ops team will make the product page live in the public catalog.

![architecture](<images/AWS Marketplace - SaaS Integration Guide.pdf.png>)

                                    SaaS-Subscription
