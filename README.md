# Azure Policy for making the TMC Pinniped LB internal

Simple exmaple of a gatekeeper mutate policy dpeloyed through azure policy manager that will switch the load balancer type to internal for pinniped. 


## Create the policy
Go to the policy section in zure and create a new defintion. use the `azure-policy.json` as the contents and select the category as kubernetes.

## Assing the policy

Assing this policy to the resource group that your clusters will be deployed in. as long as azure policy addon is enabled in the clusters this should take affect. 

## Using with TMC

When provisioning a cluster in TMC there is not an option to enable the policy addon directly, in azure you can enable autoprovisioning in the defender settings to enable this addon by default. If you have the enabled then you can skip the step of enabling it diretcly on the cluster after TMC provisions it.

If you don't have the autoprovisioning enabled, once the cluster is in creation , go to the azure console or cli and enable the policy addon, this will allow for the mutate policy to take affect as soon as possible. You may recieve an error about an ongoing operation and will need to retry or wait until the initial create completes, this will work if the cluster is still being created by TMC it just needs to happen between the create and the compute creating. 

`az aks enable-addons --resource-group $resourceGroupName --name $clusterName --addons azure-policy`


### Checking to see if the policy is working

you should see a `assignMetadata` resource in the cluster after the policy addon is enabled.

```bash
k get assignmetadata -A                            
NAME                                                 AGE
azurepolicy-pinniped-internal-c79dbb0e530e4721be88   4s
```