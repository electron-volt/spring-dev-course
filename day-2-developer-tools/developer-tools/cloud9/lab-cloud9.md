# LAB: Cloud9

## Create an environment

Navigate to Cloud9 in the AWS console.&#x20;

Click on **Create environment.**

![new environment creation](<../../../.gitbook/assets/image (92).png>)

Name your environment **yourname-cloud9.**

Click **Next step.**&#x20;

![naming the environment ](<../../../.gitbook/assets/image (116).png>)

#### Environment settings&#x20;

In the dialogue that appears, select the following:

Environment type: **New no-ingress EC2 instance**&#x20;

Instance type: **t2.micro** (or whatever is free-tier eligible in the region you are in)&#x20;

Platform: **Amazon Linux 2.**&#x20;

![environment settings](<../../../.gitbook/assets/image (28).png>)

Leave all other settings as they are.&#x20;

Choose **next step.**

You will see a Review page.&#x20;

Click **create environment**.

AWS begins to create the environment:

![it may take a while](<../../../.gitbook/assets/image (201).png>)

## Basic tour

Once your environment is ready, we will take it for a little test drive.&#x20;

There is a terminal window on the bottom of the screen.

Type in&#x20;

```
npm install readline-sync
```

like so:

![every hacking scene in Hollywood](<../../../.gitbook/assets/image (393).png>)

### New file

Once you get your prompt back, make a new file called **hello-cloud9.js**

Paste this in:

{% code title="hello-cloud9.js" %}
```javascript
var readline = require('readline-sync'); 
var i = 10; 
var input;

console.log("Hello Cloud9!"); 
console.log("i is " + i);
do { 
    input = readline.question("Enter a number (or 'q' to quit): "); 
    if (input === 'q') { 
        console.log('OK, exiting.')
    } 
    else { 
        i += Number(input); 
        console.log("i is now " + i); 
    } 
} while (input != 'q');

console.log("Goodbye!");
```
{% endcode %}

I know, super interesting code. Also bad things happen if the input is NaN.&#x20;

### Run the program&#x20;

You can find your files by clicking on the folder icon on the left side panel:

![](<../../../.gitbook/assets/image (377).png>)

Open the file **hello-cloud9.js**

There is a button to Run your program. **Click it.**&#x20;

### Debug

If you click the margin next to line 10, a red dot appears - a breakpoint.

Also notice the little i that says we are missing a semicolon on line 10?&#x20;

![](<../../../.gitbook/assets/image (261).png>)

On the right hand side panel, you'll see a bug icon - click it to open the debugger.

&#x20;

![](<../../../.gitbook/assets/image (462).png>)

#### Watch expression&#x20;

In the debug window, under Watch expressions type "input":

![watch expression: input](<../../../.gitbook/assets/image (462) (1).png>)

Next:

1. Run the code again&#x20;
2. Enter any number in the terminal&#x20;
3. Press enter

Execution has stopped:

![we are stuck here](<../../../.gitbook/assets/image (61).png>)

In the Debugger window, there is now a blue arrow ("Play") which allows you to resume:

![Blue arrow](<../../../.gitbook/assets/image (293).png>)

To stop the debugger, click the red square in the terminal window.

![Red Square](<../../../.gitbook/assets/image (21).png>)

## Clean up&#x20;

At the top left hand corner, click the cloud icon and choose Go to your dashboard.

![](<../../../.gitbook/assets/image (271).png>)

Then click the Delete button. This terminates the Cloud9 environment.&#x20;
