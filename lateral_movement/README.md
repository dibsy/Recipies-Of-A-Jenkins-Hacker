# Lateral Movement

## Internal Network Scan
- When Jenkins is deployed on premise, it has to potential to reach multiple corporate services and endpoints
- When Jenkins is deployed in cloud, it is reachable to limited on premise corporate services through direct connect.
- Therefore, it is worth performing a network scan to find the reachable services, enumerate ports and see if we can exploit any of the services in corp environment.
- We can also use build logs to find if any such corp environment is accessed through analyzing build logs.

## Enumerating services with access tokens
- If we are successful in dumping tokens, we should review what kind of privileges that token hold and what we can do with it.
- For our understanding, let us take an example of github access token.


## Executing groovy console commands via dumped jenkins-api token.
## Creating an AWS resources from their dumped tokens.
