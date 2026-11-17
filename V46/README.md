# IaC och CloudFormation

**Nima Kamali**

**AWS V42**

**Repo**    
https://github.com/nima1233/aws-MOV25


## Versionshantering
Här är en ändring som jag gjorde på min template. Varje ändring kan committas med ett meddelande som beskriver vad som har ändrats. Historiken gör det möjligt att se tidigare versioner och återgå till en fungerande version ifall något skulle bli fel. Samarbetar man med flera personer kan man se vem som ändrat vad och när.

![change](change.png)

## Återskapning
Ladda ner natverk.yaml, data.yaml, webb.yaml från https://github.com/nima1233/aws-MOV25/tree/main/V46

### Skapa key pair
AWS CLI
```
aws ec2 create-key-pair --key-name novatrix-key --query "KeyMaterial" --output text > novatrix-key.pem
```
Key pair sparar .pem filen i den mappen powershell är i.

### Stacks i ordning
Deploya först nätverk stack, när den är klar deploya data stack och när den är klar deploya sist webbstack.

### Nätverk stack
AWS CLI
```
aws cloudformation deploy --template-file natverk.yaml --stack-name novatrix-natverk --parameter-overrides Prefix=novatrix MyIp="$(curl.exe -s https://checkip.amazonaws.com)/32"
```
### Data stack
AWS CLI
```
aws cloudformation deploy --template-file data.yaml --stack-name novatrix-data
```
### Webb stack
Den pekar på en s3 buckets som redan finns, både novatrix-data-nimkam01 och backup till novatrix-backup-nimkam-01   
AWS CLI
```
aws cloudformation deploy --template-file webb.yaml --stack-name novatrix-webb --capabilities CAPABILITY_NAMED_IAM --parameter-overrides KeyName=novatrix-key DataBucket=novatrix-data-nimkam01 BackupBucket=novatrix-backup-nimkam-01
```