# LAB: Static website

S3 can also be used to serve static resources over the internet. Let's make a simple little homepage for ourselves.&#x20;

## Create a public bucket

First we want to create a new bucket that will be open to the public.&#x20;

1. Create bucket
2. Give it a unique name like **Yourname-ddmmyyyy**
3. **Uncheck** "Block all public access"
4. Leave all "Block..."  checkboxes unchecked
5. Check the box next to "I acknowledge that the current settings might result in this bucket and the objects within becoming public."
6. Create bucket.

![](<../../.gitbook/assets/image (395).png>)

## Static website hosting

Once your bucket is ready, click on it and open the Properties tab.&#x20;

Scroll all the way to the bottom for Static website hosting and click Edit.

![Keep scrolling till you get there](<../../.gitbook/assets/image (194).png>)

On the way down you will see strange things like Requester pays, Transfer acceleration, Object lock.. yes, S3 is very feature-heavy.&#x20;

### Settings&#x20;

We want to **Enable static website hosting.**&#x20;

Hosting type: **host a static website**

Index document: **index.html**

Error document: **error.html**

Leave redirection rules blank and click **Save changes.**&#x20;

![](<../../.gitbook/assets/image (445).png>)

## Upload the html

You will need two HTML files in your bucket:

**error.html**

**index.html**

The content of the files can be anything.&#x20;

You are free to make your own HTML files or use the files below.&#x20;

Upload both documents to the bucket.&#x20;

{% code title="index.html" %}
```html

<!DOCTYPE html>
<html>
<head>
  <title>My First HTML</title>
  <meta charset="UTF-8">
</head>
<body>

  <h2 title="made you look">Foo</h2>
  
  <p title="yo">Hover over me please</p>

<h2>Button</h2>
<p>Whatever you do - DO NOT PUSH THIS BUTTON</p>

<button onclick="location.href='https://yourname-0605.s3.eu-central-1.amazonaws.com/error.html'" type="button">
    Don't do it</button>

</body>
</html>




```
{% endcode %}

{% code title="error.html" %}
```html
<!DOCTYPE html>
<html>
<head>
  <title>Error</title>
  <meta charset="UTF-8">
</head>
<body>

<h1>There's been an error</h1>
<h2>There's been an error</h2>
<h3>There's been an error</h3>
<h4>There's been an error</h4>
<h5>There's been an error</h5>
<h6>Something has gone terribly wrong</h6>

</body>
</html>
```
{% endcode %}

## Permissions

You'd think that it's, right? But if you try to view your website, you will get a 403 error for Forbidden. Just because public access isn't _blocked_ does not mean that it is _allowed_.&#x20;

![](<../../.gitbook/assets/image (254).png>)

Go back to your website bucket and go to the **Permissions** tab. There you will find Bucket policy. Click **edit**.&#x20;

Paste in the following policy,

Change line 12 to contain the ARN of your bucket (you will find the ARN in the console above the Policy text area).

Make sure to include the / and \* at the end of line 12!

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::bucket-name-here/*"
            ]
        }
    ]
}
```

Then **Save changes.**&#x20;

## View your website

![index.html](<../../.gitbook/assets/image (147).png>)

![error.html](<../../.gitbook/assets/image (433).png>)

## Clean up&#x20;

Let's delete the website bucket, because it really is now open to the entire world.&#x20;

### Moral of the story

You begin to appreciate that AWS is not. going. to. let. you. expose your bucket to the public unless you are absolutely sure that is what you want. Even then, they show you warnings:

![](<../../.gitbook/assets/image (347).png>)

