Daily learning

# Infrastructure_Code_Serverless_Framework_AWS

Project developed at Bootcamp GFT Java & AWS Developer with expert guidance [Cassiano Peres](https://github.com/cassianobrexbit/ "Cassiano Peres").</br>
Learning to create an AWS cloud infrastructure with API Gateway, DynamoDB, AWS Lambda and AWS CloudFormation using the Serverless framework for development based on Infrastructure as a Code.

[LICENSE](/LICENSE)

See [original repository](https://github.com/cassianobrexbit/dio-live-serverless-2907)

## Phases

Prerequisites:
-> have an AWS account and install Node.js on the machine.
-> Install the AWS CLI: [https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html]

### Initial Setup

#### AWS Credentials

- Create user: AWS Management Console -> IAM Dashboard -> Create New User -> <-user name->
-> Permissions "Administrator Access" -> Programmatic Access -> Dowload Keys
- In the terminal: ```$ aws configure``` -> paste the previously generated credentials

#### Configure the Serverless framework

```$ npm i -g serverless```

### Project development

``` $ serverless
Login/Register: No
Update: No
Type: Node.js REST API
Name: dio-live
```

```$ cd dio-live $ code.```

- In the ```serverless.yml``` file add the region ```region: us-east-1``` within the scope of ```provider:```
- Save and deploy ```$ serverless deploy -v```

#### Structure the code

- Create the "src" directory and move the "handler.js" file into it
- Rename the file "handler.js" to "hello.js"
- Update the code

``` const hello = async (event) => {
/////
module.exports = {
    handler:hello
}
```

- Update the file "serverless.yml "

```handler: src/hello.handler```

```$ serverless deploy -v```

#### DynamoDB

Update the file serverless.yml

```resources:
  Resources:
    ItemTable:
      Type: AWS::DynamoDB::Table
      Properties:
          TableName: ItemTable
          BillingMode: PAY_PER_REQUEST
          AttributeDefinitions:
            - AttributeName: id
              AttributeType: S
          KeySchema:
            - AttributeName: id
              KeyType: HASH
```

#### Develop lambda functions

  -> Repository /src folder

  -> Get arn from table in DynamoDB AWS Console -> DynamoDB -> Overview -> Amazon Resource Name (ARN)
  -> Update serverless.yml file with the following code, below the ```region:```
  
```iam:
    role:
        statements:
          - Effect: Allow
            Action:
              - dynamodb:PutItem
              - dynamodb:UpdateItem
              - dynamodb:GetItem
              - dynamodb:Scan
            Resource:
              - arn:aws:dynamodb:us-east-1:167880115321:table/ItemTable
  ```
  
  -> Install dependencies

   ```npm init```
   ```npm i uuid aws-sdk```
  
  -> Update list of functions in file serverless.yml
  
```functions:
  hello:
    handler: src/hello.handler
    events:
      - http:
          path: /
          method: get
  insertItem:
    handler: src/insertItem.handler
    events:
      - http:
          path: /item
          method: post
  fetchItems:
    handler: src/fetchItems.handler
    events:
      - http:
          path: /items
          method: get
  fetchItem:
    handler: src/fetchItem.handler
    events:
      - http:
          path: /items/{id}
          method: get
  updateItem:
    handler: src/updateItem.handler
    events:
      - http:
          path: /items/{id}
          method: put
  ```
