## Prevasio CI/CD Container Security

AlgoSec’s CI/CD Container Security solution is an extensible security plugin platform that ... #TODO

### Action parameters
|Parameter|Description|Required|Default|Type|
|---|---|---|---|---|
||||||
|<b>Repository secrets</b>| | | | |
|`GITHUB_TOKEN`|Github PaT for checking diffs and commenting|Yes| |Secret Parameter|
|`CF_TENANT_ID`|AlgoSec Tenant ID|Yes| |Secret Parameter|
|`CF_CLIENT_ID`|AlgoSec Client ID|Yes| |Secret Parameter|
|`CF_CLIENT_SECRET`|AlgoSec Client Secret|Yes| |Secret Parameter|
||||||
|<b>General Parameters</b>| | | | |
|`WORKING_DIR`|Specify the GitHub repository's folder that contains the Dockerfile|Yes|. (root folder)|string|
|`DOCKERFILE_NAME`|Specify the dockerfile name|Yes|Dockerfile|string|
|`MIN_LEVEL_TO_BLOCK_PR`|Specify the minimum risk severity level to block the PR if at least one risk of this level is found|Yes|-1 (never block)|int|
|`product` (const)|Specify the AlgoSec's product which being used|Yes|Prevasio|string|
|`framework`(const)|Specify the scanned framework|Yes|docker|string|


### Configuration and usage
Here is an example of all possible parameters passed as environment variables to the action. 
Take into consideration that GitHub and AlgoSec credentials are mandatory in order to run this action, along with the general parameters.

#### Configuration:
Create a new client id and client secret in your Algosec Prevasio account using our user management module.
Add AlgoSec credentials to your github repo's secrets.
Add the general parameters to the action as env vars. 
The values for MIN_LEVEL_TO_BLOCK_PR are: 
1. Critical: 


```yaml
name: 'Your Repo CI/CD Yaml Workflow'
on:
  pull_request:
    branches:
      - 'main'
jobs:
  algosec-iac-connectivity-risk-analysis:
     name: 'Algosec IAC Connectivity Risk Analysis'
     runs-on: ubuntu-latest
     steps:
        - name: Checkout
          uses: actions/checkout@v3
        - name: Connectivity Risk Analysis
          uses: algosec/connectivity-risk-analysis-action@v1.0.0
          env:
            # Github's Private Access Token
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  
            # CloudFlow credentials
            CF_TENANT_ID: ${{ secrets.CF_TENANT_ID }}
            CF_CLIENT_ID: ${{ secrets.CF_CLIENT_ID }}
            CF_CLIENT_SECRET: ${{ secrets.CF_CLIENT_SECRET }}
            
            # Add your provider's keys to environment variables 
            # as secrets or use an external action to preconfigure
            
```


### Output(screenshots)
<img width="500" src="https://cloudflow.algosec.com/cloudflow/assets/devsecops-action/screenshot2.png" />
<img height="500" src="https://cloudflow.algosec.com/cloudflow/assets/devsecops-action/screenshot1.png" />
