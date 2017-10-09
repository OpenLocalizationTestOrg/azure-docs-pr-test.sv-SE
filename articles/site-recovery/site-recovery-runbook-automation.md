---
title: Azure Automation aaaAdd runbooks toorecovery planer i Azure Site Recovery | Microsoft Docs
description: "Lär dig hur Azure Site Recovery kan du utöka återställningsplaner med Azure Automation. Lär dig hur toocomplete komplexa uppgifter under återställning tooAzure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: ecece14d-5f92-4596-bbaf-5204addb95c2
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/23/2017
ms.author: ruturajd@microsoft.com
ms.openlocfilehash: 90d517200cec5527e98a0d00da466bace587b70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans"></a>Lägg till Azure Automation-runbooks toorecovery planer
I den här artikeln beskriver vi hur Azure Site Recovery kan integreras med Azure Automation toohelp du utöka din återställningsplaner. Återställningsplaner kan samordna återställning av virtuella datorer som är skyddade med Site Recovery. Återställningsplaner fungerar både för replikering tooa sekundära molnet och för replikering tooAzure. Återställningsplaner också hjälpa dig att göra hello recovery **konsekvent korrekt**, **repeterbara**, och **automatiserad**. Om du växlar över dina virtuella datorer tooAzure utökar integrering med Azure Automation din återställningsplaner. Du kan använda den tooexecute runbooks, som ger kraftfulla automation-aktiviteter.

Om du är ny tooAzure Automation kan du [registrering](https://azure.microsoft.com/services/automation/) och [hämta skriptexempel](https://azure.microsoft.com/documentation/scripts/). Mer information och toolearn hur tooorchestrate recovery tooAzure med hjälp av [återställningsplaner](https://azure.microsoft.com/blog/?p=166264), se [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/).

I den här artikeln beskriver vi hur du kan integrera Azure Automation-runbooks i din återställningsplaner. Vi använder exempel tooautomate grundläggande uppgifter som tidigare krävde manuella åtgärder. Vi beskriver också hur tooconvert en återställning av flera steg tooa enkelklickning återställningsåtgärd.

## <a name="customize-hello-recovery-plan"></a>Anpassa hello återställningsplan
1. Gå toohello **Site Recovery** resursbladet plan för återställning. I det här exemplet återställningsplanens hello två virtuella datorer som lagts till tooit, för återställning. toobegin att lägga till en runbook, klicka på hello **anpassa** fliken.

    ![Klicka hello anpassa](media/site-recovery-runbook-automation-new/essentials-rp.png)


2. Högerklicka på **grupp 1: starta**, och välj sedan **Lägg till post åtgärd**.

    ![Högerklicka på grupp 1: Starta och Lägg till post åtgärd](media/site-recovery-runbook-automation-new/customize-rp.png)

3. Klicka på **väljer du ett skript**.

4. På hello **uppdatera åtgärden** bladet, hello skriptet **Hello World**.

    ![hello Update åtgärd bladet](media/site-recovery-runbook-automation-new/update-rp.png)

5. Ange namnet på ett Automation-konto.
    >[!NOTE]
    > hello Automation-kontot kan vara i Azure-region. hello Automation-kontot måste vara i hello samma prenumeration som hello Azure Site Recovery-valvet.

6. Välj en runbook i Automation-kontot. Denna runbook är hello-skript som körs under hello körningen av hello återställningsplan efter hello återställning av hello första gruppen.

7. toosave hello skript klickar du på **OK**. hello skript har lagts till för**grupp 1: efter steg**.

    ![1:Start efter grupp](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="considerations-for-adding-a-script"></a>Överväganden för att lägga till ett skript

* Alternativ för**ta bort ett steg** eller **uppdateringsskript hello**, högerklicka på hello skript.
* Ett skript kan köras i Azure under växling vid fel från en lokal dator tooAzure. Den kan även köras på Azure som en primär plats skriptet innan avslutning, vid återställning från Azure tooan lokal dator.
* När ett skript körs den lägger in en återställning plan kontext. hello som följande exempel visar en kontext variabel:

    ```
            {"RecoveryPlanName":"hrweb-recovery",

            "FailoverType":"Test",

            "FailoverDirection":"PrimaryToSecondary",

            "GroupId":"1",

            "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",

                    "ResourceGroupName":"ContosoRG",

                    "CloudServiceName":"pod02hrweb-Chicago-test",

                    "RoleName":"Fabrikam-Hrweb-frontend-test",

                    "RecoveryPointId":"TimeStamp"}

                    }

            }
    ```

    hello i den följande tabellen listas hello namn och beskrivning för varje variabel i hello-kontexten.

    | **Variabelnamn** | **Beskrivning** |
    | --- | --- |
    | RecoveryPlanName |hello namnet på hello plan som körs. Den här variabeln hjälper dig att vidta olika åtgärder baserat på hello schemanamn för återställning. Du kan också återanvända hello skript. |
    | FailoverType |Anger om hello redundans är ett test, planerad eller oplanerad. |
    | FailoverDirection |Anger om återställning tooa primär eller sekundär plats. |
    | Grupp-ID |Identifierar hello gruppnumret i hello återställningsplan när hello plan körs. |
    | VmMap |En matris med alla virtuella datorer i hello grupp. |
    | VMMap nyckel |En unik nyckel (GUID) för varje virtuell dator. Hello är samma som hello Azure Virtual Machine Manager (VMM) ID hello VM, i tillämpliga fall. |
    | SubscriptionId |hello Azure prenumerations-ID som hello VM skapades. |
    | RoleName |hello namnet på hello Azure VM som återställs. |
    | CloudServiceName |hello Azure molntjänstnamnet hello VM som har skapats. |
    | resourceGroupName|hello Azure resursgruppens namn hello VM som har skapats. |
    | RecoveryPointId|hello tidsstämpel för när hello VM återställs. |

* Kontrollera att hello Automation-konto har hello följande moduler:
    * AzureRM.profile
    * AzureRM.Resources
    * AzureRM.Automation
    * AzureRM.Network
    * AzureRM.Compute

Alla moduler som anges i kompatibla versioner. Ett enkelt sätt tooensure att alla moduler som är kompatibla är toouse hello senaste versionerna av alla hello-moduler.

### <a name="access-all-vms-of-hello-vmmap-in-a-loop"></a>Åtkomst till alla virtuella datorer av hello VMMap i en loop
Använd följande kod tooloop över alla virtuella datorer av hello Microsoft VMMap hello:

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
 foreach($VMID in $VMinfo)
 {
     $VM = $vmMap.$VMID                
             if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
         #this check is tooensure that we skip when some data is not available else it will fail
 Write-output "Resource group name ", $VM.ResourceGroupName
 Write-output "Rolename " = $VM.RoleName
     }
 }

```

> [!NOTE]
> hello resurs grupp namn och rollen namnvärden är tomt när hello skript är en före åtgärden tooa Start-grupp. hello värden fylls i om hello VM i gruppen lyckas växling vid fel. hello skript är en efter hello Start grupp-åtgärd.

## <a name="use-hello-same-automation-runbook-in-multiple-recovery-plans"></a>Använd hello samma Automation-runbook i flera återställningsplaner

Du kan använda ett enda skript i flera återställningsplaner med externa variabler. Du kan använda [Azure Automation-variabler](../automation/automation-variables.md) toostore parametrar som du kan skicka för en återställning planera körningen. Du kan skapa enskilda variabler för varje återställningsplan genom att lägga till hello recovery schemanamn som ett prefix toohello variabel. Använd sedan hello variabler som parametrar. Du kan ändra en parameter utan att ändra hello skript, men ändå ändra hello hello sätt skript.

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a>Använda en enkel string-variabel i en runbook-skript

I det här exemplet ett skript som tar hello indata för en Nätverkssäkerhetsgrupp (NSG) och tillämpar den toohello virtuella datorer i en återställningsplan.

Använd hello recovery plan kontext för hello skriptet toodetect vilken återställningsplan körs:

```
workflow AddPublicIPAndNSG {
    param (
          [parameter(Mandatory=$false)]
          [Object]$RecoveryPlanContext
    )

    $RPName = $RecoveryPlanContext.RecoveryPlanName
```

tooapply en befintlig NSG, måste du veta hello NSG namn och hello NSG resursgruppens namn. Använd dessa variabler som indata för återställning plan skript. toodo kan skapa två variabler i hello Automation-konto tillgångar. Lägg till hello namnet på hello återställningsplan som du skapar hello parametrar för som ett prefix toohello variabelnamn.

1. Skapa en variabel toostore hello NSG. Lägga till ett prefix toohello variabelnamn med hello namnet på hello återställningsplan.

    ![Skapa en NSG-variabel](media/site-recovery-runbook-automation-new/var1.png)

2. Skapa en variabel toostore hello NSG resursgruppens namn. Lägga till ett prefix toohello variabelnamn med hello namnet på hello återställningsplan.

    ![Skapa en NSG resursgruppens namn](media/site-recovery-runbook-automation-new/var2.png)


3.  Använd hello följande referens kod tooget hello variabelvärden i hello skript:

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  Använd hello variabler i hello runbook tooapply hello NSG toohello nätverksgränssnittet för hello misslyckades över VM:

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply hello NSG tooa network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```

Skapa oberoende variabler för varje återställningsplanen så att du kan återanvända hello skript. Lägga till ett prefix med hello schemanamn för återställning. En fullständig, slutpunkt-till-slutpunkt skript i det här scenariot finns [lägga till en offentlig IP-adress och NSG tooVMs under redundanstestningen av en återställningsplan för Site Recovery](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee).


### <a name="use-a-complex-variable-toostore-more-information"></a>Använd en komplex variabeln toostore mer information

Föreställ dig ett scenario som du vill använda ett enda skript tooturn på en offentlig IP-adress på specifika virtuella datorer. Du kanske vill tooapply i ett annat scenario olika NSG: er på olika virtuella datorer (inte på alla virtuella datorer). Du kan göra ett skript som är återanvändbara för en återställningsplan. Varje återställningsplanen kan ha en variabel antal virtuella datorer. Till exempel har en SharePoint-återställning två frontwebbservrarna. En grundläggande line-of-business (LOB)-programmet har bara en klientdel. Du kan skapa olika variabler för varje återställningsplan. 

I följande exempel hello, vi en ny teknik och skapa en [komplex variabeln](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396) i hello Azure Automation-konto tillgångar. Det gör du genom att ange flera värden. Du måste använda Azure PowerShell toocomplete hello följande steg:

1. Logga in tooyour Azure-prenumeration i PowerShell:

    ```
    login-azurermaccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. toostore hello parametrar, skapa hello komplex variabeln med hello namnet på hello återställningsplan:

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. I den här komplex variabeln **VMDetails** är hello VM-ID för hello skyddade virtuella datorn. tooget hello VM-ID i hello Azure-portalen visa hello VM egenskaper. hello visar följande skärmbild en variabel som lagrar hello information om två virtuella datorer:

    ![Använd hello VM-ID som hello GUID](media/site-recovery-runbook-automation-new/vmguid.png)

4. Använd den här variabeln i din runbook. Om hello anges VM-GUID kan hittas i kontexten för hello recovery planera, installera hello NSG på hello VM:

    ```
    $VMDetailsObj = Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName
    ```

4. Gå igenom hello virtuella datorer i hello recovery plan kontexten i din runbook. Kontrollera om hello VM finns i **$VMDetailsObj**. Om det finns öppna hello egenskaper för hello variabeln tooapply hello NSG:

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            Write-output $VMDetailsObj.value.$VMID

            if ($VMDetailsObj.value.$VMID -ne $Null) { #If hello VM exists in hello context, this will not b Null
                $VM = $vmMap.$VMID
                # Access hello properties of hello variable
                $NSGname = $VMDetailsObj.value.$VMID.'NSGName'
                $NSGRGname = $VMDetailsObj.value.$VMID.'NSGResourceGroupName'

                # Add code tooapply hello NSG properties toohello VM
            }
        }
    ```

Du kan använda samma skript hello för olika återställningsplaner. Ange olika parametrar genom att lagra hello-värde som motsvarar tooa återställningsplan i olika variabler.

## <a name="sample-scripts"></a>Exempelskript

toodeploy exempel skript tooyour Automation-konto, klickar du på hello **distribuera tooAzure** knappen.

[![Distribuera tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Ett annat exempel finns i följande video hello. Den visar hur toorecover en två-lagers WordPress programmet tooAzure:


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="additional-resources"></a>Ytterligare resurser
* [Azure Automation-tjänsten kör som-konto](../automation/automation-sec-configure-azure-runas-account.md)
* [Översikt över Azure Automation](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation-översikt")
* [Azure Automation-exempelskript](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure Automation-exempelskript")
