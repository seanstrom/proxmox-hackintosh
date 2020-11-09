### If using GRUB bios

#### /etc/default/grub
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on rootdelay=10"
```

### If using UEFI bios

#### /etc/kernel/cmdline
```
amd_iommu=on rootdelay=10
```

#### /etc/modules
```
# /etc/modules: kernel modules to load at boot time.
#
# This file containes the names of kernel module that should be loaded
# at boot time, one per line. Lines beginning with "#" are ignored.

vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

### For blacklisting modules at startup
#### /etc/modprobe.d/blacklist.conf
```
blacklist nouveau
blacklist nvidia
blacklist nvidiafb
blacklist snd_hda_codec_hdmi
blacklist snd_hda_intel
blacklist snd_hda_codec
blacklist snd_hda_core
blacklist radeon
blacklist amdgpu
```

#### /etc/modprobe.d/kvm-amd.conf
```
options kvm-amd nested=1
```

#### /etc/modprobe.d/kvm.conf
```
options kvm ignore_msrs=Y
```

### For loading modules before vfio and GPU passthrough
#### /etc/modprobe.d/vfio.conf
```
softdep amdgpu pre: vfio-pci
softdep radeon pre: vfio-pci

options vfio-pci ids=1002:67df,1002:aaf0 disable_vga=1
```

### VM config for Hackintosh
#### /etc/pve/qemu-server/153.conf
```
args: -device isa-applesmc,osk="ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc" -smbios type=2 -device usb-kbd,bus=ehci.0,port=2 -cpu Penryn,kvm=on,vendor=GenuineIntel,+kvm_pv_unhalt,+kvm_pv_eoi,+hypervisor,+invtsc,+pcid,+ssse3,+sse4.2,+popcnt,+avx,+avx2,+aes,+fma,+fma4,+bmi1,+bmi2,+xsave,+xsaveopt,check
balloon: 0
bios: ovmf
boot: cdn
bootdisk: sata0
cores: 4
cpu: Penryn
efidisk0: local-lvm:vm-153-disk-1,size=4M
hostpci0: 08:00,x-vga=1
machine: q35
memory: 4096
name: catalina
net0: vmxnet3=EE:15:ED:FE:1C:86,bridge=vmbr0,firewall=1
numa: 0
ostype: other
sata0: local-lvm:vm-153-disk-0,cache=unsafe,discard=on,size=64G,ssd=1
scsihw: virtio-scsi-pci
smbios1: uuid=a3781ecb-84b2-463e-8893-fd43f746ed47
sockets: 1
usb0: host=1-7
usb1: host=3-1
vga: none
vmgenid: 96725b07-c942-40df-ae7e-47e595418cd1
```

