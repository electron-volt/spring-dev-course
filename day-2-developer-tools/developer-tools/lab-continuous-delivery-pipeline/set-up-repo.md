# Set up repo

## Setting up the source

### Prepare your GitHub

* Sign in to GitHub.&#x20;
  * If you do not already use GitHub, you can create a free account at github.com&#x20;
* Once you have logged in to your account, click on your username at the top right corner.
* Click **settings**&#x20;

![](<../../../.gitbook/assets/image (291).png>)

* On the left side panel, scroll all the way to the bottom
* Click on **<> Developer settings.**&#x20;
* Then from the left side panel, click **Personal access tokens.**&#x20;
* Click **generate new token**&#x20;
* Name: **aws labs**
* Expiration: 30 days
* Scope: **repo**

![generating a new token](<../../../.gitbook/assets/image (332).png>)

#### Copy your token

When you create your token, you are shown a string. Make sure to copy it and store it somewhere safe, because you will not be able to view it again.&#x20;

### Fork the repository&#x20;

Once you have your access token,&#x20;

* Go to this repository: [https://github.com/aws-samples/aws-elastic-beanstalk-express-js-sample](https://github.com/aws-samples/aws-elastic-beanstalk-express-js-sample)
* Fork the repository&#x20;

![forking](<../../../.gitbook/assets/image (126).png>)

* For owner use your own GitHub
* You can keep the name aws-elastic-beanstalk-express-js-sample for the repository

![forking ](<../../../.gitbook/assets/image (435).png>)

### Clone the repository

Once you have forked the repository, you will be taken to this page:

![your forked repo](<../../../.gitbook/assets/image (211).png>)

* Click on the green **Code** button&#x20;
* copy the HTTPS URL

Make sure it's _your_ repo you are cloning - the url should be&#x20;

[https://github.com/YOUR USER NAME NEEDS TO BE HERE/aws-elastic-beanstalk-express-js-sample.git](https://github.com/electron-volt/aws-elastic-beanstalk-express-js-sample.git)

Next we want to clone the repository. You can use CloudShell or your local machine.&#x20;

In the terminal, run&#x20;

```
git clone <the URL you copied>
```

then change into the aws-elastic-beanstalk-express-js-sample directory.&#x20;

## First commit

In the aws-elastic-beanstalk-express-js-sample directory

* open the file `app.js`&#x20;
* on line 5 replace 'Hello World!' with any text you want
* save changes

![Line 5 in app.js](<../../../.gitbook/assets/image (354).png>)

#### Push changes

Push your changes to your repo with&#x20;

```
git add app.js
git commit -m "changed one line"
git push
```

If you haven't used git before in this terminal, you will be asked to run&#x20;

```
git config --global user.name "yourname" 
git config --global user.email "name@example.com"
```

You will then be prompted for a username and password.&#x20;

* username: your GitHub username
* password: the string that you copied after you generated the personal access token.&#x20;

![](<../../../.gitbook/assets/image (419).png>)

## Where we're at

This is what we have now:

![we turned AWS into an infinite void of white nothingness. Great.](<../../../.gitbook/assets/image (409).png>)
