# Consideration of the best k8s solution for AsciiArtify

## Task
AsciiArtify is a startup created by two middle software developers. They want to consider what the k8s product use for theyre local development of the COncept.

## Short description
### Minikube
`miniKube` is the most widely used local Kubernetes installer. It offers an easy to use guide on installing and running single Kubernetes environments across multiple operating systems. It deploys Kubernetes as a container, VM or bare-metal and implements a Docker API endpoint that helps it push container images faster. It has advanced features like load balancing, filesystem mounts, and FeatureGates, making it a favourite for running Kubernetes locally. 

### Kind (Kubernetes IN Docker)
Primarily designed to test Kubernetes, `Kind` (Kubernetes in Docker) helps you run Kubernetes clusters locally and in CI pipelines using Docker containers as "nodes".

It is an open source CNCF certified Kubernetes installer that supports highly available multi-node clusters and builds Kubernetes release builds from its source.

### k3d
`K3d` is a platform-agnostic, lightweight wrapper that runs `K3s` (K3s is a lightweight tool designed to run production-level Kubernetes workloads for low resourced and remotely located IoT and Edge devices.K3s helps you run a simple, secure and optimised Kubernetes environment on a local computer using a virtual machine such as VMWare or VirtualBox.) in a docker container. It helps run and scale single or multi-node K3S clusters quickly without further setup while maintaining a high availability mode.

### Characteristics

| **Solutions**   | **minikube**    | **kind**     | **k3d**      |
|--------------------------------------------------|--------------------------------------------------|--------------------------------------------------|--------------------------------------------------|
| runtime | VM | container | native |
|  suported<br>architectures | AMD64 | AMD64 | AMD64, ARMv7, ARM64 |
|  supported container runtimes| Docker,CRI-O,containerd,gvisor | Docker | Docker, containerd |
|  startup time initial<br>following | 5:19 <br> 3:15 | 2:48 <br> 1:06 | 0:15 <br> 0:15|
|  memory requirements | 2GB | 8GB (Windows, MacOS) | 512MB|
|  requires root? | no | no | yes (rootless is experimental) |
|  multi-cluster support | yes | yes | no (can be achieved using containers) |
|  multi-node support | no | yes | yes |
|  project page | [minikube](https://minikube.sigs.k8s.io/docs/) | [kind](https://kind.sigs.k8s.io/) | [k3d](https://k3s.io/)|
|  advantages | + Beginer freandly tool (easy to use)<br>+ Supports multi-node clusters, allowing users to simulate more complex Kubernetes environments locally<br>+Comes with several useful add-ons and features, such as dashboard and storage provisioners|  + Quick cluster provisioning<br>+ Well integrates with Docker tools<br>+ Easy to install and use | + Definetely is the best options to simulate a real production environment locally<br>+ Similar to Kind, K3d offers fast cluster creation by leveraging Docker containers  |
|  disadvantages | - Resourse intensive<br>- Starting Minikube clusters can take some time<br>- Relies on virtualization technologies| - Designed for single-node clusters, limiting its use for scenarios that require multi-node setups<br>- Networking in Kind may not fully replicate the complexities of a production Kubernetes environment | - Need to manually configure extra virtual machines or nodes<br>- Similar to Kind, K3d relies on Docker, which might be a limitation in certain environments. |

## Demonstration
![Application on Kubernetes](./../img/625998.gif)

![Application on Kubernetes](https://github.com/vit-um/AsciiArtify/blob/main/doc/.img/622883.gif)   

## Conclusion
The final solution can be approved only after testing all the products with customer's real-word tasks. In ferst biew, the best option for AsciiArtify team's purposes is k3d.

## Additional

Podman alone cannot directly replace K3d, Kind, or Minikube for local Kubernetes development environments. 
Podman focuses on managing containers and is not a Kubernetes distribution. 
Podman does not inherently offer the features needed for local Kubernetes development provided by K3d, Kind, or Minikube.