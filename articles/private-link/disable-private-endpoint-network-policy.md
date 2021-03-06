---
title: 停用 Azure 中私人端點的網路原則
description: 瞭解如何停用私人端點的網路原則。
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: how-to
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: a4c04033f3fb5ff523f0d80bb6c978955f4c9ec2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "84737507"
---
# <a name="disable-network-policies-for-private-endpoints"></a>停用私人端點的網路原則

私人端點不支援類似網路安全性群組 (NSG) 的網路原則。 若要在指定的子網路上部署私人端點，該子網路上需要有明確的停用設定。 這項設定僅適用于私人端點。 如果是子網中的其他資源，則會根據 (NSG) 安全性規則定義的網路安全性群組來控制存取權。 
 
使用入口網站建立私人端點時，此設定會在建立過程中自動停用。 使用其他用戶端進行部署時，需要額外的步驟才能變更此設定。 您可以從 Azure 入口網站中使用 cloud shell，或 Azure PowerShell、Azure CLI 的本機安裝，或使用 Azure Resource Manager 範本來停用設定。  
 
下列範例說明如何 `PrivateEndpointNetworkPolicies` 針對名為*myVirtualNetwork*的虛擬網路停用，並在名為*myResourceGroup*的資源群組中裝載*預設*子網。

## <a name="using-azure-powershell"></a>使用 Azure PowerShell
本節說明如何使用 Azure PowerShell 停用子網私人端點原則。

```azurepowershell
$virtualNetwork= Get-AzVirtualNetwork `
  -Name "myVirtualNetwork" ` 
  -ResourceGroupName "myResourceGroup"  
   
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq 'default'} ).PrivateEndpointNetworkPolicies = "Disabled" 
 
$virtualNetwork | Set-AzVirtualNetwork 
```
## <a name="using-azure-cli"></a>使用 Azure CLI
本節說明如何使用 Azure CLI 停用子網私人端點原則。
```azurecli
az network vnet subnet update \ 
  --name default \ 
  --resource-group myResourceGroup \ 
  --vnet-name myVirtualNetwork \ 
  --disable-private-endpoint-network-policies true
```
## <a name="using-a-template"></a>使用範本
本節說明如何使用 Azure Resource Manager 範本停用子網私人端點原則。
```json
{ 
          "name": "myVirtualNetwork", 
          "type": "Microsoft.Network/virtualNetworks", 
          "apiVersion": "2019-04-01", 
          "location": "WestUS", 
          "properties": { 
                "addressSpace": { 
                     "addressPrefixes": [ 
                          "10.0.0.0/16" 
                        ] 
                  }, 
                  "subnets": [ 
                         { 
                                "name": "default", 
                                "properties": { 
                                    "addressPrefix": "10.0.0.0/24", 
                                    "privateEndpointNetworkPolicies": "Disabled" 
                                 } 
                         } 
                  ] 
          } 
} 
```
## <a name="next-steps"></a>接下來的步驟
- 深入瞭解 [Azure 私人端點](private-endpoint-overview.md)
 
