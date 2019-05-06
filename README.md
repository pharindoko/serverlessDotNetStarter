# serverlessDotNetStarter
This template is meant as a starter template for serverless framework with following scope:
- deploy C# / NET Core 2.1 solution in __AWS cloud__ using:
  - Lambda
  - Api Gateway
- debug and test solution locally in __Visual Studio Code__
- works operating system independent

## Prerequisites to install
- [NodeJS](https://nodejs.org/en/)
- [Serverless Framework CLI](https://serverless.com)
- [.NET Core](https://dotnet.microsoft.com/)
- [Visual Studio Code](https://code.visualstudio.com/)


Verify that everything is installed
```bash
# package manager for nodejs
npm -v
# serverless framework cli
sls -v
# dotnet cli
dotnet --version
```

## Installation
```bash
# clone solution
git clone https://github.com/pharindoko/serverlessDotNetSample.git
cd serverlessDotNetStarter
# install dotnet references described in csproj file
dotnet restore AwsDotnetCsharp.csproj
# install Lambda NET Mock Test Tool
# more details: https://github.com/aws/aws-lambda-dotnet/tree/master/Tools/LambdaTestTool
dotnet tool install -g Amazon.Lambda.TestTool-2.1
```

## Debug & Test locally
I followed this guideline: (Please read in case of issues)

[How to Debug .NET Core Lambda Functions Locally with the Serverless Framework](https://itnext.io/how-to-debug-net-core-lambda-functions-locally-with-the-serverless-framework-dd1670bc22e2)

Press __F5__ to start Debugging and local testing of lambda function

- Hint: Lambda Mock Test Tool should be started
- Insert this json value in input textbox for a first test:
```
{
  "httpMethod": "GET",
  "queryStringParameters": {
    "foo": "dfgdfg",
    "woot": "food"
  }
 }

```

__Mind:__ For a successful response querystringParameter __foo__ must be inserted

__Mind:__ [Verify that the right version has been set in /.vscode/launch.json:](https://github.com/pharindoko/serverlessDotNetStarter/blob/a9cacc1a598c65810a4a1458d3ce13391335fb79/.vscode/launch.json#L12)

  #### how to get the version ?
  <pre><code>
   dotnet lambda-test-tool-2.1

   Result:
   AWS .NET Mock Lambda Test Tool (<b>0.9.2</b>)
  </pre></code>

  #### Edit property in .vscode/launch.json file (placeholders marked version bold)
  <pre><code>
  "program": "/Users/uid10804/.dotnet/tools/.store/amazon.lambda.testtool-2.1/<b>{Set same toolversion}</b>/amazon.lambda.testtool-2.1/<b>{Set same toolVersion}</b>/tools/netcoreapp2.1/any/Amazon.Lambda.TestTool.dll",
  </pre></code>



## Build Package 
Mac OS or Linux
```
./build.sh
```

Windows 
```
build.cmd
```

## Deploy via Serverless Framework
```
serverless deploy
```
A cloudformation stack in AWS will be created in background containing all needed resources

#### After successful deployment you can see following output
<pre>
Service Information
service: myService
stage: dev
region: <b>us-east-1</b>
stack: myService-dev
resources: 10
api keys:
  None
endpoints:
  GET - <b>endpointUrl --> https://{api}.execute-api.us-east-1.amazonaws.com/dev/hello</b>
functions:
  hello: myService-dev-hello
layers:
  None

</pre>

## Test endpoint after deployment
2 simple options:
- [Use postman as UI Tool](https://www.getpostman.com/)
- Use curl
```
curl https://{api}.execute-api.us-east-1.amazonaws.com/dev/hello?foo=test
```
__Mind:__ For a successful response querystringParameter __foo__ must be inserted

## FAQ

######  How to add an api key

1. Setup API Key in serverless.yml file
https://serverless.com/framework/docs/providers/aws/events/apigateway/#setting-api-keys-for-your-rest-api

###### How to add additional lambda functions

1. Create a new C# Function in Handler.cs or use another file
2. Add a new function to serverless.yml and reference the C# Function as handler
https://serverless.com/framework/docs/providers/aws/guide/functions/

###### Destroy the stack in the cloud
```
sls remove
```
###### I deployed the solution but I get back a http 500 error

1. For a successful response querystringParameter __foo__ must be inserted
2. Check Cloudwatch Logs in AWS - the issue should be describe there.

###### How can I change the lambda region or stack name

Please have a look to the serverless guideline: https://serverless.com/framework/docs/providers/aws/guide/deploying/
