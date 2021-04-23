Thank you for installing voter, this is a pressure testing application for VoltDB. It be designed to run with VoltDB on K8S.
It means frist you need deploy VoltDB on K8S, you can look over this for more detail
https://docs.voltdb.com/KubernetesAdmin/
And then you can start up the application by enter into the pods and execute the command:
/opt/voter/run.sh init   ##Create database object such as table, view, procedure which be required by voter.
/opt/voter/run.sh client  ##Run the application to make testing. 

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

Your release is named {{ $.Release.Name }}.

To learn more about the release, run:

  $ helm status {{ $.Release.Name }}
  $ helm get manifest {{ $.Release.Name }}

