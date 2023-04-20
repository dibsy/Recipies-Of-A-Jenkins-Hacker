# Jenkins Security Basics

### Terminiologies

- Controllers - The master node. It manages and schedule jobs which are usually carried out by the workers/agents. The interface allows us to administer and control various tasks like managing credentials, jobs, users, plugins, etc. 
- Agents - They are the workers which executes the job requested by the controller. It communicates with the controller via the JNLP protocol. The commmunication is usually encrypted.
- Ephimeral - They are created. They are used. Then they are gone ! No one remembers them ! Usually a secure approach to run agents.


### Intersting Endpoints
- curl https://xxxx/job/yyyy/api/json
- /asynchPeople/
- /credentials/
