# Deploying Helm Charts with Dependencies and Subcharts

In Helm, user's can define dependencies and subcharts that they can reference in their main Helm Chart. Helm will go download the dependencies and subcharts from existing repository or seperate repositories. 

Helm Subchart and Dependency Support is available for Kubernetes Deployment Types that are deploying Helm Charts as the Manifest source and our Native Helm Deployment Swimlane.

To Configure the subcharts, you will need define them in your service. You will need to define the sub chart name which is a path to the chart that resides in the parent chart.

For any dependencies to be resolved, you will need to configure the Helm Command with a Flag `Template` with `--dependency-update` this will allow Harness to go fetch your dependencies that you have defined in your chart.yaml. 

Harness supports Canary, Blue-Green and Rolling Deployments with the Helm Subcharts and Dependencies feature when deploying with Kubernetes. 

Harness supports deploying a basic strategy with native Helm and the Helm Subchart and dependency capabilities.




```YAML
service:
  name: K8sHelmSubChart
  identifier: K8sHelmSubChart
  serviceDefinition:
    type: Kubernetes
    spec:
      manifests:
        - manifest:
            identifier: m1
            type: HelmChart
            spec:
              store:
                type: Github
                spec:
                  connectorRef: gitHubAchyuth
                  gitFetchType: Branch
                  folderPath: parent-chart
                  branch: main
              subChartName: first-child
              skipResourceVersioning: false
              enableDeclarativeRollback: false
              helmVersion: V3
              commandFlags:
                - commandType: Template
                  flag: "--dependency-update"
  gitOpsEnabled: false

```