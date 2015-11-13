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
- to save the configuration and ease portability:
```
$ eb config save
```

## HeathCheck support

By default, eb signals the role you provide cannot be assumed by elastic beanstalk. So the healthCheck does not work and you are sad.

To have a role that can be assumed by Elastic Beanstalk. Add the following inside the statements of the role trust relationships:
```
{
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "elasticbeanstalk.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
}
````
2 examples:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    },
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "elasticbeanstalk.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
````
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "elasticbeanstalk.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "elasticbeanstalk"
        }
      }
    }
  ]
}
```

Once yo have that, inside the health check configuration, the service role will be available
## Resources

- How does Elastic BeanStalk work: http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/concepts.html
- Enhanced Health Reporting and Monitoring: http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/health-enhanced.html
- Creating role to delegate permissions to an AWS service: http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html
- Google your error messages
 
