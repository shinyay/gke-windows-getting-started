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

#### Create Password
```
$ gcloud compute reset-windows-password
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
