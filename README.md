uclalib_role_argocd
=========

Ansible role to install/configure ArgoCD within the UCLA Library's infrastructure

This includes installing the following additional components:
  - Helm
  - External Secrets
  - Cert Manager

Prerequistes
------------

It is expected you have the following already provisioned/created:
  - RHEL 8 Host
  - K3s installed
  - Secrets in our AWS Paramter Store:
    - External Secret server key
    - Sectigo TLS key
    - ArgoCD LDAP config
    - ArgoCD Slack token

Role Variables
--------------

  - `k3s_kubeconfig_path`
      ```
       Path to the k3s kubeconfig file
      ```
  
  - `helm_version`
      ```
      The version of Helm to install
      Check for available versions at https://github.com/helm/helm/releases
      Check https://helm.sh/docs/topics/version_skew/ to match helm version with k8s versions
      !!! Be sure to set this in the appropriate argocd host var
      ```

  - `external_secrets_helm_chart_repo`
    ```
    URL to the External Secrets Helm Chart Repo
    ```

  - `external_secrets_version`
    ```
    The CHART VERSION of External Secrets to install
    Example: 0.7.2
    Reference the External Secrets Helm Chart repo to match the CHART VERSION with the APP VERSION
    Example: CHART VERSION: 0.7.2 -----> APP VERSION: v0.7.2
    helm search repo external-secrets --versions
    !!! Be sure to set this in the appropriate argocd host var
    ```

  - `cert_manager_helm_chart_repo`
    ```
    URL to the Cert Manager Helm Chart Repo
    ```

  - `cert_manager_version`
    ```
    The CHART VERSION of Cert Manager to install
    Example: v1.11.1
    Reference the Cert Manager Helm Chart repo to match the CHART VERSION with the APP VERSION
    Example: CHART VERSION: v1.11.1 -----> APP VERSION: v1.11.1
    helm search repo jetstack/cert-manager --versions
    !!! Be sure to set this in the appropriate argocd host var
    ```

  - `argocd_helm_chart_repo`
    ```
    URL to the ArgoCD Helm Chart Repo
    ```

 - `argocd_version`
    ```
    The CHART VERSION of ArgoCD to install
    Example: 5.34.6
    Reference the Argo Helm Chart repo to match the CHART VERSION with the APP VERSION
    Example: CHART VERSION: 5.34.6 -----> APP VERSION: v2.7.3
    helm search repo argo/argo-cd --versions
    !!! Be sure to set this in the appropriate argocd host var
    ```

 - `gitops_kubernetes_repo_url`
    ```
    GitHub repo URL holding our env-specific values file for ArgoCD
    ```

 - `argocd_env_values_path`
    ```
    Path to the env-specific helm values file for argocd
    Relative the temp work directory where the gitops kubernetes repo is cloned
    Example: gitops_kubernetes/infrastructure/deployment/argocd.library.ucla.edu-values.yaml
    !!! Be sure to set this in the appropriate argocd host var
    ```

Example Playbook
----------------

```
---

- name: uclalib_argocd.yml
  hosts: argocd

  roles:
    - name: uclalib_role_argocd
```
