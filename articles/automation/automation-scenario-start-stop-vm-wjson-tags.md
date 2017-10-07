---
title: "aaaUse JSON-formaterad taggar tooschedule Azure VM tillstånd | Microsoft Docs"
description: "Den här artikeln visar hur toouse JSON strängar på taggar tooautomate hello schemaläggning av VM-start och stopp."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: f6bbf1dea1c193e5d1010f12f3b1ed63562f9daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-toocreate-a-schedule-for-azure-vm-startup-and-shutdown"></a>Azure Automation-scenario: använda JSON-formaterad taggar toocreate ett schema för Virtuella Azure-start och stopp
Kunder vill ofta tooschedule hello start och stopp av virtuella datorer toohelp minska kostnaderna för prenumerationen eller stöder affärsmässiga och tekniska krav.

hello kan följande scenario du tooset in automatisk start och stopp av dina virtuella datorer med hjälp av en tagg som kallas schemat på en resursgruppsnivå eller virtuell dator i Azure. Det här schemat kan konfigureras från söndag tooSaturday med ett starttiden och avstängning tid.

Vi har några out box-alternativ. Exempel på dessa är:

* [Skaluppsättningar för den virtuella datorn](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) med Autoskala inställningar som gör att du tooscale in eller ut.
* [DevTest Labs](../devtest-lab/devtest-lab-overview.md) -tjänsten som har inbyggda hello-funktionen för schemaläggning av åtgärder för start och stopp.

Dessa alternativ stöder dock endast specifika scenarier och går inte att använda tooinfrastructure-som en tjänst (IaaS) virtuella datorer.

När hello schema taggen är tillämpade tooa resursgrupp, är det också tillämpade tooall virtuella datorer i resursgruppen. Om ett schema är också direkt tooa VM, företräde hello senaste schema filer i hello följande ordning:

1. Schemalägga tillämpade tooa resursgruppen.
2. Schemalägga tillämpade tooa resursgrupp och den virtuella datorn i hello resursgrupp
3. Schemalägga tillämpade tooa virtuell dator

Det här scenariot huvudsakligen tar en JSON-sträng med ett bestämt format och lägger till den som hello värde för en tagg som kallas schema. Sedan en runbook visas alla resursgrupper och virtuella datorer och identifierar hello scheman för varje virtuell dator baserat på hello scenarier som anges ovan. Därefter loop genom hello virtuella datorer som har scheman som är anslutna och utvärderar vilka måste åtgärdas. Till exempel avgör vilka virtuella datorer måste toobe Stoppad, avstängd eller ignoreras.

Dessa runbooks autentisera med hjälp av hello [Azure kör som-konto](automation-sec-configure-azure-runas-account.md).

## <a name="download-hello-runbooks-for-hello-scenario"></a>Hämta hello runbooks för hello scenario
Det här scenariot består av fyra runbooks i PowerShell-arbetsflöde som du kan hämta från hello [TechNet-galleriet](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) eller hello [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) databasen för det här projektet.

| Runbook | Beskrivning |
| --- | --- |
| Testa ResourceSchedule |Kontrollerar schema för varje virtuell dator och utför stängs av eller startas beroende på hello schema. |
| Lägg till ResourceSchedule |Lägger till hello schema taggen tooa virtuell dator eller resurs grupp. |
| Uppdatera ResourceSchedule |Ändrar hello befintlig schema tagg genom att ersätta den med en ny. |
| Ta bort ResourceSchedule |Tar bort hello schema taggen från en virtuell dator eller resurs-grupp. |

## <a name="install-and-configure-this-scenario"></a>Installera och konfigurera det här scenariot
### <a name="install-and-publish-hello-runbooks"></a>Installera och publicera hello runbooks
När du har hämtat hello runbooks kan du importera dem med hello proceduren i [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).  Publicera varje runbook när den har importerats till ditt Automation-konto.

### <a name="add-a-schedule-toohello-test-resourceschedule-runbook"></a>Lägga till en schema toohello ResourceSchedule Test-runbook
Följ dessa steg tooenable hello schema för hello Test ResourceSchedule runbook. Detta är hello-runbook som kontrollerar vilka virtuella datorer ska startas, stänga av eller vänster är.

1. Hello Azure-portalen, öppna ditt Automation-konto och klicka sedan på hello **Runbooks** panelen.
2. På hello **Test ResourceSchedule** bladet, klickar du på hello **scheman** panelen.
3. På hello **scheman** bladet, klickar du på **lägga till ett schema**.
4. På hello **scheman** bladet väljer **länka ett schema tooyour runbook**. Välj sedan **skapa ett nytt schema**.
5. På hello **nytt schema** bladet anger hello namn för det här schemat, till exempel: *HourlyExecution*.
6. För hello schema **starta**, ange hello start tid tooan timme ökning.
7. Välj **återkommande**, och sedan för **upprepas varje intervall**väljer **1 timme**.
8. Kontrollera att **ställa in ett utgångsdatum** har angetts för**nr**, och klicka sedan på **skapa** toosave nya schemat.
9. På hello **schema Runbook** alternativ bladet väljer **parametrar och körinställningar**. I hello Test ResourceSchedule **parametrar** bladet ange hello namn för din prenumeration i hello **SubscriptionName** fältet.  Detta är endast hello-parameter som krävs för hello runbook.  När du är klar klickar du på **OK**.

hello schemat för runbook bör se ut som följande hello när den har slutförts:

![Konfigurerade ResourceSchedule Test-runbook](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-hello-json-string"></a>Formatet hello JSON-strängen
Den här lösningen i grunden tar en JSON sträng med ett bestämt format och lägger till den som hello värde för en tagg kallas du schema. En runbook visas alla resursgrupper och virtuella datorer och identifierar hello scheman för varje virtuell dator.

hello slinga över hello virtuella datorer som har kopplade scheman och kontrollerar du vilka åtgärder som ska vidtas. hello följande är ett exempel på hur hello lösningar ska formateras:

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

Här är några detaljerad information om den här strukturen:

1. hello formatet för den här JSON-strukturen är optimerad toowork runt hello 256 tecken begränsning i ett enda Taggvärde i Azure.
2. *TzId* representerar hello tidszon hello virtuell dator. Detta ID kan erhållas med hjälp av hello TimeZoneInfo .NET-klass i ett PowerShell-session--**[System.TimeZoneInfo]:: GetSystemTimeZones()**.

   ![GetSystemTimeZones i PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * Veckodagar representeras med ett numeriskt värde på noll toosix. hello värdet noll är lika med söndag.
   * hello starttiden representeras med hello **S** attributet och dess värde är i ett 24-timmarsformat.
   * hello slutet eller avstängning representeras med hello **E** attributet och dess värde är i ett 24-timmarsformat.

     Om hello **S** och **E** attribut varje har värdet noll (0), hello virtuella datorn ska vara kvar i det nuvarande tillståndet hello tiden för utvärderingen.
3. Om du vill tooskip utvärderingen för en viss dag i veckan hello Lägg inte till ett avsnitt för att hello veckodag. Följande exempel endast måndag utvärderas i hello och hello andra hello veckodagar ignoreras:

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a>Taggen resursgrupper eller virtuella datorer
tooshut av virtuella datorer måste tootag hello virtuella datorer eller hello resursgrupper där de är placerade. Virtuella datorer som inte har en schema-tagg utvärderas inte. Därför är inte de starta eller stänga av.

Det finns två sätt tootag resursgrupper eller virtuella datorer med den här lösningen. Du kan göra det direkt från hello-portalen. Eller så kan du använda hello Lägg till ResourceSchedule, Update-ResourceSchedule och ta bort ResourceSchedule runbooks.

### <a name="tag-through-hello-portal"></a>Tagga hello-portalen
Följ dessa steg tootag en virtuell dator eller resursgrupp i hello portal:

1. Förenkla hello JSON-strängen och kontrollera att det inte finns några blanksteg.  JSON-strängen ska se ut så här:

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. Välj hello **taggen** ikon för en virtuell dator eller resurs gruppera tooapply schemat.

   ![Alternativet för VM-tagg](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. Taggar har definierats efter ett nyckel/värde-par. Typen **schema** i hello **nyckeln** fältet och sedan klistra in JSON-strängen för hello i hello **värdet** fältet. Klicka på **Spara**. Din nya taggen bör nu visas i hello lista med taggar för din resurs.

   ![Taggen för VM-schema](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a>Taggen från PowerShell
Alla importerade runbooks innehåller hjälpinformation hello början på hello-skript som beskriver hur tooexecute hello runbooks direkt från PowerShell. Du kan anropa hello Lägg till ScheduleResource och uppdatera ScheduleResource runbooks från PowerShell. Det gör du genom att skicka parametrar som gör att du toocreate eller uppdatera hello schema tagg på en virtuell dator eller resurs utanför hello-portalen.

toocreate, lägga till och ta bort taggar via PowerShell, måste du först för[konfigurera din PowerShell-miljö för Azure](/powershell/azure/overview). Du kan fortsätta med hello följande steg när du har slutfört hello-installationen.

### <a name="create-a-schedule-tag-with-powershell"></a>Skapa en schema-tagg med PowerShell
1. Öppna ett PowerShell-session. Använd sedan följande exempel tooauthenticate med Kör som-konto och toospecify en prenumeration hello:

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Definiera ett schema för hash-tabell. Här är ett exempel på hur det ska konstrueras:

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. Definiera hello-parametrar som krävs av hello runbook. I följande exempel hello, utvecklar vi en virtuell dator:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    Om du ange en resursgrupp, ta bort hello *VMName* parametern från hello $params hash-tabell enligt följande:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. Kör hello Lägg till ResourceSchedule runbook med följande parametrar toocreate hello schema taggen hello:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. tooupdate en virtuell dator eller grupp Resurstagg, köra hello **uppdatering ResourceSchedule** runbook med hello följande parametrar:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a>Ta bort en schema-tagg med PowerShell
1. Öppna ett PowerShell-session och kör följande tooauthenticate med Kör som-konto och tooselect hello och ange en prenumeration:

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Definiera hello-parametrar som krävs av hello runbook. I följande exempel hello, utvecklar vi en virtuell dator:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    Om du tar bort en tagg från en resursgrupp, ta bort hello *VMName* parametern från hello $params hash-tabell enligt följande:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. Kör hello ta bort ResourceSchedule runbook tooremove hello schema tagg:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. tooupdate en virtuell dator eller grupp Resurstagg, köra hello ta bort ResourceSchedule runbook med hello följande parametrar:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> Vi rekommenderar att du proaktivt övervaka dessa runbooks (och hello tillstånd för virtuell dator) tooverify dina virtuella datorer som ska stängas av och startas om i enlighet därmed.
>

tooview hello information om hello Test ResourceSchedule runbook jobbet i hello Azure-portalen väljer hello **jobb** för hello runbook. hello jobbet sammanfattning visar hello indataparametrar och hello utdata strömma, dessutom toogeneral information om hello jobbet och eventuella undantag om de inträffade.

Hej **jobbsammanfattning** innehåller meddelanden från hello utdata, varningar och fel dataströmmar. Välj hello **utdata** panelen tooview detaljerade resultat från hello runbook-körningen.

![Testa ResourceSchedule utdata](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a>Nästa steg
* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md).
* toolearn mer om runbook-typer och fördelar och begränsningar finns [Azure Automation runbook-typer](automation-runbook-types.md).
* Mer information om PowerShell-skript stödfunktioner finns [stöd för intern PowerShell-skriptet i Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
* toolearn mer om runbook-loggning och utdata, se [Runbook-utdata och meddelanden i Azure Automation](automation-runbook-output-and-messages.md).
* Mer om Azure kör som-konto och hur tooauthenticate runbooks med hjälp av det, se toolearn [autentisera runbooks med Kör som-kontot Azure](automation-sec-configure-azure-runas-account.md).
