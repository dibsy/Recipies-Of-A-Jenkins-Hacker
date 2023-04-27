# Credentials Dumping

## Credentials in Description
- While this sounds very hypothetical, it is very much real.
- Possible reason for rewriting credentials in credentials description makes it easier to remember purpose, or for making debugging easy.

## Dumping credentials from logs


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
