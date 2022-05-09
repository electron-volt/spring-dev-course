# 🥂 End of day 2

## Clean up&#x20;

Excellent work, well done!&#x20;

Before you go though, let's make sure to clean up all the resources that we created today.&#x20;

* S3 buckets
  * there are buckets created by Elastic Beanstalk and CodePipeline.&#x20;
* Cloud9 IDE's
* CodeCommit repos
* CodeBuild projects
* CodeArtifact domains&#x20;
* CodePipeline pipelines
* Elastic Beanstalk web applications.&#x20;

## Unable to delete Elastic Beanstalk S3 bucket

Go to the bucket

Go to Permissions tab&#x20;

Find the bucket policy&#x20;

Click "Edit"

Find the code block inside {} that says ... "Deny" .... "Action": "s3:deleteBucket"&#x20;

Remove the code block (and the , after it)&#x20;

Save.&#x20;

Now you should be able to delete the bucket.&#x20;
