## Openshift AB DEPLOYMENT demonstration with a simple PHP Application
![](abdepoy.PNG)




In this demo, we will check how to minimize the downtime of a Application while updating or running two version of the Application.   
create a common `service` and a common `route` for the pods, expose the pods by using common `service` & `route`

AB Deployment
`oc new-project abdeployment`

Create php app with out route and add  label  

`abgroupmember=true`

`app name = app-a`

`git url : https://github.com/venatrix/ab-demo.git`

`oc get dc`

`oc edit dc/app-a`

go to spec: > selector: > deploymentconfig:

add under `app-a`

`abgroupmember: "true"`


add new service

`oc expose dc/app-a --name=ab-service --selector=abgroupmember=true --generator=service/v1`

create route the service `ab-service`

`oc expose service ab-service --name=ab-route --hostname=<your url>`

scale replica
`oc scale dc/appp-a --replica=4`

To check the trafic
`while true;do curl <URL>; echo " "; sleep 1; done; `

edit index.ph from version 1 to 2

Create new php app with out route and add label abgroupmember=true

`app name = app-b`

Add Label `abgroupmember: "true"`

git url :` https://github.com/venatrix/ab-demo.git`

check with curl `while true;do curl <URL>; echo " "; sleep 1; done;`


