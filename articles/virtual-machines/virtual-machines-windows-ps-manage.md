---
title: "使用 Resource Manager 和 PowerShell 管理 VM | Microsoft Docs"
description: "使用 Azure Resource Manager 与 PowerShell 来管理虚拟机。"
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 48930854-7888-4e4c-9efb-7d1971d4cc14
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 45a45b616b4de005da66562c69eef83f2f48cc79
ms.openlocfilehash: 63e822de6ae50be33590048140e06e89526282ee


---
# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>使用 Resource Manager 与 PowerShell 来管理 Azure 虚拟机
## <a name="install-azure-powershell"></a>安装 Azure PowerShell
有关安装最新版 Azure PowerShell、选择订阅和登录到帐户的信息，请参阅[如何安装和配置 Azure PowerShell](/powershell/azureps-cmdlets-docs)。

## <a name="set-variables"></a>设置变量
本文中的所有命令都需要虚拟机所在资源组的名称以及要管理的虚拟机的名称。 将 **$rgName** 的值替换为包含虚拟机的资源组的名称。 将 **$vmName** 的值替换为 VM 的名称。 创建变量。

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>显示有关虚拟机的信息
获取虚拟机信息。

    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

它会返回类似于此示例的内容：

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>停止虚拟机
停止正在运行的虚拟机。

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

系统会提示你进行确认：

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

输入 **Y** 停止虚拟机。

几分钟后，将返回类似于以下示例的内容：

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>启动虚拟机
启动虚拟机（如果已停止）。

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

几分钟后，将返回类似于以下示例的内容：

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

如果想要重新启动已运行的虚拟机，请按下文所述使用 **Restart-AzureRmVM**。

## <a name="restart-a-virtual-machine"></a>重新启动虚拟机
重新启动正在运行的虚拟机。

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

它会返回类似于此示例的内容：

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>删除虚拟机
删除虚拟机。  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [!NOTE]
> 可以使用 **-Force** 参数跳过确认提示。
> 
> 

如果未使用 -Force 参数，系统会提示确认：

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

它会返回类似于此示例的内容：

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>更新虚拟机
本示例演示如何更新虚拟机的大小。

    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

它会返回类似于此示例的内容：

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

有关虚拟机的可用大小列表，请参阅 [Azure 中的虚拟机大小](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 。

## <a name="add-a-data-disk-to-a-virtual-machine"></a>将数据磁盘添加到虚拟机
此示例演示了如何将数据磁盘添加到现有的虚拟机。

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

添加的磁盘未进行初始化。 若要初始化该磁盘，可以登录到该磁盘，然后通过磁盘管理进行初始化。 如果在创建磁盘时在其上安装了 WinRM 和证书，则可以通过远程 PowerShell 初始化该磁盘。 还可以使用自定义脚本扩展： 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

脚本文件可以包含类似于下面的代码来初始化磁盘：

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>后续步骤
如果部署出现问题，请参阅[使用 Azure 门户对资源组部署进行故障排除](../resource-manager-troubleshoot-deployments-portal.md)




<!--HONumber=Dec16_HO2-->

