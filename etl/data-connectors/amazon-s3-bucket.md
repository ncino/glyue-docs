# Amazon S3 Bucket

## Supported Tasks

* Extract

## Fields

<table><thead><tr><th width="134.74609375">Field</th><th width="257.953125">Description</th><th>Example</th></tr></thead><tbody><tr><td>AWS Region</td><td>The AWS region where the S3 bucket is hosted in.</td><td><code>US East (N. Virginia)</code></td></tr><tr><td>AWS Bucket Name</td><td>The name of the S3 bucket.</td><td><code>my-files</code></td></tr><tr><td>AWS IAM Access Key</td><td>The access key of an AWS user who can read and/or write to the S3 bucket.<br><br>For information on how to obtain and access key and secret, see <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/access-keys-admin-managed.html#admin-create-access-key">the AWS documentation</a>.</td><td><code>LiQROVpcI3idxr0d</code></td></tr><tr><td>AWS IAM Secret Key</td><td>The secret key associated with the access key.</td><td><code>XmfXej6VntB8M4hqnfLuFUePJm6xpXzc</code></td></tr><tr><td>Assume Role</td><td>A toggle whether to assume a role on another AWS account.</td><td><i class="fa-toggle-off">:toggle-off:</i></td></tr><tr><td>IAM Role ARN</td><td>The Amazon Resource Name (ARN) for the role the user should assume.<br><br>Only required when "Assume Role" is toggled on.</td><td><code>arn:aws:iam::123456789012:role/S3Access</code></td></tr><tr><td>External ID</td><td>The unique ID assocaited with the role.<br><br>For more information, see <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_third-party.html#id_roles_third-party_external-id">the AWS documentation</a>.<br><br>Only required when "Assume Role" is toggled on.</td><td><code>f81d4fae-7dec-11d0-a765-00a0c91e6bf6</code></td></tr><tr><td>Audit Session Name</td><td>An identifier that'll appear in AWS CloudTrail logs for auditing purposes.<br><br>Only required when "Assume Role" is toggled on.</td><td><code>example-s3-session</code></td></tr></tbody></table>

