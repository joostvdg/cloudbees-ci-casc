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
    * checks out repository
    * copies files to `${JENKINS_HOME}/jcasc-bundles-store`
* create a Master with a naming matching a bundle

## Update Bundle Configuration

If you're not sure what you'd want to configure in the bundle, or which plugins you really need.

You can first create a Managed Master how you want it to be. Then export its CasC configuration by the built-in `casc-exporter`.

You do this, by going to the following URL `<masterUrl>/core-casc-export`.

## Freestyle Job

URL to checkout: `https://github.com/joostvdg/jenkins-examples.git`



### XML

```xml

```

## Repository Structure

Limited to the most up to date bundles.

```bash
.
├── README.md
├── casc-for-oc
│   ├── README.md
│   ├── casc-oc-config-map.yaml
│   └── helm-values.yaml
├── mc1
│   ├── bundle.yaml
│   ├── items.yaml
│   └── jenkins.yaml
├── mc21
│   ├── bundle.yaml
│   ├── jenkins.yaml
│   ├── plugin-catalog.yaml
│   └── plugins.yaml
└── shared
    ├── bundle.yaml
    ├── jenkins
    │   ├── jenkins.yaml
    │   ├── podtemplate-golang.yaml
    │   ├── podtemplate-maven-jdk17.yaml
    │   └── shared-libraries.yaml
    ├── plugin-catalog.yaml
    └── plugins.yaml
```