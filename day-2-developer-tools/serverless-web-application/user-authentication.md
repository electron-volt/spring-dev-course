# User authentication

Next we are going to use Cognito to manage user's accounts. The feature we are going to use is Cognito user pools, which is a user directory in AWS.&#x20;

When users visit the website they will first register a new user account. For the purposes of this workshop we'll only require them to provide an email address and password to register.&#x20;

After users submit their registration,&#x20;

* Amazon Cognito will send a confirmation email with a verification code to the address they provided.&#x20;
* To confirm their account, users will return to your site and enter their email address and the verification code they received.&#x20;
  * You can also confirm user accounts using the Amazon Cognito console if want to use fake email addresses for testing.

## Create a Cognito user pool&#x20;

Navigate to Cognito.&#x20;

Note: if your console looks different from the screenshots, look for a banner at the top of the screen offering the "new console experience" or something. If you click it, it will take you to the new layout.&#x20;

Click **Create user pool**&#x20;

![](<../../.gitbook/assets/image (299) (1).png>)

Then fill in these details:

![](<../../.gitbook/assets/image (296).png>)

Click Next to get to password requirements.&#x20;

#### Security requirements

You can proceed with the Cognito defaults for passwords or choose your own.&#x20;

We do not want MFA for this lab.&#x20;

![](<../../.gitbook/assets/image (155) (1) (1) (1).png>)

#### Sign-in

![](<../../.gitbook/assets/image (461).png>)

#### Email

For email, **send email with Cognito.**&#x20;

![](<../../.gitbook/assets/image (155) (1).png>)

#### User pool&#x20;

When asked for user pool name: **yourname-WildRydes**.&#x20;

![](<../../.gitbook/assets/image (213).png>)

#### Initial app client

For app client name, use **yourname-WildRydesWebApp.**

![Do not generate a client secret](<../../.gitbook/assets/image (236).png>)

#### **Review and create**

You have a chance to review your settings.&#x20;

Then click **Create user pool.**&#x20;

### User pool id and App client id

In AWS Cognito, go to your new user pool. In the overview you will find User pool ID.&#x20;

![](<../../.gitbook/assets/image (397) (1).png>)

Then go to tab **App integration** and scroll all the way down. You will find your Client ID there.&#x20;

![](<../../.gitbook/assets/image (411).png>)

## Modify config.js

In the Cloud9 IDE, navigate to directory js. Using your favourite text editor, open js/config.js.

![initial config.js](<../../.gitbook/assets/image (144).png>)

Replace the placeholder values&#x20;

* userPoolId
* userPoolClientID
* region

with real ones.&#x20;

Leave api and invokeUrl empty - this part comes later.&#x20;

Push changes to the git repo.&#x20;

## Validate

Wait while Amplify redeploys your app. Once it is ready, visit the website and press the Giddy up! button.&#x20;

![registration page](<../../.gitbook/assets/image (54).png>)

You can use a fake address like **test@foo.bar.**&#x20;

### Verify in Cognito

Back in the Cognito console, we see our test user:

![](<../../.gitbook/assets/image (231) (1).png>)

Click the user name, then from actions select confirm account.

![](<../../.gitbook/assets/image (325).png>)

Then&#x20;

* go to your-amplify-domain/signin.html&#x20;
* sign in using your fake email and the password you made up.&#x20;

This is a sign of success:

![uuh capitol hill](<../../.gitbook/assets/image (136) (1).png>)

