---
title: aaaRunbook indataparametrar | Microsoft Docs
description: "Indataparametrarna för Runbook öka hello flexibilitet runbooks genom att låta dig toopass data tooa runbook när den startas. Den här artikeln beskrivs olika scenarier där indataparametrar används i runbooks."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 4d3dff2c-1f55-498d-9a0e-eee497e5bedb
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sngun
ms.openlocfilehash: f3abaf92382e7d41019616bafb14af23cf98dd9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-input-parameters"></a>Indataparametrar för Runbook
Indataparametrarna för Runbook öka hello flexibilitet runbooks genom att låta dig toopass data tooit när den startas. hello-parametrar kan hello runbook åtgärder toobe mål för specifika scenarier och miljöer. I den här artikeln går du igenom olika scenarier där indataparametrar används i runbooks.

## <a name="configure-input-parameters"></a>Konfigurera parametrar för indata
Indataparametrar kan konfigureras i PowerShell och PowerShell-arbetsflöde grafiska runbook-flöden. En runbook kan ha flera parametrar med olika datatyper eller inga parametrar alls. Ange parametrarna kan vara obligatoriska eller valfria, och du kan tilldela ett standardvärde för valfria parametrar. Du kan tilldela värden toohello indataparametrar för en runbook när du startar det genom en hello tillgängliga metoder. De här metoderna omfattar att starta en runbook från hello-portalen eller en webbtjänst. Du kan också starta som en underordnad runbook som anropas infogad i en annan runbook.

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a>Konfigurera indataparametrar i runbooks med PowerShell och PowerShell-arbetsflöde
PowerShell och [PowerShell-arbetsflöde runbooks](automation-first-runbook-textual.md) stöder indataparametrar som definierats via hello följande attribut i Azure Automation.  

| **Egenskap** | **Beskrivning** |
|:--- |:--- |
| Typ |Krävs. hello-datatyp som förväntas för hello parametervärde. .NET-typ är giltig. |
| Namn |Krävs. hello namnet på hello-parameter. Detta måste vara unika inom hello runbook och kan bara innehålla bokstäver, siffror eller understreck. Det måste börja med en bokstav. |
| Obligatorisk |Valfri. Anger om ett värde måste anges för hello-parametern. Om du väljer för**$true**, och sedan ett värde måste anges när hello runbook startas. Om du väljer för**$false**, och sedan ett värde är valfritt. |
| Standardvärde |Valfri.  Anger ett värde som ska användas för hello-parametern som ett värde inte skickas när hello runbook startas. Ett standardvärde kan anges för alla parametrar och görs automatiskt hello parametern valfria oavsett hello obligatoriska inställningen. |

Windows PowerShell stöder flera attribut för indataparametrarna än de som anges här, t.ex validering, alias, och parametern anger. Azure Automation stöder för närvarande endast hello indataparametrar som anges ovan.

En parameterdefinition i PowerShell-arbetsflöde runbooks har hello följande allmänna formulär där flera parametrar avgränsas med kommatecken.

   ```
     Param
     (
         [Parameter (Mandatory= $true/$false)]
         [Type] Name1 = <Default value>,

         [Parameter (Mandatory= $true/$false)]
         [Type] Name2 = <Default value>
     )
   ```

> [!NOTE]
> När du definierar parametrar, om du inte anger hello **obligatoriska** attribut, och sedan som standard hello parametern anses vara valfri. Även om du anger ett standardvärde för en parameter i PowerShell-arbetsflöde runbooks behandlas av PowerShell som en valfri parameter, oavsett hello **obligatoriska** attributvärdet.
> 
> 

Exempelvis ska vi konfigurera hello indataparametrar för en PowerShell-arbetsflödesrunbook som matar ut information om virtuella datorer, en enda virtuell dator eller alla virtuella datorer inom en resursgrupp. Denna runbook innehåller två parametrar som visas i följande skärmbild hello: hello namnet på virtuell dator och hello namnet på hello resursgrupp.

![Automation PowerShell-arbetsflöde](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

I den här parameterdefinitionen hello parametrar **$VMName** och **$resourceGroupName** är enkla parametrar av typen string. PowerShell och PowerShell-arbetsflöde runbooks stöder dock alla enkla typer och komplexa typer som **objekt** eller **PSCredential** för indataparametrarna.

Om din runbook har ett objekt Indataparametern, toopass i ett värde-par som sedan använda en PowerShell hash-tabell med (namn, värde). Till exempel om du har följande parameter i en runbook hello:

     [Parameter (Mandatory = $true)]
     [object] $FullName

Du kan sedan överföra hello följande värde toohello parameter:

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a>Konfigurera indataparametrar i grafiska runbook-flöden
för[konfigurerar en grafisk runbook](automation-first-runbook-graphical.md) med indataparametrar, skapar vi en grafisk runbook som matar ut information om virtuella datorer, antingen en enskild virtuell dator eller alla virtuella datorer inom en resursgrupp. Konfigurera en runbook består av två viktiga aktiviteter som beskrivs nedan.

[**Autentisera Runbooks med Kör som-kontot Azure** ](automation-sec-configure-azure-runas-account.md) tooauthenticate med Azure.

[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) tooget hello egenskaperna för en virtuella datorer.

Du kan använda hello [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) aktivitetsnamn toooutput hello av virtuella datorer. Hej aktiviteten **Get-AzureRmVm** accepterar två parametrar hello **virtuellt datornamn** och hello **resursgruppens namn**. Eftersom de här parametrarna kan kräver olika värden varje gång du startar hello runbook kan du lägga till indataparametrar tooyour runbook. Här följer stegen hello tooadd indataparametrar:

1. Välj hello grafisk runbook från hello **Runbooks** bladet och klicka sedan på [ **redigera** ](automation-graphical-authoring-intro.md) den.
2. Hej runbook-redigeraren, klicka på **ingående och utgående** tooopen hello **ingående och utgående** bladet.
   
    ![Grafisk Automation-runbook](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. Hej **ingående och utgående** bladet visar en lista över indataparametrar som definierats för hello runbook. På det här bladet kan du lägga till en ny Indataparametern eller redigera hello konfigurationen av en befintlig indataparameter. tooadd en ny parameter för hello runbook, klickar du på **lägga till indata** tooopen hello **runbookinmatningsparameter** bladet. Där kan konfigurera du hello följande parametrar:
   
   | **Egenskap** | **Beskrivning** |
   |:--- |:--- |
   | Namn |Krävs.  hello namnet på hello-parameter. Detta måste vara unika inom hello runbook och kan bara innehålla bokstäver, siffror eller understreck. Det måste börja med en bokstav. |
   | Beskrivning |Valfri. Beskrivning av hello syftet indataparameter. |
   | Typ |Valfri. hello-datatyp som förväntas för hello parametervärde. Parametrar som stöds är **sträng**, **Int32**, **Int64**, **Decimal**, **booleskt**,  **DateTime**, och **objektet**. Om datatypen inte är markerat används som standard för**sträng**. |
   | Obligatorisk |Valfri. Anger om ett värde måste anges för hello-parametern. Om du väljer **Ja**, och sedan ett värde måste anges när hello runbook startas. Om du väljer **inga**, och sedan ett värde inte krävs när hello runbook startas och du kan endast ange ett standardvärde. |
   | Standardvärde |Valfri. Anger ett värde som ska användas för hello-parametern som ett värde inte skickas när hello runbook startas. Ett standardvärde kan anges för en parameter som inte är obligatoriskt. tooset ett standardvärde, Välj **anpassad**. Det här värdet används om inte ett annat värde anges när hello runbook startas. Välj **ingen** om du inte vill tooprovide något standardvärde. |
   
    ![Lägga till nya indata](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. Skapa två parametrar med följande egenskaper kommer att användas av hello hello **Get-AzureRmVm** aktiviteten:
   
   * **Parameter1:**
     
     * Namn - VMName
     * Typ - sträng
     * Obligatoriska - Nej
   * **Parameter2:**
     
     * Namn - resourceGroupName
     * Typ - sträng
     * Obligatoriska - Nej
     * Standardvärde - anpassad
     * Anpassade standardvärde - \<namnet på hello resursgruppen som innehåller hello virtuella datorer >
5. När du lägger till hello parametrar, klickar du på **OK**.  Du kan nu visa dem i hello **indata och utdata bladet**. Klicka på **OK** igen, och klicka sedan på **spara** och **publicera** din runbook.

## <a name="assign-values-tooinput-parameters-in-runbooks"></a>Tilldela värden tooinput parametrar i runbooks
Du kan skicka värden tooinput parametrar i runbooks i hello följande scenarier.

### <a name="start-a-runbook-and-assign-parameters"></a>Starta en runbook och tilldela parametrar
En runbook kan startas på många olika sätt: genom hello Azure-portalen med en webhook, med PowerShell-cmdletar, med hello REST API eller med hello SDK. Nedan diskuterar vi olika metoder för att starta en runbook och tilldela parametrar.

#### <a name="start-a-published-runbook-by-using-hello-azure-portal-and-assign-parameters"></a>Starta en publicerad runbook med hjälp av hello Azure-portalen och tilldela parametrar
När du [starta hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), hello **starta Runbook** blad öppnas och du kan konfigurera värden för hello-parametrar som du nyss skapade.

![Börja använda hello-portalen](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

Du kan se hello-attribut som har angetts för parametern hello hello etikett under hello textrutan. Attribut är obligatoriska eller valfria, typ och standardvärdet. I hello hjälp pratbubblor nästa toohello parameternamn ser du alla hello nyckelinformation som du behöver toomake beslut om inkommande parametervärden. Informationen omfattar om en parameter är obligatoriska eller valfria. Den innehåller också hello typ och standardvärdet (eventuella) och annan användbar information.

![Hjälp pratbubblor](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> Stöd för parametrar av typen String **tom** String värden.  Ange **[EmptyString]** i hello Indataparametern rutan skickar en tom sträng toohello-parameter. Dessutom stöder inte typen strängparametrar **Null** värden som skickas. Om du inte klarar alla värdet toohello strängparameter sedan tolkas PowerShell det som null.
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>Starta en publicerad runbook med hjälp av PowerShell-cmdlets och tilldela parametrar
* **Azure Resource Manager-cmdlets:** du kan starta en Automation-runbook som har skapats i en resursgrupp med hjälp av [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).
  
  **Exempel:**
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* **Azure Service Management-cmdlets:** du kan starta en automation-runbook som har skapats i en standard-resursgrupp med hjälp av [Start AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).
  
  **Exempel:**
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> När du startar en runbook med hjälp av PowerShell-cmdletar, en standardparameter **MicrosoftApplicationManagementStartedBy** skapas med hello värdet **PowerShell**. Du kan visa den här parametern i hello **Jobbdetaljer** bladet.  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>Starta en runbook med hjälp av ett SDK och tilldela parametrar
* **Azure Resource Manager-metod:** du kan starta en runbook med hjälp av hello SDK för ett programmeringsspråk. Nedan visas ett C# kodfragment för att starta en runbook i Automation-konto. Du kan visa alla hello kod i vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).  
  
  ```
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```
* **Azure Service Management-metod:** du kan starta en runbook med hjälp av hello SDK för ett programmeringsspråk. Nedan visas ett C# kodfragment för att starta en runbook i Automation-konto. Du kan visa alla hello kod i vår [GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).
  
  ```      
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```
  
  toostart den här metoden skapar en ordlista toostore hello runbook-parametrar, **VMName** och **resourceGroupName**, och deras värden. Starta hello runbook. Nedan visas hello C# kodfragment för att anropa hello-metod som har definierats ovan.
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters toohello dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call hello StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-hello-rest-api-and-assign-parameters"></a>Starta en runbook med hjälp av hello REST-API och tilldela parametrar
Ett runbook-jobb kan skapas och igång med hello Azure Automation REST-API med hjälp av hello **PLACERA** metod med hello följande begärande-URI.

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

I hello Begärd URI, ersätter du hello följande parametrar:

* **prenumerations-id:** ditt Azure-prenumeration-ID.  
* **molntjänstnamnet-:** hello namnet på hello molnet toowhich hello begäran ska skickas.  
* **Automation-kontonamn:** hello namnet på ditt automation-konto som ligger inom hello angivna Molntjänsten.  
* **jobb-id:** hello GUID för hello jobbet. GUID i PowerShell kan skapas med hjälp av hello **[GUID]::NewGuid(). ToString()** kommando.

Använd hello begärandetexten i ordning toopass parametrar toohello runbook-jobbet. Det tar hello följande två egenskaper i JSON-format:

* **Runbook-namn:** krävs. hello namn på hello runbook hello jobbet toostart.  
* **Runbook-parametrar:** valfritt. En ordlista med hello parameterlista i (namn, värde) format där namnet ska vara av typen sträng och värdet kan vara ett giltigt JSON-värde.

Om du vill toostart hello **Get-AzureVMTextual** runbook som har skapats tidigare med **VMName** och **resourceGroupName** som parametrar och använda hello följande JSON-format för hello frågans brödtext.

   ```
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WSVMClassic",
         "resourceGroupName":”WSCS1”}
        }
    }
   ```

En HTTP-statuskod 201 returneras om hello jobbet har skapats. Mer information om svarshuvuden och hello svarstexten finns toohello artikel om hur för[skapa ett runbook-jobb med hjälp av hello REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)

### <a name="test-a-runbook-and-assign-parameters"></a>Testa en runbook och tilldela parametrar
När du [test hello utkastversionen för din runbook](automation-testing-runbook.md) med alternativet hello test hello **testa** blad öppnas och du kan konfigurera värden för hello-parametrar som du nyss skapade.

![Testa och tilldela parametrar](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-tooa-runbook-and-assign-parameters"></a>Länka ett schema tooa runbook och tilldela parametrar
Du kan [länka ett schema](automation-schedules.md) tooyour runbook så att hello runbook startar vid en viss tid. Du kan tilldela indataparametrar när du skapar hello schema och hello runbook ska använda dessa värden när den startas efter hello schema. Du kan inte spara hello schema förrän alla obligatoriska parametervärden har angetts.

![Schemalägga och tilldela parametrar](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Skapa en webhook för en runbook och tilldela parametrar
Du kan skapa en [webhook](automation-webhooks.md) för din runbook och konfigurera indataparametrarna för runbook. Du kan inte spara hello webhook förrän alla obligatoriska parametervärden har angetts.

![Skapa webhook och tilldela parametrar](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

När du kör en runbook med hjälp av en webhook hello fördefinierade Indataparametern  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  skickas tillsammans med hello indataparametrar som du har definierat. Du kan klicka på tooexpand hello **WebhookData** parameter för mer information.

![WebhookData-parameter](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a>Nästa steg
* Mer information om runbook ingående och utgående finns [Azure Automation: runbook indata, utdata och kapslade runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).
* Mer information om olika sätt toostart en runbook finns [starta en runbook](automation-starting-a-runbook.md).
* tooedit en textrepresentation runbook finns för[redigera textrepresentation runbooks](automation-edit-textual-runbook.md).
* tooedit en grafisk runbook finns för[grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md).

