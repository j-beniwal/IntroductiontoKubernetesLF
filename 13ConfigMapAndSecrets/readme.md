# Chapter 13. ConfigMaps and Secrets #

## Introduction and Learning Objectives ##

### Introduction ###

While deploying an application, we may need to pass such runtime parameters like configuration details, permissions, passwords, keys, certificates, or tokens.

Let's assume we need to deploy ten different applications for our customers, and for each customer, we need to display the name of the company in the UI. Then, instead of creating ten different Docker images for each customer, we may just use the same template image and pass customers' names as runtime parameters. In such cases, we can use the ConfigMap API resource.

Let's assume we need to deploy ten different applications for our customers, and for each customer, we need to display the name of the company in the UI. Then, instead of creating ten different Docker images for each customer, we may just use the same template image and pass customers' names as runtime parameters. In such cases, we can use the ConfigMap API resource.

Similarly, when we want to pass sensitive information, we can use the Secret API resource. In this chapter, we will explore ConfigMaps and Secrets. 

### Learning Objectives ###

By the end of this chapter, you should be able to:

* Discuss configuration management for applications in Kubernetes using ConfigMaps.
* Share sensitive data (such as passwords) using Secrets.

## ConfigMaps and Secrets ##

### ConfigMaps ###

[ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/) allow us to decouple the configuration details from the container image. Using ConfigMaps, we pass configuration data as key-value pairs, which are consumed by Pods or any other system components and controllers, in the form of environment variables, sets of commands and arguments, or volumes. We can create ConfigMaps from literal values, from configuration files, from one or more files or directories.

### Create a ConfigMap from Literal Values and Display Its Details ###

A ConfigMap can be created with the kubectl create command, and we can display its details using the kubectl get command.

Create the ConfigMap

    $ kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2

Display the ConfigMap Details for my-config

    $ kubectl get configmaps my-config -o yaml

With the -o yaml option, we are requesting the kubectl command to produce the output in the YAML format. As we can see, the object has the ConfigMap kind, and it has the key-value pairs inside the data field. The name of ConfigMap and other details are part of the metadata field. 

### Create a ConfigMap from a Configuration File ###

First, we need to create a configuration file with the following content:

    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: customer1
    data:
      TEXT1: Customer1_Company
      TEXT2: Welcomes You
      COMPANY: Customer1 Company Technology Pct. Ltd.

where we specify the kind, metadata, and data fields, targeting the v1 endpoint of the API server.

If we name the file with the configuration above as customer1-configmap.yaml, we can then create the ConfigMap with the following command:

    $ kubectl create -f customer1-configmap.yaml

### Create a ConfigMap from a File ###

First, we need to create a file permission-reset.properties with the following configuration data:

    permission=read-only
    allowed="true"
    resetCount=3

We can then create the ConfigMap with the following command:

    $ kubectl create configmap permission-config --from-file=<path/to/>permission-reset.properties

### Use ConfigMaps Inside Pods ###

As Environment Variables

Inside a Container, we can retrieve the key-value data of an entire ConfigMap or the values of specific ConfigMap keys as environment variables.

In the following example all the myapp-full-container Container's environment variables receive the values of the full-config-map ConfigMap keys:

    ...
      containers:
      - name: myapp-full-container
        image: myapp
        envFrom:
        - configMapRef:
          name: full-config-map
    ...

In the following example the myapp-specific-container Container's environment variables receive their values from specific key-value pairs from two separate ConfigMaps, config-map-1 and config-map-2:

    ...
      containers:
      - name: myapp-specific-container
        image: myapp
        env:
        - name: SPECIFIC_ENV_VAR1
          valueFrom:
            configMapKeyRef:
              name: config-map-1
              key: SPECIFIC_DATA
        - name: SPECIFIC_ENV_VAR2
          valueFrom:
            configMapKeyRef:
              name: config-map-2
              key: SPECIFIC_INFO
    ...

With the above, we will get the SPECIFIC_ENV_VAR1 environment variable set to the value of SPECIFIC_DATA key from config-map-1 ConfigMap, and SPECIFIC_ENV_VAR2 environment variable set to the value of SPECIFIC_INFO key from config-map-2 ConfigMap.

As Volumes

We can mount a vol-config-map ConfigMap as a Volume inside a Pod. For each key in the ConfigMap, a file gets created in the mount path (where the file is named with the key name) and the content of that file becomes the respective key's value:

    ...
      containers:
      - name: myapp-vol-container
        image: myapp
        volumeMounts:
        - name: config-volume
          mountPath: /etc/config
      volumes:
      - name: config-volume
        configMap:
          name: vol-config-map
    ...

For more details, please explore the documentation on using [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/). 

### Secrets ###

Let's assume that we have a Wordpress blog application, in which our wordpress frontend connects to the MySQL database backend using a password. While creating the Deployment for wordpress, we can include the MySQL password in the Deployment's YAML file, but the password would not be protected. The password would be available to anyone who has access to the configuration file.

In this scenario, the [Secret](https://kubernetes.io/docs/concepts/configuration/secret/) object can help by allowing us to encode the sensitive information before sharing it. With Secrets, we can share sensitive information like passwords, tokens, or keys in the form of key-value pairs, similar to ConfigMaps; thus, we can control how the information in a Secret is used, reducing the risk for accidental exposures. In Deployments or other resources, the Secret object is referenced, without exposing its content.

It is important to keep in mind that by default, the Secret data is stored as plain text inside etcd, therefore administrators must limit access to the API server and etcd. However, Secret data can be encrypted at rest while it is stored in etcd, but this feature needs to be enabled at the API server level. 

### Create a Secret from Literal and Display Its Details ###

To create a Secret, we can use the kubectl create secret command:

    $ kubectl create secret generic my-password --from-literal=password=mysqlpassword

The above command would create a secret called my-password, which has the value of the password key set to mysqlpassword.

After successfully creating a secret we can analyze it with the get and describe commands. They do not reveal the content of the Secret. The type is listed as Opaque.

    $ kubectl get secret my-password

    $ kubectl describe secret my-password

### Create a Secret Manually ###

We can create a Secret manually from a YAML configuration file. The example file below is named mypass.yaml. There are two types of maps for sensitive information inside a Secret: data and stringData.

With data maps, each value of a sensitive information field must be encoded using base64. If we want to have a configuration file for our Secret, we must first create the base64 encoding for our password:

    $ echo mysqlpassword | base64

and then use it in the configuration file:

    apiVersion: v1
    kind: Secret
    metadata:
      name: my-password
    type: Opaque
    data:
      password: bXlzcWxwYXNzd29yZAo=

Please note that base64 encoding does not mean encryption, and anyone can easily decode our encoded data:

    $ echo "bXlzcWxwYXNzd29yZAo=" | base64 --decode

Therefore, make sure you do not commit a Secret's configuration file in the source code.

With stringData maps, there is no need to encode the value of each sensitive information field. The value of the sensitive field will be encoded when the my-password Secret is created: 

    apiVersion: v1
    kind: Secret
    metadata:
      name: my-password
    type: Opaque
    stringData:
      password: mysqlpassword

Using the mypass.yaml configuration file we can now create a secret with kubectl create command: 

    $ kubectl create -f mypass.yaml

### Create a Secret from a File and Display Its Details ###


To create a Secret from a File, we can use the kubectl create secret command. 

First, we encode the sensitive data and then we write the encoded data to a text file:

    $ echo mysqlpassword | base64
    $ echo -n 'bXlzcWxwYXNzd29yZAo=' > password.txt

Now we can create the Secret from the password.txt file:

    $ kubectl create secret generic my-file-password --from-file=password.txt

After successfully creating a secret we can analyze it with the get and describe commands. They do not reveal the content of the Secret. The type is listed as Opaque.

    $ kubectl get secret my-file-password
    $ kubectl describe secret my-file-password

### Use Secrets Inside Pods ###

Secrets are consumed by Containers in Pods as mounted data volumes, or as environment variables, and are referenced in their entirety or specific key-values.

Using Secrets as Environment Variables

Below we reference only the password key of the my-password Secret and assign its value to the WORDPRESS_DB_PASSWORD environment variable:

    ....
    spec:
      containers:
      - image: wordpress:4.7.3-apache
        name: wordpress
        env:
        - name: WORDPRESS_DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-password
              key: password
    ....

Using Secrets as Files from a Pod
We can also mount a Secret as a Volume inside a Pod. The following example creates a file for each my-password Secret key (where the files are named after the names of the keys), the files containing the values of the Secret:

    ....
    spec:
      containers:
      - image: wordpress:4.7.3-apache
        name: wordpress
        volumeMounts:
        - name: secret-volume
          mountPath: "/etc/secret-data"
          readOnly: true
      volumes:
      - name: secret-volume
        secret:
          secretName: my-password
    ....

For more details, you can explore the documentation on managing [Secrets](https://kubernetes.io/docs/tasks/configmap-secret/).

### Learning Objectives (Review) ###

You should now be able to:

* Discuss configuration management for applications in Kubernetes using ConfigMaps.
* Share sensitive data (such as passwords) using Secrets.
