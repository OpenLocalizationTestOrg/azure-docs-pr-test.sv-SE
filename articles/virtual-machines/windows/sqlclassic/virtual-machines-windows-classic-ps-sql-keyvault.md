---
title: "aaaIntegrate Nyckelvalv med SQL Server på virtuella Windows-datorer i Azure (klassisk) | Microsoft Docs"
description: "Lär dig hur tooautomate hello konfigurationen av SQL Server-kryptering för användning med Azure Key Vault. Det här avsnittet beskrivs hur toouse Azure Key Vault-integrering med SQL Server-datorer för att skapa i hello klassiska distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: ab8d41a7-1971-4032-ab71-eb435c455dc1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/17/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 54664875b76dac7271d5a9f00b3f41fdc9c08491
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-classic"></a>Konfigurera Azure Key Vault-integrering för SQLServer på virtuella Azure-datorer (klassisk)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-ps-sql-keyvault.md)
> * [Klassisk](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Översikt
Det finns flera funktioner för SQL Server-kryptering, t.ex [transparent datakryptering (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [kryptering på kolumnen (r)](https://msdn.microsoft.com/library/ms173744.aspx), och [säkerhetskopia av krypteringsnyckeln](https://msdn.microsoft.com/library/dn449489.aspx). Dessa typer av kryptering kräver toomanage och lagra hello Kryptografiska nycklar som du använder för kryptering. hello Azure Key Vault (AKV)-tjänsten är utformad tooimprove hello säkerhet och hantering av dessa nycklar i en säker och hög tillgänglighet på. Hej [SQL Server-anslutningen](http://www.microsoft.com/download/details.aspx?id=45344) och aktiverar SQL Server toouse nycklarna från Azure Key Vault.

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Om du kör SQL Server med lokala datorer, finns [steg som du kan följa tooaccess Azure Key Vault från din lokala SQL Server-datorn](https://msdn.microsoft.com/library/dn198405.aspx). Men för SQL Server på virtuella Azure-datorer, du kan spara tid genom att använda hello *Azure Key Vault-integrering* funktion. Med några Azure PowerShell-cmdlets tooenable funktionen kan du automatisera hello konfiguration krävs för en SQL-VM tooaccess nyckelvalvet.

När den här funktionen aktiveras den automatiskt hello SQL Server-anslutningen installerar, konfigurerar hello EKM-providern tooaccess Azure Key Vault och skapar hello autentiseringsuppgifter tooallow du tooaccess ditt valv. Om du har använt hello stegen i hello tidigare nämnts lokalt dokumentation, ser du att den här funktionen automatiserar steg 2 och 3. hello enda fortfarande behövs toodo manuellt är toocreate hello nyckelvalvet och nycklar. Därifrån är hello hela installationen av SQL-VM automatisk. När installationen har slutförts som den här funktionen, kan du köra T-SQL-instruktioner toobegin kryptera dina databaser eller säkerhetskopieringar som vanligt.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>Konfigurera AKV-integreringen
Använd PowerShell tooconfigure Azure Key Vault-integrering. hello följande avsnitt ger en översikt över hello krävs parametrar och exempel PowerShell-skript.

### <a name="install-hello-sql-server-iaas-extension"></a>Installera hello SQL Server IaaS-tillägg
Första, [installera hello SQL Server IaaS tillägget](../classic/sql-server-agent-extension.md).

### <a name="understand-hello-input-parameters"></a>Förstå hello indataparametrar
hello följande tabell visar hello parametrar krävs toorun hello PowerShell-skript i hello nästa avsnitt.

| Parameter | Beskrivning | Exempel |
| --- | --- | --- |
| **$akvURL** |**hello key vault-URL** |”https://contosokeyvault.vault.azure.net/” |
| **$spName** |**Tjänstens huvudnamn** |”fde2b411 33d 5-4e11-af04eb07b669ccf2” |
| **$spSecret** |**Tjänstens huvudnamn hemlighet** |”9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =” |
| **$credName** |**Namn på autentiseringsuppgifter**: AKV-integreringen skapar autentiseringsuppgifter på SQL-servern, så att hello VM toohave åtkomst toohello nyckelvalvet. Välj ett namn för autentiseringsuppgifterna. |”mycred1” |
| **$vmName** |**Namn på virtuell dator**: hello namnet på en tidigare skapad SQL-VM. |”myvmname” |
| **$serviceName** |**Tjänstnamnet**: hello Molntjänsten namnet som associeras med hello SQL VM. |”mycloudservicename” |

### <a name="enable-akv-integration-with-powershell"></a>Aktivera AKV-integreringen med PowerShell
Hej **ny AzureVMSqlServerKeyVaultCredentialConfig** cmdlet skapar ett konfigurationsobjekt för hello Azure Key Vault-integrering funktion. Hej **Set AzureVMSqlServerExtension** konfigurerar den här integreringen med hello **KeyVaultCredentialSettings** parameter. Hej följande steg visar hur toouse dessa kommandon.

1. I Azure PowerShell först konfigurera hello indataparametrar med dina specifika värden som beskrivs i föregående hello-avsnitt i det här avsnittet. hello följande skript är ett exempel.
   
        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2. Sedan används hello följande skript tooconfigure och aktivera AKV-integreringen.
   
        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

hello SQL IaaS Agent tillägg uppdateras hello SQL VM med den nya konfigurationen.

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

