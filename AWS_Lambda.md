# AWS Lambda

The Source Code is available: https://github.com/5thComrade/learn_lambda

### Serverless Framework

Serverless Framework aims to ease the pain of creating, deploying, managing and debugging Lambda functions.
It integrates well with CI/CD tools.
It has CloudFormation support so your entire stack can be deployed using this framework.

### Install the serverless framework

Open CloudShell on AWS Console. Run the following command in CloudShell

```sh
sudo npm install -g serverless
```

If you want to check if serverless framework was installed correctly, run the following command in the terminal

```sh
serverless
```

The best part of using AWS CloudShell is you don't need to configure AWS Credentials, the framework will pick the credentials from the user that is logged in.

### Deploying first function with Serverless framework

When you run the "serverless" command, you'll notice a list of most common templates to choose from. However, if you want to view all the possible templates run the
following command

```sh
serverless create --help
```

If you've choosen what template you want to use run the following command to create the serverless project.

```sh
sls create --template <name_of_the_template> --path <name_of_the_folder>
```

If you don't want to deploy the entire stack again but instead want to just deploy the function run the below command

```sh
sls deploy function -f <function_name>
```

#### How to invole functions locally

Run the following command to invoke functions locally

```sh
sls invoke -f <function_name>
```

Invoke the function and see its logs

```sh
sls invoke -f <function_name> --log
```

How to remove a function completely

```sh
sls remove
```

---
