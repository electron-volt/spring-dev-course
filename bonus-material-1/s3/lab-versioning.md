# LAB: Versioning

## Enable versioning

Let's go to the S3 console and find our bucket.&#x20;

Click on the name of the bucket and then the properties tab:

![Properties tab](<../../.gitbook/assets/image (274).png>)

Under Bucket Versioning, click Edit.&#x20;

You have two options for versioning:

* suspend&#x20;
* enable

We want to select "**enable**" and **Save changes.**&#x20;

## Upload file

#### Create and upload version 1

* Make a text file in an editor.&#x20;
* It can say "This is version 1".&#x20;
* Save it as version1.txt
* Upload the file version1.txt to your bucket.&#x20;

#### Create and upload version 2

Then in the text editor, change the text of file version1.txt to say "This is version 2".&#x20;

Save changes and upload again.&#x20;

Your bucket should now only have one object: **version1.txt.**&#x20;

Click on the name of the object, then click the Versions tab.

&#x20;

![Versions of version1.txt](<../../.gitbook/assets/image (457) (1).png>)

Select both and click Open. They open in different tabs. First one:

![We wouldn't want to lose this literary gem so I'm glad it's safe](<../../.gitbook/assets/image (22).png>)

Second one:

![punctuation is important people](<../../.gitbook/assets/image (259).png>)

I know, "these labs really are simple".&#x20;

## The end

There you have it, S3 versioning. Versioning itself is very simple. Where things get hairy is when you have some kind of replication to another bucket and then you delete a file in one bucket - does the deletion also propagate to the other bucket?&#x20;

What if you delete the replica from the other bucket, does the original get destroyed?&#x20;

For those labs we'd need replication.&#x20;

If we did a lab where we set up cross-account or cross-region replication along with different encryption keys in different regions and versioning enabled, you would be begging for these simple labs to return...&#x20;

## Clean up&#x20;

Let's attempt to delete the contents of a bucket that has versioning enabled.&#x20;

Go ahead and delete version1.txt from your bucket.

The bucket appears empty now as it has no objects in it. However when you go to delete the bucket, AWS say the bucket is not empty: that is because of versioning.&#x20;

You can use the Empty button to first force the bucket to empty, then delete the bucket.&#x20;
