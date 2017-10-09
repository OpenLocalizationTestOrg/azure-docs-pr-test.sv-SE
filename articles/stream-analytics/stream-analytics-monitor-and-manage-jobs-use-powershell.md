---
title: aaaMonitor och hantera Stream Analytics-jobb med PowerShell | Microsoft Docs
description: "Lär dig hur toouse Azure PowerShell cmdlet: ar och toomonitor och hantera Stream Analytics-jobb."
keywords: Azure powershell, azure powershell-cmdlets, powershell-kommando, powershell-skript
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 514f454e-d18c-4081-8304-ab48577e15e8
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 44abc82f1c44a5ebc1701badd6547b84dac239b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Övervaka och hantera Stream Analytics-jobb med Azure PowerShell-cmdlets
Lär dig hur toomonitor och hantera Stream Analytics-resurser med Azure PowerShell-cmdlets och powershell-skript som kör grundläggande Stream Analytics-jobb.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Förutsättningar för att köra Azure PowerShell-cmdlets för Stream Analytics
* Skapa en Azure-resursgrupp i din prenumeration. hello följande är ett exempel Azure PowerShell-skript. Azure PowerShell information finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview);  

Azure PowerShell 0.9.8:  

         # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

         # Log in tooyour Azure account
        Login-AzureRmAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> Stream Analytics-jobb som skapats inte övervaka aktiverad som standard.  Du kan manuellt aktivera övervakning i hello Azure Portal genom att gå toohello jobbet övervakaren sidan och klicka på knappen för hello aktivera eller du kan göra detta via programmering genom att följa hello stegen finns i [Azure Stream Analytics - övervakaren dataström Analytics-jobb genom att programmera](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell-cmdlets för Stream Analytics
hello följande Azure PowerShell-cmdlets kan använda toomonitor och hantera Azure Stream Analytics-jobb. Observera att Azure PowerShell har olika versioner. 
**I hello exempel listade hello första kommandot avser Azure PowerShell 0.9.8, är hello andra kommandot för Azure PowerShell 1.0.** hello Azure PowerShell 1.0 kommandon har alltid ”AzureRM” i hello-kommandot.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Visar en lista över alla Stream Analytics-jobb som definierats i hello Azure-prenumeration eller angivna resursgruppen eller hämtar jobbinformation om ett specifikt jobb inom en resursgrupp.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Detta PowerShell-kommando returnerar information om alla hello Stream Analytics-jobb i hello Azure-prenumeration.

**Exempel 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Detta PowerShell-kommando returnerar information om alla hello Stream Analytics-jobb i hello resursgruppen StreamAnalytics-standard-Central-US.

**Exempel 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Detta PowerShell-kommando returnerar information om hello Stream Analytics-jobbet StreamingJob i hello resursgruppen StreamAnalytics-standard-Central-US.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Hämtar information om specifika indata eller en lista över alla hello indata som definieras i en angiven Stream Analytics-jobbet.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Detta PowerShell-kommando returnerar information om alla hello indata som definierats i hello jobbet StreamingJob.

**Exempel 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Detta PowerShell-kommando returnerar information om hello indata med namnet EntryStream som definierats i hello jobbet StreamingJob.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Visar en lista över hello utdata som definieras i en angiven Stream Analytics-jobbet eller hämtar information om en specifik utdata.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Detta PowerShell-kommando returnerar information om hello utdata som definierats i hello jobbet StreamingJob.

**Exempel 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Detta PowerShell-kommando returnerar information om hello utdata med namnet utdata som definierats i hello jobbet StreamingJob.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Hämtar information om hello kvoten för strömmande enheter i ett visst område.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Detta PowerShell-kommando returnerar information om hello kvoten och användning av enheter för strömning i hello centrala USA region.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Hämtar information om en omvandling definieras i ett Stream Analytics-jobb.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Detta PowerShell-kommando returnerar information om hello omvandling kallas StreamingJob hello jobb StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Nya AzureStreamAnalyticsInput | Ny AzureRMStreamAnalyticsInput
Skapar en ny inmatning inom ett Stream Analytics-jobb eller uppdaterar befintliga angivna indata.

hello kan namnet på hello indata anges i hello JSON-fil eller på hello-kommandoraden. Om båda anges måste hello namn på kommandoraden för hello vara hello samma som hello något i hello-filen.

Om du anger indata som redan finns och inte anger hello – Force-parametern hello cmdlet ber oavsett tooreplace hello befintliga indata.

Om du anger hello – Force-parametern och ange en befintlig ange namn, hello indata ersätts utan bekräftelse.

Detaljerad information om hello JSON-filstruktur och innehåll finns i toohello [skapa indata (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] avsnitt i hello [Stream Analytics Management REST API Referensbiblioteket][stream.analytics.rest.api.reference].

**Exempel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Detta PowerShell-kommando skapar en ny indata från hello filen Input.json. Om befintliga indata med hello-namnet som angetts i hello inkommande definitionsfilen har redan definierats hello cmdlet ber om huruvida tooreplace den.

**Exempel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Detta PowerShell-kommando skapar en ny inmatning hello jobb kallas EntryStream. Om en befintlig indata med det här namnet har redan definierats hello cmdlet ber om huruvida tooreplace den.

**Exempel 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Detta PowerShell-kommando ersätter hello definition av hello befintliga indatakälla kallas EntryStream hello definition från hello-fil.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Nya AzureStreamAnalyticsJob | Ny AzureRMStreamAnalyticsJob
Skapar ett nytt Stream Analytics-jobb i Microsoft Azure eller uppdaterar hello definitionen för en befintlig angivna jobbet.

hello namnet på hello jobb kan anges i hello JSON-fil eller på hello-kommandoraden. Om båda anges måste hello namn på kommandoraden för hello vara hello samma som hello något i hello-filen.

Om du anger ett namn som redan finns och inte anger hello – Force-parametern hello cmdlet ber oavsett tooreplace hello befintligt jobb.

Om du anger hello – Force-parametern och ange Jobbnamnet på en befintlig hello jobbdefinitionen ersätts utan bekräftelse.

Detaljerad information om hello JSON-filstruktur och innehåll finns i toohello [skapa Stream Analytics-jobbet] [ msdn-rest-api-create-stream-analytics-job] avsnitt i hello [Stream Analytics Management REST API-referens Biblioteket][stream.analytics.rest.api.reference].

**Exempel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Detta PowerShell-kommando skapar ett nytt jobb från hello definition i JobDefinition.json. Om ett befintligt projekt med hello-namnet som angetts i definitionsfilen för hello jobbet har redan definierats hello cmdlet ber om huruvida tooreplace den.

**Exempel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Detta PowerShell-kommando ersätter hello jobbdefinitionen för StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Nya AzureStreamAnalyticsOutput | Ny AzureRMStreamAnalyticsOutput
Skapar en ny utdata inom ett Stream Analytics-jobb eller uppdaterar befintliga utdata.  

hello namn för hello utdata kan anges i hello JSON-fil eller på hello-kommandoraden. Om båda anges måste hello namn på kommandoraden för hello vara hello samma som hello något i hello-filen.

Om du anger ett utgående som redan finns och inte anger hello – Force-parametern hello cmdlet ber oavsett tooreplace hello befintliga utdata.

Om du anger hello – Force-parametern och ange en befintlig utdata namn, hello utdata ersätts utan bekräftelse.

Detaljerad information om hello JSON-filstruktur och innehåll finns i toohello [skapa utdata (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] avsnitt i hello [Stream Analytics Management REST API Referensbiblioteket][stream.analytics.rest.api.reference].

**Exempel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Detta PowerShell-kommando skapar en ny utdata som kallas ”utdata” hello jobb StreamingJob. Om en befintlig utdata med det här namnet har redan definierats hello cmdlet ber om huruvida tooreplace den.

**Exempel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Detta PowerShell-kommando ersätter hello definitionen för ”utdata” hello jobb StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Nya AzureStreamAnalyticsTransformation | Ny AzureRMStreamAnalyticsTransformation
Skapar en ny omvandling inom ett Stream Analytics-jobb eller uppdaterar befintliga hello-omvandling.

hello namnet på hello omvandling kan anges i hello JSON-fil eller på hello-kommandoraden. Om båda anges måste hello namn på kommandoraden för hello vara hello samma som hello något i hello-filen.

Om du anger en omvandling som redan finns och inte anger hello – Force-parametern hello cmdlet ber oavsett tooreplace hello omvandling av befintliga.

Om du anger hello – Force-parametern och ange namnet på en befintlig omvandling hello omvandling ersätts utan bekräftelse.

Detaljerad information om hello JSON-filstruktur och innehåll finns i toohello [skapa omvandling (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] avsnitt i hello [Stream Analytics Management REST API-referensbiblioteket][stream.analytics.rest.api.reference].

**Exempel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Detta PowerShell-kommando skapar en ny omvandling kallas StreamingJobTransform hello jobb StreamingJob. Om en befintlig transformation har redan definierats med samma namn, hello cmdlet ber om huruvida tooreplace den.

**Exempel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Detta PowerShell-kommando ersätter hello definitionen av StreamingJobTransform hello jobb StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Ta bort AzureStreamAnalyticsInput | Ta bort AzureRMStreamAnalyticsInput
Tar bort specifika indata asynkront från en Stream Analytics-jobbet i Microsoft Azure.  
Om du anger hello – Force-parametern, hello indata kommer att tas bort utan bekräftelse.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Det här PowerShell-kommandot tar bort hello inkommande EventStream hello jobb StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Ta bort AzureStreamAnalyticsJob | Ta bort AzureRMStreamAnalyticsJob
Tar bort ett specifikt Stream Analytics-jobb i Microsoft Azure asynkront.  
Om du anger hello – Force-parametern hello-jobbet kommer att tas bort utan bekräftelse.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Detta PowerShell-kommando bort hello jobbet StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Ta bort AzureStreamAnalyticsOutput | Ta bort AzureRMStreamAnalyticsOutput
Tar bort en specifik utdata från ett Stream Analytics-jobb i Microsoft Azure asynkront.  
Om du anger hello – Force-parametern hello utdata kommer att tas bort utan bekräftelse.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Det här PowerShell-kommandot tar bort hello utdata utdata i hello jobbet StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob
Asynkront distribuerar och startar en Stream Analytics-jobb i Microsoft Azure.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Detta PowerShell-kommando startar hello jobbet StreamingJob med anpassade utdata starttid ange tooDecember 12, 2012, 12:12:12 UTC.

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Stoppa AzureStreamAnalyticsJob | Stoppa AzureRMStreamAnalyticsJob
Asynkront stoppar ett Stream Analytics-jobb körs i Microsoft Azure och frigör resurser som var som användes. hello förblir jobbdefinitionen och metadata tillgängliga i din prenumeration via både hello Azure-portalen och management API: er, så att hello jobb kan redigeras och startas om. Du debiteras inte för ett jobb i hello stoppats tillstånd.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Det här PowerShell-kommandot stoppar hello jobbet StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Testa AzureStreamAnalyticsInput | Testa AzureRMStreamAnalyticsInput
Testerna hello möjligheten för Stream Analytics tooconnect tooa angivna indata.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Det här PowerShell-kommandot tester hello anslutningsstatus hello indata EntryStream i StreamingJob.  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Testa AzureStreamAnalyticsOutput | Testa AzureRMStreamAnalyticsOutput
Testerna hello möjligheten för Stream Analytics tooconnect tooa angetts utdata.

**Exempel 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Det här PowerShell-kommandot tester hello anslutningsstatus hello utdata utdata i StreamingJob.  

## <a name="get-support"></a>Få support
För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

