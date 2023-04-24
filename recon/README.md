- Traverse /api/xml and look for the domains and executors ( Demo Recorded )
- build logs paths

- Build Logs
- 
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
