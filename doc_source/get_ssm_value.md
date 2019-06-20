--------

This documentation is for the developer preview release \(public beta\) of the AWS Cloud Development Kit \(AWS CDK\)\. Releases might lack important features and might have future breaking changes\.

--------

# Get a Value from a Systems Manager Parameter Store Variable<a name="get_ssm_value"></a>

You can get the value of an AWS Systems Manager Parameter Store variable, depending on whether you want the latest version of a plain string, a particular version of a plain string, or a particular version of a secret string\. It isn't possible to retrieve the latest version of a secure string\. To read the latest version of a secret, you have to read the secret from AWS Secrets Manager \(see [Get a Value from AWS Secrets Manager](get_secrets_manager_value.md)\)\.

To read a particular version of a Systems Manager Parameter Store plain string value at AWS CloudFormation deployment time, use [ParameterStoreString](https://docs.aws.amazon.com/cdk/api/latest/docs/@aws-cdk_aws-ssm.ParameterStoreString.html)\. If you don't supply a `version` value, you get the latest version\.

```
import ssm = require('@aws-cdk/aws-ssm');

const parameterString = new ssm.StringParameter.fromStringParameterAttributes(this, 'MyParameter', {
    parameterName: 'my-parameter-name',
    version: 1,
});

const myvalue = parameterString.stringValue;
```

To read a particular version of a Systems Manager Parameter Store `SecureString` value at AWS CloudFormation deployment time, use [ParameterStoreSecureString](https://docs.aws.amazon.com/cdk/api/latest/docs/@aws-cdk_aws-ssm.ParameterStoreSecureString.html)\. You must supply a `version` value\.

```
import ssm = require('@aws-cdk/aws-ssm');

const secureString = new ssm.StringParameter.fromSecureStringParameterAttributes(this, 'MySecretParameter', {
    parameterName: 'my-secret-parameter-name',
    version: 1,
});

const myvalue = secureString.stringValue;
```

Use the [put\-parameter](https://docs.aws.amazon.com/cli/latest/reference/ssm/put-parameter.html) CLI command to add a string parameter to the system, such as when testing:

```
aws ssm put-parameter --name "my-parameter-name" --type "String" --value "my-parameter-value"
```

The command returns an ARN you can use for the example\.