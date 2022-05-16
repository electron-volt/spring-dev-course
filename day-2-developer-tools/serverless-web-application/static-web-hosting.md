# Static web hosting

## Host web app in Amplify&#x20;

We are going to use AWS Amplify to host the code we just pushed into CodeCommit.&#x20;

Navigate to AWS Amplify.&#x20;

Click **get started.**&#x20;

Under Host your web app, click **get started again.**&#x20;

![](<../../.gitbook/assets/image (206) (1).png>)

In "From your existing code", choose CodeCommit

![](<../../.gitbook/assets/image (154).png>)

In "Add repository branch", pick the wildrydes-site repo and master branch.&#x20;

![what is a monorepo?](<../../.gitbook/assets/image (287) (1).png>)

Check the box for **Allow AWS Amplify to automatically deploy all files hosted in your project root directory.**

Click **Next**, then **Save and deploy.**&#x20;

Wait while Amplify creates the web app.&#x20;

![steady as she goes](<../../.gitbook/assets/image (156).png>)

When the deployment is done, click the https://master...amplifyapp.com link to view the site!&#x20;

#### Problems with the deployment?&#x20;

Do not modify the IAM role attached to your Amplify app. \
If the first Amplify deployment does not work, then move on to the next step.&#x20;

After you have pushed your changes to your CodeCommit repository, Amplify will trigger a new deployment and that should run.&#x20;

## Modify the site

Let's see what happens if we want to make a change to our app.

In the Cloud9 IDE, make a change to **index.html:**&#x20;

change the title of the site to \<title>Wild Rydes - Rydes of the future!\</title>

![](<../../.gitbook/assets/image (299).png>)

Then&#x20;

`git add index.html`

`git commit -m "changed title"`&#x20;

`git push`&#x20;

The app will go from this

![](<../../.gitbook/assets/image (227).png>)

To this:

![](<../../.gitbook/assets/image (160).png>)

## Status

We have our web application up and running. We've seen how easy it is to host web apps in Amplify and how changes made to our repo are immediately deployed in the app.&#x20;
