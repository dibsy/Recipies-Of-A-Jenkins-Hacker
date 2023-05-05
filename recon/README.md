# Recon

- Depending on how views authorization are set, we can try the below process without any credentials
- The below process will also work with credentials.

### Domains and executors

We can get multiple juicy information from the endpoint ```http://jenkins-server/api/json```
- Num Executors of Built-In - If this is set 0, then the builds are carried out in agents, if not then the controller is also used for jobs
- url - In somecases this will point to the domain, or subdomain. From the analysis of the exposed Jenkins controllers we found this url information can help us to find the organization which this controller belongs to. We tried performing reverse-ip lookups but in somecases it failed or pointed to the cloud infrastructure.
- Project Names
- Build Informations

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
