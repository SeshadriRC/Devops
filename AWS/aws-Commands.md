**EC2**
***Describe***
```
aws ec2 describe-instances
aws ec2 stop-instances --instance-ids i-01c39afa27fa64ca9
aws ec2 describe-instances --instance-ids i-01c39afa27fa64ca9 --query "Reservations[*].Instances[*].State.Name" --output text
aws ec2 terminate-instances --instance-ids i-01c39afa27fa64ca9
```

## IAM

**get current user**
```
aws sts get-caller-identity
```

**list iam users**
```
aws iam list-users | grep sesha-read
```
**Rename user**
```
aws iam update-user \
    --user-name cli-user \
    --new-user-name cli_user
```

**List attached policies**
```
aws iam list-attached-user-policies --user-name sesha-read
```
**Remove Policy from a user**
```
aws iam detach-user-policy \
    --user-name sesha-read \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

aws iam detach-user-policy \
    --user-name sesha-read \
    --policy-arn arn:aws:iam::466567470934:policy/AmazonS3ReadOnlyAccess

```
**VPC**  
***Describe***
```
aws ec2 describe-vpcs
```

aws ec2 create-snapshot --volume-id <VOLUME_ID> --description "DevOps practice snapshot"
aws ec2 release-address --allocation-id <ALLOCATION_ID>

