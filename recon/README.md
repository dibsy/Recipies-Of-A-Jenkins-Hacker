# Recon

- Depending on how views authorization are set, we can try the below process without any credentials
- The below process will also work with credentials.

### Domains and executors ( Demo Recorded )

The two interesting information we can find from this endpoint is 
- Num Executors of Built-In - If this is set 0, then the builds are carried out in agents, if not then the controller is also used for jobs
- url - In somecases this will point to the domain, or subdomain. From the analysis of the exposed Jenkins controllers we found this url information can help us to find the organization which this controller belongs to. We tried performing reverse-ip lookups but in somecases it failed or pointed to the cloud infrastructure.

``` xml
<hudson _class="hudson.model.Hudson">
<assignedLabel>
<name>built-in</name>
</assignedLabel>
<mode>NORMAL</mode>
<nodeDescription>the Jenkins controller's built-in node</nodeDescription>
<nodeName/>
<numExecutors>0</numExecutors>
<job _class="hudson.model.FreeStyleProject">
<name>Test1</name>
<url>http://52.91.169.109:8080/job/Test1/</url>
<color>blue</color>
</job>
<overallLoad/>
<primaryView _class="hudson.model.AllView">
<name>all</name>
<url>http://52.91.169.109:8080/</url>
</primaryView>
<quietingDown>false</quietingDown>
<slaveAgentPort>-1</slaveAgentPort>
<unlabeledLoad _class="jenkins.model.UnlabeledLoadStatistics"/>
<url>http://52.91.169.109:8080/</url>
<useCrumbs>true</useCrumbs>
<useSecurity>true</useSecurity>
<view _class="hudson.model.AllView">
<name>all</name>
<url>http://52.91.169.109:8080/</url>
</view>
</hudson>
```

### Build Logs
 
Build Path logs are available at
``` 
https://SERVER/job/JOB_NAME/BUILD_NUMBER/consoleText
```

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
