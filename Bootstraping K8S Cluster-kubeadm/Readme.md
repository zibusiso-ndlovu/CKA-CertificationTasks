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


     
