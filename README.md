# Problem Statement

```plaintext
 allow access of vm based on following criteria  
  if vm acess from office or home
     access = allow 
     port 1000-1010 OPEN 
 if vm access from outside office or home
     access = deny
     port 1000-1010 CLOSE
```

# Access Control Criteria for VM

This repository contains file called firewall.rule configurations to enforce access control criteria for a virtual machine.

## Considerations

This chart assumes that the firewall rules are already present on the Linux VM where the application will be deployed. The firewall rules are defined in the /etc/firewall.rule file and are applied to the VM. As a result, the application deployed using this chart will be accessible according to the rules defined in the firewall.rule file.

## Firewall Rules

### Allow Access from Office or Home Network

```plaintext
# Allow access from office or home network to ports 1000-1010
config rule
    option name 'Allow-Access-from-Office-Home'
    option src 'lan'  # Assuming LAN interface corresponds to office or home network
    option family 'ipv4'
    option src_ip '192.168.3.0/24'
    option dest_port '1000-1010'
    option proto 'tcp'
    option target 'ACCEPT'
```

### Deny Access from Outside Office or Home Network

```plaintext
# Deny access from outside office or home network to ports 1000-1010
config rule
    option name 'Deny-Access-from-Outside'
    option src '!lan'  # Not LAN means not from office or home network
    option family 'ipv4'
    option dest_port '1000-1010'
    option proto 'tcp'
    option target 'REJECT'

```
Note: I have used internet/GPT to for above problem statement, to understand, syntax

# Application Helm-chart
- This Helm chart focuses on configuring the application deployment within the context of the existing firewall rules. It does not directly modify or manage the firewall rules themselves
- Application helm chart is located at k8s/helm-chart
- Template:-->k8s/helm-chart/application.yaml

### Feature

- Configure environment variables and secrets for the application.
- Define PersistentVolume (PV) and PersistentVolumeClaim (PVC) for data persistence.
- Configure Kubernetes resources such as Services, Ingress, and Autoscaling.
- Define resource requests and limits for the application pods.
- Probes