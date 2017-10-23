In addition to the [Create Custom Image](/doc/product/213/4942) feature, Tencent Cloud also supports importing local or other platform system disk image files into CVM custom images with the import feature. After importing, you can use the imported image to create a CVM or reinstall the operating system of an existing CVM.

## Importable Linux images
The local Linux image to be imported needs to meet the following criteria:


| Image property | Criteria |
|---------|---------|
| Operating system | CentOS, Redhat, Ubuntu, Debian, CoreOS, OpenSUSE, SUSE releases.<br>Supports 32-bit and 64-bit |
| Image format | raw, vhd, qcow2, vmdk |
| File system type | ext3 or ext4 file systems using MBR partition (GPT partition not supported) |
| System disk size | Up to 50 GB. Supports only system disk images, but not data disk images |
| Network | Do not support multiple network APIs, but only eth0. <br> Do not support IPv6 addresses. <br> When you use the imported image to create a CVM, Tencent Cloud will create a network configuration file in the system and save it in`/etc/qcloud-network-config.ini. This configuration file contains IP, subnet mask, gateway, DNS and other information. The user can log in to the CVM to configure the network after creating a CVM using this image.  |
| Driver | The virtio driver for the KVM platform must be installed |
| Kernel restriction | Native kernel is preferred, for modifications may cause failed importing of virtual machines. <br>The Red Hat Enterprise Linux (RHEL) image imported must have the BYOL license. You need to purchase the product serial number and service from the manufacturer.  |


## Importable Windows images
The local Windows image to be imported needs to meet the following criteria:


| Image property | Criteria |
|---------|---------|
| Operating system | Microsoft Windows Server 2008 R2 (standard edition, datacenter edition, enterprise edition), Microsoft Windows Server 2012 R2 (standard edition), <br>**Supports 64-bit systems only** |
| Image format | raw, vhd, qcow2, vmdk |
| File system type | NTFS file system with MBR partition (GPT partition not supported) |
| System disk size | Not more than 50 GB. Supports only system disk images, but not data disk images |
| Network | Do not support multiple network APIs, but only eth0. <br> Do not support IPv6 addresses. <br> When you use the imported image to create a CVM, Tencent Cloud will create a network configuration file in the system and save it in`C:\qcloud-network-config.ini. This configuration file contains IP, subnet mask, gateway, DNS and other information. The user can log in to the CVM to configure the network after creating a CVM using this image.  |
| Driver | The virtio driver for the KVM platform must be installed. However, the virtio driver is not installed on the Windows system by default. You can install the driver by using [Tencent Cloud Software Package](http://windowsvirtio-10016717.file.myqcloud.com/InstallQCloud.exe) on the original external platform machine and then export it as the local image.  |
| Others |  The imported Windows image DO NOT provide [Windows Activation](https://cloud.tencent.com/doc/product/213/%E6%AD%A3%E7%89%88%E6%BF%80%E6%B4%BB) services

## Import images via console
> Please make sure your Tencent Cloud account has applied for the import permission. If not, please apply for it by submitting a ticket. 

1) Log in to [CVM Console](https://console.cloud.tencent.com/cvm/).

2) Click **Image** from the left panel.

3) Click **Custom Image** tab and select **Import Image**.

4) Activate Tencent Cloud COS as required, upload the image files that meet the requirements to the COS, and click **Next**.

5) Enter information required. Make sure the COS file URL you enter is accurate, and then click **Start Importing**.

6) The operation result is sent to your phone (SMS) or registered Email account. 