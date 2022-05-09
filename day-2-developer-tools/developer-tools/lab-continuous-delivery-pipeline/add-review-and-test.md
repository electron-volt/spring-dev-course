# Add review and test

Next we will add an approval action to a stage at the point where you want the pipeline execution to stop so we can manually approve or reject the action.&#x20;

Manual approvals are useful to have someone else review a change before deployment. If the action is approved, the pipeline execution resumes. If the action is rejected—or if no one approves or rejects the action within seven days—the result is the same as the action failing, and the pipeline execution does not continue.

What we'll do:

* Add a review stage to your pipeline
* Make a change to our GitHub repo
* Manually approve a change before it is deployed.

## Add review stage

Go to CodePipeline and click on the pipeline we created.&#x20;

* Click the **white "Edit" button** near the top of the page.
* Click the **white "Add stage"** button between the "Build" and "Deploy" stages.

![](<../../../.gitbook/assets/image (337).png>)

* In the "Stage name" field type **"Review."**
* In the "Review" stage, click the **white "Add action group" button**.
* Under "Action name" type **"Manual\_Review."**
* From the "Action provider" select **"Manual approval."**

![](<../../../.gitbook/assets/image (243).png>)

Ignore all the other fields like SNS topic, URL etc.&#x20;

* Click the **orange "Done" button**.
* Click the **orange "Save" button at the right top corner of the screen** to confirm the changes.&#x20;

You will now see your pipeline with four stages: "Source," "Build," "Review," and "Deploy."

![](<../../../.gitbook/assets/image (20).png>)

## Push a change to the repository

In your terminal (local or CloudShell), open the file app.js.&#x20;

Change the text on line 5 again.&#x20;

Use git add app.js, git commit -m "changed line 5" and git push to push your changes.&#x20;

You will be asked for your GitHub username and personal access token.&#x20;

Once the changes have been pushed to the repo, go back to AWS CodePipeline.&#x20;

## Approve the review

I made a commit with message "changed line 5" and pushed it to GitHub.&#x20;

If you go look at Elastic Beanstalk, the web application will not show your most recent changes.&#x20;

Here is what is happening over at CodePipeline:

![waiting for manual approval](<../../../.gitbook/assets/image (412).png>)

Back in CodePipeline, press the white Review button. A dialogue opens:



![Review](<../../../.gitbook/assets/image (28) (1).png>)

You can type a comment like "All looks good!"&#x20;

Then press Approve. The wheels are turning:

![Pipeline moves to the next stage](<../../../.gitbook/assets/image (154).png>)

You should also see a green banner that says "Action Manual\_review was approved".

Eventually the pipeline is complete.&#x20;

## View the changes

Go to Elastic Beanstalk and find the URL of your web app. Open it and marvel at the changes that have occurred:

![](<../../../.gitbook/assets/image (439).png>)

Elastic Beanstalk should now reflect the most recent changes pushed to GitHub.&#x20;
