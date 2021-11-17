# Running Python on AWS

## Overview

This workshop teaches you how to use Python with AWS services. It will then teach you how to host your Python scripts on AWS so that they can be invoked by common event patterns.

## Task 1: Lab Preparation

This lab was written with a common IDE in mind, AWS Cloud9. You can use any IDE that you want, but you may have to install tools such as Python and AWS SAM CLI to complete the instructions.

If you want to use Cloud9, this task will walk you through launching an AWS CloudFormation template that creates the network, Cloud9 environment, and IAM roles that will be used.

1. On the navigation bar, in the unified search bar, search for and choose `CloudFormation`

1. Choose <span style="background-color:fff; font-weight:bold; font-size:.7em; color:#545b64; position:relative; top:-1px; border-color:#545b64; border-radius:2px; border-width:1px; border-style:solid; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Create stack** <i class="fas fa-caret-down"></i></span>

1. Choose **With new resources (standard)**.

1. For Amazon S3 URL enter `https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/pythonForAWSSupplimental/template.yml`

1. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Next**</span>

1. Enter a stack name such as `python-lab`

1. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Next**</span>

1. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Next**</span>

1. Check the box for **I acknowledge that AWS CloudFormation might create IAM resources.**, and choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Next**</span>

This stack will take about 3 minutes to launch. It creates a VPC with a public subnet, a role that you will use for your lambda functions, and an AWS Cloud9 environment.

## Task 2: Complete the Learn Python on AWS Workshop

The **Learn Python on AWS Workshop** will teach you the basics of the python programming language using Amazon Web Services (AWS).

It is aimed at beginners who have never programmed in python and it uses similar methods of explaining the basics as other books and tutorials on the python programming language.

It differs in that all the examples aim to get you started building on AWS rather than learning how to build a game, website, manipulate or visualize data.

[https://learn-to-code.workshop.aws/](https://learn-to-code.workshop.aws/)

Using the Cloud9 Environment you created in Task 1, complete steps 2.2 through 13 of this workshop.

Don't worry if you have to break up the workshop over multiple days. Each section in the workshop stands on it's on and you are not required to use the same environment to complete each step. If your sandbox environment expires you can repeat task 1 of these instructions and then pick up where you left off.

## Task 3: How to run your scripts without servers on AWS

You have learned the basics of the python language and how to interact with AWS services. These skills are important to manipulate and use cloud services.  But what if you didn't want to run the scripts from your workstation or on-demand?  The usefulness of these applications are increased exponentially when you can run them from a web address, on a schedule, or in response to an event.

To do this, you need a computational service to run your python scripts in response to these types of interactions. That service is AWS Lambda.

### Task 2.1: The Lambda handler function

The Lambda function handler is the method in your function code that processes events. When your function is invoked, Lambda runs the handler method. When the handler exits or returns a response, it becomes available to handle another event.

You can use the following general syntax when creating a function handler in Python:

```python
def handler_name(event, context): 
    ...
    return some_value
```

The handler function provides the same functionality in Lambda that your main function did in the Python workshop. It allows you to control the flow of your script.

You do not have to call the function from your code. Lambda does that when the Lambda function is invoked.

You can leave the conditional check for **__main__** in your code since lambda doesn't run under that namespace. If you want to be able to run the same script from lambda and your cli, you can still use code like the code below.

```python
def lambda_handler(event, context):
    # TODO implement
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }

if __name__=="__main__":
    event = {}
    lambda_handler(event, None)
```

The event object would be statically set as a test event or you can use the argument parser to run your code from the cli.

What are the two arguments added to the lambda_handler function?

### Task 2.2: The event and context required arguments

When Lambda invokes your function handler, the Lambda runtime passes two arguments to the function handler:

- The first argument is the [event object](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-concepts.html#gettingstarted-concepts-event). An event is a JSON-formatted document that contains data for a Lambda function to process. The Lambda runtime converts the event to an object and passes it to your function code. It is usually of the Python dict type. It can also be list, str, int, float, or the NoneType type.

    The event object contains information from the invoking service. When you invoke a function, you determine the structure and contents of the event. When an AWS service invokes your function, the service defines the event structure. For more information about events from AWS services, see [Using AWS Lambda with other services](https://docs.aws.amazon.com/lambda/latest/dg/lambda-services.html).

- The second argument is the context object. A [context object](https://docs.aws.amazon.com/lambda/latest/dg/python-context.html) is passed to your function by Lambda at runtime. This object provides methods and properties that provide information about the invocation, function, and runtime environment.

### Task 2.3: Create a simple lambda function in the console

You create a Node.js Lambda function using the Lambda console. Lambda automatically creates default code for the function.

To create a Lambda function with the console

10. On the navigation bar, in the unified search bar, search for and choose `Lambda`

1. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Create Function**</span>

1. Under Basic information, do the following:

- For Function name, enter `test-function`.

- For Runtime, choose **Python 3.9**

13. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Create Function**</span>

Lambda creates a Python function and an execution role that grants the function permission to upload logs. The Lambda function assumes the execution role when you invoke your function, and uses the execution role to create credentials for the AWS SDK and to read data from event sources.

### Task 2.4: Edit the function code

The Lambda function created has the code stored in the lambda_function.py file. You can see the contents of this file in the Code Source section of the page.

The script, imports the built-in json dependency and has one function, the lambda_handler. This initial code, will just return an object with a success status code and string in the body.

14. Replace **# TODO implement** with the following code.

```python
    print(event, context.get_remaining_time_in_millis())
```

15. Choose <span style="background-color:fff; font-weight:bold; font-size:.7em; color:#545b64; position:relative; top:-1px; border-color:#545b64; border-radius:2px; border-width:1px; border-style:solid; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Deploy**</span>

If you change your code in the editor window, you have to deploy it before it becomes available for the next invocation. The code you added, will print the event object and get the number of milliseconds that the lambda function can run before it times out from the context object.

### Task 2.5: Test your lambda function

16. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Test**</span>

1. For **Event-name**, enter `test_event`

1. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Create**</span>

1. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Test**</span> again.

In this task you first created a test event. You left the default event in since anything will work for this function, but you can modify that test event if your lambda function expects a valid event to be passed.

The Execution results tab opened when you tested the function with this new test event. You can see the Response was returned and the full log output from the lambda function. The second line of the log shows your event and an integer which is the number of milliseconds left that the lambda function can run.

For small lambda functions using the console to create them will work but this isn't the most efficient method that you can use when developing lambda functions.

### Task 2.6: Clean up this lambda function

20. Choose <span style="background-color:fff; font-weight:bold; font-size:.7em; color:#545b64; position:relative; top:-1px; border-color:#545b64; border-radius:2px; border-width:1px; border-style:solid; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Actions** <i class="fas fa-caret-down"></i></span>

1. Choose **Delete function**.

1. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Delete**</span>

1. Choose <span style="background-color:#ec7211; font-weight:bold; font-size:.7em; color:white; position:relative; top:-1px; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Delete**</span>

## Task 4: Deployment with a zip package

For the remaining steps, it will be assumed that you're using Cloud9 as your IDE.  You are not limited to doing this in Cloud9 but the instructions will not match if you use another editor.

### Task 4.1: Deploying a Lambda function without dependencies

A lot of the python lambda functions your create will not need any additional dependencies. As you saw in the workshop, the only dependencies that you needed to install was boto3. That package is already included in the runtime for Python when you invoke a lambda function you just have to import it like any other built-in package.

24. In the AWS Cloud9 console, create a new folder in the environment window name `my-math-function`. You can also run the following code in the terminal window.

```python
mkdir my-math-function
```

25. Create a new file in the **my-math-function** folder named `math-function.py`

1. Copy the following code to the **math-function.py** file.

```python
import argparse

parser = argparse.ArgumentParser(description="Performs an arithmetic operation on two numbers.")

parser.add_argument(
    '--operand1',
    dest="operand1",
    type=int,
    help="The first number to use in your function.",
    required=False
    )

parser.add_argument(
    '--operand2',
    dest="operand2",
    type=int,
    help="The second number to use in your function.",
    required=False
    )

parser.add_argument(
    '--operation',
    type=str,
    help="add, subtract, multiply, divide, or modulus. The operation to perform on the numbers.",
    required=False
    )

args = parser.parse_args()

def main(**kwargs):
    operand1 = kwargs["operand1"]
    operand2 = kwargs["operand2"]
    operation = kwargs["operation"]

    if operation == 'add':
        return operand1 + operand2
    elif operation == 'subtract':
        return operand1 - operand2
    elif operation == 'multiply':
        return operand1 * operand2
    elif operation == 'divide':
        return operand1 / operand2
    elif operation == 'modulus':
        return operand1 % operand2
    else:
        return 'unsupported operation'

if __name__=="__main__":
    print(main(**vars(args)))
```

27. Save the file and run it in the terminal with the following code:

```bash
cd my-math-function
python math-function.py --operand1 2 --operand2 3 --operation add
```

**Expected Output**

<span style="font-family:Courier">5</span>

To modify this code to run in lambda, you will need to change the input of the main function to accept an event and context object.  You will also need to change the 3 variables in the main function to get their values from the event instead of kwargs.

You can optionally make this code function from lambda or the command line. To do that you will also need to move the arg parsing logic to only run if the namespace is **__main__** and to pass the correct objects to the event.  The new code should match the following code block.

```python
import argparse

def main(event, context):
    operand1 = event["operand1"]
    operand2 = event["operand2"]
    operation = event["operation"]

    if operation == 'add':
        return operand1 + operand2
    elif operation == 'subtract':
        return operand1 - operand2
    elif operation == 'multiply':
        return operand1 * operand2
    elif operation == 'divide':
        return operand1 / operand2
    elif operation == 'modulus':
        return operand1 % operand2
    else:
        return 'unsupported operation'

if __name__=="__main__":
    parser = argparse.ArgumentParser(description="Performs an arithmetic operation on two numbers.")

    parser.add_argument(
        '--operand1',
        dest="operand1",
        type=int,
        help="The first number to use in your function.",
        required=False
        )

    parser.add_argument(
        '--operand2',
        dest="operand2",
        type=int,
        help="The second number to use in your function.",
        required=False
        )

    parser.add_argument(
        '--operation',
        type=str,
        help="add, subtract, multiply, divide, or modulus. The operation to perform on the numbers.",
        required=False
        )

    args = parser.parse_args()
    args = vars(args)
    event = {
        "operand1": args["operand1"],
        "operand2": args["operand2"],
        "operation": args["operation"]
    }
    print(main(event, None))
```

28. Save and run the code in your terminal to confirm it still works

```bash
python math-function.py --operand1 2 --operand2 3 --operation add
```

**Expected Output**

<span style="font-family:Courier">5</span>

<i class="fas fa-info-circle fa-lg" style="color:blue"></i> **Important:** If you only care that it works in lambda, you can remove everything from **if \_\_name\_\_=="\_\_main\_\_":** to the end of the file.

29. Add the math-function.py fil to the root of a .zip file.

```bash
zip my-math-function.zip math-function.py
```

**Expected Output**

<span style="font-family:Courier">adding: math-function.py (deflated 68%)</span>

30. Run the following command to create a variable with the IAM Role arn for your function. This role was created when you launched the Cloudformation that accompanies this workshop.

```bash
roleArn=$(aws iam list-roles --output text --query 'Roles[?contains(RoleName, `LambdaExRole`) == `true`].Arn')
```

31. Use the following command in your terminal to create the lambda function.

```bash
aws lambda create-function \
--function-name math-function  \
--handler math-function.main \
--runtime python3.9 \
--role $roleArn \
--zip-file fileb://my-math-function.zip
```

**Expected Output**

<span style="font-family:Courier">{</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"LastUpdateStatus": "Successful",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"FunctionName": "math-function",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"LastModified": "2021-11-11T15:00:58.583+0000",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"RevisionId": "edaddb04-f29e-462b-9779-c65a5ea1abdc",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"MemorySize": 128,</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"State": "Active",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Version": "$LATEST",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Role": "arn:aws:iam::334142322932:role/python-lab-LambdaExRole-1F3OGUZKI0WGF",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Timeout": 3,</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Handler": "math-function.main",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Runtime": "python3.9",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"TracingConfig": {</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Mode": "PassThrough"</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;},</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"CodeSha256": "JxW2Fmkb7121mzIzjX9DihF5i9IcLnx5oS6+hyooA0g=",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Description": "",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"CodeSize": 643,</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"FunctionArn": "arn:aws:lambda:us-west-2:334142322932:function:math-function",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"PackageType": "Zip"</span><br/>
<span style="font-family:Courier">}</span><br/>

32. Test the function with AWS Explorer

The **AWS: Explorer** is built into AWS Cloud9. This tool lets you browse and interact with AWS resources in your account.

33. In the left gutter of the Cloud9 window, choose the AWS logo.

1. Expand the region and then expand Lambda.

1. Right click (Context click) on the math-function and choose **Invoke on AWS**.

1. In the code box after **Or, use a sample request payload from a template** enter the following JSON object for the input event.

```json
{
    "operand1": 2,
    "operand2": 3,
    "operation": "add"
}
```

37. Choose <span style="ssb_grey;white-space: nowrap;">**Invoke**</span>

The results of your lambda invocation will be displayed in an **AWS Lambda** tab where the terminal window is.
**Expected Output**

<span style="font-family:Courier">Loading response...</span><br />
<span style="font-family:Courier">Invocation result for arn:aws:lambda:us-west-2:334142322932:function:math-function</span><br />
<span style="font-family:Courier">Logs:</span><br />
<span style="font-family:Courier">START RequestId: f3ddfb26-fed3-40cd-8c01-3a3d3ef67b81 Version: $LATEST</span><br />
<span style="font-family:Courier">END RequestId: f3ddfb26-fed3-40cd-8c01-3a3d3ef67b81</span><br />
<span style="font-family:Courier">REPORT RequestId: f3ddfb26-fed3-40cd-8c01-3a3d3ef67b81&nbsp;&nbsp;&nbsp;&nbsp;Duration: 1.10 ms&nbsp;&nbsp;&nbsp;&nbsp;Billed Duration: 2 ms&nbsp;&nbsp;Memory Size:
128 MB&nbsp;&nbsp;Max Memory Used: 37 MB&nbsp;&nbsp;Init Duration: 116.37 ms</span><br /><br />
<br /><span style="font-family:Courier">Payload:</span><br />
<span style="font-family:Courier">5</span><br />

### Task 4.2: Deploying a Lambda function with dependencies

Now that you know how to make your code run in AWS Lambda, how do you add dependencies? What do you put in your deployment package?  Create a new function that needs an external package.

38. In the AWS Cloud9 console, create a new folder in the environment window name `my-web-request-function`. You can also run the following code in the terminal window.

```python
cd ~/environment/
mkdir my-web-request-function
```

39. Create a new file in the **my-web-request-function** folder named `web-request-function.py`

1. Copy the following code to the **web-request-function.py** file.

```python
import requests

def main(event, context):   
    response = requests.get("https://www.google.com/")
    print(response.text)
    return response.text

if __name__ == "__main__":   
    main('', '')
```

The **requests** package isn't a built-in package that comes with python, you have to install it with pip. The cleanest way to support multiple projects on the same machine is to use a virtual environment, but that isn't required. The following steps show you how to create a lambda deployment package without and with a virtual environment. You can practice both methods but only one is required to create the package.

**Option 1:**

<details>
<summary>Without a Python virtual environment</summary>
<p>

1. Install the requests library to a new package directory.

```bash
pip install --target ./my-web-request-function requests
```

2. Zip the contents of the **my-web-request-function** into a deployment package.

```bash
cd my-web-request-function
zip  -r my-deployment-package.zip .
```

</p>
</details>
<br/>

**Option 2:**

<details>
<summary>With a Python virtual environment</summary>
<p>

1. Create a virtual environment in your function folder.

```bash
cd my-web-request-function
python -m venv function-env
source function-env/bin/activate
```

2. Install the requests library in your virtual environment.

```bash
pip install requests
```

3. Deactivate the virtual environment.

```bash
deactivate
```

4. Create a deployment package with the installed libraries at the root.

```bash
cd function-env/lib/python3.7/site-packages
zip -r ../../../../my-deployment-package.zip .
```

<i class="fas fa-info-circle fa-lg" style="color:blue"></i> **Important:** The version of python installed by default on Cloud9 is version 3.7.  The requests package for 3.7 works for 3.9 but other packages may have compatibility issues. If there is a package that isn't compatible with Python 3.9 you can choose a different runtime version for your Lambda function.

5. Add function code files to the root of your deployment package.

```bash
cd ~/environment/my-web-request-function
zip -g my-deployment-package.zip web-request-function.py
```

</p>
</details>

#### Deploy the Lambda function to AWS

48. Run the following command to create a variable with the IAM Role arn for your function. This role was created when you launched the Cloudformation that accompanies this workshop.

```bash
roleArn=$(aws iam list-roles --output text --query 'Roles[?contains(RoleName, `LambdaExRole`) == `true`].Arn')
```

49. Use the following command in your terminal to create the lambda function.

```bash
aws lambda create-function \
--function-name web-request-function  \
--handler web-request-function.main \
--runtime python3.9 \
--role $roleArn \
--zip-file fileb://my-deployment-package.zip
```

**Expected Output**

<span style="font-family:Courier">{</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"LastUpdateStatus": "Successful",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"FunctionName": "web-request-function",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"LastModified": "2021-11-11T15:00:58.583+0000",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"RevisionId": "edaddb04-f29e-462b-9779-c65a5ea1abdc",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"MemorySize": 128,</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"State": "Active",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Version": "$LATEST",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Role": "arn:aws:iam::334142322932:role/python-lab-LambdaExRole-1F3OGUZKI0WGF",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Timeout": 3,</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Handler": "web-request-function.main",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Runtime": "python3.9",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"TracingConfig": {</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Mode": "PassThrough"</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;},</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"CodeSha256": "JxW2Fmkb7121mzIzjX9DihF5i9IcLnx5oS6+hyooA0g=",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"Description": "",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"CodeSize": 643,</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"FunctionArn": "arn:aws:lambda:us-west-2:334142322932:function:math-function",</span><br/>
<span style="font-family:Courier">&nbsp;&nbsp;&nbsp;&nbsp;"PackageType": "Zip"</span><br/>
<span style="font-family:Courier">}</span><br/>

50. In the left gutter of the Cloud9 window, choose the AWS logo.

1. Expand the region and then expand Lambda.

<i class="fas fa-info-circle fa-lg" style="color:blue"></i> **Important:** If you do not see your new function, refresh the browser window.

52. Right click (Context click) on the web-request-function and choose **Invoke on AWS**.

1. This function doesn't require an event so you can just choose <span style="ssb_grey;white-space: nowrap;">**Invoke**</span>

The Expected output will show a lot of information. The entire web page returned is written to the log and returned from the function. Update the code and Lambda function to only return and log the status code to just confirm a successful response.

54. Open the **web-request-function.py** file and update your function with the following code.

```bash
import requests

def main(event, context):   
    response = requests.get("https://google.com/")
    print('Status:', response.status_code)
    return 'Status: ' + str(response.status_code)

if __name__ == "__main__":   
    main('', '')
```

55. Save the file and replace the file in your deployment package with this one.

```bash
zip -g my-deployment-package.zip web-request-function.py
```

56. On the AWS: Explorer, right click(context click) on the **web-request-function** and choose **Upload Lambda...**.

1. Choose **ZIP Archive**, expand **my-web-request-function**, select **my-deployment-package.zip**, choose <span style="ssb_green;white-space: nowrap;">Open</span>

1. Choose <span style="ssb_blue;white-space: nowrap;">**Yes**</span>

<i class="fas fa-sticky-note fa-lg" style="color:orange"></i> **Note:** In the background, **AWS: Explorer** runs the **aws lambda update-function-code** method.

Test the function again to see the results of your change.

59. Right click (Context click) on the web-request-function and choose **Invoke on AWS**.

1. This function doesn't require an event so you can just choose <span style="ssb_grey;white-space: nowrap;">**Invoke**</span>

**Expected Output**

<span style="font-family:Courier">Loading response...</span><br/>
<span style="font-family:Courier">Invocation result for arn:aws:lambda:us-west-2:334142322932:function:web-request-function</span><br/>
<span style="font-family:Courier">Logs:</span><br/>
<span style="font-family:Courier">START RequestId: 8488b152-b03c-49a5-a46a-4c56109e1fcf Version: $LATEST</span><br/>
<span style="font-family:Courier">Status: 200</span><br/>
<span style="font-family:Courier">END RequestId: 8488b152-b03c-49a5-a46a-4c56109e1fcf</span><br/>
<span style="font-family:Courier">REPORT RequestId: 8488b152-b03c-49a5-a46a-4c56109e1fcf&nbsp;&nbsp;Duration: 480.66 ms&nbsp;&nbsp;&nbsp;&nbsp;Billed Duration: 481 ms&nbsp;&nbsp;Memory Size: 128 MB&nbsp;&nbsp;&nbsp;&nbsp;Max Memory Used: 50 MB&nbsp;&nbsp;Init Duration: 332.08 ms</span><br/><br/><br/>
<span style="font-family:Courier">Payload:</span><br/>
<span style="font-family:Courier">"Status: 200"</span><br/>

## Task 5: Deployment with AWS SAM

Now that you know the basics of how to run your code with AWS Lambda you can learn an easier way to deploy your application with AWS SAM. AWS SAM handles packaging and deployment of your serverless applications with a simplified AWS CloudFormation template syntax.

SAM also creates backend resources to invoke your functions in response to common events.

### Task 5.1 Create the SAM code package

Create a new folder in your Cloud9 environment to hold your SAM application and structure.

61. Create a new folder named `sam-app`, using the following code.

```bash
cd ~/environment
mkdir sam-app
```

62. Create a new folder in the **sam-app** folder to hold your python code.

```bash
cd ~/environment/sam-app
mkdir web-request
```

There are two files required for each function you create, the python script file and a requirements.txt file. The requirements file is required even if it's empty.

63. Create the requirements.txt and app.py file.

```bash
cd web-request
echo "requests" > requirements.txt
touch app.py
```

64. Copy the following code into the **app.py** file and save it.

```python
import requests

def main(event, context):   
    response = requests.get("https://google.com")
    print('Status:', response.status_code)
    return 'Status: ' + str(response.status_code)

if __name__ == "__main__":   
    main('', '')
```

65. Modify the app.py file to return a standard response with the following code.

```python
import requests
import json

def lambdaResponse(status_code, body):
    response = {}
    response['statusCode'] = status_code
    response['body'] = json.dumps(body)
    return response

def main(event, context):   
    response = requests.get("https://google.com")
    print('Status:', response.status_code)
    return lambdaResponse(response.status_code, {'Status': response.reason})

if __name__ == "__main__":   
    print(main('', ''))
```

In this modification, you created a formal response object to be returned.  This object is in the format required by the API gateway with Lambda proxy integration which will be used first. There was a slight modification to the function if run under *__main__* as well so that you can see the response that you are returning.

66. Test the code locally on your Cloud9 instance.

```python
pip install -r requirements.txt
python app.py
```

**Expected Output**
<span style="font-family:Courier">Status: 200</span><br />
<span style="font-family:Courier">{'statusCode': 200, 'body': '{"Status": "OK"}'}</span><br />

### Task 5.2 Create the SAM template

AWS SAM templates are an extension of AWS CloudFormation templates, with additional components that make them easier to work with. You can see the full AWS SAM specification documentation [here](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-specification.html)

67. In the **sam-app** folder, create a `template.yml` file.

```bash
cd ~/environment/sam-app
touch template.yml
```

68. Copy the following code into the **template.yml** file.

```yaml
Transform: AWS::Serverless-2016-10-31

Resources:
  webRequestFunction:
    Type: AWS::Serverless::Function
    Properties:
      Runtime: python3.9
      CodeUri: web-request/
      Handler: app.main
```

This simple template specifies the Serverless transform version that it is using. After than there is one resource created that describes the lambda function and where the code it located, relative to the template.

Instead of upgrading the version of python that is installed on Cloud9 to build this SAM application, you will use a container for the proper runtime.  First, you have to modify your AWS Cloud9 instance.

AWS Cloud9 instances are deployed with the least amount of resources possible to be cost efficient. It is common to require additional space for large development projects, especially if container images are used. To avoid compatibility issues, you will use a container image to build your AWS SAM application for deployment. This requires more disk space to download that container image. More information on what happens during this step is linked [here](https://docs.aws.amazon.com/cloud9/latest/user-guide/move-environment.html#move-environment-resize).

69. Run the following code to download the resize script and change your Cloud9 disk to 15GB.

```bash
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/pythonForAWSSupplimental/resize.sh
sh resize.sh 15
```

Now that there is enough disk space to download the container image, you can build you SAM application.

70. Run the following code to build your SAM application.

```bash
sam build --use-container
```

This command inspects your template to make sure it is valid, downloads any dependencies needed for your code, and bundles it all for deployment.

### Task 5.3 Test locally

With SAM you can test your application before deploying it to AWS.

71. Use the following command to invoke your function locally.

```bash
sam local invoke webRequestFunction
```

This command will use a container for the proper runtime of your function and then invoke it just as the lambda service would. If you needed to pass an event object, you can do that. Your test function does not need one so it was not included.  For information on that syntax enter `sam local invoke -h`

### Task 5.4 Deploy with AWS SAM

Now that you have confirmed the functionality of your lambda code locally, you can deploy it to AWS.

72. Use the following command to deploy your SAM application.

```bash
sam deploy --stack-name sam-app --capabilities CAPABILITY_IAM --resolve-s3
```

This command will take the artifacts that it bundled with the **sam build** command and use AWS Cloudformation to deploy them.  The **stack-name** argument will be the name of your SAM application and CloudFormation stack. The **capabilities** argument acknowledges that this deployment will create IAM resources.  Finally, the **resolve-s3** flag will instruct it to create an S3 bucket to save your artifacts in. Optionally, you could have specified and existing bucket.

### Task 5.5 Test the function with AWS Explorer

73. Refresh the web browser window to force AWS Explorer to update.

1. In the left gutter of the Cloud9 window, choose the AWS logo.

1. Expand the region and then expand Lambda.

1. Right click (Context click) on the function name that starts with **sam-app-webRequestFunction** and choose **Invoke on AWS**.

1. This function doesn't require an event so you can just choose <span style="ssb_grey;white-space: nowrap;">**Invoke**</span>

You will see the familiar output in the **AWS Lambda** window.

## Task 6: Add Event Sources to invoke your lambda function

Now that you can run your Python code on AWS just like you do in your development environment. How does that help you?

Your Lambda function is invoked in response to an event. So far, you have used the manual invoke command through AWS Explorer and the Console. This task will use AWS SAM to show other ways that you can invoke your Lambda functions.

### Task 6.1: Access your function directly from the internet

The first method is to use an Amazon API gateway. With Serverless applications, the client's web browser handles the static responsive UI elements and the API is used to access dynamic server side functionality and data. This could be translation services, database access, or text to speech services for just a examples.

To make your lambda function accessible with Amazon API Gateway, you just need to describe the event that it should respond to. AWS SAM takes care of everything else.

78. Add an AWS API Gateway Event handler to your function in the template.yml file.

```yaml
Transform: AWS::Serverless-2016-10-31

Resources:
  webRequestFunction:
    Type: AWS::Serverless::Function
    Properties:
      Runtime: python3.9
      CodeUri: web-request/
      Handler: app.main
      Events:
        ApiEvent:
            Type: Api
            Properties:
              Path: /webRequestFunction
              Method: get
```

79. Save the file and use the following command to build and start the api locally for testing.

```bash
cd ~/environment/sam-app/
sam build --use-container
sam local start-api -p 8080
```

80. On the Cloud9 menu bar, choose **Preview** and choose **Preview Running Application**

At first, you will see <span style="font-family:Courier">{"message":"Missing Authentication Token"}</span>. This is because the only event configured for the API responds to the **/webRequestFunction** path.

81. Add `webRequestFunction` to the end of the url and press the enter key.

**Expected Output**

<span style="font-family:Courier">{"Status": "OK"}</span>

82. Close the browser preview window and press ctrl+c to stop the local api.

1. Deploy the web application again.

```bash
sam deploy --stack-name sam-app --capabilities CAPABILITY_IAM --resolve-s3
```

Since you will use the same --stack-name, this will apply any changes to the existing application your deployed.

Use the AWS Explorer to view and test your API.

84. Refresh the Cloud9 web browser window to force AWS Explorer to update.

1. In the left gutter of the Cloud9 window, choose the AWS logo.

1. Expand the region and then expand API Gateway.

1. Right click (Context click) on the gateway name **sam-app** and choose **Invoke on AWS**.

1. For **Select a resource**, select **/webRequestFunction**.

1. Choose <span style="background-color:fff; font-weight:bold; font-size:.7em; color:#545b64; position:relative; top:-1px; border-color:#545b64; border-radius:2px; border-width:1px; border-style:solid; padding-top:5px; padding-bottom:5px; padding-left:20px; padding-right:20px;white-space: nowrap;">**Invoke**</span>

You will see the log output from the gateway and an OK status returned.

Now, test the function with your web browser.

90. Right click (Context click) on the gateway name **sam-app** and choose **Copy URL**.

1. Choose the **Prod** URL to copy it to your clipboard.

1. Open a new web browser tab and paste the url from your clipboard, append `webRequestFunction` to the end, press the Enter key.

AWS Explorer -> API Gateway -> Copy URL -> Prod -> Paste in new web browser tab -> append  `/webRequestFunction` and go.

The actual result will vary based on your web browser but you should see and OK status returned.

### Task 6.2: Invoke your function on a schedule

For a lot of use cases, invoking with an API makes sense. The particular function you developed may make more sense to invoke on a schedule instead. A very common use case is to monitor the health of a website.

For this to make sense you should record the metrics. Modify your code to accept a url in the event and put key data from the response in an custom Cloudwatch metric.

93. Update the code in your app.py to match the following code.

```python
import requests
import json
import boto3

cw = boto3.client('cloudwatch')

def main(event, context):
    print(event)
    url = event["url"]
    response = requests.get(url)
    rStatusCode = response.status_code
    rLatency = response.elapsed.total_seconds()
    cw.put_metric_data(
        Namespace= 'CustomURLPoller',
        MetricData=[
            {
                'MetricName': 'latency',
                'Dimensions': [
                    {
                        'Name': 'website',
                        'Value': url
                    }
                ],
                'Value': rLatency,
                'Unit': 'Seconds'
            },
            {
                'MetricName': '_'.join([str(rStatusCode),'status','code']),
                'Dimensions': [
                    {
                        'Name': 'website',
                        'Value': url
                    }
                ],
                'Value': 1,
                'Unit': 'Count'
            }
            ]
        )
    return str(rStatusCode) + ' ' + str(rLatency)

if __name__ == "__main__":
    event = {'url': 'https://www.google.com'}
    print(main(event, ''))
```

You imported boto3 in your code so that you can use it to put metrics in Amazon CloudWatch. you created a boto3 client for cloudwatch.  You pull the url to be polled from the event and extract the status code and seconds elapsed from the response.  With those values you put data into two cloudwatch custom metrics. You also modified the code when running under \_\_main\_\_ so that you can perform a test without using lambda.

You removed the response object from the code that is required by API gateway.

Test your function in the Cloud9 console. Even though it isn't required to add boto3 to your dependencies file for Lambda, you have to install it to your Cloud9 environment.

94. Run the following code to install boto3 and test your app changes.

```bash
pip install boto3
cd ~/environment/sam-app/web-request
python app.py
```

**Expected Output**

<span style="font-family:Courier">200 0.105468</span>

The expected output should show the status code and the time it took to receive a response.

In the AWS console browser tab

95. In the AWS console browser tab, on the navigation bar, in the unified search bar, search for and choose `CloudWatch`

1. In the left navigation, choose **Metrics**, **All metrics**.

1. Choose **CustomURLPoller** and then **website**.

You should see the custom metrics here that you python function added.  You can add these to a Cloudwatch dashboard to monitor the health of a website.  To make that useful, you need to test the website at regular intervals.

98. Update the template.yml file so that the lambda function is invoked every minute instead of with an API.

```yaml
Transform: AWS::Serverless-2016-10-31

Resources:
  webRequestFunction:
    Type: AWS::Serverless::Function
    Properties:
      Runtime: python3.9
      CodeUri: web-request/
      Handler: app.main
      Policies:
        - CloudWatchPutMetricPolicy: {}
      Events:
        urlEvent:
            Type: Schedule
            Properties:
              Input: '{"url": "https://www.google.com"}'
              Schedule: 'rate(1 minute)'
```

You changed the event to run on a schedule of every 1 minute. You also pass the url to the event that you want to monitor. Finally, you used an AWS SAM policy template to give you function permissions to write the Cloudwatch metrics.

99. Save the template.yml file and use the following code to build and deploy it.

```bash
cd ~/environment/sam-app
sam build --use-container
sam deploy --stack-name sam-app --capabilities CAPABILITY_IAM --resolve-s3
```

If you go to Cloudwatch, you can see that the website is polled every 1 minute. with the Schedule property, you can use rate expressions as you did here or cron expressions for more granularity. You can also add more than one event if you need to. For this application you may want to have multiple events to poll different websites.

### Task 6.3: Cleanup your sam application

To remove the sam application you have two options, you can either delete the CloudFormation stack that was created or you can use the sam cli to delete it.

1. Run the following command to delete the SAM application.

```bash
sam delete --stack-name sam-app
```

1. Press **y** for yes twice.

## Challenge Task 7: Create an application the will process files uploaded to S3

Using what you have learned in the workshop, can you create a AWS Serverless application that processes files uploaded to S3 as JSON documents and puts them in DynamoDB?

The application should meet the following requirements:

- All resources are created with one SAM template
- Resources include, one S3 bucket, one DynamoDB table, one Lambda function and all events and permissions required.
- The Lambda function should only be triggered when an object that ends with ".json" is created or updated.

In addition to this lab, the following links may be useful.

- [Boto3 DynamoDB documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/dynamodb.html)
- [Boto3 S3 documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html)
- [AWS SAM Serverless Function reference](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-function.html)
- [AWS SAM Simple Table reference](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-simpletable.html)
- [AWS CloudFormation S3 Bucket reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket.html)

Tips

- The S3 bucket will be created with a regular CloudFormation Resource, not a serverless resource.
- You can add multiple AWS SAM policy templates to the same function.
- You can choose your own JSON structure, but remember that you will have to specify the partition key when you create the table and every record will have to include that attribute.
- The S3 event will only give you the bucket name and object key, you will then have to read the object to process the data.
