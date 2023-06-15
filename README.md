# CloudFormation template for Home Assistant Alexa Smart Home Integration

[template.yaml](template.yaml) is a CloudFormation template that creates a Lambda function within your AWS account that can be associated with an Alexa Smart Home skill. This removes the need to perform the manual configuration steps listed within Home Assistant's documentation on [Create an AWS Lambda Function](https://www.home-assistant.io/integrations/alexa.smart_home/#create-an-aws-lambda-function).

The Lambda function is created using the same source code referenced in the Home Assistant documentation.

## Pre-requisites

 * Home Assistant is configured with an SSL/TLS certificate. Using the [Duck DNS](https://www.home-assistant.io/addons/duckdns/) add-on automatically obtains a certificate via Let's Encrypt. The [Set up encryption using Let's Encrypt](https://www.home-assistant.io/blog/2015/12/13/setup-encryption-using-lets-encrypt/) blog post provides pointers if you do not want to use Duck DNS.
 * Home Assistant is available from the Internet over port 443 (HTTPS). Whilst Alexa's documentation states that the authorization server must be available on port 443, port 8443 also appears to work.
    * Many ISPs now provide access to the IPv4 Internet via CG-NAT. This means that initiating connections from the Internet to your Home Assistant instance will not work. In this case, contact your ISP to determine if you can disable CG-NAT.
 * An [Amazon Developer](https://developer.amazon.com/) account.
 * An [Amazon Web Services](https://aws.amazon.com/free/) account.

## Steps

1. Follow the steps within Home Assistant's documentation on [Create an Amazon Alexa Smart Home Skill](https://www.home-assistant.io/integrations/alexa.smart_home/#create-an-amazon-alexa-smart-home-skill). Take a note of your skill ID.
1. Deploy the CloudFormation template. Note you will need to deploy to a specific AWS region depending on your Alexa skill's locale.
    * US East (N.Virginia) region for English (US) or English (CA) skills
    * EU (Ireland) region for English (UK), English (IN), German (DE), Spanish (ES) or French (FR) skills
    * US West (Oregon) region for Japanese and English (AU) skills
1. Add the following stanza to your Home Assistant configuration file (`/config/configuration.yaml`). Note this is a minimal configuration, you can configure further per Home Assistant's documentation on [Alexa Smart Home Component Configuration](https://www.home-assistant.io/integrations/alexa.smart_home/#alexa-smart-home-component-configuration). In particular, you may wish to filter entities that Alexa can discover. Without performing this step, you won't be able to perform device/entity discovery. Be sure to restart Home Assistant Core.

    ```
    alexa:
    smart_home:
    ```

1. Follow the steps within Home Assistant's documentation on [Configure the Smart Home Service Endpoint](https://www.home-assistant.io/integrations/alexa.smart_home/#configure-the-smart-home-service-endpoint). You will use the Lambda function ARN (available in the `LamdaFnArn` stack output).
1. Follow the steps within Home Assistant's documentation on [Account Linking](https://www.home-assistant.io/integrations/alexa.smart_home/#account-linking). These steps will result in the Alexa skill being enabled and linked to your Home Assistant instance. Devices can be discovered during this process.