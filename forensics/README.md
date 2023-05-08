# Forensics with Jenkins

## Workspace forensics
- Looking into private repo
- Downloading private repo
- Scanning for data on those private repo

## Build Log Analysis
- Not only we can find credentials from build logs, but interesting urls too!
- From analyzing the build logs we can sometimes find infromation about the software tech-stack
- We can also predict the security posture / security maturity of the software being built. 

## Artifact forensics in workspace

We can download the artifacts from the workspace. Then we can do the following tasks
- Decompile / Reverse engineer the binaries
- We can look through the workspace and look if any new resources from debug time has been left off or not


## Artifact forensics by pulling down binaries from repositories
- Developers may create custom packages / binaries from their builds ( nexus,docker registry, package registries like pip,npm )
- These builds are usually pushed to artifact repositories or registries
- Jenkins can talk with artifact repositories, hence we can try to download artifacts from those respositories.
- Then we can decompile / Reverse engineer the binaries.
- It is not unusual to find hardcoded credentials, urls,certificates from these binaries.

## Artifacts forensics by pulling down binaries from cloud environments ( Demo )
- Similarly like the previous, but from the cloud environment.
- Endless possibilities ! S3, CloudWatch Logs, Lambda functions images, and so on !


## Non-Ephemeral builds ( Demo )
- If the build agent is not emphemeral, succesive builds may have access to the previous data
- This means if the build agents are reused among various projects, we can get access to the debug information from different projects or executions.
