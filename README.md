# voter-chart
This is a pressure testing application for VoltDB. It be designed to run with VoltDB on K8S.

It means frist you need deploy VoltDB on K8S, you can look over this for more detail
https://docs.voltdb.com/KubernetesAdmin/
And then you can start up the application by enter into the pods and execute the command:

run.sh init   ##Create database object such as table, view, procedure which be required by voter.

run.sh client  ##Run the application to make testing.

You can get more detail about voter by access the web pages.
https://github.com/VoltDB/voltdb/tree/master/examples/voter

Notes:
You can control various characteristics of voter by modifying the parameters
passed into the java application in the "client" function of the run.sh script.
But now you deploy this application by helm, so you don't have to modifying the run.sh script,
you can passed the parameters by helm command, like this

helm upgrade voter presstesting/ --reuse-values --set voter.duration=20 \
                    --set voter.displayinterval=5  --set voter.warmup=5  \
                    --set voter.ratelimit=2000  --set voter.contestants=4 \
                    --set voter.maxvotes=3

And all the default values of the parameters was given by values.ymal in the chart package.
It works within helm rules, you can provide a customized file to replace.

In addtion, there are two parameters be used to connect to VoltDB service by K8S coreDNS.

voltdb.release  ##It should be set the voltdb release name, default is "mydb". You'd better set it to "voltdb".

voltdb.namespace  ##The voltdb cluster's namespace on K8S, default is "default".

Add the Helm repo if you do not already have it:

helm repo add voter https://will20150909.github.io/voter-chart

To install the chart:

helm install voter voter/voltdb-voter

To run the voter:

1.enter into the pods, the pod's name begin with prefix "{helm_release_name}-voltdb-voter".

kubectl exec -it voter-voltdb-voter-xxxxxx -- bash

2.run the shell script

cd /opt/voter

./run.sh init

./run.sh client
