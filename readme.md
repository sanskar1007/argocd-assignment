# Steps Followed to achieve this
1. Create two gke cluster, deploy argocd and connecte both the clusters to argocd using argocd cli.
2. Connect github repo to argocd using ssh keys.
3. Create two AppProject, one for staging and one for production. These are present in projects folder.
Note:- By staging and production here i mean two different clusters.
4. The environments folder contains two file staging and production. staging.yaml file creates an argo application for all the resources present in staging folder. All these resources will be created inside staging project on staging cluster. production.yaml file creates an argo application for all the resources present in production folder. These resources are deployed on production cluster and are present in production argo project.
5. Production and staging folder contains various deployments, apps and applicationset. Apart from this, these directory contains four types of hooks which will genrate flock alert. Sync, PostSync, PreSync and SyncFail are the events that will create these alerts.
6. busybox and wordpress-helm-chart folder contains helm charts, that i have created and are used by different argo applications and applicationset in both the clusters.
7. Apps folder is used to deploy same kustomize application(same base) in both clusters. The apps folder contains two different folders. 
(i)Base: It contains the deployment file, that will be used to deploy redis in both the clusters.
(ii)Overlay: It contains kustomize files for both the clusters.
8. All the syncs have been automated using 
syncPolicy:
    automated:
      prune: true
      selfHeal: true
9. Using github ui configure a webhook by adding argo public url. This webhook will send a signal to argo cd every time a push is made. Argocd will trigger a refresh as soon as it recieves the signal