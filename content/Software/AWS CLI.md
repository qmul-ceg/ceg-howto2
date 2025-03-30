# AWS CLI
## Install
- Install AWS CLI v2- available in QMUL Software Center CPC Profile as 'AWS Command Line Interface v2 (x64)'
	- note this is v2 - online documentation and advice may refer to v1
## Configuration
- Requires AWS IAM long-term credentials
	- follow Step 2: https://docs.aws.amazon.com/cli/latest/userguide/cli-authentication-user.html
	- AWS Access Key information (from https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html):
		- Access keys are long-term credentials for an IAM user or the AWS account root user. You can use access keys to sign programmatic requests to the AWS CLI or AWS API (directly or using the AWS SDK). For more information, see [Signing AWS API requests](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-signing.html).
		- Access keys consist of two parts: an access key ID (for example, `AKIAIOSFODNN7EXAMPLE`) and a secret access key (for example, `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`). You must use both the access key ID and secret access key together to authenticate your requests.
		- When you create an access key pair, save the access key ID and secret access key in a secure location. The secret access key is available only at the time you create it. If you lose your secret access key, you must delete the access key and create a new one. For more details, see [Resetting lost or forgotten passwords or access keys for AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys_retrieve.html).
		- You can have a maximum of two access keys per user.
- At Command Prompt `aws configure` ~> enter info at requests:
```
AWS Access Key ID [None]: {AWS access key}
AWS Secret Access Key [None]: {AWS secret key}
Default region name [None]: eu-west-2
Default output format [None]: table
```
- `output format default` is 'json' but 'table' or 'yaml' is more human readable.
	-  https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-output-format.html
- creates `credentials` and `config` in `%USERPROFILE%\.aws\`.
	- Alternatively these files can be copied between user devices.
## Example Commands
- `aws s3 ls` - list all available S3 buckets
- `aws s3 cp C:\UPLOAD\test.csv s3://qmulceg-data` - copy *test.csv* to *S3 qmulceg-data*
- Reference Guide: https://awscli.amazonaws.com/v2/documentation/api/latest/index.html