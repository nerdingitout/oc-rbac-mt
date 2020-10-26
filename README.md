# Role-based Access Control and Multi-tenancy on Red Hat OpenShift
Tutorial - OpenShift Role Based Access Control &amp; Multi-Tenancy

## Content
- [Prerequisites](https://github.com/nerdingitout/oc-rbac-mt/blob/main/README.md#prerequisities)
- [Overview](https://github.com/nerdingitout/oc-rbac-mt#overview)
- [Login and Create Project](https://github.com/nerdingitout/oc-rbac-mt#login-and-create-porject)
- [Create Users](https://github.com/nerdingitout/oc-rbac-mt#create-users)
- [Impersonate Users](https://github.com/nerdingitout/oc-rbac-mt#impersonate-users)
- [Create Role Bindings](https://github.com/nerdingitout/oc-rbac-mt#create-role-bindings)
- [Create & Deploy Pod](https://github.com/nerdingitout/oc-rbac-mt#create--deploy-pod)
- [Switching to Another User](https://github.com/nerdingitout/oc-rbac-mt#switching-to-another-user)
- [Summary](https://github.com/nerdingitout/oc-rbac-mt#summary)

## Prerequisities
For this tutorial you will need:<br>
- Red Hat OpenShift Cluster 4.3 on IBM Cloud.<br>
- oc CLI (can be downloaded from this <a href="https://mirror.openshift.com/pub/openshift-v4/clients/oc/4.3/">link</a>.<br>
## Overview
In this tutorial we will attempt to use Role Bases Access Control (RBAC) to limit certain users to specific project (namespaces) and ensure their workloads are isolated from other users. Users are only entitiled to view and manage resources to which they are authorized using Role Bindings. Their resources & views are separated from other users on the same OpenShift cluster using the same set of shared HW resources. Therefore, we can achieve multi-tenancy using OpenShift Container Platform RBAC and Role Binding.

## Login and Create Porject
First, go to the web console and click on your username at the top left then 'Copy Login Command', then display the token and copy the ```oc login``` command in your terminal.<br>
![login](https://user-images.githubusercontent.com/36239840/97104809-26821500-16d0-11eb-936e-c2b7fb914523.JPG)
<br>Next, we will create two projects for the two users we will be creating in the next step, copy the following commands in your terminal.<br>
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


## Create Role Bindings
In this step, we will create role bindings to give users access to their respective projects. First, switch back to Admin user if you are still impersonating click on 'Stop Impersonating' that's shown at the top in the Impersonating User message.<br>
![stop impersonating](https://user-images.githubusercontent.com/36239840/97105732-ad39f080-16d6-11eb-8751-dea4a7155e99.JPG)
We can easily create Role Bindings through the web console. First, go to  <b>User Management &#8594; Role Bindings</b> then click 'Create Binding', it will redirect you to 'Create Role Binding' page with a form that looks like in the following picture. Fill in the form the following information:<br><br>
<b>Binding Type:</b> Namespace Role Binding (RoleBinding)<br>
<b>Name:</b> first-user-rb<br>
<b>Namespace:</b> my-first-project<br>
<b>Role Name:</b> cluster-admin<br>
<b>Subject:</b> User<br>
<b>Subject Name:</b> first-user<br>
![first user rb](https://user-images.githubusercontent.com/36239840/97106206-a5c81680-16d9-11eb-8de0-e359dddfb825.JPG)
<br>Go to <b>User Management &#8594; Users</b> then click on 'first-user' and go to Role Bindings. You will find the role 'cluster-admin' has been successfully associated with 'first-user' through 'first-user-rb' RoleBinding for the namepspace 'my-first-project'.
![first user details](https://user-images.githubusercontent.com/36239840/97106556-df9a1c80-16db-11eb-9a80-ce1a55edc7d1.JPG)
## Impersonate User & Deploy Application
In this step, we will impersonate first-user like we did previously, but this time, the user will have admin permisisons to my-first-project, which means they can do everything an admin can do on that namespace. Go to <b>User Management &#8594; Users</b> and click 'Impersonate User "first-user"'. You will notice the new changes for the user, and now the user can view the 'my-first-project' namespace as shown in the screenshot below. It is the only project this user can view because we limited first-user to my-first-project namespace<br>
![first user project 1](https://user-images.githubusercontent.com/36239840/97106921-d6aa4a80-16dd-11eb-92d9-f80e4386d396.JPG)
<br>If you go to <b>Workloads &#8594; Pods</b> you will see that first-user has access to the pods as well, but no pods have been created yet.<br>
![pods first user 1](https://user-images.githubusercontent.com/36239840/97107127-4a008c00-16df-11eb-8a51-c0184c2ba031.JPG)

## Create & Deploy Pod
In this step, we will create a pod as 'first-user' in 'my-first-project' namespace. First, switch to the Developer Perspective on the web console and from the topology create a new application from 'Container Image' as shown in the screenshot.<br>
![topology](https://user-images.githubusercontent.com/36239840/97107189-b11e4080-16df-11eb-934d-8002722d31e0.JPG)
You will be redirected to a new page, search in the external registry for ```ibmcom/guestbook:v1``` image, keep the default settings as is and click 'Create'
![ibmimagedeploy](https://user-images.githubusercontent.com/36239840/97107249-00647100-16e0-11eb-9fea-7a1fc2cee04d.JPG)
<br>Once created, you will be redirected to the topology view which will show the pod as the following image. Notice that the light blue circle around the pod will turn dark blue once it successfully builds. As 'first-user' you can successfully deploy a containerized application (guestbook:v1) in Project 'my-first-project' and can view the Pods & Deployment resources associated with the application.<br>
![pod topology](https://user-images.githubusercontent.com/36239840/97107327-618c4480-16e0-11eb-9631-b556382fa513.JPG)
<br>You can access the deployed application by clicking on the small square at the top corner of the pod, which will open a new page for you that looks like the following.<br>
![guest book url](https://user-images.githubusercontent.com/36239840/97107463-263e4580-16e1-11eb-9091-7e6793bac6f8.JPG)
<br>If you switch back to the Administrator perspective, you will notice that the new pod has been added to the list of pods for the project 'my-first-project'.<br>
![pods added](https://user-images.githubusercontent.com/36239840/97107632-fe031680-16e1-11eb-87ab-63f00b50de1c.JPG)

## Switching to Another User
Now let's stop the impersonation for User first-user, and switch to User second-user and verify if we can see the Pods User first-user created.
As expected, we cannot see Project my-first-project anymore as User second-user is not authorized to view Project my-first-project.
![second user pods](https://user-images.githubusercontent.com/36239840/97107793-d496ba80-16e2-11eb-8ca8-71bf673a7b8d.JPG)

## Summary
This marks the end of the tutorial in which you have demonstrated how multi-tenancy can be achieved using OpenShift Role Based Access Control and limiting Users to their specific Projects (namespaces).
