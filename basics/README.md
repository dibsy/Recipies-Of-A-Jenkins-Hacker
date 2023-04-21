# Jenkins Security Basics

### Terminiologies

- Controllers - The master node. It manages and schedule jobs which are usually carried out by the workers/agents. The interface allows us to administer and control various tasks like managing credentials, jobs, users, plugins, etc. 
- Agents - They are the workers which executes the job requested by the controller. It communicates with the controller via the JNLP protocol. The commmunication is usually encrypted.
- Ephimeral - They are created. They are used. Then they are gone ! No one remembers them ! Usually a secure approach to run agents.

### Jenkins Views

- We can organize how the jobs will look in dashboard using views
- We can control what kind of view will be served to the end user based on the privileges
- There can be views avaiable without any need for authentication.

### Intersting Endpoints

- Controller Endpoints
   - /asynchPeople/
   - /credentials/
- Build Endpoints
   - /job/

### How jobs are triggered 

- Manually

- Via Pull Requests/New Branch

- Via Automation
   - https://plugins.jenkins.io/build-token-root/
