---
title: "aaaSaved sökningar och aviseringar i OMS-lösningar | Microsoft Docs"
description: "Lösningar i OMS inkluderar vanligtvis sparade sökningar i logganalys tooanalyze data som samlas in av hello-lösning.  De kan också definiera aviseringar toonotify hello användaren eller automatiskt utföra åtgärder i svaret tooa kritiska problem.  Den här artikeln beskriver hur toodefine logganalys sparade sökningar och aviseringar i en ARM-mall så att de kan tas med i hanteringslösningar."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 93d7c5bbf061473833ca6c0a8e4d8e10d923f3ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-log-analytics-saved-searches-and-alerts-toooms-management-solution-preview"></a>Lägga till logganalys sparade sökningar och aviseringar tooOMS lösning (förhandsgranskning)

> [!NOTE]
> Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen. Ett schema som beskrivs nedan är ämne toochange.   


[Lösningar för hantering i OMS](operations-management-suite-solutions.md) inkluderar vanligtvis [sparade sökningar](../log-analytics/log-analytics-log-searches.md) i logganalys tooanalyze data som samlas in av hello-lösning.  De kan också definiera [aviseringar](../log-analytics/log-analytics-alerts.md) toonotify hello användaren eller automatiskt utföra åtgärder i svaret tooa kritiska problem.  Den här artikeln beskriver hur toodefine logganalys sparade sökningar och aviseringar i en [resurshantering mallen](../resource-manager-template-walkthrough.md) så att de kan tas med i [hanteringslösningar](operations-management-suite-solutions-creating.md).

> [!NOTE]
> hello exempel i den här artikeln använder parametrar och variabler är antingen obligatorisk eller vanliga toomanagement lösningar som beskrivs i [och skapa lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)  

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du redan är bekant med för[skapar en lösning för](operations-management-suite-solutions-creating.md) och hello struktur för ett [ARM-mallen](../resource-group-authoring-templates.md) och lösningsfilen.


## <a name="log-analytics-workspace"></a>Log Analytics-arbetsyta
Alla resurser i logganalys finns i en [arbetsytan](../log-analytics/log-analytics-manage-access.md).  Enligt beskrivningen i [OMS arbetsytan och Automation-konto](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello arbetsytan ingår inte i hello hanteringslösning men det måste finnas innan hello lösningen är installerad.  Hello lösning installationen misslyckas om den inte är tillgänglig.

hello heter hello arbetsytan i hello namn för varje logganalys-resurs.  Detta görs i hello lösningen med hello **arbetsytan** parameter som hello följande exempel på en savedsearch resurs.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"


## <a name="saved-searches"></a>Sparade sökningar
Inkludera [sparade sökningar](../log-analytics/log-analytics-log-searches.md) i en lösning tooallow användare tooquery data som samlas in av din lösning.  Sparade sökningar visas under **Favoriter** i hello OMS-portalen och **sparade sökningar** i hello Azure-portalen.  En sparad sökning krävs också för varje avisering.   

[Logganalys sparad sökning](../log-analytics/log-analytics-log-searches.md) resurser har en typ av `Microsoft.OperationalInsights/workspaces/savedSearches` och ha hello följande struktur.  Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn. 

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }



Var och en av hello egenskaperna för en sparad sökning beskrivs i hello i den följande tabellen. 

| Egenskap | Beskrivning |
|:--- |:--- |
| category | hello kategori för hello sparad sökning.  Alla sparade sökningar i hello som delar samma lösning ofta en enda kategori så att de grupperas tillsammans i hello-konsolen. |
| visningsnamn | Namnet toodisplay för hello sparad sökning i hello-portalen. |
| DocumentDB | Fråga toorun. |

> [!NOTE]
> Du kan behöva toouse escape-tecken i hello fråga om den innehåller tecken som kan tolkas som JSON.  Om din fråga var till exempel **typ: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write”**, bör vara skriven i hello lösningsfilen som **typ: AzureActivity OperationName:\" Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Aviseringar
[Logga Analytics varningar](../log-analytics/log-analytics-alerts.md) skapas av Varningsregler som kör en sparad sökning med regelbundna intervall.  Om hello resultaten av hello-frågan matchar angivet villkor, skapas en avisering post och en eller flera åtgärder körs.  

Varningsregler i en hanteringslösning består av följande tre olika resurser hello.

- **Sparad sökning.**  Definierar hello loggen sökning som ska köras.  Flera Varningsregler kan dela en sparad sökning.
- **Schema.**  Definierar hur ofta hello loggen sökningen ska köras.  Varje varningsregeln ska ha ett och endast ett schema.
- **Åtgärd för aviseringen.**  Varje regel för varning har en åtgärd resurs med en typ av **avisering** som definierar hello information om hello aviseringen, till exempel hello kriterier för när en avisering ska skapas och hello aviseringens allvarlighetsgrad.  hello åtgärd resursen kommer du även definiera ett e-post och runbook-svar.
- **Webhook-åtgärd (valfritt).**  Om hello varningsregeln ska anropa en webhook, så det krävs en ytterligare åtgärd resurs med en typ av **Webhook**.    

Sparad sökning resurser som beskrivs ovan.  hello beskrivs andra resurser som nedan.


### <a name="schedule-resource"></a>Schema för resurs

En sparad sökning kan ha en eller flera scheman med ett schema som representerar en separat avisering regel. hello definierar schema hur ofta hello sökning körs och hello tidsintervall under vilka hello data hämtas.  Schemalägga resurser har en typ av `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` och ha hello följande struktur. Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn. 


    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }



hello egenskaper för schema resurser beskrivs i hello i den följande tabellen.

| Elementnamn | Krävs | Beskrivning |
|:--|:--|:--|
| aktiverad       | Ja | Anger om hello avisering aktiveras när den skapas. |
| interval      | Ja | Hur ofta hello fråga körs i minuter. |
| QueryTimeSpan | Ja | Lång tid i minuter under vilka tooevaluate resultat. |

hello schema resursen ska beror på hello sparad sökning så att den har skapats innan hello schema.


### <a name="actions"></a>Åtgärder
Det finns två typer av åtgärden resursen som anges av hello **typen** egenskapen.  Ett schema kräver en **avisering** åtgärd som definierar hello information om hello varningsregeln och vilka åtgärder som vidtas när en avisering skapas.  Det kan också innehålla en **Webhook** åtgärd om en webhook ska anropas från hello avisering.  

Åtgärden resurser har en typ av `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.  

#### <a name="alert-actions"></a>Aviseringsåtgärder

Varje schemat har en **avisering** åtgärd.  Detta definierar hello information om hello aviseringen och eventuellt meddelande- och reparationsloggarna åtgärder.  Ett meddelande skickas ett e-tooone eller flera adresser.  En startar en runbook i Azure Automation tooattempt tooremediate hello identifierat problemet.

Aviseringsåtgärder har hello följande struktur.  Detta inkluderar vanliga parametrarna och variablerna så att du kan kopiera och klistra in det här kodstycket i din lösningsfilen och ändra hello parameternamn. 



    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "[variables('Alert').Name]",
            "description": "[variables('Alert').Description]",
            "severity": "[variables('Alert').Severity]",
            "threshold": {
                "operator": "[variables('Alert').Threshold.Operator]",
                "value": "[variables('Alert').Threshold.Value]",
                "metricsTrigger": {
                    "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                    "operator": "[variables('Alert').Trigger.Operator]",
                    "value": "[variables('Alert').Trigger.Value]"
                },
            },
            "emailNotification": {
                "recipients": [
                    "[variables('Alert').Recipients]"
                ],
                "subject": "[variables('Alert').Subject]"
            },
            "remediation": {
                "runbookName": "[variables('Alert').Remedition.RunbookName]",
                "webhookUri": "[variables('Alert').Remedition.WebhookUri]"
            }
        }
    }

hello egenskaper för Aviseringsåtgärd resurser beskrivs i följande tabeller hello.

| Elementnamn | Krävs | Beskrivning |
|:--|:--|:--|
| Typ | Ja | Typ av hello-åtgärd.  Detta blir **avisering** för aviseringsåtgärder. |
| Namn | Ja | Visningsnamn för hello aviseringen.  Detta är hello-namnet som visas i hello varningsregeln hello-konsolen. |
| Beskrivning | Nej | Valfri beskrivning av hello avisering. |
| Allvarsgrad | Ja | Allvarlighetsgrad för aviseringen hello-posten från hello följande värden:<br><br> **Kritiska**<br>**Varning**<br>**Information** |


##### <a name="threshold"></a>Tröskelvärde
Det här avsnittet krävs.  Den definierar hello egenskaper för hello tröskelvärde.

| Elementnamn | Krävs | Beskrivning |
|:--|:--|:--|
| Operatorn | Ja | Operatorn hello jämförelse från hello följande värden:<br><br>**gt = större än<br>lt = mindre än** |
| Värde | Ja | hello värdet toocompare hello resultat. |


##### <a name="metricstrigger"></a>MetricsTrigger
Det här avsnittet är valfritt.  Inkludera det för ett mått mätning aviseringen.

> [!NOTE]
> Mått mätning aviseringar är för närvarande i förhandsversion. 

| Elementnamn | Krävs | Beskrivning |
|:--|:--|:--|
| TriggerCondition | Ja | Anger om hello tröskelvärde för totala antalet överträdelser eller på varandra följande överträdelser från hello följande värden:<br><br>**Totalt antal<br>i följd** |
| Operatorn | Ja | Operatorn hello jämförelse från hello följande värden:<br><br>**gt = större än<br>lt = mindre än** |
| Värde | Ja | Antalet hello tider hello villkor måste vara uppfyllda tootrigger hello avisering. |

##### <a name="throttling"></a>Begränsning
Det här avsnittet är valfritt.  Inkludera det här avsnittet om du vill toosuppress aviseringar från hello samma regel för vissa tidsperiod när en avisering har skapats.

| Elementnamn | Krävs | Beskrivning |
|:--|:--|:--|
| DurationInMinutes | Ja om begränsning element som ingår | Antal minuter toosuppress aviseringar efter en från hello samma varningsregeln har skapats. |

##### <a name="emailnotification"></a>EmailNotification
 Det här avsnittet är valfria inkludera den om du vill att hello avisering toosend e tooone eller flera mottagare.

| Elementnamn | Krävs | Beskrivning |
|:--|:--|:--|
| mottagare | Ja | Kommaavgränsad lista med e-adresser toosend meddelande när en avisering skapas som i följande exempel hello.<br><br>**[ "recipient1@contoso.com", "recipient2@contoso.com" ]** |
| Ämne | Ja | Ämnesrad hello e-post. |
| Bifogad fil | Nej | Bifogade filer stöds inte för närvarande.  Om det här elementet finns det ska vara **ingen**. |


##### <a name="remediation"></a>Reparation
Det här avsnittet är valfritt att inkludera det om du vill att en runbook toostart i svaret toohello avisering. |

| Elementnamn | Krävs | Beskrivning |
|:--|:--|:--|
| RunbookName | Ja | Namnet på hello runbook toostart. |
| WebhookUri | Ja | URI för hello webhook för hello runbook. |
| Förfallodatum | Nej | Datum och tid då hello reparation upphör att gälla. |

#### <a name="webhook-actions"></a>Webhook-åtgärder

Webhook-åtgärder kan du starta en process genom att anropa en URL och du kan också tillhandahålla en nyttolast toobe skickas. De är liknande tooRemediation åtgärder förutom de är avsedda för webhooks som kan anropa processer än Azure Automation-runbooks. De ger också hello ytterligare alternativ för att tillhandahålla en nyttolast toobe levereras toohello fjärrprocess.

Om aviseringen ska anropa en webhook, så den behöver en åtgärd resurs med en typ av **Webhook** i tillägg toohello **avisering** åtgärd resurs.  

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }

hello egenskaper för Webhook åtgärd resurser beskrivs i följande tabeller hello.

| Elementnamn | Krävs | Beskrivning |
|:--|:--|:--|
| typ | Ja | Typ av hello-åtgärd.  Detta blir **Webhook** för webhook-åtgärder. |
| namn | Ja | Visningsnamn för hello-åtgärd.  Detta visas inte i hello-konsolen. |
| wehookUri | Ja | URI för hello webhooken. |
| CustomPayload | Nej | Anpassad nyttolast toobe skickas toohello webhooken. hello format beror på vilken hello webhook förväntas. |




## <a name="sample"></a>Exempel

Följande är ett exempel på en lösning som omfattar som innehåller hello följande resurser:

- Sparad sökning
- Schema
- Aviseringsåtgärden
- Webhook-åtgärd

Hej exempel använder [standardlösningen parametrar](operations-management-suite-solutions-solution-file.md#parameters) variabler som ofta används i en lösning som motsats toohardcoding värdena i resursdefinitionerna hello.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "accountName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Automation account"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "regionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Automation account"
            }
          },
          "pricingTier": {
            "type": "string",
            "metadata": {
              "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
          },
          "recipients": {
            "type": "string",
            "metadata": {
              "Description": "List of recipients for hello email alert separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion": "2015-11-01-preview",
    
          "MySearch": {
            "displayName": "Error records by hour",
            "query": "Type=MyRecord_CL | measure avg(Rating_d) by Instance_s interval 60minutes",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "Critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "Notification": {
              "Recipients": [
                "[parameters('recipients')]"
              ],
              "Subject": "Sample alert"
            },
            "Remediation": {
              "RunbookName": "MyRemediationRunbook",
              "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=TluBFH3GpX4IEAnFoImoAWLTULkjD%2bTS0yscyrr7ogw%3d"
            },
            "Webhook": {
              "Name": "MyWebhook",
              "Uri": "https://MyService.com/webhook",
              "Payload": "{\"field1\":\"value1\",\"field2\":\"value2\"}"
            }
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
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
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
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
              "EmailNotification": {
                "Recipients": "[variables('MyAlert').Notification.Recipients]",
                "Subject": "[variables('MyAlert').Notification.Subject]",
                "Attachment": "None"
              },
              "Remediation": {
                "RunbookName": "[variables('MyAlert').Remediation.RunbookName]",
                "WebhookUri": "[variables('MyAlert').Remediation.WebhookUri]"
              }
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Webhook.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]",
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name, '/actions/',variables('MyAlert').Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Webhook",
              "Name": "[variables('MyAlert').Webhook.Name]",
              "WebhookUri": "[variables('MyAlert').Webhook.Uri]",
              "CustomPayload": "[variables('MyAlert').Webhook.Payload]"
            }
          }
        ]
    }


hello följande parameterfilen ger exempel värden för den här lösningen.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "recipients": {
                "value": "recipient1@contoso.com;recipient2@contoso.com"
            }
        }
    }


## <a name="next-steps"></a>Nästa steg
* [Lägga till vyer](operations-management-suite-solutions-resources-views.md) tooyour hanteringslösning.
* [Lägg till Automation-runbooks och andra resurser](operations-management-suite-solutions-resources-automation.md) tooyour hanteringslösning.

