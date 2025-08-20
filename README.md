# hyperv-nvidia-gpu-passthrough
A condensed set of basic instructions in order to get your NVIDIA gpu working with Hyper-V

# HyperV GPU Passthrough Instructions

# Verify VM settings
	- Generation 2
	- Dynamic Memory Unchecked
	- Configure Virtual switch for External Network
	- Disable Enhanced Sessions
	- Disable Checkpoints

# Verify GPU Compatibility
	- Check with Powershell ISE: Get-VMHostPartitionableGpu

# Copy the nvdisplay folder to VM
	- HOST - C:\Windows\System32\DriverStore\FileRepository\nv_dispi.inf_amd64_[UUID]
	- GUEST - C:\Windows\System32\HostDriverStore\FileRepository\nv_dispi.inf_amd64_[UUID]

# Run PowerShell ISE as Administrator 
  - Set-ExecutionPolicy Unrestricted

# Turnoff the VM and run script
  - $vm = "**your VM name**"

if (Get-VMGpuPartitionAdapter -VMName $vm -ErrorAction SilentlyContinue) {
        Remove-VMGpuPartitionAdapter -VMName $vm
}
  
Set-VM -GuestControlledCacheTypes $true -VMName $vm
Set-VM -LowMemoryMappedIoSpace 1Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 32Gb -VMName $vm

  - Add-VMGpuPartitionAdapter -VMName $vm

# Removing GPU-P
	- Remove-VMGpuPartitionAdapter -VMName "Your VM name"

# Black screen issues - Troubleshooting	
	- If host monitor in portrait turn into landscape mode
	- Load into Parsec then connect via Moonlight at same time check display settings to show on 1 or 2 only
