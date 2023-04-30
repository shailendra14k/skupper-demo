# Deploymnet files for the Skupper demo 
Blog:- https://medium.com/@shailendra14k/deploy-the-skupper-networks-89800323925c

#Frontend and backend app
Its a simple camel route, frontend app prints the log message received from the backed app. backed app send the message with cluster name and pod details.

Frontend Route:- image quay.io/shailendra14k/frontend:6.1

```
  <route id="api2">
     <from id="api2" uri="undertow:http://0.0.0.0:8180/frontend"/>
     <toD id="to1" uri="http4://{{env:backendapp}}?bridgeEndpoint=true"/>
     <log id="log2" message="${body}"/>
  </route>
```

Backend Route:- image quay.io/shailendra14k/backend:6.1

```
  <route id="api2">
     <from id="api2" uri="undertow:http://0.0.0.0:8180/backend"/>
     <setBody id="_setBody2">
        <constant>"Backend called from Cluster:- {{env:ClusterName}} and POD hostName {{env:HOSTNAME}}"</constant>
     </setBody>
     <log id="log2" message=">>> hello backend"/>
  </route>
```

Sample Logs message received in the frontend pod
~~~
13:53:29.784 [XNIO-2 task-3] INFO  api2 - "Backend called from Cluster:- ARO-Azure-SouthIndia and POD hostName backend-544d78859c-zkf87"
13:53:29.787 [XNIO-2 task-16] INFO  api2 - "Backend called from Cluster:- ARO-Azure-SouthIndia and POD hostName backend-544d78859c-zkf87"
13:53:29.979 [XNIO-2 task-6] INFO  api2 - "Backend called from Cluster:- ARO-Azure-CentralIndia and POD hostName backend-85f4fd745c-xgtfg"
13:53:29.981 [XNIO-2 task-14] INFO  api2 - "Backend called from Cluster:- ARO-Azure-CentralIndia and POD hostName backend-85f4fd745c-vgbvr"
13:53:29.989 [XNIO-2 task-8] INFO  api2 - "Backend called from Cluster:- ARO-Azure-SouthIndia and POD hostName backend-544d78859c-zkf87"
~~~
