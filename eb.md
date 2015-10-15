# Quick notes on Elastic Beansteak with maven and Java


## AWS-specific prerequisites

- Install `aws` CLI: https://aws.amazon.com/fr/cli/
- Install `eb` CLI: http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html

## Setting up a Web tier

- create a webapp archetype (called eblab here). e.g.
```
mvn archetype:generate -DarchetypeGroupId=org.glassfish.jersey.archetypes \
    -DarchetypeArtifactId=jersey-quickstart-webapp -DarchetypeVersion=2.22.1
```


- go inside and init Elastic Beanstalk: `eb init`

- create a Tomcat 8 / Java 8 environment. Here `sacc` is an existing role that has enough privileges to create instances for example
 `eb create --service-role sacc`

- It will takes time and fail because the default instance profile has no role. Fix:
 `aws iam add-role-to-instance-profile --instance-profile-name aws-elasticbeanstalk-ec2-role --role sacc`

- and go again. It will takes time but the environment will be running with nothing cool inside
 `eb create --service-role sacc`

- indicates `eb deploy` should deploy only the resulting WAR by editing `.elasticbeanstalk/config.yml`:
```
deploy:
  artifact: target/eblab.war
```

- to run locally:
```
$ mvn tomcat:run
$ open http://localhost:8080/eblab/webapi/myresource
```

- to deploy:
```
$ mvn package
$ eb deploy
$ open http://eblab-dev.elasticbeanstalk.com/webapi/myresource
```
