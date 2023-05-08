# Enumeration

- Depending on how authorization matrix is configured either we will be presented with a Jenkins View or Authentication Window
- If there is no authentication process involved, the below techniques will work.
- The techniques also work if you have credentials that authenticates you and present you with a Jenkins View.

### Domains and executors

We can get multiple juicy information from the endpoint ```http://jenkins-server/api/json```
- Number of Executors for Built-In
  - If the value is 0, then the builds are carried out in workers/agents, if not then the controller's built-in is also used for build jobs!
  - In case you run your builds on agents/worker and they go offline, the builds will run on the Built-In if the non-zero value is present.

- url
  - In somecases this will point to the domain, or subdomain. From the analysis of the exposed Jenkins controllers we found this url information can help us to find the organization which this controller belongs to. We tried performing reverse-ip lookups but in somecases it failed or pointed to the cloud infrastructure.
- Project Names.
- Job/Build Information.

``` json
{
  "_class" : "hudson.model.Hudson",
  "assignedLabels" : [
    {
      "name" : "built-in"
    }
  ],
  "mode" : "NORMAL",
  "nodeDescription" : "the Jenkins controller's built-in node",
  "nodeName" : "",
  "numExecutors" : 2,
  "description" : null,
  "jobs" : [
  <SNIP>
  "primaryView" : {
    "_class" : "hudson.model.AllView",
    "name" : "all",
    "url" : "http://52.91.169.109:8080/"
  },
  "quietDownReason" : null,
  "quietingDown" : false,
  "slaveAgentPort" : -1,
  "unlabeledLoad" : {
    "_class" : "jenkins.model.UnlabeledLoadStatistics"
  },
  "url" : "http://52.91.169.109:8080/",
  "useCrumbs" : true,
  "useSecurity" : true,
  "views" : [
    {
      "_class" : "hudson.model.ListView",
      "name" : "Random",
      "url" : "http://52.91.169.109:8080/view/Random/"
    },
    {
      "_class" : "hudson.model.AllView",
      "name" : "all",
      "url" : "http://52.91.169.109:8080/"
    }
  ]
}
```

### Build Logs
 
Build Path logs are available at
``` 
https://SERVER/job/JOB_NAME/BUILD_NUMBER/consoleText
```
We can download these logs are find information like
- internal urls
- credentials
- personal information




If we are planning to automate our analysis, we can grab the JOB name from http://server/api/json?pretty=true. The jobs object will have the job name
``` json
{
  "_class" : "hudson.model.Hudson",
  "assignedLabels" : [
    {
      "name" : "built-in"
    }
  ],
  "mode" : "NORMAL",
  "nodeDescription" : "the Jenkins controller's built-in node",
  "nodeName" : "",
  "numExecutors" : 0,
  "description" : null,
  "jobs" : [
    {
      "_class" : "hudson.model.FreeStyleProject",
      "name" : "Test1",
      "url" : "http://52.91.169.109:8080/job/Test1/",
      "color" : "blue"
    }
  ],
  "overallLoad" : {
    
  },
  "primaryView" : {
    "_class" : "hudson.model.AllView",
    "name" : "all",
    "url" : "http://52.91.169.109:8080/"
  },
  "quietDownReason" : null,
  "quietingDown" : false,
  "slaveAgentPort" : -1,
  "unlabeledLoad" : {
    "_class" : "jenkins.model.UnlabeledLoadStatistics"
  },
  "url" : "http://52.91.169.109:8080/",
  "useCrumbs" : true,
  "useSecurity" : true,
  "views" : [
    {
      "_class" : "hudson.model.AllView",
      "name" : "all",
      "url" : "http://52.91.169.109:8080/"
    }
  ]
}
```

### Artifacts

Artifacts from the build are available at 

```SERVER/job/JOB_NAME/lastSuccessfulBuild/artifact/ARTIFACT-NAME```

```SERVER/job/JOB_NAME/lastSuccessfulBuild/artifact/*zip*/archive.zip```

```http://3.82.198.32:8080/job/Pipeline1/lastSuccessfulBuild/artifact/*zip*/archive.zip```

### Users

- https://azure-build.debian.net/asynchPeople/api/json?pretty
``` json
{
  "_class" : "hudson.model.View$AsynchPeople$People",
  "users" : [
    {
      "lastChange" : null,
      "project" : null,
      "user" : {
        "absoluteUrl" : "http://52.91.169.109:8080/user/system",
        "fullName" : "SYSTEM"
      }
    },
    {
      "lastChange" : null,
      "project" : null,
      "user" : {
        "absoluteUrl" : "http://52.91.169.109:8080/user/superadmin",
        "fullName" : "SuperAdmin"
      }
    }
  ]
}
```
### Question to ask with the obtained information

- Are those obatined internal urls reachable?
- Can I try to access those url if I have a pipeline that I can modify?
- Do these username are the the same ones used for login purpose in the corporate enviroment?
- Can I do a password spray with the username obtained?
- Can I replicate a resource url for a 404 error?
- Can I find any public github account of the user? ( **if you find any account, do search for any credentials through the repos in that account as there is a chance to get some corporate credentials or information which has been exposed accidentally** )

