## Description

### Secrets
When creating a new connection for Airflow, you should use Kubernetes secrets & mount it to the containers for Airflow. Below are the steps.


#### Creating the K8S Secret

* Pre-requisites: 
  1. You must have access to the public certificate which was bootstrapped to the sealed-secrets service on the cluster.
  2. You must have `kubseal` installed
  3. You need a secret file similar to the one below. Note, the connection must be in standard URI format expected by Airflow.

`mysecret.yaml`
```
apiVersion: v1
kind: Secret
metadata:
  name: my-cool-airflow-connection
  namespace: airflow
type: Opaque
stringData:
   AIRFLOW_CONN_MY_COOL_AIRFLOW_CONNECTION: https://login:password@hello:port/world
```

Once you have this file created, seal the secret by running:

```
export PUBLICKEY="/path/to/cert.crt"
kubeseal --cert "${PUBLICKEY}" --scope cluster-wide < mysecret.yaml -o yaml >> my_sealed_secret.yaml
```

This creates a file called `my_sealed_secret.yaml` which contains the encrypted secret.

### Storing the secret

1. Navigate to your team's github repo & add the secret under `sealed-secrets/airflow`.
2. Navigate to the `argo-master-apps/airflow/application.yaml` file in the repo, and add your secret to the `values` section in the:

```
airflow:
  secret:
    - envName: "AIRFLOW_CONN_MY_COOL_AIRFLOW_CONNECTION" #This has to be prefixed with `AIRFLOW_CONN_` for Airflow to find the secret. The name of the connection will be `my_cool_airflow_connection` in Airflow.
      secretName: "my-cool-airflow-connection" # Name of the K8S secret.
      secretKey: "AIRFLOW_CONN_MY_COOL_AIRFLOW_CONNECTION" # This is the secret key from the `stringData` section above.
```
* See: https://airflow.apache.org/docs/apache-airflow/stable/howto/connection.html#storing-connections-in-environment-variables for details on environment variables & connections.


#### NOTE: If you are adding new secrets to git, they will not appear locally until you restart your Scheduler pod!