---
title:       "Cloud BuildでApp Engineにデプロイできなくなったので roles/iam.serviceAccountUser を追加した"
subtitle:    ""
description: ""
date:        2020-11-18
author:      ""
image:       ""
tags:        ["gcp", "appengine", "cloudbuild"]
slug:        "cloudbuild-failed-to-deploy-to-appengine"
---

このブログをGitHub Actions経由でCloud Buildを使ってApp Engineにデプロイしている。  
ある日突然ビルドでエラーになってデプロイできなくなった。

```sh
Beginning deployment of service [default]...
╔════════════════════════════════════════════════════════════╗
╠═ Uploading 254 files to Google Cloud Storage              ═╣
╚════════════════════════════════════════════════════════════╝
File upload done.
ERROR: (gcloud.app.deploy) PERMISSION_DENIED: You do not have permission to act as '***@appspot.gserviceaccount.com'
- '@type': type.googleapis.com/google.rpc.ResourceInfo
  description: You do not have permission to act as this service account.
  resourceName: ***@appspot.gserviceaccount.com
  resourceType: serviceAccount
Error: The process '/opt/hostedtoolcache/gcloud/318.0.0/x64/bin/gcloud' failed with exit code 1
```

App Engineのデフォルトサービスアカウントとしての権限がないらしいが、今までできていたものができなくなった。  
ぐぐったら、サービスアカウントに `roles/iam.serviceAccountUser` を付与してあげればよさそう。 

> [Cloud Build fails to deploy to Google App Engine - You do not have permission to act as @appspot.gserviceaccount.com](https://stackoverflow.com/questions/64236468/cloud-build-fails-to-deploy-to-google-app-engine-you-do-not-have-permission-to)

[ドキュメント](https://cloud.google.com/appengine/docs/standard/go/roles) には最近(2020/11現在) 必要な権限が追加されたっぽい (※今の所英語版のみ)

