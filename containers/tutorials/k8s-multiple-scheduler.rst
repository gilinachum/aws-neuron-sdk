
.. _k8s-multiple-scheduler:

In cluster environments where there is no access to default scheduler, the neuron scheduler extension can be used with another scheduler.  A new scheduler is added (along with the default scheduler) and then the pod's that needs to run the neuron workload
use this new scheduler. Neuron scheduler extension is added to this new scheduler. EKS natively does not yet support the neuron scheduler extension and so in the EKS environment this is the only way to add the neuron scheduler extension.

* Make sure :ref:`Neuron device plugin<k8s-neuron-device-plugin>` is running
* Download the my scheduler :download:`my-scheduler.yml </src/k8/my-scheduler.yml>`
* Download the scheduler extension :download:`k8s-neuron-scheduler-eks.yml </src/k8/k8s-neuron-scheduler-eks.yml>`
* Apply the neuron-scheduler-extension

    ::

        kubectl apply -f k8s-neuron-scheduler-eks.yml


* Get the neuron scheduler extension cluster ip

    ::

        kubectl get service --namespace kube-system k8s-neuron-scheduler -o jsonpath='{.spec.clusterIP}'

* Modify the urlPrefix in my-scheduler.yml. Replace 127.0.0.1 with cluster ip above.

    ::

        "urlPrefix": "http://127.0.0.1:12345 to "urlPrefix": "http://<cluster-ip-from-above>:12345

* Apply the modified my-scheduler.yml

    ::

        kubectl apply -f my-scheduler.yml

* Check there are no errors in the my-scheduler pod logs and the k8s-neuron-scheduler pod is bound to a node

    ::

        kubectl logs -n kube-system my-scheduler-79bd4cb788-hq2sq

    ::

        I1012 15:30:21.629611       1 scheduler.go:604] "Successfully bound pod to node" pod="kube-system/k8s-neuron-scheduler-5d9d9d7988-xcpqm" node="ip-192-168-2-25.ec2.internal" evaluatedNodes=1 feasibleNodes=1


* When running new pod's that need to use the neuron scheduler extension, make sure it uses the my-scheduler as the scheduler. Sample pod spec is below

    ::

        apiVersion: v1
        kind: Pod
        metadata:
        name: <POD_NAME>
        spec:
        restartPolicy: Never
        schedulerName: my-scheduler
        containers:
            - name: <POD_NAME>
            command: ["<COMMAND>"]
            image: <IMAGE_NAME>
            resources:
                limits: 
                cpu: "4"
                memory: 4Gi
                aws.amazon.com/neuroncore: 9
                requests:
                cpu: "1"
                memory: 1Gi

* Once the neuron workload pod is run, make sure logs in the k8s neuron scheduler has successfull filter/bind request


    ::

        kubectl logs -n kube-system k8s-neuron-scheduler-5d9d9d7988-xcpqm


    ::

        2022/10/12 15:41:16 POD nrt-test-5038 fits in Node:ip-192-168-2-25.ec2.internal
        2022/10/12 15:41:16 Filtered nodes: [ip-192-168-2-25.ec2.internal]
        2022/10/12 15:41:16 Failed nodes: map[]
        2022/10/12 15:41:16 Finished Processing Filter Request...

    ::

        2022/10/12 15:41:16 Executing Bind Request!
        2022/10/12 15:41:16 Determine if the pod %v is NeuronDevice podnrt-test-5038
        2022/10/12 15:41:16 Updating POD Annotation with alloc devices!
        2022/10/12 15:41:16 Return aws.amazon.com/neuroncore
        2022/10/12 15:41:16 neuronDevUsageMap for resource:aws.amazon.com/neuroncore in node: ip-192-168-2-25.ec2.internal is [false false false false false false false false false false false false false false false false]
        2022/10/12 15:41:16 Allocated ids for POD nrt-test-5038 are: 0,1,2,3,4,5,6,7,8
        2022/10/12 15:41:16 Try to bind pod nrt-test-5038 in default namespace to node ip-192-168-2-25.ec2.internal with &Binding{ObjectMeta:{nrt-test-5038    8da590b1-30bc-4335-b7e7-fe574f4f5538  0 0001-01-01 00:00:00 +0000 UTC <nil> <nil> map[] map[] [] []  []},Target:ObjectReference{Kind:Node,Namespace:,Name:ip-192-168-2-25.ec2.internal,UID:,APIVersion:,ResourceVersion:,FieldPath:,},}
        2022/10/12 15:41:16 Updating the DevUsageMap since the bind is successful!
        2022/10/12 15:41:16 Return aws.amazon.com/neuroncore
        2022/10/12 15:41:16 neuronDevUsageMap for resource:aws.amazon.com/neuroncore in node: ip-192-168-2-25.ec2.internal is [false false false false false false false false false false false false false false false false]
        2022/10/12 15:41:16 neuronDevUsageMap for resource:aws.amazon.com/neurondevice in node: ip-192-168-2-25.ec2.internal is [false false false false]
        2022/10/12 15:41:16 Allocated devices list 0,1,2,3,4,5,6,7,8 for resource aws.amazon.com/neuroncore
        2022/10/12 15:41:16 Allocated devices list [0] for other resource aws.amazon.com/neurondevice
        2022/10/12 15:41:16 Allocated devices list [0] for other resource aws.amazon.com/neurondevice
        2022/10/12 15:41:16 Allocated devices list [0] for other resource aws.amazon.com/neurondevice
        2022/10/12 15:41:16 Allocated devices list [0] for other resource aws.amazon.com/neurondevice
        2022/10/12 15:41:16 Allocated devices list [1] for other resource aws.amazon.com/neurondevice
        2022/10/12 15:41:16 Allocated devices list [1] for other resource aws.amazon.com/neurondevice
        2022/10/12 15:41:16 Allocated devices list [1] for other resource aws.amazon.com/neurondevice
        2022/10/12 15:41:16 Allocated devices list [1] for other resource aws.amazon.com/neurondevice
        2022/10/12 15:41:16 Allocated devices list [2] for other resource aws.amazon.com/neurondevice
        2022/10/12 15:41:16 Return aws.amazon.com/neuroncore
        2022/10/12 15:41:16 Succesfully updated the DevUsageMap [true true true true true true true true true false false false false false false false]  and otherDevUsageMap [true true true false] after alloc for node ip-192-168-2-25.ec2.internal
        2022/10/12 15:41:16 Finished executing Bind Request...



