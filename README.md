# openshift-utils
## Jenkins local slaves
Only works for OpenShift 3.6 (or Origin 1.6)
1. Create two imagestreams (see attachments) one for maven, one for nodejs
 
 ```$ oc create -n openshift jenkins-slave-image-streams.yaml```

2. Adapt the jenkins-ephemeral and add the configmap creation with two new templates (local-nodejs, local-maven). This templates will have the `alwaysPullImage: true` and `image: docker-registry.default.svc:5000/openshift/jenkins-slave-maven-rhel7`
3. Create the adapted template (see attachments for adapted template and standalone configmap I added)
 
 ```$ oc create -n openshift -f jenkins-ephemeral-template.yaml ```

Now the steps to create a new project and a new app is as follows:
 ```
 $ oc new-project myjenkins
 $ oc new-app jenkins-ephemeral-local-slaves
 ```

Finally you will see the two existing templates (maven and nodejs) and the two new added (local-maven and local-nodejs) because they cannot be overwritten.
With this it is possible to create a project that uses this local-maven template.
