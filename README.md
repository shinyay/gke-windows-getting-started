# GKE Windows Getting Started

Overview

## Description

## Demo
### Windows Server Jumpbox
#### Verify Image Name
```
 $ gcloud compute images list \
     --project windows-cloud \
     --no-standard-images | grep containers

```

#### Create Instance
```
$ gcloud beta compute instances create shinyay-windows \
    --zone=us-central1-a \
    --machine-type=n2-standard-2 \
    --image=windows-server-2019-dc-core-for-containers-v20210309 \
    --image-project=windows-cloud \
    --boot-disk-size=32GB \
    --boot-disk-type=pd-standard
```

#### Verify Instance Setup
```
$ gcloud compute instances get-serial-port-output shinyay-windows --zone us-central1-a
```

#### Create Firewall Rule
- HTTP
- HTTPS
- RDP

```
$ gcloud compute firewall-rules create default-allow-http \
    --project=(gcloud config get-value project) \
    --direction=INGRESS \
    --priority=1000 \
    --network=default \
    --action=ALLOW \
    --rules=tcp:80 \
    --source-ranges=0.0.0.0/0
$ gcloud compute firewall-rules create default-allow-https \
    --project=(gcloud config get-value project) \
    --direction=INGRESS \
    --priority=1000 \
    --network=default \
    --action=ALLOW \
    --rules=tcp:443 \
    --source-ranges=0.0.0.0/0
$ gcloud compute firewall-rules create default-allow-rdp \
    --project=(gcloud config get-value project) \
    --direction=INGRESS \
    --priority=1000 \
    --network=default \
    --action=ALLOW \
    --rules=tcp:3389 \
    --source-ranges=0.0.0.0/0
```

#### Create Password
```
$ gcloud compute reset-windows-password shinyay-windows
```

#### Verify Docker Pull
You can pull OS Base image before the host OS version.
```
>ver
Microsoft Windows [Version 10.0.17763.1817]
```
```
$ docker pull mcr.microsoft.com/windows/servercore:ltsc2019
$ docker pull mcr.microsoft.com/windows/nanoserver:1809

REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
mcr.microsoft.com/windows/servercore   ltsc2019            3eaa9ebbf51f        2 weeks ago         5.25GB
mcr.microsoft.com/windows/nanoserver   1809                47284f980c64        2 weeks ago         252MB
```
#### Service Account for Artifact Registry
#### Service Account for Container Registry
##### Client PC
Create Service Account
```
$ gcloud iam service-accounts create ce-windows-gcr --display-name "GCR from Windows Server"
```

Verify SA
```
$ gcloud iam service-accounts list --filter 'displayName:GCR from Windows Server' --format 'value(email)'
```

Bind Role to Service Account
```
$ gcloud projects add-iam-policy-binding (gcloud config get-value project) \
    --member serviceAccount:(\
      gcloud iam service-accounts list \
        --filter='displayName:GCR from Windows Server' \
        --format 'value(email)') \
    --role roles/storage.admin
```

Verify Role
```
$ gcloud projects get-iam-policy (gcloud config get-value project)
```

Create Key for Service Account
```
$ gcloud iam service-accounts keys create key.json \
    --iam-account (\
      gcloud iam service-accounts list \
        --filter="displayName:GCR from Windows Server" \
        --format 'value(email)')

```

##### Windows Jumpbox Server
gcloud Credential Helper
```
C:> gcloud auth configure-docker
```

key.json from Client to Jumpbox
```
C:> echo {  "type": "service_account",......iam.gserviceaccount.com"} > key.json
```

Activate Service account
```
C:> gcloud auth activate-service-account --key-file key.json
```

Tag for GCR
```
C:> docker tag mcr.microsoft.com/windows/servercore:ltsc2019 gcr.io/[PROJECT_ID]/servercore:ltsc2019
```

Push for GCR
```
C:> docker push gcr.io/[PROJECT_ID]/servercore:ltsc2019
```

## Features

- feature:1
- feature:2

## Requirement

## Usage

## Installation

## References

## Licence

Released under the [MIT license](https://gist.githubusercontent.com/shinyay/56e54ee4c0e22db8211e05e70a63247e/raw/34c6fdd50d54aa8e23560c296424aeb61599aa71/LICENSE)

## Author

[shinyay](https://github.com/shinyay)
