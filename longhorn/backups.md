# Configure backup longhorn to the minio

On the minio enviroment create user and policy to access bucket to backups
Sample:
user: backup-user
policy: backups

Policy:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "GrantLonghornBackupstoreAccess0",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:ListBucket",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::<your-bucket-name>",
        "arn:aws:s3:::<your-bucket-name>/*"
      ]
    }
  ]
}
```

Create secret on namespace longhorn-system, this give longhorn access to services minio.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: minio-secret
  namespace: longhorn-system
type: Opaque
data:
  AWS_ACCESS_KEY_ID: [user]
  AWS_SECRET_ACCESS_KEY: [password]]
  AWS_ENDPOINTS: [endpoint service minio]  
```
the [manifest](minio-backup.yaml) to deploy


- On the configuration longhorn set endpoint minio in the section backup target:  s3://backups@dummyregion/longhorn

- And Backup Target Credential Secret set name of secret created: minio-secret




