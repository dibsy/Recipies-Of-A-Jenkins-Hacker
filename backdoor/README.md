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

## Agents SSH Keys

- We can dump the SSH keys from the Jenkins Controller
- Once we have the SSH keys we can montitor the logon events and exfiltrate the build data or snoop on it for debug logs or credentials.

## Malicious Agents
