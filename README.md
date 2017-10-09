# Preface #

This document describes the functionality provided by the xlr-xld-advanced-plugin.

See the **[XL Release Documentation](https://docs.xebialabs.com/xl-release/index.html)** for background information on XL Release and release concepts.

# Overview #

The xlr-xld-advanced-plugin is a XL Release plugin that allows you to perform slightly more advanced tasks on XL Deploy. 
It depends on the xlr-xldeploy-plugin - so make sure you have that installed already. You can find this in the **[Xebia Labs Community](https://github.com/xebialabs-community/xlr-xldeploy-plugin)**
This plugin was created to ensure that the community plugin was not polluted, whilst retaining the ability to perform these extra functions. 

## Installation ##

Place the latest released version under the `plugins` dir. If needed append the following to the `script.policy` under `conf`:

```
permission java.io.FilePermission "plugins/*", "read";
permission java.io.FilePermission "conf/logback.xml", "read";
```

This plugin requires XLR 7.x+

## Types ##

+ BulkUndeployTask (compatible with XL Deploy 4.5.1 and up)
  * `applicationFolder` - Folder containing the Applications you want to Undeploy
  * `environment` Environment you want to undeploy from (fully qualified starting with Environments/...)
  * `containerType` Type of Group task to be added (case sensitive): Parallel,Sequential
  * `recurse` If True, recurses through child directories of Application Folder 
  * `orchestrators` (Comma separated list of orchestrators to be used: `parallel-by-deployment-group, parallel-by-container`)
  * `deployedApplicationProperties` (Dictionary containing all the deployed application properties to be set (except orchestrators). e.g.: `{"maxContainersInParallel": "2"}`)
  * `continueIfStepFails` (Will try to continue if a step in the deployment task fails)
  * `numberOfContinueRetrials` (Number of times to retry a step)
  * `rollbackOnError` (Whether rollback should be done if the deployment fails)
  * `pollingInterval` (Number of seconds to wait before polling the task status)
  * `numberOfPollingTrials` (Number of times to poll for task status)
  * `failOnPause` (If checked task will fail if the deployment enters a STOPPED state, for example if the xld-pause-plugin is in use. Set to True by default for backwards compatibility)

+ Add/Remove Member
  * `server` - Server to query
  * `username` - Override username
  * `password` - Override password
  * `ciID` - ID of the Configuration Item you want to add or remove from the environment
  * `envID` - The Environment you wish to affect
  * `action` - Enum - ADD/REMOVE - Which action you wish to take

+ Rename CI
  * `server` - Server to query
  * `username` - Override username
  * `password` - Override password
  * `ciID` - ID of the Configuration Item you want to rename
  * `newName` - The new name for the CI - name only, not the full path
  * `return_type` - Hidden - JSON or XML

+ Get Environment Delta (Differences between 2 Environments)
  * `server` - Server to query
  * `username` - Override username
  * `password` - Override password
  * `currentEnvironment` - ID of the environment you wish to change
  * `mirrorEnvironment` - ID of the environment you wish to mirror
  * `to_add` - Return list of package IDs missing from current
  * `to_remove` - Return list of package IDs present on current which are not present on mirror

+ Get Environment Hosts 
  * `server` - Server to query
  * `ciID` - Environment you wish to poll
  * `username` - Override username
  * `password` - Override password
  * `tags_to_check` - List of Tags, which you want the hosts for. If left blank, will return all hosts.
  * `host_list` - List of all hostnames
  * `host_dict` - Map of CI IDs and Hostnames

+ Get Application Checklist 
  * `server` - Server to query
  * `username` - Override username
  * `password` - Override password
  * `ci_ID` - ID of the Application you wish to get the checklist for
  * `createReleaseVariables` - Boolean. If True, creates/updates variables in the release relating to the Checklist
  * `unique` - Boolean. If True (and createReleaseVariables is True) will create variables as follows: APPLICATION_VERSION_VARIABLENAME 
  * `variableDict` - Return map of the checklist as it currently stands for the Application
