Installing Kubeadm 

environment : ubuntu

1. Make sure Firewall and Network Security systems are in place. Make sure that the br_netfilter module is loaded

   run the following command.
   
   #lsmod | grep br_netfilter
   
   it returns the following if its loaded:
   
      br_netfilter           28672  0
      bridge                176128  1 br_netfilter

   
   if it does not return anything run the following command to load it explicitly on all the nodes.
   
   #sudo modprobe br_netfilter
   

2. Once its done, run the following set of commands to create the new kernel parameters on all the nodes:

     # cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
          br_netfilter
          EOF
          
     # cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
          net.bridge.bridge-nf-call-ip6tables = 1
          net.bridge.bridge-nf-call-iptables = 1
           EOF
           
    # sudo sysctl --system
        

3.  Install container run time enviroment (In this case Docker)

   $ sudo apt-get update
    
   $ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
   $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   
   $ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
  Install Docker Engine
  
  $  sudo apt-get update
  $  sudo apt-get install docker-ce docker-ce-cli containerd.io
  
4. Installing kubeadm, kubelet and kubectl 

   $ sudo apt-get update
   $ sudo apt-get install -y apt-transport-https ca-certificates curl
   $ sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
   
   $ echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
   
   $ sudo apt-get update
   $ sudo apt-get install -y kubelet kubeadm kubectl
   $ sudo apt-mark hold kubelet kubeadm kubectl
   
  5. Use kubeadm to create a cluster

   $ kubeadm init --apiserver-advertise-address=<ip-address> --pod-network-cidr=<cidr-block>
   
 6. Post Installation Tasks
   
7.  Verification :
   
   kubectl get nodes

     
