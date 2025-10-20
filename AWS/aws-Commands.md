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


## RDS

**create db**
```
aws rds create-db-instance \
    --db-instance-identifier mariadb-testing \
    --db-instance-class db.t3.micro \
    --engine mariadb \
    --master-username admin \
    --master-user-password <your-password> \
    --allocated-storage 20 \
    --storage-type gp2 \
    --publicly-accessible \
    --no-multi-az \
    --backup-retention-period 0 \
    --storage-encrypted \
    --region ap-south-1
```

**delete DB**
```
aws rds delete-db-instance \
    --db-instance-identifier mariadb-testing \
    --skip-final-snapshot \
    --delete-automated-backups \
    --region ap-south-1
```
**delete parameter group**
```
aws rds delete-db-parameter-group --db-parameter-group-name <parameter-group-name> --region <region>
```

**Mariadb**
```
sudo apt-get install mariadb-client

mysql -h mariadb-testing.c5k88omakd8d.ap-south-1.rds.amazonaws.com -P 3306 -u admin -p Sesha123#

telnet mariadb-testing.c5k88omakd8d.ap-south-1.rds.amazonaws.com 3306

mysql -h YOUR_ENDPOINT -P 3306 -u YOUR_MASTER_USERNAME -p

SHOW DATABASES;
Use mysql;
show tables;
show grants for current_user();
```
