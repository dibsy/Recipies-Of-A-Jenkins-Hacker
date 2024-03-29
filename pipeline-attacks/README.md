# Pipeline Attacks

- Some attacks like Replaying the Pipeline / Configuring the job by an unauthenticated user is possible due to misconfiguration.


## Attacking via a compromised GitHub code
- If we are able to access the SCM ( like GitHub, GitLab, etc), we can look for the Jenkinfile.
- If we have enough privileges( write access available ) , we can modify the Jenkinfile with our own modification.
- Next we wait for the trigger to happen.

## Replaying the pipeline
- Depending on how the permissions are misconfigured, we can run builds without having build permissions!

<img src="pipeline-replay.png">

<img src="pipeline-replay-modify.png">

## Configurable Jobs
- Changing the configuration due to permission misconfiguration ( Freestyle Projects )
- Add a build step ( execute a shell script )

## Attacking via a Pull Request
- Opensource projects in github/gitlab allows contributors to create a Pull Request
- To determine if the PR is able to be merged or the tests are working correctly, the maintainers configures the Build System to start a new build.
- As an attacker if we can change the Jenkinsfile and the build runs the contents of the modified Jenkinsfile, we can carry out attacks of credential dumping,pivoting,etc


## Attacking using a build token

- When we want to intiate build via API / Automation , we can use buildtoken for example http://<host:ip>/job/Test1/build?token=BUILDIT
- If we have acccess to such build token and have enough privileges to modify the source code we can try this technique.
- Out of curiosity I wondered it really possible to find such tokens from the public SCM, and I was surprised ;)
 
<img src="buildbytoken.png">
