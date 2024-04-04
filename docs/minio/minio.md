# Configure backup longhorn to the minio

On the minio enviroment create user and policy to access bucket to backups
Sample: 
- bucket: __backups__  
- user: __backup-user__  
- policy: __backup-policy__  

Policy:
```json
{
	"Version": "2012-10-17",
	"Statement": 
	[
		{
			"Action": 
			[
				"s3:PutBucketPolicy",
				"s3:GetBucketPolicy",
				"s3:DeleteBucketPolicy",
				"s3:ListAllMyBuckets",
				"s3:ListBucket"
			],
			"Effect": "Allow",
			"Resource": 
			[
				"arn:aws:s3:::backups"
			],
			"Sid": ""
		},
		{
			"Action": 
			[
				"s3:AbortMultipartUpload",
				"s3:DeleteObject",
				"s3:GetObject",
				"s3:ListMultipartUploadParts",
				"s3:PutObject"
			],
			"Effect": "Allow",
			"Resource": 
			[
				"arn:aws:s3:::backups/*"
			],
			"Sid": ""
		}
	]
}
```

Create secret on namespace longhorn-system, this give longhorn access to services minio.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: minio-access
  namespace: longhorn-system
type: Opaque
data:
  AWS_ACCESS_KEY_ID: [user]
  AWS_SECRET_ACCESS_KEY: [password]
  AWS_ENDPOINTS: [endpoint service minio]  
```
the [manifest](minio-backup.yaml) to deploy


- On the configuration longhorn set endpoint minio in the section backup target:  s3://backups@dummyregion/longhorn

- And Backup Target Credential Secret set name of secret created: minio-secret

- set region on configuration minio: us-west-1


