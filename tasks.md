# Airflow Operator Refactoring Tasks

Based on hdfs-operator PR #255: Upgrade kubebuilder scaffold to 4.10.1

## Overview
This refactoring upgrades the airflow-operator to use kubebuilder 4.10.1 scaffold, matching the changes made in hdfs-operator PR #255.

## Main Categories

### 1. Core Configuration Files
- [ ] Update PROJECT file to add `cliVersion: 4.10.1`
- [ ] Update .gitignore with new patterns
- [ ] Move .chainsaw.yaml to test/e2e/.chainsaw.yaml

### 2. Makefile Updates
- [ ] Update Makefile structure and organization
  - [ ] Reorganize variables (move VERSION, REGISTRY, etc. to top)
  - [ ] Remove ENVTEST_K8S_VERSION variable
  - [ ] Update manifests target (remove `crd:generateEmbeddedObjectMeta=true`)
  - [ ] Add quotes around tool invocations (e.g., "$(CONTROLLER_GEN)")
  - [ ] Update test target to use setup-envtest instead of envtest
  - [ ] Add KIND_CLUSTER variable
  - [ ] Add setup-test-e2e target
  - [ ] Update test-e2e target with KIND cluster setup/teardown
  - [ ] Add cleanup-test-e2e target
  - [ ] Add lint-config target
  - [ ] Move build variables to ##@ Build section
  - [ ] Update docker-buildx target
  - [ ] Update build-installer target
  - [ ] Remove chart and chart-publish targets (if they exist)
  - [ ] Update install/uninstall targets with new logic
  - [ ] Update deploy/undeploy targets
  - [ ] Add helm-related targets (helm-chart-package, helm-chart-publish, etc.)
  - [ ] Add chainsaw target for e2e tests

### 3. cmd/main.go Updates
- [ ] Reorganize imports (move metrics/filters and webhook imports)
- [ ] Add certificate configuration flags:
  - [ ] metricsCertPath, metricsCertName, metricsCertKey
  - [ ] webhookCertPath, webhookCertName, webhookCertKey
- [ ] Add showVersion flag
- [ ] Update version flag handling
- [ ] Update webhook server initialization with certificate options
- [ ] Update metrics server comments and certificate handling
- [ ] Update controller-runtime package version references in comments
- [ ] Add blank line after controller setup

### 4. config/crd/ Updates
- [ ] Update config/crd/kustomization.yaml
- [ ] Remove config/crd/patches/cainjection_in_airflowclusters.yaml
- [ ] Remove config/crd/patches/webhook_in_airflowclusters.yaml

### 5. config/default/ Updates
- [ ] Create config/default/cert_metrics_manager_patch.yaml
- [ ] Update config/default/kustomization.yaml:
  - [ ] Update comments about patches
  - [ ] Add METRICS-WITH-CERTS patch section
  - [ ] Update WEBHOOK and CERTMANAGER sections
  - [ ] Add extensive replacements configuration for cert-manager
  - [ ] Update replacement targets for metrics certificates
  - [ ] Update replacement targets for webhook certificates
  - [ ] Add scaffold markers for CRD kustomize ca injection
- [ ] Update config/default/metrics_service.yaml

### 6. config/manager/ Updates
- [ ] Update config/manager/manager.yaml with new annotations and structure

### 7. config/rbac/ Updates
- [ ] Create config/rbac/airflowcluster_admin_role.yaml
- [ ] Update config/rbac/airflowcluster_editor_role.yaml:
  - [ ] Update comments
  - [ ] Simplify labels
- [ ] Update config/rbac/airflowcluster_viewer_role.yaml:
  - [ ] Update comments
  - [ ] Simplify labels
- [ ] Update config/rbac/kustomization.yaml to include admin role
- [ ] Update config/rbac/leader_election_role.yaml - simplify labels
- [ ] Update config/rbac/leader_election_role_binding.yaml - simplify labels
- [ ] Update config/rbac/role_binding.yaml - simplify labels
- [ ] Update config/rbac/service_account.yaml - simplify labels

### 8. config/prometheus/ Updates
- [ ] Update config/prometheus/monitor.yaml

### 9. config/network-policy/ Updates
- [ ] Update config/network-policy/allow-metrics-traffic.yaml

### 10. GitHub Workflows Updates
- [ ] Update .github/workflows/test.yml
- [ ] Update .github/workflows/chart-lint-test.yml:
  - [ ] Add job dependencies
  - [ ] Update helm action versions
  - [ ] Fix typo in error message
  - [ ] Add helm chart package step
  - [ ] Update helm install command
- [ ] Update .github/workflows/publish.yml
- [ ] Update .github/workflows/release.yml

### 11. Test Directory Updates
- [ ] Remove test/e2e/kind-config.yaml (if exists)
- [ ] Move .chainsaw.yaml to test/e2e/.chainsaw.yaml

## Verification Steps
- [ ] Review all changes against hdfs-operator PR diff
- [ ] Ensure no functionality is broken
- [ ] Verify all file paths are correct
- [ ] Check that all airflow-specific references are maintained
- [ ] Verify build and test commands work

## Notes
- All references to "hdfs" should remain "airflow" in airflow-operator
- All references to "HdfsCluster" should remain "AirflowCluster"
- Package paths should use github.com/zncdatadev/airflow-operator
- Domain remains kubedoop.dev
