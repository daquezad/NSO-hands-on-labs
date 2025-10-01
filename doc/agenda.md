# Agenda

## Day 1

<!-- *   NSO Hands-On session
*   Cisco Network Services Orchestrator
    *   Connect to the Workstation
*   1.1 - Install NSO and NEDs
*   1.2 – Registering XRd routers
*   1.3 - Configure Devices using NSO
*   1.4 - Use Rollback’s
*   1.5 - Use NSO capabilities to detect Out-of-Band device configurations
*   1.6 - Create Device Groups and Device Templates
*   1.7 - Create a Simple NSO Service
*   1.8 - Create Role Based and Resource Based Access Control rules
*   -->



[1 - Connect to the Workstation
[3](#connect-to-the-workstation)](#connect-to-the-workstation)

[1.1 - Install NSO and NEDs
[4](#install-nso-and-neds)](#install-nso-and-neds)

[1.2 -- Registering XRd routers
[9](#registering-xrd-routers)](#registering-xrd-routers)

[1.3 - Configure Devices using NSO
[11](#configure-devices-using-nso)](#configure-devices-using-nso)

[1.4 - Use Rollback's [13](#use-rollbacks)](#use-rollbacks)

[1.5 - Use NSO capabilities to detect Out-of-Band device configurations
[15](#use-nso-capabilities-to-detect-out-of-band-device-configurations)](#use-nso-capabilities-to-detect-out-of-band-device-configurations)

[1.6 - Create Device Groups and Device Templates
[18](#create-device-groups-and-device-templates)](#create-device-groups-and-device-templates)

[1.7 - Create a Simple NSO Service
[23](#create-a-simple-nso-service)](#create-a-simple-nso-service)

[1.8 - Create Role Based and Resource Based Access Control rules
[37](#create-role-based-and-resource-based-access-control-rules)](#create-role-based-and-resource-based-access-control-rules)



<details>
<summary>Click here to show solution</summary>
```bash
ssh developer@10.10.20.50
```
</details>


```bash title="Repository content"
cd ${HOME}/nso_cicd
tree
.
├── packages
│   └── loopback
│       ├── load-dir
│       │   └── loopback.fxs
│       ├── package-meta-data.xml
│       ├── src
│       │   ├── Makefile
│       │   └── yang
│       │       └── loopback.yang
│       ├── templates
│       │   └── loopback-template.xml
│       └── test
│           ├── internal
│           │   ├── lux
│           │   │   ├── basic
│           │   │   │   ├── Makefile
│           │   │   │   └── run.lux
│           │   │   └── Makefile
│           │   └── Makefile
│           └── Makefile
├── pipelines
│   └── gitlab-ci.yml
├── pipeline_utils
│   └── environments.yml
└── tests
    └── loopback-test
        └── loopback-test.py

14 directories, 13 files
```



```yaml linenums="1" title="Gitlab runner .gitlab-ci.yml"
  ---
  dummy-job:
    script:
      - echo "This pipeline is triggered successfully!"
```