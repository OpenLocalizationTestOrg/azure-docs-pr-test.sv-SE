---
title: "aaaManage Azure endpoint åtkomst styra listor | PowerShell | Klassiska | Microsoft Docs"
description: "Lär dig hur toomanage ACL: er med PowerShell"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c84e40af-f351-4572-b3f0-d572d46bafe7
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: a7ca241ea108a266085bfb689b742d781e58da1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-hello-classic-deployment-model"></a>Hantera åtkomstkontrollistor för slutpunkten med PowerShell i hello klassiska distributionsmodellen
Du kan skapa och hantera nätverket åtkomstkontrollistor (ACL) för slutpunkter med hjälp av Azure PowerShell eller hello-hanteringsportalen. I det här avsnittet hittar du procedurer för ACL vanliga uppgifter som du kan utföra med hjälp av PowerShell. Hello lista över Azure PowerShell cmdlets finns [Azure Management-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721). Mer information om åtkomstkontrollistor finns [vad är ett nätverk åtkomstkontrollista (ACL)?](virtual-networks-acl.md). Om du vill toomanage din ACL: er med hjälp av hello-hanteringsportalen, se [hur tooSet in slutpunkter tooa virtuella](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Hantera nätverket ACL: er med hjälp av Azure PowerShell
Du kan använda Azure PowerShell-cmdlets toocreate, ta bort och konfigurera (uppsättning) nätverket åtkomstkontrollistor (ACL). Innehåller några exempel på några av hello sätt kan du konfigurera en ACL med hjälp av PowerShell.

tooretrieve en fullständig lista över hello ACL PowerShell-cmdlets, du kan använda något av följande hello:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Skapa en ACL i nätverket med regler som tillåter åtkomst från ett fjärranslutet undernät
hello exemplet nedan visar hur-toocreate en ny ACL som innehåller regler. Det här används sedan tooa virtuell datorslutpunkt. hello ACL-regler i hello exemplet nedan kommer att tillåta åtkomst från ett fjärranslutet undernät. toocreate en ny nätverket ACL med tillståndet regler för ett fjärranslutet undernät, öppna ett Azure PowerShell ISE. Kopiera och klistra in hello skriptet nedan, konfigurera hello skript med egna värden och sedan köra hello skript.

1. Skapa hello nya ACL-objekt i nätverket.
   
        $acl1 = New-AzureAclConfig
2. Ange en regel som tillåter åtkomst från ett fjärranslutet undernät. I hello exemplet nedan kan du ange regel *100* (som har högre prioritet än 200 och högre) tooallow hello fjärrundernät *10.0.0.0/8* komma åt toohello virtuell datorslutpunkt. Ersätt hello värden med din egen konfigurationskrav. hello namnet ”SharePoint ACL-config” ska ersättas med hello eget namn som du vill toocall den här regeln.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. Upprepa hello cmdlet, ersätter hello värden med din egen konfigurationskrav för ytterligare regler. Vara säker på att toochange hello regel nummer ordning tooreflect hello order som du vill hello regler toobe tillämpas. hello lägre nummer i regeln företräde framför hello högre nummer.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. Därefter kan du antingen skapa en ny slutpunkt (Lägg till) eller ange hello ACL för en befintlig slutpunkt (anges). I det här exemplet ska vi lägga till en ny virtuell dator-slutpunkt som kallas ”web” och uppdatera hello virtuella slutpunkten med hello ACL-inställningar.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. Sedan kombinera hello-cmdlets och köra hello-skript. I det här exemplet hello kombinerade cmdlets skulle se ut så här:
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Ta bort en nätverket ACL-regel som tillåter åtkomst från ett fjärranslutet undernät
hello exemplet nedan sätt-tooremove en regel för Portåtkomstkontroll.  tooremove ett nätverk regeln med tillståndet regler för ett fjärranslutet undernät, öppna ett Azure PowerShell ISE. Kopiera och klistra in hello skriptet nedan, konfigurera hello skript med egna värden och sedan köra hello skript.

1. Första steget är tooget hello nätverket ACL-objekt för hello virtuella slutpunkt. Sedan tar du bort hello regeln. I det här fallet bort vi den efter regeln-ID. Detta tar endast bort hello regel-ID 0 från hello ACL. Hello ACL-objekt tas inte bort från hello virtuella slutpunkten.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. Därefter måste du tillämpa hello nätverket ACL objektet toohello virtuell datorslutpunkt och uppdatera hello virtuell dator.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Ta bort en ACL i nätverket från en virtuell datorslutpunkt
I vissa fall kanske du vill tooremove ett nätverk ACL-objekt från en virtuell dator-slutpunkt. toodo som öppnas i ett Azure PowerShell ISE. Kopiera och klistra in hello skriptet nedan, konfigurera hello skript med egna värden och sedan köra hello skript.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Nästa steg
[Vad är ett nätverk åtkomstkontrollista (ACL)?](virtual-networks-acl.md)

