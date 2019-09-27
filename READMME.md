*** Overview ***

Before deploying this integration first of all update the configuation file that will be used for the target environment.

This Salesforce system API can be deployed to CloudHub using maven. Below is an example of how to deploy to a test environment;

Note: The Anypoint client id and secret are for the environment of the business group

```
mvn -V -B -Dmule.env=test -Danypoint.environment=Sandbox -Danypont.business_group="Test" -Danypoint.username=xxx -Danypont.password=yyyy -Danypoint.platform.client_id=aaaaaa -Danypoint.platform.client_secret=bbbbb deploy -DmuleDeploy
```