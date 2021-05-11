# GSP302 : Configure a Firewall and a Startup Script with Deployment Manager :-

```bash
mkdir deployment_manager
cd deployment_manager
gsutil cp gs://spls/gsp302/* .
```

```bash
nano qwiklabs.jinja
```

### Delete all the content of qwiklabs.jinja and paste :-

```jinja
resources:
- type: compute.v1.instance
  name: vm-test
  properties:
    zone: {{ properties["zone"] }}
    machineType: https://www.googleapis.com/compute/v1/projects/{{ env["project"] }}/zones/{{ properties["zone"] }}/machineTypes/f1-micro
    # For examples on how to use startup scripts on an instance, see:
    #   https://cloud.google.com/compute/docs/startupscript
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        diskName: disk-{{ env["deployment"] }}
        sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/family/debian-9
    networkInterfaces:
    - network: https://www.googleapis.com/compute/v1/projects/{{ env["project"] }}/global/networks/default
      # Access Config required to give the instance a public IP address
      accessConfigs:
      - name: External NAT
        type: ONE_TO_ONE_NAT
    tags:
      items:
        - http
    metadata:
      items:
      - key: startup-script
        value: |
          #!/bin/bash
          apt-get update
          apt-get install -y apache2
- type: compute.v1.firewall
  name: default-allow-http
  properties:
    network: https://www.googleapis.com/compute/v1/projects/{{ env["project"] }}/global/networks/default
    targetTags:
    - http
    allowed:
    - IPProtocol: tcp
      ports:
      - '80'
    sourceRanges:
    - 0.0.0.0/0

```

### To save press, ctrl + o -> Enter -> ctrl + x

```bash
nano qwiklabs.yaml
```

### Delete the content of qwiklabs.yaml and paste :-

```yaml
imports:
  - path: qwiklabs.jinja

resources:
  - name: qwiklabs
    type: qwiklabs.jinja
    properties:
      zone: us-central1-a
```

```bash
gcloud deployment-manager deployments create test --config=qwiklabs.yaml
```
