---
title: "Allow Pulling Image from GCR in Different Projects"
date: 2020-05-21T13:18:00+09:00
tags: [gcp, gcr, docker, gke]
categories: [note, howto]
---

I was trying to use my docker image that is pushed to the dev gcp project's Goole Container Registry (gcr) in my staging project deployment gke. The first thing I did was that I just did 

```
kubectl apply -R -f .
```

in my staging environment and I got an permission error as expected. I had to find a way to allow my staging project permission to be dev's gcr so I was searching all over the Internet. After following Google's document and couple of Medium posts, I was surprised that I could not find any way to make it work. After playing around for a bit I finally got it to work, and I better memo it here before I forget again.

First step is you have to find out the your project number of your staging project (project that you want to give permission to), which is shown in the Dashboard (the firs page) of the GCP console.

![1](/images/2020_05_21_2020_05_21_allow_pulling_image_from_gcp_in_different_projects_01.png)

Memo copy paste this number somewhere and then go to the IAM page and looks for a service account that look like this.

```
{YOUR_PROJECT_NUMBER}-compute@developer.gserviceaccount.com
```

Copy the service account and then go to the IAM page of the dev project (the project where the image is). Give `Compute Image User` and `Storage Object Viewer` to the service account above. Resulting in something like this

![1](/images/2020_05_21_2020_05_21_allow_pulling_image_from_gcp_in_different_projects_02.png)

Now tell the kubectl to restart and repull the image

```
kubectl rollout restart -R -f .
```

and you should be able to pull the image.