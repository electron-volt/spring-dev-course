# LAB: Create bucket

In this lab we are going to create an S3 bucket.&#x20;

There are instructions for both the CLI and the console. You can choose one option or do both.&#x20;

## Create a bucket&#x20;

### In the console

Navigate to the S3 console.

Depending on whether or not your account already has buckets, your landing page may look different. It should have a button that says **Create bucket** there somewhere:

![](<../../.gitbook/assets/image (463).png>)

Do the following:

* Create bucket
* Choose a name. It has to be globally unique, so try **your-name-ddmmyyyy**
* Pick a region&#x20;
* Object ownership: ACLs **disabled**&#x20;
* Block public access: block _all_ public access
* Versioning: **disable**
* No tags
* No encryption&#x20;
* Create bucket.&#x20;

### In the CLI&#x20;

Here is the command to create bucket eve-test2-10012022 in the eu-north-1 region:

```
aws s3api create-bucket \
 --bucket eve-test2-10012022 \
 --region eu-north-1 \
 --create-bucket-configuration LocationConstraint=eu-north-1
```

Note: if you do not specify region and LocationConstraint, it will throw an error.&#x20;

This command returns the following:

```json
{
    "Location": "http://eve-test2-10012022.s3.amazonaws.com/"
}
```

Now this shows why bucket names need to be globally unique: they are a part of the DNS name of the bucket.

### **Comparison**

Here are the two buckets, test was created in the console and test2 via the CLI.&#x20;

![S3 buckets](<../../.gitbook/assets/image (241).png>)

Notice the difference in Access? This is your first taste of the complexity of S3 public access.&#x20;

"Objects can be public" does not mean that objects are public - it just means that they _might_ be. IT means that they are not explicitly kept from being public.&#x20;

## Add a file

Let's try adding a file to both buckets.&#x20;

### In the console

Click the name of either bucket. Then select upload and put an object into the bucket.&#x20;

An object can be between 0 KB and 5 TB in size. The filetype can be anything: mp3, jpg, docx, iso, pdf, vmdk... to S3 it's all just 1's and 0's.&#x20;

### In the CLI

Here is the command to upload file `s3_temp_file.txt` to bucket `bucketname`:

```
aws s3 cp s3_temp_file.txt s3://bucketname
```

## Clean up

Empty buckets don't cost anything. You only pay for the objects based on their storage class (topic of the next page) and the object size.&#x20;

Still it is instructive to see that S3 will not let you delete a bucket unless it is empty. So for **one** of your buckets (doesn't matter which)

* empty the bucket&#x20;
* delete the bucket once it is empty.&#x20;

We are going to need a bucket in a future lab, so only delete one bucket.&#x20;
