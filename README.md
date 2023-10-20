# B-DOP-500-LIL-5-1-bernstein-leo.neumuller
## How to use:
### Setup:
Set the postgres username and password in base64:

```
❯ echo -n "username" | base64
dXNlcm5hbWU=
```

```
❯ echo -n "password" | base64
cGFzc3dvcmQ=
```

Change the user and password in the postgres.secret.yaml file:

```
apiVersion: v1
kind: Secret
metadata:  
  name: postgres-secret
  namespace: default
data:
  username: "dXNlcm5hbWU="
  password: "cGFzc3dvcmQ="
```

### Run the app:

Apply the config files to your cluster
```
kubectl apply -f .
```
Create a table on the postgres database

```
echo "CREATE TABLE votes (id text PRIMARY KEY, vote text NOT NULL);" | kubectl exec -i deployment/postgres-deployment -- psql -U YOUR_USERNAME
```

Add fake dns to your etc/host file to be able to access the poll and result endpoint:

```
echo """$(kubectl get nodes -o jsonpath='{ $.items[*].status.addresses[?(@.type=="ExternalIP")].address }') poll.dop.io result.dop.io""" | sudo tee -a /etc/hosts
```

You can now access poll.dop.io:30021 and result.dop.io:30021 on your browser
