# Using Code Snippets Create a State to Send an Amazon SNS message<a name="tutorial-code-snippet"></a>

In this tutorial you will generate a code snippet that sends a text message using Amazon Simple Notification Service \(SNS\)\. Step Functions integrates with some other AWS services, and in this tutorial you will pass parameters directly to Amazon SNS from your state machine definition\.

For more information, on how Step Functions integrates with other AWS services directly from the Amazon States Language, see:
+ [Service Integrations](concepts-connectors.md)
+ [Code Snippets](concepts-code-snippets.md)
+ [Pass Parameters to a Service API](connectors-parameters.md)

**Topics**
+ [Step 1: Generate a Code Snippet](#tutorial-code-snippet-1)
+ [Step 2: Update Your State Machine Definition](#tutorial-code-snippet-2)
+ [Step 3: Start an Execution](#tutorial-code-snippet-3)

## Step 1: Generate a Code Snippet<a name="tutorial-code-snippet-1"></a>

To generate a code snippet, you must start by editing a state machine definition\. 

1. From the Step Functions console, select **Get started** or **Create state machine**\.

1. Choose **Author with code snippets** and type a name for your state machine\.

   The default `HelloWorld` state machine is displayed in the **State machine definition**\.  
![\[Hello World definition\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-code-snippet-sns.png)

1. Select the **Generate Code Snippet** drop\-down and choose **Amazon SNS: Publish a message**\.

   The **Generate SNS Publish task state** window is displayed\.

1. Under **Destination**, select **Enter phone number**, and enter your cellphone number\.

   Use the format `[+][country code][subscriber number including area code]`\. For example: `+12065550123`\.

1. Under **Message**, select **Enter message** and type some text that you want to send as an SMS message\.
**Note**  
You can also choose **Specify message at runtime with state input**\. This option will allow you to use a reference path to select a message from the input of your state machine execution\. For more information see:  
[Input and Output Processing in Step Functions](concepts-input-output-filtering.md)
[Reference Paths](amazon-states-language-input-output-processing.md#amazon-states-language-reference-paths)
[Pass State Input as Parameters Using Paths](connectors-parameters.md#connectors-parameters-path)

As you configure options on the **Generate SNS Publish task state** page, the **Preview** section updates with the Amazon States Language code for a task state with the necessary options\. For instance, if you select these options:

![\[SNS state options\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-code-snippet-sns-options.png)

With these options selected, the generated code snippet displayed in the **Preview** area is:

```
"Amazon SNS: Publish a message": {
  "Type": "Task",
  "Resource": "arn:aws:states:::sns:publish",
  "Parameters": {
    "Message": "Hello from Step Functions!",
    "PhoneNumber": "+12065550123"
  },
  "Next": "NEXT_STATE"
}
```

**Note**  
Under the **Task state options** section you can also configure `Retry`, `Catch`, and `TimeoutSeconds` options\. See [Error Handling](concepts-error-handling.md)\.

## Step 2: Update Your State Machine Definition<a name="tutorial-code-snippet-2"></a>

Now that you have configured your Amazon SNS options, paste the generated code snippet into your state machine definition and update the existing Amazon States Language code\.

1. Once you have entered the configuration options, and have reviewed the code in the **Preview** section, select **Copy to clipboard**\.

1. Place your cursor after the closing bracket of the `HelloWorld` state in your state machine definition\.  
![\[Place cursor at end of the HelloWorld state\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-code-snippet-sns-place-cursor.png)

   Enter a comma, press Enter to start a new line, and paste your code snippet into your state machine definition\.

1. Change the last line of the `Amazon SNS: Publish a message` state from *`"Next": "NEXT_STATE"`* to *`"End": true`*\.

1. Change the last line of the `HelloWorld` state from *`"End": true`* to *`"Next": "Amazon SNS: Publish a message"`*\.

1. Choose ![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-getting-started-refresh.png)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/)![\[refresh\]](http://docs.aws.amazon.com/step-functions/latest/dg/) in the **Visual Workflow** pane\. Check the visual workflow to ensure your new state is included\.  
![\[Review visual workflow\]](http://docs.aws.amazon.com/step-functions/latest/dg/images/tutorial-code-snippet-sns-visual-workflow.png)

1. \(Optional\) Indent the JSON to make your code easier to read\. Your state machine definition should look like this\.

   ```
   {  
      "StartAt":"HelloWorld",
      "States":{  
         "HelloWorld":{  
            "Type":"Pass",
            "Result":"Hello World!",
            "Next":"Amazon SNS: Publish a message"
         },
         "Amazon SNS: Publish a message":{  
            "Type":"Task",
            "Resource":"arn:aws:states:::sns:publish",
            "Parameters":{  
               "Message":"Hello from Step Functions!",
               "PhoneNumber":"+12065550123"
            },
            "End":true
         }
      }
   }
   ```

1. Choose **Next**\.

1. Create or enter an IAM role\.
   + To create a new IAM role for Step Functions, select **Create an IAM role for me**, and enter a **Name** for your role\.
   + If you have [previously created an IAM role](procedure-create-iam-role.md) with the correct permissions for your state machine, select **Choose an existing IAM role**\. Select a role from the drop\-down, or provide an ARN for that role\. 
**Note**  
If you delete the IAM role that Step Functions creates, Step Functions can't recreate it later\. Similarly, if you modify the role \(for example, by removing Step Functions from the principals in the IAM policy\), Step Functions can't restore its original settings later\. 

1. Choose **Create state machine**\.

## Step 3: Start an Execution<a name="tutorial-code-snippet-3"></a>

Once it has been created, the page from your new state machine is displayed\.

1. Review the details of your state machine, including the Amazon Resource Name \(ARN\), the related IAM ARN, and the state machine definition\.

1. Select the **Executions** tab and choose **Start execution**\.

1. \(Optional\) Enter a name for your execution\. 
**Note**  
If we had chosen **Specify message at runtime with state input** when creating our Amazon SNS code snippet, we would include a message in the **Input \- optional**\. For now you can use the default state input\.

   Choose **Start execution**\.

If you configured a valid cell phone number in your code snippet, you should have received a text message from Amazon SNS that was triggered directly by your state machine execution\.