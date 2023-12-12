## Розширення kubectl (Extending kubectl)

Create the given script from the task:

1. Let's add checking of necessary parameters in the begining of the script:
```sh
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <node or pod> <namespace>"
    exit 1
fi
```

2. Let's define aarguments to use them than:
```sh
# Define command-line arguments
RESOURCE_TYPE=$1
NAMESPACE=$2
```

3. Make changes into script body, add the line to output data:
```sh
# Retrieve resource usage statistics from Kubernetes
kubectl top $RESOURCE_TYPE -n $NAMESPACE| tail -n +2 | while read line
do
  # Extract CPU and memory usage from the output
  NAME=$(echo $line | awk '{print $1}')
  CPU=$(echo $line | awk '{print $2}')
  MEMORY=$(echo $line | awk '{print $3}')

  # Output the statistics to the console
  echo "$RESOURCE_TYPE, $NAMESPACE, $NAME, $CPU, $MEMORY" 
done
```

Create a script's folder and a file kubeplugin and than add the script into a system's folder in accordance to [instruction](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/), give the right permission to execusion:
```sh
$ mkdir scripts

$ cd scripts

$ cat > kubeplugin
#!/bin/bash

# Checking of getting both entering arguments
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <node or pod> <namespace>"
    exit 1
fi

# Define command-line arguments
RESOURCE_TYPE=$1
NAMESPACE=$2

# Retrieve resource usage statistics from Kubernetes
kubectl top $RESOURCE_TYPE -n $NAMESPACE| tail -n +2 | while read line
do
  # Extract CPU and memory usage from the output
  NAME=$(echo $line | awk '{print $1}')
  CPU=$(echo $line | awk '{print $2}')
  MEMORY=$(echo $line | awk '{print $3}')

  # Output the statistics to the console
  echo "$RESOURCE_TYPE, $NAMESPACE, $NAME, $CPU, $MEMORY" 
done
^d

$ chmod 755 kubeplugin

$ ls -al kubeplugin
drwxrwxrwx+ 2 codespace codespace 4096 Dec 12 10:45 .
drwxrwxrwx+ 6 codespace root      4096 Dec 12 10:45 ..  
-rwxr-xr-x  1 codespace codespace  472 Dec 12 10:45 kubeplugin

$ sudo cp ./kubeplugin /usr/local/bin/kubectl-kubeplugin
```

Check the plugins list
```sh
$ kubectl plugin list
The following compatible plugins are available:
/root/.krew/bin/kubectl-krew
/root/.krew/bin/kubectl-tail
/usr/local/bin/kubectl-kubeplugin
```
             
Check that the built command working:
```sh
$ kubectl top pod -n kube-system
NAME                                     CPU(cores)   MEMORY(bytes)   
coredns-77ccd57875-jxzdp                 2m           12Mi            
local-path-provisioner-957fdf8bc-g8nn7   1m           8Mi             
metrics-server-648b5df564-d9xm8          4m           19Mi            
svclb-traefik-7b0720ec-jqrkn             0m           0Mi             
svclb-traefik-7b0720ec-pccpf             0m           0Mi             
svclb-traefik-7b0720ec-spw46             0m           0Mi             
svclb-traefik-7b0720ec-w2czk             0m           0Mi             
traefik-64f55bb67d-q5xwq                 1m           30Mi   
```

And than use the plugin:
```sh
$ kubectl kubeplugin pod kube-system
pod, kube-system, coredns-77ccd57875-jxzdp, 2m, 12Mi
pod, kube-system, local-path-provisioner-957fdf8bc-g8nn7, 1m, 9Mi
pod, kube-system, metrics-server-648b5df564-d9xm8, 4m, 16Mi
pod, kube-system, svclb-traefik-7b0720ec-jqrkn, 0m, 2Mi
pod, kube-system, svclb-traefik-7b0720ec-pccpf, 0m, 0Mi
pod, kube-system, svclb-traefik-7b0720ec-spw46, 0m, 2Mi
pod, kube-system, svclb-traefik-7b0720ec-w2czk, 0m, 2Mi
pod, kube-system, traefik-64f55bb67d-q5xwq, 1m, 31Mi

$ kubectl kubeplugin node kube-system
node, kube-system, k3d-k3d-cluster-898-agent-0, 54m, 2%
node, kube-system, k3d-k3d-cluster-898-agent-1, 40m, 2%
node, kube-system, k3d-k3d-cluster-898-agent-2, 41m, 2%
node, kube-system, k3d-k3d-cluster-898-server-0, 33m, 1%
```