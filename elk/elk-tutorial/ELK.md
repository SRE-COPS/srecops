
#  ELK Stack

The ELK Stack began as a collection of three open-source products — Elasticsearch, Logstash, and Kibana — all developed, managed and maintained by Elastic. The introduction and subsequent addition of Beats turned the stack into a four legged project.

Elasticsearch is a full-text search and analysis engine, based on the Apache Lucene open source search engine. 

Logstash is a log aggregator that collects data from various input sources, executes different transformations and enhancements and then ships the data to various supported output destinations. It’s important to know that many modern implementations of ELK do not include Logstash. To replace its log processing capabilities, most turn to lightweight alternatives like Fluentd, which can also collect logs from data sources and forward them to Elasticsearch. 

Kibana is a visualization layer that works on top of Elasticsearch, providing users with the ability to analyze and visualize the data. And last but not least — Beats are lightweight agents that are installed on edge hosts to collect different types of data for forwarding into the stack.

Together, these different components are most commonly used for monitoring, troubleshooting and securing IT environments (though there are many more use cases for the ELK Stack such as business intelligence and web analytics). Beats and (formerly) Logstash take care of data collection and processing, Elasticsearch indexes and stores the data, and Kibana provides a user interface for querying the data and visualizing it.



## Setup ELK Stack on K8 cluster.
* First Install elk CRD
    ```bash
    kubectl create -f https://download.elastic.co/downloads/eck/2.3.0/crds.yaml
    kubectl get crd
    ```
* Install the operator with its RBAC rules. this will create namespace called elastic-system and install elastic operator on it.
    ```bash
    kubectl apply -f https://download.elastic.co/downloads/eck/2.3.0/operator.yaml
    kubectl get all -n elastic-system
    ```

* Deploy Elasticsearch Cluster with kibana.
    ```bash
    kubectl create -f https://github.com/SRE-COPS/srecops/blob/main/elk/es-dploymentva.yaml

    kubectl create -f  https://github.com/SRE-COPS/srecops/blob/main/elk/kibana.yaml

    kubectl get pods
    kubectl get svc
    ```


## Setup Filebeat
* LaMa log file to review
    ```bash
    /usr/sap/NRL/J00/j2ee/cluster/server0/log/*.log
    /usr/sap/NRL/J00/j2ee/cluster/server0/log/*.trc
    /usr/sap/hostctrl/work/sapstartsrv.log

    ```
* Every node has filebeat install to ingest logs on directpry - /tmp/elkmedia/
    * Configuration file.
    ```bash
    vi /etc/filebeat/filebeat.yml
    ```
    filebeat -e
    

## Setup Logstash
* Logstash is setup on server : log-702
    ```bash
    /usr/share/logstash/bin/logstash -f /etc/logstash/logstash-3-test.conf - -->> for screen output
    /usr/share/logstash/bin/logstash -f /etc/logstash/logstash-3.conf --> to redirect to the Elastic
    ```



## Demo Environment:

* Port forward Elasticsearch and Kibana on the master node.
   ```bash
    log-700
    su - rke
    kubectl port-forward service/my-elasticsearch-es-http 9200:9200 --address='0.0.0.0' &
        kubectl port-forward service/my-elasticsearch-kb-http 5601:5601 --address=0.0.0.0 &
    ```
* Get secret for kibana.
    ```bash
    kubectl get secret my-elasticsearch-es-elastic-user -o=jsonpath='{.data.elastic}' | base64 --decode
    ```
* Launch kibana.
    ```bash
    https://lvmql700:5601/

    username: elastic
    ```

* get the logs from LaMa system.
* Generate Dashboard in ELK.



### Error log information
    * Errors 
    ```bash
    #com.sap.nw.lm.aci.monitor.api.validation.RuntimeValidator
    #BC-VCM-LVM

    Command returned: Internal Error [OperationOSC

    #2.0#2024 01 31 14:12:05:243#+0100#Error#com.sap.tc.vcm.engine.operation.handler.util.HostControlHandlerUtility#
#BC-VCM-LVM#sap.com/tc~vcm~engine~app#C0000A60A106E4520000000100004F18#1717950201409470#sap.com/tc~vcm~monitor~app#com.sap.tc.vcm.engine.operation.handler.util.HostControlHandlerUtility.executeHostAgentOperation(SAPHostControlInterface, HostAgentOperation<T>, OperationContext, boolean)#Administrator#0##C670C53CB95E11EEB62CF7740A60A106#c670c53cb95e11eeb62cf7740a60a106##0#LaMa_VAL_RuCuVa_P82.vhhilp82db.fra3.hec.hilti.com#Plain##

    ```
    * To fetch all errors :
        - event.original.keyword : *Error*
        - message : *HostControlHandlerUtility*


    ```bash

    ```

    Dashboard that contain information:
        - HostControlHandlerUtility 
         - message : *HostControlHandlerUtility*

        - Authentication errors: 
            - Could not find lvmadm_local

        - monitor.api.validation.RuntimeValidator
        - manager.CsDataFetcher
     








