# Mopidy

## Run in gcloud vm free tier, archlinux image

```sh
gcloud compute instances create $INSTANCE_NAME \
    --deletion-protection \
    --description="Mopidy" \
    --hostname=$INSTANCE_HOSTNAME \
    --image-project=arch-linux-gce \
    --image-family=arch \
    --machine-type=f1-micro \
    --zone=us-central1-a
```
