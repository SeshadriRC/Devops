**EC2**
***Describe***
```
aws ec2 describe-instances
aws ec2 stop-instances --instance-ids i-01c39afa27fa64ca9
aws ec2 describe-instances --instance-ids i-01c39afa27fa64ca9 --query "Reservations[*].Instances[*].State.Name" --output text
aws ec2 terminate-instances --instance-ids i-01c39afa27fa64ca9
```




**VPC**  
***Describe***
```
aws ec2 describe-vpcs
```

aws ec2 create-snapshot --volume-id <VOLUME_ID> --description "DevOps practice snapshot"
aws ec2 release-address --allocation-id <ALLOCATION_ID>
