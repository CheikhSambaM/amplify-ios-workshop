+++
title = "Update Amplify"
chapter = false
weight = 20
+++

Amazon Cognito does support Identity Federation out of the box with [Login With Amazon](https://login.amazon.com/), [Login with Google](https://developers.google.com/identity/sign-in/web/sign-in), [Login with Facebook](https://developers.facebook.com/docs/facebook-login/), [Signin with Apple](https://aws.amazon.com/blogs/security/how-to-set-up-sign-in-with-apple-for-amazon-cognito/), or any [OIDC](https://openid.net/connect/) or [SAMLv2](https://en.wikipedia.org/wiki/SAML_2.0) compliant identity provider.  

We use AWS Amplify command line to update the Amazon Cognito configuration on the backend and add support for Login With Facebook.

{{% notice warning %}}
It is important to not overwrite the values we entered previously, be sure to follow instructions carefully.
{{% /notice %}}

In a terminal, type:

```bash
cd $PROJECT_DIRECTORY
amplify update auth
```

1. What do you want to do? Choose **Update OAuth social providers** and **press enter** 

1. Select the identity providers you want to configure for your user pool:  Use the arrow keys to highlight **Facebook** and press **space**, then press enter.

1. Enter your Facebook App ID for your OAuth flow:.  Enter the **Facebook App Id** that you noted in previous section. (for example `2600000000004701`)

1. Enter your Facebook App Secret for your OAuth flow. Enter the **Facebook App Secret** that you noted in previous section. (for example `89xxxxxxxxxxxxxxxxxxxxxxxxxx605`)

`amplify` creates the required resources to depoy your storage service in the cloud.

![amplify update auth](/images/70-20-amplify-1.png)

## Create the API backend in the cloud

In a Terminal, assuming you are still in your project directory, type:

```bash
amplify push
```

1. Are you sure you want to continue? Review the table and verify the Auth service is being Updated.  Accept the default (**Yes**) and press enter.

Amplify updates the backend infrastructure, it adds the federation configuration to Amazon Cognito.  After a while, you should see the familiar message :

```text
✔ All resources are updated in the cloud
```

