# Configuration As Code

This is about [Configuration as Code for CloudBees CI for Controllers](https://docs.cloudbees.com/docs/cloudbees-ci/latest/casc-controller/).

There is a subfolder named `casc-for-oc`, containing examples and information for [Configuration as Code for CloudBees CI for Operations Center](https://docs.cloudbees.com/docs/cloudbees-ci/latest/casc-oc/)


## Up-to-date Bundles

Due to some rework, not all Bundles are up-to-date to the October 2021 release of CloudBees CI.
These Bundles are:

* `shared`: a "parent" Bundle to inherit from
* `mc1`: a minimal Bundle, configuring as little as possible (very limited plugin set)
* `mc21`: more elaborate Bundle, adding a lot more plugins (such as BlueOcean)

## Steps

* install a git client on Operations Center
    * for example: `github-branch-source`
* create a Freestyle job
    * check out from your repository with the casc Bundles
    * use the `Synchronize bundles from workspace with internal storage` build step
* create a Controller and select an available Bundle

## Update Bundle Configuration

If you're not sure what you'd want to configure in the bundle, or which plugins you really need.

You can first create a Managed Master how you want it to be. Then export its CasC configuration by the built-in `casc-exporter`.

You do this, by going to the following URL `<masterUrl>/core-casc-export`.

## Freestyle Job

URL to checkout: `https://github.com/joostvdg/cloudbees-ci-casc.git`
Use the `Synchronize bundles from workspace with internal storage` build step.

Note: this only works if the Bundles are at the top level

![CasC Sync Step](casc-sync-step-example.png)

### CasC YAML

```YAML
- kind: freeStyle
  displayName: casc-sync-new
  name: casc-sync-new
  disabled: false
  description: 'My CasC Bundle Synchronization job'
  concurrentBuild: false
  builders:
  - casCBundlesSyncBuildStep: {}
  blockBuildWhenUpstreamBuilding: false
  blockBuildWhenDownstreamBuilding: false
  scm:
    gitSCM:
      userRemoteConfigs:
      - userRemoteConfig:
          url: https://github.com/joostvdg/cloudbees-ci-casc.git
      branches:
      - branchSpec:
          name: '*/main'
  scmCheckoutStrategy:
    standard: {}
```

### XML

```xml
<project>
<description/>
<keepDependencies>false</keepDependencies>
<properties/>
<scm class="hudson.plugins.git.GitSCM" plugin="git@4.8.3">
<configVersion>2</configVersion>
<userRemoteConfigs>
<hudson.plugins.git.UserRemoteConfig>
<url>https://github.com/joostvdg/cloudbees-ci-casc.git</url>
</hudson.plugins.git.UserRemoteConfig>
</userRemoteConfigs>
<branches>
<hudson.plugins.git.BranchSpec>
<name>*/main</name>
</hudson.plugins.git.BranchSpec>
</branches>
<doGenerateSubmoduleConfigurations>false</doGenerateSubmoduleConfigurations>
<submoduleCfg class="empty-list"/>
<extensions/>
</scm>
<canRoam>true</canRoam>
<disabled>false</disabled>
<blockBuildWhenDownstreamBuilding>false</blockBuildWhenDownstreamBuilding>
<blockBuildWhenUpstreamBuilding>false</blockBuildWhenUpstreamBuilding>
<triggers/>
<concurrentBuild>false</concurrentBuild>
<builders>
<com.cloudbees.casc.jenkins.server.CasCBundlesSyncBuildStep plugin="cloudbees-casc-server@1.29">
<purgeDeleted>true</purgeDeleted>
</com.cloudbees.casc.jenkins.server.CasCBundlesSyncBuildStep>
</builders>
<publishers/>
<buildWrappers/>
</project>
```

## Repository Structure

Limited to the most up to date bundles.

```bash
.
├── community1
│   ├── bundle.yaml
│   ├── jenkins
│   │   ├── jenkins.yaml
│   │   ├── podtemplate-golang.yaml
│   │   ├── podtemplate-maven-jdk17.yaml
│   │   └── shared-libraries.yaml
│   ├── plugin-catalog.yaml
│   └── plugins.yaml
├── community2
│   ├── bundle.yaml
│   ├── jenkins.yaml
│   └── plugins.yaml
├── mc1
│   ├── bundle.yaml
│   ├── items.yaml
│   └── jenkins.yaml
├── mc21
│   ├── bundle.yaml
│   ├── jenkins.yaml
│   ├── plugin-catalog.yaml
│   └── plugins.yaml
├── purple
│   ├── bundle.yaml
│   ├── items
│   │   └── pipeline-example.yaml
│   └── jenkins.yaml
└── shared
    ├── bundle.yaml
    ├── jenkins
    │   ├── main.yaml
    │   ├── podtemplate-golang.yaml
    │   ├── podtemplate-maven-jdk17.yaml
    │   └── shared-libraries.yaml
    ├── plugin-catalog.yaml
    └── plugins.yaml
```

#### Bundle YAML Equivalent

```yaml
apiVersion: "1"
version: "4"
id: "shared"
description: "Shared Bundle"
availabilityPattern: ".*"
jcasc:
  - "jenkins/"
plugins:
  - "plugins.yaml"
plugins:
  - "plugins.yaml"
catalog:
  - "plugin-catalog.yaml"
```

## Community Bundles

### Kubernetes Credentials Provider

```sh
2021-10-15 09:44:13.250+0000 [id=28] INFO c.c.j.p.k.KubernetesCredentialProvider#startWatchingForSecrets: retrieving secrets with selector: jenkins.io/credentials-type, LabelSelector(matchExpressions=[], matchLabels=null, additionalProperties={})
2021-10-15 09:44:15.446+0000 [id=28] SEVERE c.c.j.p.k.KubernetesCredentialProvider#startWatchingForSecrets: Failed to initialise k8s secret provider, secrets from Kubernetes will not be available
io.fabric8.kubernetes.client.KubernetesClientException: Failure executing: GET at: https://10.44.0.1/api/v1/namespaces/cbci/secrets?labelSelector=jenkins.io%2Fcredentials-type. Message: Forbidden!Configured service account doesn't have access. Service account may have been revoked. secrets is forbidden: User "system:serviceaccount:cbci:jenkins" cannot list resource "secrets" in API group "" in the namespace "cbci".
 at io.fabric8.kubernetes.client.dsl.base.OperationSupport.requestFailure(OperationSupport.java:639)
 at io.fabric8.kubernetes.client.dsl.base.OperationSupport.assertResponseCode(OperationSupport.java:576)
 at io.fabric8.kubernetes.client.dsl.base.OperationSupport.handleResponse(OperationSupport.java:543)
 at io.fabric8.kubernetes.client.dsl.base.OperationSupport.handleResponse(OperationSupport.java:504)
 at io.fabric8.kubernetes.client.dsl.base.OperationSupport.handleResponse(OperationSupport.java:487)
 at io.fabric8.kubernetes.client.dsl.base.BaseOperation.listRequestHelper(BaseOperation.java:163)
 at io.fabric8.kubernetes.client.dsl.base.BaseOperation.list(BaseOperation.java:672)
 at io.fabric8.kubernetes.client.dsl.base.BaseOperation.list(BaseOperation.java:86)
 at com.cloudbees.jenkins.plugins.kubernetes_credentials_provider.KubernetesCredentialProvider.startWatchingForSecrets(KubernetesCredentialProvider.java:116)
 ```

 You need to patch the permissions of the `jenkins` service account.

### Service Account Role for Permissions

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: secrets-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```

### Role Binding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-secrets-jenkins
subjects:
# You can specify more than one "subject"
- kind: ServiceAccount
  name: jenkins # "name" is case sensitive
roleRef:
  # "roleRef" specifies the binding to a Role / ClusterRole
  kind: Role #this must be Role or ClusterRole
  name: secrets-reader # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io
```