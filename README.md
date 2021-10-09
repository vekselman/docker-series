## Source code of changes you can find in
https://github.com/vekselman/docker-series/tree/docker-series-continuous-integration-jenkins-end

## Three main changes ware made to be able to run the build

### 1. In Dockerfiles
Change from:
```yaml
FROM microsoft/aspnetcore-build as build-image
```
to
```yaml
FROM mcr.microsoft.com/dotnet/sdk:2.1 as build-image
```
And from:
```yaml
FROM microsoft/aspnetcore
```
to
```yaml
FROM mcr.microsoft.com/dotnet/aspnet:2.1
```

### 2. In docker-compose.integration.yml file
Source of volume to "/docker-entrypoint-initdb.d" of "DB" service   
must be specified as from "Host", cause container engine is on host.   
So example of working volume is:
```yaml
volumes:
      - dbdata:/var/lib/mysql
      - /home/leonid/docker-series/_MySQL_Init_Script:/docker-entrypoint-initdb.d
```
### 3. In _MySQL_Init_Script/init.sql file
Add use of accounowner db as default before main sql queries   
Change from:
```sql
DROP TABLE IF EXISTS `account`;
```
to
```sql
USE accountowner;
DROP TABLE IF EXISTS `account`;
```
