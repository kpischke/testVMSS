This ARM template is inspired by Taylor NEWILL template:

   - RawCluster tempate: https://github.com/tanewill/5clickTemplates/tree/master/RawCluster  
   - RawCluster2 tempate: https://github.com/tanewill/5clickTemplates/tree/master/RawClusterV2 


# Simple deployment of a VM Scale Set of Linux VMs with a jumpbox

<table>
<tr>
<td>
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fkpischke%2FtestVMMS%2Fvmss-existingVnet.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
    <figcaption>Deploy to an existing VNet</figcaption>
    </td>
    </tr>
    </table>
<br>
This template allows you to deploy a simple VM Scale Set of Linux VMs using the latest HPC version of CentOS (7.3). 
You should have a jumpbox with a public IP address in the same virtual network. So you can connect to the jumpbox via your public IP address, then connect from there to VMs in the scale set via private IP addresses.


About the VNET:
- vmss-existingVnet.json : use an existing VNET created into an existing Ressource Group

## Architecture


### View of ARM template:

### Delpoyed in Azure: 

## Use

To ssh into one of the VMs in the scale set, go to portal.azure.com to find the private IP address of the VM/VM Scale Set, make sure you are ssh'ed into the jumpbox, then execute the following command:

<pre class="prettyprint copy-to-clipboard " >ssh {username}@{vm-private-ip-address}</pre>

Then log on of the Compute node using the same account and load the MPI environement variables with:

On CentOS (7.3) do:
<pre class="prettyprint copy-to-clipboard " >source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh</pre>

You are now ready to launch your first test:

<i>Run a simple MPI command</i>
<pre class="prettyprint copy-to-clipboard " >mpirun -ppn 1 -n 2 -hostfile /home/$USER/nodenames.txt -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname</pre>

<i>Run an MPI benchmark</i>
<pre class="prettyprint copy-to-clipboard " >mpirun -ppn 1 -n 2 -hostfile /home/$USER/nodenames.txt -env I_MPI_FABRICS=dapl     -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong</pre>

## NOTES

The NFS share from the jump box is created on a fast/temporary drive on the VM (/mnt/ressource/scratch), so it will be lost in case if you STOP and then START of the VM from Azure CLI/Portal or PS.

## Still to do

#<img src="https://raw.githubusercontent.com/thovarMS/beegfs-shared-slurm-on-centos7.2/master/workInProgress.png" align="middle" />

#- test deployment using H series with SLES

## Warning

#<img src="https://raw.githubusercontent.com/thovarMS/Images/master/warning.png" align="middle" />
