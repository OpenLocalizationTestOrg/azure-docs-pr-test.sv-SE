---
title: "aaaAzure Automation resurser i OMS-lösningar | Microsoft Docs"
description: "Lösningar i OMS inkluderar vanligtvis runbooks i Azure Automation tooautomate processer, till exempel att samla in och bearbetning av övervakningsdata.  Den här artikeln beskriver hur tooinclude runbooks och deras relaterade resurser i en lösning."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 82a156f89bf77ce25e52e5e4596261ec07a24dae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-automation-resources-tooan-oms-management-solution-preview"></a>Lägga till Azure Automation resurser tooan OMS hanteringslösning (förhandsgranskning)
> [!NOTE]
> Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen. Ett schema som beskrivs nedan är ämne toochange.   


[Lösningar för hantering i OMS](operations-management-suite-solutions.md) inkluderar vanligtvis runbooks i Azure Automation tooautomate processer, till exempel att samla in och bearbetning av övervakningsdata.  Dessutom toorunbooks, Automation-konton innehåller tillgångar som variabler och scheman som stöder hello runbooks som används i hello-lösning.  Den här artikeln beskriver hur tooinclude runbooks och deras relaterade resurser i en lösning.

> [!NOTE]
> hello exempel i den här artikeln använder parametrar och variabler är antingen obligatorisk eller vanliga toomanagement lösningar som beskrivs i [och skapa lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du redan är bekant med hello följande information.

- Hur för[skapar en lösning för](operations-management-suite-solutions-creating.md).
- Hej struktur för en [lösningsfilen](operations-management-suite-solutions-solution-file.md).
- Hur för[skapar Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)

## <a name="automation-account"></a>Automation-konto
Alla resurser i Azure Automation finns i en [Automation-konto](../automation/automation-security-overview.md#automation-account-overview).  Enligt beskrivningen i [OMS arbetsytan och Automation-konto](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello Automation-konto ingår inte i hello hanteringslösning men det måste finnas innan hello lösningen är installerad.  Hello lösning installationen misslyckas om den inte är tillgänglig.

hello ingår varje Automation resurs hello namnet på dess Automation-konto.  Detta görs i hello lösningen med hello **accountName** parameter som hello följande exempel på en runbook-resurs.

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
Du bör innehålla alla runbooks som används av hello lösning i hello lösningsfilen så att de skapas när hello lösningen är installerad.  Du får inte innehålla hello brödtext hello runbook i hello mallen, så du bör publicera hello runbook tooa allmän plats där den kan nås av alla användare som installerar din lösning.

[Azure Automation-runbook](../automation/automation-runbook-types.md) resurser har en typ av **Microsoft.Automation/automationAccounts/runbooks** och ha hello följande struktur. Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn. 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


hello i den följande tabellen beskrivs hello egenskaper för runbooks.

| Egenskap | Beskrivning |
|:--- |:--- |
| runbookType |Anger hello hello runbook. <br><br> Skript - PowerShell-skript <br>PowerShell - PowerShell-arbetsflöde <br> GraphPowerShell - grafisk PowerShell-skriptet runbook <br> GraphPowerShellWorkflow - grafisk PowerShell-arbetsflödesrunbook |
| logProgress |Anger om [vidare poster](../automation/automation-runbook-output-and-messages.md) ska genereras för hello runbook. |
| logVerbose |Anger om [utförliga poster](../automation/automation-runbook-output-and-messages.md) ska genereras för hello runbook. |
| description |Valfri beskrivning för hello runbook. |
| publishContentLink |Anger hello innehållet i hello runbook. <br><br>URI - Uri toohello innehållet i hello runbook.  Det här är en .ps1-fil för PowerShell-skript och runbooks och en exporterade grafiska runbook-fil för en grafisk runbook.  <br> version - versionen av hello runbook för egna spårning. |


## <a name="automation-jobs"></a>Automation-jobb
När du startar en runbook i Azure Automation skapas ett automation-jobb.  Du kan lägga till en automation resurs tooyour lösning tooautomatically jobbstart en runbook när hello hanteringslösning installeras.  Den här metoden är brukar användas toostart runbooks som används för inledande konfiguration av hello lösning.  toostart en runbook med jämna mellanrum, skapa en [schema](#schedules) och en [Jobbschema](#job-schedules)

Resurser för jobbet har en typ av **Microsoft.Automation/automationAccounts/jobs** och ha hello följande struktur.  Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn. 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

hello egenskaper för automation-jobb beskrivs i följande tabell hello.

| Egenskap | Beskrivning |
|:--- |:--- |
| runbook |Enda namn entitet med hello namnet på hello runbook toostart. |
| parameters |Entitet för varje parametervärde som krävs av hello runbook. |

hello jobbet innehåller hello runbook-namn och eventuella parametern värden toobe skickas toohello runbook.  hello jobb bör [beror på](operations-management-suite-solutions-solution-file.md#resources) hello-runbook som startar sedan hello runbook måste skapas innan hello jobb.  Om du har flera runbooks som ska startas kan du definiera deras inbördes ordning genom att ett jobb är beroende av andra jobb som ska köras första.

hello namnet på en resurs för jobbet måste innehålla en GUID som tilldelas vanligtvis av en parameter.  Du kan läsa mer om GUID-parametrar i [och skapa lösningar i Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).  


## <a name="certificates"></a>Certifikat
[Azure Automation-certifikat](../automation/automation-certificates.md) har en typ av **Microsoft.Automation/automationAccounts/certificates** och ha hello följande struktur. Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



hello egenskaper för certifikat resurser beskrivs i följande tabell hello.

| Egenskap | Beskrivning |
|:--- |:--- |
| base64Value |Base 64-värde för hello certifikat. |
| tumavtrycket |Tumavtryck för certifikat som hello. |



## <a name="credentials"></a>Autentiseringsuppgifter
[Autentiseringsuppgifter för Azure Automation](../automation/automation-credentials.md) har en typ av **Microsoft.Automation/automationAccounts/credentials** och ha hello följande struktur.  Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn. 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

i hello i den följande tabellen beskrivs hello egenskaper för autentiseringsuppgifter resurser.

| Egenskap | Beskrivning |
|:--- |:--- |
| Användarnamn |Användarnamn för hello autentiseringsuppgifter. |
| lösenord |Lösenordet för hello autentiseringsuppgifter. |


## <a name="schedules"></a>Scheman
[Azure Automation-scheman](../automation/automation-schedules.md) har en typ av **Microsoft.Automation/automationAccounts/schedules** och ha hello hello följande struktur. Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

hello egenskaper för schema resurser beskrivs i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| description |Valfri beskrivning för hello schema. |
| startTime |Anger hello starttiden för ett schema som ett DateTime-objekt. En sträng kan tillhandahållas om det kan vara konverterade tooa giltigt DateTime. |
| IsEnabled |Anger om hello schema har aktiverats. |
| interval |hello typ av intervall för hello schema.<br><br>dag<br>timme |
| frequency |Frekvens som hello schemat ska utlösas i antal dagar eller timmar. |

Måste ha en starttid med ett större värde än hello aktuell tid.  Du kan inte ange det här värdet med en variabel eftersom du har ingen möjlighet att veta när den är pågående toobe installerad.

Använd någon av följande två olika metoder när du använder schemalägga resurser i en lösning hello.

- Använda en parameter för hello starttiden för hello schema.  Då hello användaren tooprovide ett värde när de installerar hello lösning.  Om du har flera scheman, kan du använda en enda parameter-värdet för fler än ett.
- Skapa hello scheman med hjälp av en runbook som startar när hello lösningen är installerad.  Detta tar bort hello behovet av hello användaren toospecify en gång, men du inte kan innehålla hello schema i din lösning så tas den bort när hello lösningen tas bort.


### <a name="job-schedules"></a>Jobbscheman
Jobbet schemalägga resurser länkar en runbook med ett schema.  De har en typ av **Microsoft.Automation/automationAccounts/jobSchedules** och ha hello hello följande struktur.  Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


hello egenskaper för jobbscheman beskrivs i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| namn på schema |Enskild **namn** entitet med hello namnet på hello schema. |
| Runbook-namn  |Enskild **namn** entitet med hello namnet på hello runbook.  |



## <a name="variables"></a>Variabler
[Azure Automation-variabler](../automation/automation-variables.md) har en typ av **Microsoft.Automation/automationAccounts/variables** och ha hello följande struktur.  Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

hello egenskaper för variabeln resurser beskrivs i följande tabell hello.

| Egenskap | Beskrivning |
|:--- |:--- |
| description | Valfri beskrivning för hello-variabeln. |
| isEncrypted | Anger om hello variabeln ska krypteras. |
| typ | Den här egenskapen har för närvarande ingen effekt.  hello datatypen för variabel hello bestäms av hello startvärde. |
| värde | Värdet för hello variabel. |

> [!NOTE]
> Hej **typen** egenskapen för närvarande har ingen effekt på hello variabel skapas.  hello datatyp för hello variabel bestäms av hello värde.  

Om du ställer in hello ursprungligt värde för variabeln hello måste den konfigureras som hello rätt datatyp.  hello följande tabell innehåller hello olika datatyper tillåten och deras syntax.  Observera att värdena i JSON förväntade tooalways stå inom citattecken med några specialtecken inom hello citattecken.  Till exempel ett strängvärde skulle anges med citattecken runt hello sträng (med hjälp av hello escape-tecken (\\)) när ett numeriskt värde skulle anges med en uppsättning av citattecken.

| Datatyp | Beskrivning | Exempel | Löser för|
|:--|:--|:--|:--|
| Sträng   | Värdet omges av dubbla citattecken.  | ”\"Hello world\"” | ”Hello world” |
| numeriskt  | Numeriskt värde med enkla citattecken.| "64" | 64 |
| Booleskt värde  | **SANT** eller **FALSKT** inom citattecken.  Observera att det här värdet måste vara gemener. | ”true” | SANT |
| Datum och tid | Serialiserad date-värde.<br>Du kan använda hello ConvertTo Json-cmdlet i PowerShell toogenerate värdet för ett visst datum.<br>Exempel: get-date ”2017-5/24 13:14:57” \| ConvertTo Json | ”\\/Date(1495656897378)\\/” | 2017-05-24 13:14:57 |

## <a name="modules"></a>Moduler
Management-lösningen behöver inte toodefine [global modul](../automation/automation-integration-modules.md) används av dina runbooks eftersom de alltid är tillgängliga i ditt Automation-konto.  Du behöver tooinclude en resurs för alla moduler som används av dina runbooks.

[Integreringsmoduler](../automation/automation-integration-modules.md) har en typ av **Microsoft.Automation/automationAccounts/modules** och ha hello följande struktur.  Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


i hello i den följande tabellen beskrivs hello egenskaper för modulen resurser.

| Egenskap | Beskrivning |
|:--- |:--- |
| contentLink |Anger hello innehållet i hello-modulen. <br><br>URI - Uri toohello innehållet i hello-modulen.  Det här är en .ps1-fil för PowerShell-skript och runbooks och en exporterade grafiska runbook-fil för en grafisk runbook.  <br> version - Version av hello-modulen för egna spårning. |

Hej runbook ska beror på hello modulen resurs tooensure som har skapats innan hello runbook.

### <a name="updating-modules"></a>Uppdatera moduler
Om du uppdaterar en lösning som innehåller en runbook som använder ett schema och hello ny version av lösningen har en ny modul som används av denna runbook, använda hello runbook hello gammal version av hello-modulen.  Du bör inkludera hello följande runbooks i din lösning och skapa ett jobb toorun dem före andra runbooks.  Se till att alla moduler som är uppdaterade som krävs innan hello runbooks har lästs in.

* [Uppdatera ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) säkerställer att alla hello-moduler som används av runbooks i din lösning är hello senaste versionen.  
* [ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) ska registrera om alla hello schema resurser tooensure att hello runbooks kopplade toothem med Använd hello senaste moduler.




## <a name="sample"></a>Exempel
Följande är ett exempel på en lösning som omfattar som innehåller hello följande resurser:

- Runbook.  Det här är en exempel-runbook som lagras i en offentlig GitHub-databas.
- Automation-jobb som startar hello runbook när hello lösningen är installerad.
- Schemat och jobb kan du schemalägga toostart hello runbook med jämna mellanrum.
- Certifikat.
- Autentiseringsuppgifter.
- Variabel.
- Modul.  Detta är hello [OMSIngestionAPI modulen](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) för att skriva data tooLog Analytics. 

Hej exempel använder [standardlösningen parametrar](operations-management-suite-solutions-solution-file.md#parameters) variabler som ofta används i en lösning som motsats toohardcoding värdena i resursdefinitionerna hello.


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for shedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello schedule link toorunbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a>Nästa steg
* [Lägg till en vy tooyour lösning](operations-management-suite-solutions-resources-views.md) toovisualize insamlade data.
