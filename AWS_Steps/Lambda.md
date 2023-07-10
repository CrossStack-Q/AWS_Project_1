# Math function

## import the JSON utility package
import json
## import the Python math library
import math

## import the AWS SDK (for Python the package name is boto3)
import boto3
## import two packages to help us with dates and date formatting
from time import gmtime, strftime

## create a DynamoDB object using the AWS SDK
dynamodb = boto3.resource('dynamodb')
## use the DynamoDB object to select our table
table = dynamodb.Table('PowerOfMathDatabase')
## store the current time in a human readable format in a variable
now = strftime("%a, %d %b %Y %H:%M:%S +0000", gmtime())

## define the handler function that the Lambda service will use an entry point
def lambda_handler(event, context):

## extract the two numbers from the Lambda service's event object
    mathResult = math.pow(int(event['base']), int(event['exponent']))

## write result and time to the DynamoDB table using the object we instantiated and save response in a variable
    response = table.put_item(
        Item={
            'ID': str(mathResult),
            'LatestGreetingTime':now
            })

## return a properly formatted JSON object
    return {
    'statusCode': 200,
    'body': json.dumps('Your result is ' + str(mathResult))
    }




# Test

{
  "base": 3,
  "exponent": 4
}


# in the config 
add permission
inline policy --> powerofmath




## policy code is


{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "VisualEditor0",
			"Effect": "Allow",
			"Action": [
				"dynamodb:PutItem",
				"dynamodb:DeleteItem",
				"dynamodb:GetItem",
				"dynamodb:Scan",
				"dynamodb:Query",
				"dynamodb:UpdateItem"
			],
			"Resource": "arn:aws:dynamodb:us-east-2:824373245011:table/PowerOfMathDatabase"
		}
	]
}


## arn from the DynamoDB