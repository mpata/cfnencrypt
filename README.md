# cfnencrypt

AWS CloudFormation Custom Resource to encrypt strings using AWS KMS.

## Introduction
At the moment of this writing, there's no way to create a AWS::SSM::Parameter with a "SecureString" type ([doc](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssm-parameter.html#aws-resource-ssm-parameter-properties)). This is just a workaround that limitation, use it at your own risk.

## Custom Resource
### Properties
* KeyId: KMS Key Id
* String: String to be encrypted using KMS

### Return Values
* EncryptedString: Base64 encoded CiphertextBlob

### Usage
### Custom Resource
* You'll have to declare a custom resource per encrypted string
```yaml
  EncryptedServicePassword:
    Type: Custom::StringEncryption
    Properties:
      ServiceToken: !GetAtt LambdaEncryptionHelper.Arn
      KeyId: !Ref KMSHelperKey
      String: !Ref ExampleParameter
  ServicePasswordEncryptedSSMParameter:
    Type: 'AWS::SSM::Parameter'
    Properties:
      Description: Service Password Encrypted SSM Parameter
      Type: String
      Value: !GetAtt EncryptedServicePassword.EncryptedString
```

### Installation
* Include the following resources from the supplied template(custom-resource-encrypt.yaml) in your CloudFormation template
  * LambdaEncryptionHelper
  * LambdaEncryptionHelperRole
  * KMSHelperKey

### Usage
* Declare a custom resource with the following properties:
  * ServiceToken: !GetAtt LambdaEncryptionHelper.Arn
  * KeyId: !Ref KMSHelperKey
  * String: \<whatever you want>
