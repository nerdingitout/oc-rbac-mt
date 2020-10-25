# Role Based Access Control and Multi-tenancy on Red Hat OpenShift
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
## Overview
In this tutorial we will attempt to use Role Bases Access Control (RBAC) to limit certain users to specific project (namespaces) and ensure their workloads are isolated from other users. Users are only entitiled to view and manage resources to which they are authorized using Role Bindings. Their resources & views are separated from other users on the same OpenShift cluster using the same set of shared HW resources. Therefore, we can achieve multi-tenancy using OpenShift Container Platform RBAC and Role Binding.

## Log in and Create Porject
First, go to the web console and click on your username at the top left then 'Copy Login Command', then display the token and copy the ```oc login``` command in your terminal.<br>
![login](https://user-images.githubusercontent.com/36239840/97104809-26821500-16d0-11eb-936e-c2b7fb914523.JPG)
Next, we will create two projects for the two users we will be creating in the next step, copy the following commands in your terminal.<br>
```
oc create namespace my-first-project
```
```
oc create namespace my-second-project
```
Once created, you can find the projects you created in the project list.<br>
![projects](https://user-images.githubusercontent.com/36239840/97105004-84632c80-16d1-11eb-9689-64afe4e8679a.JPG)

## Create Users
In this step, we will create two users through the command line. Copy the following commands in your terminal.<br>
```
oc create user first-user --full-name="first user"
```
```
oc create user second-user --full-name="second user"
```
Once created, you can find the new users listed in Users list.<br>
![users](https://user-images.githubusercontent.com/36239840/97105173-7235be00-16d2-11eb-92a0-315f90abde85.JPG)
<br>
Notice, that as an Admin user, you can view all projects and pods in the cluster.<br> Go to <b>Home &#8594; Projects</b> to view al projects.
![all projects](https://user-images.githubusercontent.com/36239840/97105271-0738b700-16d3-11eb-86cd-267ea26c316b.JPG)
<br> Go to <b>Workloads &#8594; Pods</b> to view all pods (make sure that you have all projects is selected at the top)
![all pods](https://user-images.githubusercontent.com/36239840/97105447-271caa80-16d4-11eb-9a7c-418f444ec939.JPG)
<br>
## Impersonate Users
If you switch to another user (any of the new users created), you will notice that you can't view any projects or pods.<br>
Go to <b>User Management &#8594; Users</b> then click 'Impersonate Users' on one of the new users you hace created as shown in the screenshot below.
![impersonate user](https://user-images.githubusercontent.com/36239840/97105489-84b0f700-16d4-11eb-94cf-1c9a1fdf1566.JPG)
If you go to <b>Home &#8594; Projects</b>, you will notice that the user can't view any projects/pods or create any new projects/pods. That's because the user doesn't have any Role Binding yet, which means it does not have any privileges and/or Permissions to create/view/manage Projects and Pods.<br>
![first user projects](https://user-images.githubusercontent.com/36239840/97105506-9e523e80-16d4-11eb-9e68-6d53ee8e1c09.JPG)
![first user pods](https://user-images.githubusercontent.com/36239840/97105576-4e27ac00-16d5-11eb-8e2c-50921f432312.png)


