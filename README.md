# IP Ranges preview documentation

The IP Ranges feature is a way for CircleCI customers to configure access to restricted environments from CircleCI jobs. As a part of this feature, CircleCI provides a list of IP addresses associated with the CircleCI service, and customers can opt certain jobs into using those IP addresses.

## Feature availability

The feature is currently available in closed preview. Only customers that have been invited can access the feature at the moment.

## Limitations in the current version

* Only available on the Docker executor.
  * Does not include `remote_docker` VMs.
  * We are looking into making this functionality available on other executors in the future.
* Only available to customers on paid plans (Performance, Scale, Custom).

## Pricing

Pricing will be calculated based on network data usage of the jobs opted into the IP Ranges feature. Only the traffic of the opted-in jobs will be counted — you can of course mix jobs with and without the IP Ranges feature within the same workflow or pipeline.

Specific rates per GB are still to be confirmed.

We are planning on excluding data usage due to workspaces, caches, and artifacts from the IP Ranges charges. Keep in mind that data usage for these features may be charged separately in the future.

## Example config that uses the IP Ranges feature

Here’s a config snippet that runs a simple `curl` command with a deterministic IP:

```yaml
version: 2.1
jobs:
  build:
    circleci_ip_ranges: true # opts the job into the IP ranges
    docker:
      - image: curlimages/curl
    steps:
      - run: curl -s http://whatismyip.akamai.com
workflows:
  build-workflow:
    jobs:
      - build
```

Note: `http://whatismyip.akamai.com` is a service offered by Akamai that returns the IP address used to make a request in the response. We use it here for illustrative purposes, please replace the steps in the job above with your job content.

## List of IP addresses associated with the IP Ranges feature

All jobs that are using the IP Ranges feature will have one of the following IP addresses associated with them:

```
"107.22.40.20",
"18.215.226.36",
"3.228.208.40",
"3.228.39.90",
"3.91.130.126",
"34.194.144.202",
"34.194.94.201",
"35.169.17.173",
"35.174.253.146",
"52.20.179.68",
"52.21.153.129",
"52.22.187.0",
"52.3.128.216",
"52.4.195.249",
"52.5.58.121",
"52.72.72.233",
"52.72.73.201",
"54.144.204.41",
"54.161.182.76",
"54.162.196.253",
"54.164.161.41",
"54.167.72.230",
"54.205.138.102",
"54.209.115.53",
"54.211.118.70",
"54.226.126.177",
"54.81.162.133",
"54.83.41.200",
"54.92.235.88"
```

IPs for core services (used to trigger jobs, exchange information about users between CircleCI and Github etc):

```
"18.214.70.5",
"52.20.166.242",
"35.174.249.131",
"18.214.156.84",
"54.236.156.101",
"3.210.128.175"
```

Please note that jobs can use any of the addresses above.

During the preview phase, this list can change at any time. Once the feature is generally available, the list will be stable. We are planning on offering a way to receive a notification when the list of IP addresses is updated.

A machine-consumable list of the IP address ranges *for jobs* can be found on a [DNS A record](https://dnsjson.com/jobs.knownips.circleci.com/A.json).

A machine-consumable list of the IP address ranges *for core services* can be found on a separate [DNS A record](https://dnsjson.com/core.knownips.circleci.com/A.json).

A machine-consumable list of both *jobs and core services IP address ranges* can be found on a separate [DNS A record](https://dnsjson.com/all.knownips.circleci.com/A.json).
