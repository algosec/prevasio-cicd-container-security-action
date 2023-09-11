## Prevasio CI/CD Container Security

AlgoSec-Prevasio CI/CD Container Security solution is an extensible security plugin platform that provides an automated scan for Docker Containers.
AlgoSec-Prevasio will build, simulate runtime, and scan the image statically and dynamically for security risks as part of the user GitHub repo CI process.
The action is available only to registered AlgoSec Prevasio CNAPP users. For the official trial, click [here](https://www.algosec.com/products/algosec-cloud/).

---
### Action parameters
|Parameter|Description|Required|Default|Type|
|---|---|---|---|---|
||||||
|<b>Repository secrets</b>| | | | |
|`GITHUB_TOKEN`|Github PAT for checking diffs and commenting|Yes| |Secret Parameter|
|`ALGOSEC_TENANT_ID`|AlgoSec Tenant ID|Yes| |Secret Parameter|
|`ALGOSEC_CLIENT_ID`|AlgoSec Client ID|Yes| |Secret Parameter|
|`ALGOSEC_CLIENT_SECRET`|AlgoSec Client Secret|Yes| |Secret Parameter|
||||||
|<b>General Parameters</b>| | | | |
|`WORKING_DIR`|Specify the GitHub repository's folder that contains the Dockerfile|No|. (root folder)|string|
|`DOCKERFILE_NAME`|Specify the Dockerfile name|No|Dockerfile|string|
|`MIN_LEVEL_TO_BLOCK_PR`|Specify the minimum risk severity level to block the PR if at least one risk of this level is found|No|-1 (never block)|int|

---  
### Configurations
Create client id and client secret in your Algosec Prevasio account using our user management module.
Add AlgoSec credentials to your github repo's secrets.
Note:
* GitHub and AlgoSec credentials are mandatory in order to run the action
* If the `WORKING_DIR`, `DOCKERFILE_NAME` and `MIN_LEVEL_TO_BLOCK_PR` parameters, are not provided, the default values are taken
* The severty levels for the `MIN_LEVEL_TO_BLOCK_PR` are - Critical: 0, High: 1, Medium: 2. If it set to -1, the PR won't be blocked
* The branch is also configurable. You can change the name of the branch under [on -> pull_request -> branches]
* The permissions assigned to the job are mandatory in order to enable it to read the repository content and to write the scan results as a comment in the PR

**In order to enable the action to block the PR, follow the next steps:**    
For the action to be definable as a required check, it should be manually run:
1. Go to the repository **Actions** tab
2. Choose the workflow that runs **algosec-prevasio-cicd-container-security** job
3. Run the workflow  

Create a new branch protection rule to define the action as a required check:
1. Go to the repository **Settings** tab
2. Click on **Branches** on the left sidebar
3. Click **Add rule** / **Add branch protection rule**
4. Specify the branch the action runs on
5. Enable **Require status checks to pass before merging**
6. Add **Algosec Prevasio CI/CD Container Security** as a required check
7. Create the rule  

---
### Example usage
```yaml
name: 'Your Repo CI/CD Yaml Workflow'
on:
  pull_request:
    branches:
      - 'main'
  workflow_dispatch:
jobs:
  algosec-prevasio-cicd-container-security:
     name: 'Algosec Prevasio CI/CD Container Security'
     runs-on: ubuntu-latest
     permissions:
        contents: read
        pull-requests: write
     steps:
        - name: Checkout
          uses: actions/checkout@v3
        - name: CI/CD Container Security
          uses: algosec/prevasio-cicd-container-security-action@v1.0.0
          env:
            # Github's Private Access Token
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  
            # AlgoSec credentials
            CF_TENANT_ID: ${{ secrets.CF_TENANT_ID }}
            CF_CLIENT_ID: ${{ secrets.CF_CLIENT_ID }}
            CF_CLIENT_SECRET: ${{ secrets.CF_CLIENT_SECRET }}
            
            # General parameters
            WORKING_DIR: .
            DOCKERFILE_NAME: Dockerfile
            MIN_LEVEL_TO_BLOCK_PR: 1
            
```  

---
Output(screenshots)

<img height="500" src="https://cloudflow.algosec.com/cloudflow/assets/devsecops-action/prevasio-scan-blocked.png" />
<img width="500" src="https://cloudflow.algosec.com/cloudflow/assets/devsecops-action/prevasio-scan-passed.png" />
