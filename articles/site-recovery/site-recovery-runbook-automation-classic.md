---
title: Azure automation aaaAdd runbooks toorecovery planer i hello klassiska portal | Microsoft Docs
description: "Den här artikeln beskrivs hur Azure Site Recovery kan du nu tooextend återställningsplaner med hjälp av Azure Automation toocomplete komplicerade uppgifter under återställning tooAzure"
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: f24eaa62-9dea-4fce-92e1-a72513ca0289
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 3bb7420911afbce289b656f28823b1923e8af0c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans-in-hello-classic-portal"></a>Lägg till Azure automation-runbooks toorecovery planer i hello klassiska portalen
Den här självstudiekursen beskrivs hur Azure Site Recovery kan integreras med Azure Automation tooprovide utökningsbarhet toorecovery planer. Återställningsplaner kan samordna återställning av dina virtuella datorer som skyddas med Azure Site Recovery för replikering tooAzure scenarier och replikering toosecondary moln. De hjälper också att hello recovery **konsekvent korrekt**, **repeterbara**, och **automatiserad**. Om du misslyckas över dina virtuella datorer tooAzure integrering med Azure Automation utökar återställningsplaner och ger dig kapaciteten tooexecute runbooks, vilket ger kraftfulla automation-aktiviteter.

Registrera dig om du inte har lyssnat på om Azure Automation ännu, [här](https://azure.microsoft.com/services/automation/) och hämta sina exempelskript [här](https://azure.microsoft.com/documentation/scripts/). Läs mer om [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) och hur tooorchestrate recovery tooAzure med hjälp av recovery planerar [här](https://azure.microsoft.com/blog/?p=166264).

I den här korta kursen ska vi titta på hur du kan integrera Azure Automation-runbooks i återställningsplaner. Kommer att automatisera enkla uppgifter som tidigare krävde manuella åtgärder och se hur tooconvert en flera steg återställning till en återställningsåtgärd med enkelklickning. Vi kommer också titta på hur du felsöker ett enkelt skript om det händer.

## <a name="protect-hello-application-tooazure"></a>Skydda hello programmet tooAzure
Låt oss börja med ett enkelt program som består av två virtuella datorer. Här, har vi HRweb-programmet för Fabrikam. Fabrikam-HRweb-klient- och Fabrikam-Hrweb-servergränssnitten är hello två virtuella datorer skyddas tooAzure med hjälp av Azure Site Recovery. tooprotect hello virtuella datorer med Azure Site Recovery gör hello nedan.

1. Aktivera skydd för virtuella datorer.
2. Kontrollera att hello virtuella datorer har slutfört den inledande replikeringen och replikerar.
3. Vänta tills hello den inledande replikeringen har slutförts och hello replikeringsstatus står skyddade.

## ![](media/site-recovery-runbook-automation/01.png)
I den här självstudiekursen skapar vi en återställningsplan för hello Fabrikam HRweb programmet toofailover hello programmet tooAzure. Vi kommer sedan integrera den med en runbook som skapar en slutpunkt på hello redundansväxlats virtuella Azure-datorn tooserve webbsidor på port 80.

Först ska vi skapa en återställningsplan för vårt program.

## <a name="create-hello-recovery-plan"></a>Skapa hello återställningsplan
toorecover hello programmet tooAzure måste toocreate en återställningsplan.
Med hjälp av en återställningsplan som du kan ange hello ordning för återställning av virtuella datorer. hello virtuella datorn placeras i grupp 1 ska återställa och starta först och sedan hello virtuell dator i grupp 2 följer.

Skapa en återställningspunkt planera som ser ut som nedan.

![](media/site-recovery-runbook-automation/12.png)

Mer om återställningsplaner dokumentationen tooread [här](https://msdn.microsoft.com/library/azure/dn788799.aspx "här").

Därefter skapar vi hello nödvändiga artefakter i Azure Automation.

## <a name="create-hello-automation-account-and-its-assets"></a>Skapa hello automation-kontot och dess tillgångar
Du behöver ett Azure Automation-konto toocreate runbooks. Om du inte redan har ett konto navigerar tooAzure Automation fliken med ![](media/site-recovery-runbook-automation/02.png)och skapa ett nytt konto.

1. Ge hello konto en namnet tooidentify med.
2. Ange en geografisk region där du vill att tooplace hello-konto.

Det rekommenderas tooplace hello konto i hello samma region som hello ASR valvet.

![](media/site-recovery-runbook-automation/03.png)

Skapa sedan hello följande tillgångar i hello konto.

### <a name="add-a-subscription-name-as-asset"></a>Lägg till ett prenumerationsnamn som tillgång
1. Lägg till en ny inställning ![](media/site-recovery-runbook-automation/04.png) i hello Azure Automation-tillgångar och välj för![](media/site-recovery-runbook-automation/05.png)
2. Välj hello variabeltyp som **sträng**
3. Ange variabelnamnet som **AzureSubscriptionName**

   ![](media/site-recovery-runbook-automation/06.png)
4. Ange ditt faktiska namn i Azure-prenumeration som hello variabelvärdet.

   ![](media/site-recovery-runbook-automation/07_1.png)

Du kan identifiera hello namnet på din prenumeration från hello inställningssidan för ditt konto på hello Azure-portalen.

### <a name="add-an-azure-login-credential-as-asset"></a>Lägg till en Azure-inloggning autentiseringsuppgift som tillgång
Azure Automation använder Azure PowerShell tooconnect toothe prenumeration och fungerar på det hello-artefakter. För att göra detta måste autentiseras med ditt Microsoft-konto eller ett arbets- eller skolkonto.
Du kan lagra hello autentiseringsuppgifter i en tillgång toobe används på ett säkert sätt av hello runbook.

1. Lägg till en ny inställning ![](media/site-recovery-runbook-automation/04.png) i hello Azure Automation-tillgångar och välj![](media/site-recovery-runbook-automation/09.png)
2. Välj hello autentiseringstyp som **Windows PowerShell-autentiseringsuppgift**
3. Ange hello namn som **AzureCredential**

   ![](media/site-recovery-runbook-automation/10.png)
4. Ange hello användarnamn och lösenord toosign i med.

Båda dessa inställningar är nu tillgänglig i dina tillgångar.

![](media/site-recovery-runbook-automation/11.png)

Mer information om hur tooconnect tooyour prenumeration via PowerShell ges [här](/powershell/azure/overview).

Därefter skapar du en runbook i Azure Automation kan lägga till en slutpunkt för hello frontend virtuella datorn efter redundans.

## <a name="azure-automation-context"></a>Azure automation-kontexten
ASR skickar en kontext variabeln toohello runbook toohelp du skriva deterministiska skript. Något gick argumentera att hello namnen på hello Molntjänsten och hello virtuell dator är förutsägbar, men händer att det inte är alltid hello fall på grund av toocertain scenarier, till exempel hello en där hello namnet på hello virtuell dator kan ha ändrats på grund av toounsupported tecken i Azure. Därför informationen skickas toohello ASR återställningsplan som en del av hello *kontexten*.

Nedan visas ett exempel på hur hello kontexten variabeln ser ut.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


hello tabellen nedan innehåller namn och beskrivning för varje variabel i hello-kontexten.

| **Variabelnamn** | **Beskrivning** |
| --- | --- |
| RecoveryPlanName |Namnet på planen som körs. Hjälper till att du vidtar åtgärder baserat på namn med hello samma skript |
| FailoverType |Anger om hello växling vid fel är testa planerad eller oplanerad. |
| FailoverDirection |Ange om återställning är tooprimary eller sekundär |
| Grupp-ID |Identifiera hello gruppnumret inom hello återställningsplan när hello plan körs |
| VmMap |Matris med alla hello virtuella datorer i gruppen hello |
| VMMap nyckel |Unik nyckel (GUID) för varje virtuell dator. Det har hello samtidigt som hello-ID för VMM för hello virtuell dator i tillämpliga fall. |
| RoleName |Namnet på hello Azure VM som återställs |
| CloudServiceName |Azure Cloud Service namn under vilka hello virtuell dator skapas. |

tooidentify hello VmMap Key i hello kontexten du kan också gå toohello VM egenskapssidan i ASR och titta på hello VM-GUID-egenskap.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Skapa en Automation-runbook
Nu skapa hello runbook tooopen port 80 på hello frontend virtuella datorn.

1. Skapa en ny runbook i hello Azure Automation-konto med namnet hello **OpenPort80**

   ![](media/site-recovery-runbook-automation/14.png)
2. Navigera toohello författare vy över hello runbook och ange hello utkastläge.
3. Ange först hello variabeln toouse som hello recovery plan kontext

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. Sedan Anslut toohello prenumeration med hjälp av hello autentiseringsuppgifter och prenumeration namn

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect tooAzure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   Observera att du använder hello Azure tillgångar – **AzureCredential** och **AzureSubscriptionName** här.
5. Nu ange information om hello och hello GUID för hello virtuell dator som du vill tooexpose hello slutpunkt. I det här fallet hello frontend virtuella datorn.

   ```
       # Specify hello parameters toobe used by hello script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   Anger hello Azure endpoint-protokollet, lokal port på hello VM och dess mappade offentlig port. Dessa variabler är parametrar som krävs av hello Azure kommandon som lägga till slutpunkter tooVMs. Hej VMGUID innehåller hello hello virtuell dator som du behöver toooperate på GUID.
6. hello-skriptet kommer nu extrahera hello kontext för hello angivna VM-GUID och skapa en slutpunkt på hello virtuell dator som den refererar till.

   ```
       #Read hello VM GUID from hello context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke hello necessary pipeline commands tooadd a Azure Endpoint tooa specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. När den är klar, klicka på Publicera ![](media/site-recovery-runbook-automation/20.png) tooallow din toobe för skript som är tillgänglig för körning.

hello fullständiga skriptet anges nedan som referens

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect tooAzure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify hello parameters toobe used by hello script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read hello VM GUID from hello context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke hello necessary pipeline commands tooadd an Azure Endpoint tooa specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-hello-script-toohello-recovery-plan"></a>Lägg till hello skriptet toohello återställningsplan
När hello skriptet är klar kan lägga du toohello återställningsplan som du skapade tidigare.

1. Välj tooadd ett skript i hello återställningsplan som du har skapat, efter grupp 2. ![](media/site-recovery-runbook-automation/15.png)
2. Ange ett skriptnamn. Detta är ett eget namn för det här skriptet för visar inom hello återställningsplan.
3. Hello redundans tooAzure skript – Välj i hello Azure Automation-kontot.
4. Hello Azure Runbooks, Välj hello runbook som du skapade.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Primär skript
När du kör en redundansväxling tooAzure, kan du också välja tooexecute primära skript. Dessa skript körs på hello VMM-servern under växling vid fel.
Primär klientsidans skript finns bara tillgänglig enbart för före avstängning och efter avstängning steg. Det beror på att vi räknar med hello primär plats toobe vanligtvis inte tillgängligt när en katastrof inträffar.
Under en oplanerad redundans endast om du väljer i för primär plats, görs ett försök toorun hello primära skript. Om de inte kan nås eller timeout, hello redundans fortsätter hello toorecover virtuella datorer.
Primär skript finns icke för VMware/fysisk/Hyper-v-platser utan VMM skyddade tooAzure - under växling vid fel tooAzure.
Men när återställning från Azure tooon lokal primär skript (Runbooks) kan användas för alla mål förutom VMware.

## <a name="test-hello-recovery-plan"></a>Testplan hello återställning
När du har lagt till hello runbook toohello planen kan du initiera ett redundanstest och se hur det fungerar. Det är alltid rekommenderas toorun en testa redundans tootest dina program och hello recovery plan tooensure att det inte finns några fel.

1. Välj hello återställningsplan och initiera testa redundans.
2. Under hello plan körning kan du se om hello runbook har körts eller inte via dess status.

   ![](media/site-recovery-runbook-automation/17.png)
3. Du kan också se hello detaljerad status för runbook-körningen på hello Azure Automation-jobb sidan för hello runbook.

   ![](media/site-recovery-runbook-automation/18.png)
4. När hello växling vid fel är klar, förutom hello runbook Körningsresultat, kan du se om hello körningen har lyckats eller inte genom att besöka hello Azure virtuella sidan och titta på hello slutpunkter.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Exempelskript
När vi har gått igenom automatisera en ofta används uppgiften att lägga till en slutpunkt tooan virtuella Azure-datorn i den här självstudiekursen, kan du göra ett antal andra kraftfulla automation-aktiviteter med hjälp av Azure automation. Microsoft och hello Azure Automation-gruppen ger du runbook-exempel som kan hjälpa dig komma igång med dina egna lösningar och verktyg för runbooks som du kan använda som byggblock för större automation-aktiviteter. Börja använda dem från hello-galleriet och skapa kraftfulla enkelklickning återställningsplaner för dina program med Azure Site Recovery.

## <a name="additional-resources"></a>Ytterligare resurser
[Översikt över Azure Automation](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation-översikt")

[Exempel på Azure automatiseringsskript](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "exempel Azure Automation-skript")
