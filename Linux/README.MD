### Local to EC2:
``` bash
scp -i <KeyPath> <LocalSourcePath> <Username>@<PublicIP>:<DestinationPathOnEC2>
```

### EC2 to Local:
``` bash
scp -i <KeyPath> <Username>@<PublicIP>:<SourcePathOnEC2> <LocalDestinationPath>
```
