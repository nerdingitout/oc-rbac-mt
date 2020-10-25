# oc-rbac-mt
Tutorial - OpenShift Role Based Access Control &amp; Multi-Tenancy

# Content
- Prerequisites
- Overview
- Create Project
- Create Users
- Create Role Bindings
- Create Pod

# Prerequisities
For this tutorial you will need:<br>
- Red Hat OpenShift Cluster 4.3 on IBM Cloud.<br>
- oc CLI (can be downloaded from this <a href="https://mirror.openshift.com/pub/openshift-v4/clients/oc/4.3/">link</a>.<br>
# Overview
In this tutorial we will attempt to use Role Bases Access Control (RBAC) to limit certain users to specific project (namespaces) and ensure their workloads are isolated from other users. Users are only entitiled to view and manage resources to which they are authorized using Role Bindings. Their resources & views are separated from other users on the same OpenShift cluster using the same set of shared HW resources. Therefore, we can achieve multi-tenancy using OpenShift Container Platform RBAC and Role Binding.
