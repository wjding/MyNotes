## Setup OS Login
### Use gcloud to add pub keys for ssh
Use console:
```
gcloud compute os-login ssh-keys add \
    --key-file key-file-path \
    --ttl expire-time
```
