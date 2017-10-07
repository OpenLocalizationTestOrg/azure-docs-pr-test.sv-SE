---
title: "HDInsight-aaaCustomize kluster med skriptåtgärder - Azure | Microsoft Docs"
description: "Lägga till anpassade komponenter tooLinux-baserade HDInsight-kluster med skriptåtgärder. Skriptåtgärder avses Bash-skript som kan använda toocustomize hello klusterkonfigurationen eller lägga till ytterligare tjänster och verktyg som Hue, Solr eller R."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48e85f53-87c1-474f-b767-ca772238cc13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: ff22680a8a50b21985f6941f1edaf1dcf863d13f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-action"></a>Anpassa Linux-baserat HDInsight-kluster med skriptåtgärder

HDInsight tillhandahåller ett konfigurationsalternativ som kallas **skriptåtgärd** som anropar anpassade skript för att anpassa hello klustret. Dessa skript är används tooinstall ytterligare komponenter och ändra konfigurationsinställningarna. Skriptåtgärder kan användas under eller efter att klustret har skapats.

> [!IMPORTANT]
> hello möjlighet toouse skriptåtgärder på ett kluster som redan körs är endast tillgängligt för Linux-baserade HDInsight-kluster.
>
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Skriptåtgärder kan också vara publicerade toohello Azure Marketplace som ett HDInsight-program. Vissa av hello exemplen i det här dokumentet visar hur du kan installera ett HDInsight-program med hjälp av åtgärden skriptkommandon från PowerShell och hello .NET SDK. Mer information om HDInsight-program finns [publicera HDInsight-program hello Azure Marketplace](hdinsight-apps-publish-applications.md).

## <a name="permissions"></a>Behörigheter

Om du använder en domänansluten HDInsight-kluster, finns det två Ambari-behörigheter som krävs vid användning av skriptåtgärder med hello-kluster:

* **AMBARI. KÖR\_anpassad\_kommandot**: hello Ambari administratörsroll har denna behörighet som standard.
* **KLUSTER. KÖR\_anpassad\_kommandot**: båda hello HDInsight-klustrets administratör och Ambari-administratören har denna behörighet som standard.

Mer information om hur du arbetar med behörigheter med domänanslutna HDInsight finns [hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-manage.md).

## <a name="access-control"></a>Åtkomstkontroll

Om du inte är Hej administratör/ägaren av din Azure-prenumeration, ditt konto måste ha minst **deltagare** åtkomst toohello resursgruppen som innehåller hello HDInsight-kluster.

Dessutom, om du skapar ett HDInsight-kluster någon med minst **deltagare** åtkomst toohello Azure-prenumeration måste redan har registrerat hello provider för HDInsight. Providerregistrering sker när en användare med deltagare åtkomst toohello prenumeration skapar en resurs för hello första gången på hello prenumeration. Detta kan också inträffa utan att en resurs skapas, om en [leverantör registreras med REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Mer information om hur du arbetar med access management finns i hello följande dokument:

* [Kom igång med åtkomsthantering i hello Azure-portalen](../active-directory/role-based-access-control-what-is.md)
* [Använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](../active-directory/role-based-access-control-configure.md)

## <a name="understanding-script-actions"></a>Förstå skriptåtgärder

En skriptåtgärd är bara ett Bash-skript som du anger en URI för och parametrar för. hello skriptet körs på noderna i hello HDInsight-kluster. hello följande är egenskaper och funktioner för skriptåtgärder.

* Måste vara lagrade på en URI som är tillgänglig från hello HDInsight-kluster. hello följande är möjliga lagringsplatser:

    * En **Azure Data Lake Store** konto som kan nås av hello HDInsight-kluster. Information om hur du använder Azure Data Lake Store med HDInsight finns i [skapar ett HDInsight-kluster med Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

        När du använder ett skript som lagras i Data Lake Store hello URI-formatet är `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.

        > [!NOTE]
        > hello service principal HDInsight använder tooaccess Data Lake Store måste ha läsbehörighet toohello skript.

    * En blobb i en **Azure Storage-konto** som är antingen hello primära eller ytterligare lagringskonto för hello HDInsight-kluster. HDInsight har beviljats åtkomst tooboth för dessa typer av lagringskonton när klustret skapas.

    * En offentlig fildelning tjänsten Azure Blob, GitHub, OneDrive, Dropbox, t.ex.

        Till exempel URI: er, se hello [exempelskript skript åtgärd](#example-script-action-scripts) avsnitt.

        > [!WARNING]
        > HDInsight stöder endast __allmänna__ Azure Storage-konton. Den stöder för närvarande inte hello __Blob storage__ kontotyp.

* Kan vara begränsade för**körs på vissa nodtyper**, för exempel huvudnoderna eller arbetsnoderna.

  > [!NOTE]
  > När det används med HDInsight Premium kan du ange att hello skript ska användas på hello kantnod.

* Kan vara **beständiga** eller **ad hoc-**.

    **Beständiga** skript är tillämpade tooworker noder tillagda toohello kluster när hello skriptet körs. Till exempel när du ökar hello klustret.

    Ett bestående skript kan också använda ändringar nodtypen tooanother, till exempel en huvudnod.

  > [!IMPORTANT]
  > Beständiga skriptåtgärder måste ha ett unikt namn.

    **Ad hoc-** skript är inte beständiga. De är inte tillämpade tooworker noder tillagda toohello kluster när hello skriptet har körts. Du kan därefter befordra en ad hoc-skriptet tooa bestående skript eller nedgradera ett bestående skript tooan ad hoc-skript.

  > [!IMPORTANT]
  > Skriptåtgärder användas när klustret skapas sparas automatiskt.
  >
  > Skript som misslyckas inte är beständiga, även om du uttryckligen har angett att de ska.

* Kan acceptera **parametrar** som används av hello skript under körning.
* Kör med **rätt behörighet för rotmappen** på hello klusternoder.
* Kan användas via hello **Azure-portalen**, **Azure PowerShell**, **Azure CLI**, eller **HDInsight .NET SDK**

hello klustret sparar en historik över alla skript som har varit kördes. hello historik är användbart när du behöver toofind hello-ID för ett skript för befordran eller degradering åtgärder.

> [!IMPORTANT]
> Det finns inget automatiskt sätt tooundo hello ändringar som gjorts av en skriptåtgärd. Antingen manuellt återställa hello eller ange ett skript som kastar dem.


### <a name="script-action-in-hello-cluster-creation-process"></a>Skriptåtgärder i hello klusterskapandeprocessen

Används när klustret skapas med skriptåtgärder avses skiljer sig från skriptet åtgärder som kördes på ett befintligt kluster:

* hello skript är **automatiskt beständiga**.
* En **fel** i hello skript kan orsaka hello klustret skapas processen toofail.

hello följande diagram illustrerar när skriptet körs under skapandeprocessen hello:

![HDInsight-kluster anpassning och faserna när klustret skapas][img-hdi-cluster-states]

hello skriptet körs medan HDInsight konfigureras. I det här skedet hello hello skript körs parallellt på alla noderna i klustret hello och körs med rotprivilegier på hello noder.

> [!NOTE]
> Eftersom hello skriptet körs med rot nivån behörighet på hello klusternoder, kan du utföra åtgärder som att stoppa och starta tjänster, inklusive Hadoop-relaterade tjänster. Om du stoppar tjänster måste du kontrollera att tjänsten för hello Ambari och andra Hadoop-relaterade tjänster är igång innan hello skriptet är klar. De här tjänsterna krävs toosuccessfully fastställa hello hälsa och status för hello klustret medan det skapas.


När klustret skapas, kan du använda flera skriptåtgärder samtidigt. Dessa skript anropas i hello ordning som de har angetts.

> [!IMPORTANT]
> Skriptåtgärder måste slutföras inom 60 minuter eller tidsgräns. Under klusteretablering, körs hello skriptet samtidigt med andra processer för installation och konfiguration. Konkurrens om resurser, till exempel CPU-tid eller nätverket bandbredd kan orsaka hello skriptet tootake längre toofinish än i din utvecklingsmiljö.
>
> toominimize hello tid det tar toorun hello skript, Undvik aktiviteter som att hämta och sammanställa program från källan. Före kompilera program och lagra hello binary i Azure Storage.


### <a name="script-action-on-a-running-cluster"></a>Skriptåtgärder på ett kluster som körs

Till skillnad från skriptet åtgärder som används när klustret skapas ett fel i ett skript kördes på ett kluster som redan körs orsakar automatiskt inte hello klustertillstånd toochange tooa misslyckades. När ett skript är klar ska hello klustret returnera tooa ”körs”.

> [!IMPORTANT]
> Även om hello klustret har tillståndet 'körs', innehåller hello misslyckade skriptet brutna saker. Ett skript kan till exempel ta bort filer som behövs hello klustret.
>
> Åtgärder som skript köras med rotprivilegier, så bör du se till att du förstår vad ett skript som gör innan den tillämpas tooyour klustret.

När du använder ett skript tooa kluster, hello klustrets tillstånd ändras toofrom **kör** för**godkända**, sedan **HDInsight configuration**, och slutligen tillbaka för**Kör** för lyckad skript. hello Skriptstatus loggas i historiken för skriptåtgärder hello och du kan använda denna information toodetermine om hello skript har lyckats eller misslyckats. Till exempel hello `Get-AzureRmHDInsightScriptActionHistory` PowerShell-cmdlet kan vara används tooview hello status för ett skript. Returnerar information liknande toohello följande text:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!NOTE]
> Om du har ändrat hello kluster (admin) användarlösenord när hello klustret har skapats, misslyckas skriptet åtgärder har körts för det här klustret. Om du har några beständiga skriptåtgärder arbetsnoder som mål, misslyckas dessa skript när du skalar hello klustret.

## <a name="example-script-action-scripts"></a>Exempelskript för skriptåtgärder

Skriptet åtgärd skript kan användas via hello följande verktyg:

* Azure Portal
* Azure PowerShell
* Azure CLI
* HDInsight .NET SDK

HDInsight tillhandahåller skript tooinstall hello följande komponenter i HDInsight-kluster:

| Namn | Skript |
| --- | --- |
| **Lägg till ett Azure Storage-konto** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxaddstorageaccountv01/Add-Storage-Account-v01.SH. Se [Lägg till ytterligare lagringsutrymme tooan HDInsight-kluster](hdinsight-hadoop-add-storage.md). |
| **Installera Hue** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.SH. Se [installerar och använder Hue på HDInsight-kluster](hdinsight-hadoop-hue-linux.md). |
| **Installera Presto** |https://Raw.githubusercontent.com/hdinsight/Presto-hdinsight/Master/installpresto.SH. Se [installerar och använder Presto på HDInsight-kluster](hdinsight-hadoop-install-presto.md). |
| **Installera Solr** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsolrconfigactionv01/solr-Installer-v01.SH. Se [installerar och använder Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md). |
| **Installera Giraph** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxgiraphconfigactionv01/giraph-Installer-v01.SH. Se [installerar och använder Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md). |
| **Läsa in Hive-bibliotek** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsetupcustomhivelibsv01/Setup-customhivelibs-v01.SH. Se [lägga till Hive-bibliotek i HDInsight-kluster](hdinsight-hadoop-add-hive-libraries.md). |
| **Installera eller uppdatera Mono** | https://hdiconfigactions.BLOB.Core.Windows.NET/Install-Mono/Install-Mono.Bash. Se [installera eller uppdatera Mono på HDInsight](hdinsight-hadoop-install-mono.md). |

## <a name="use-a-script-action-during-cluster-creation"></a>Använd en skriptåtgärd när klustret skapas

Det här avsnittet innehåller exempel på hello olika sätt som du kan använda script actions när du skapar ett HDInsight-kluster.

### <a name="use-a-script-action-during-cluster-creation-from-hello-azure-portal"></a>Använd en skriptåtgärd när klustret skapas från hello Azure-portalen

1. Skapa ett kluster, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Stoppa när du når hello __klustret sammanfattning__ avsnitt.

2. Från hello __klustret sammanfattning__ avsnitt, Välj hello __redigera__ länka för __avancerade inställningar__.

    ![Avancerade inställningar länk](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. Från hello __avancerade inställningar__ väljer __skript åtgärder__. Från hello __skript åtgärder__ väljer __+ skicka nya__

    ![Skicka en ny skriptåtgärd](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. Använd hello __väljer du ett skript__ post tooselect en fördefinierad skript. Välj toouse ett anpassat skript __anpassade__ och ange sedan hello __namn__ och __Bash skript URI__ för skriptet.

    ![Lägga till ett skript i hello väljer skriptform](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    hello i den följande tabellen beskrivs hello element i hello formulär:

    | Egenskap | Värde |
    | --- | --- |
    | Välj ett skript | toouse egna skript väljer __anpassad__. Annars väljer du ett av hello tillhandahålls skript. |
    | Namn |Ange ett namn för hello skriptåtgärder. |
    | Bash-skript URI |Ange hello URI toohello skript som är anropade toocustomize hello-kluster. |
    | Zookeeper-HEAD/Worker |Ange hello noder (**Head**, **Worker**, eller **ZooKeeper**) på vilken hello anpassning skriptet körs. |
    | Parametrar |Ange hello parametrar, om det krävs av hello skript. |

    Använd hello __spara den här skriptåtgärden__ post tooensure som hello skript används under skalning åtgärder.

5. Välj __skapa__ toosave hello skript. Du kan sedan använda __+ skicka nya__ tooadd ett annat skript.

    ![Flera skriptåtgärder](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    När du är klar att lägga till skript använder hello __Välj__ knappen och sedan hello __nästa__ knappen tooreturn toohello __klustret sammanfattning__ avsnitt.

3. toocreate hello kluster, Välj __skapa__ från hello __klustret sammanfattning__ val.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Använd en skriptåtgärd från Azure Resource Manager-mallar

hello exemplen i det här avsnittet visar hur toouse skript åtgärder med Azure Resource Manager-mallar.

#### <a name="before-you-begin"></a>Innan du börjar

* Information om hur du konfigurerar en arbetsstation toorun HDInsight Powershell-cmdlets finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).
* Anvisningar för hur toocreate mallar, se [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).
* Om du inte har använt Azure PowerShell med Resource Manager, se [med hjälp av Azure PowerShell med Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).

#### <a name="create-clusters-using-script-action"></a>Skapa kluster med skriptåtgärder

1. Kopiera hello följande mall tooa plats på datorn. Den här mallen installerar Giraph på hello headnodes- och arbetsroller noder i klustret hello. Du kan också kontrollera om hello JSON-mall är giltig. Klistra in innehållet i mallen [JSONLint](http://jsonlint.com/), ett online JSON-verktyget för verifiering.

            {
            "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "clusterLocation": {
                    "type": "string",
                    "defaultValue": "West US",
                    "allowedValues": [ "West US" ]
                },
                "clusterName": {
                    "type": "string"
                },
                "clusterUserName": {
                    "type": "string",
                    "defaultValue": "admin"
                },
                "clusterUserPassword": {
                    "type": "securestring"
                },
                "sshUserName": {
                    "type": "string",
                    "defaultValue": "username"
                },
                "sshPassword": {
                    "type": "securestring"
                },
                "clusterStorageAccountName": {
                    "type": "string"
                },
                "clusterStorageAccountResourceGroup": {
                    "type": "string"
                },
                "clusterStorageType": {
                    "type": "string",
                    "defaultValue": "Standard_LRS",
                    "allowedValues": [
                        "Standard_LRS",
                        "Standard_GRS",
                        "Standard_ZRS"
                    ]
                },
                "clusterStorageAccountContainer": {
                    "type": "string"
                },
                "clusterHeadNodeCount": {
                    "type": "int",
                    "defaultValue": 1
                },
                "clusterWorkerNodeCount": {
                    "type": "int",
                    "defaultValue": 2
                }
            },
            "variables": {
            },
            "resources": [
                {
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-05-01-preview",
                    "dependsOn": [ ],
                    "tags": { },
                    "properties": {
                        "accountType": "[parameters('clusterStorageType')]"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-03-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Storage/storageAccounts/', parameters('clusterStorageAccountName'))]"
                    ],
                    "tags": { },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "hadoop",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterUserPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [
                                {
                                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                    "isDefault": true,
                                    "container": "[parameters('clusterStorageAccountContainer')]",
                                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                                }
                            ]
                        },
                        "computeProfile": {
                            "roles": [
                                {
                                    "name": "headnode",
                                    "targetInstanceCount": "[parameters('clusterHeadNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installGiraph",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                },
                                {
                                    "name": "workernode",
                                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installR",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                }
                            ]
                        }
                    }
                }
            ],
            "outputs": {
                "cluster":{
                    "type" : "object",
                    "value" : "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                }
            }
        }
2. Starta Azure PowerShell och logga in tooyour Azure-konto. När du har angett dina autentiseringsuppgifter returnerar hello kommando information om ditt konto.

        Add-AzureRmAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...
3. Om du har flera prenumerationer, ange hello prenumerations-ID som du vill att toouse för distribution.

        Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [!NOTE]
    > Du kan använda `Get-AzureRmSubscription` tooget en lista över alla prenumerationer som är kopplade till ditt konto, vilket inkluderar hello prenumerations-ID för varje kriterium.

4. Om du inte har en befintlig resursgrupp kan du skapa en resursgrupp. Ange hello namnet för hello resursgrupp och plats som du behöver för din lösning. En sammanfattning av hello ny resursgrupp returneras.

        New-AzureRmResourceGroup -Name myresourcegroup -Location "West US"

        ResourceGroupName : myresourcegroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. toocreate en distribution för din resursgrupp kör hello **ny AzureRmResourceGroupDeployment** kommando och ange hello nödvändiga parametrar. hello parametrarna inkluderar hello följande data:

    * Ett namn för din distribution
    * hello namnet på resursgruppen
    * hello sökväg eller URL toohello mall du skapade.

  Om mallen kräver parametrar, måste du skicka dessa parametrar samt. I det här fallet kräver hello skript åtgärd tooinstall R på hello klustret inte några parametrar.

        New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>

    Du kan ange tooprovide värden för hello parametrar som definierats i mallen för hello.

1. När hello resursgruppen har distribuerats, visas en sammanfattning av hello-distribution.

          DeploymentName    : mydeployment
          ResourceGroupName : myresourcegroup
          ProvisioningState : Succeeded
          Timestamp         : 8/14/2017 7:00:27 PM
          Mode              : Incremental
          ...

2. Du kan använda hello följande cmdlets tooget information om hello om distributionen misslyckas.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Använd en skriptåtgärd när klustret skapas från Azure PowerShell

I det här avsnittet ska vi använda hello [Lägg till AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) cmdlet tooinvoke skript med hjälp av skriptåtgärder toocustomize ett kluster. Innan du fortsätter bör du kontrollera att du har installerat och konfigurerat Azure PowerShell. Information om hur du konfigurerar en arbetsstation toorun HDInsight PowerShell-cmdlets finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).

hello följande skript visar hur tooapply skriptåtgärder när du skapar ett kluster med hjälp av PowerShell:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

Det kan ta flera minuter innan hello klustret har skapats.

### <a name="use-a-script-action-during-cluster-creation-from-hello-hdinsight-net-sdk"></a>Använd en skriptåtgärd när klustret skapas från hello HDInsight .NET SDK

Hej HDInsight .NET SDK innehåller klientbibliotek som gör det enklare toowork med HDInsight från ett .NET-program. Kodexempel är finns [skapa Linux-baserade kluster i HDInsight med hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## <a name="apply-a-script-action-tooa-running-cluster"></a>Tillämpa en skriptåtgärd tooa kör kluster

I det här avsnittet lär du dig hur tooapply skript åtgärder tooa kör klustret.

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-portal"></a>Tillämpa en skriptåtgärd tooa kör kluster från hello Azure-portalen

1. Från hello [Azure-portalen](https://portal.azure.com), Välj ditt HDInsight-kluster.

2. Översikt för hello HDInsight-kluster, Välj hello **skriptåtgärder** panelen.

    ![Skriptet åtgärder sida vid sida](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > Du kan också välja **alla inställningar** och välj sedan **skriptåtgärder** från hello inställningar.

3. Välj hello överkant hello skriptåtgärder avsnittet **skicka nya**.

    ![Lägg till ett skript tooa kör kluster](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. Använd hello __väljer du ett skript__ post tooselect en fördefinierad skript. Välj toouse ett anpassat skript __anpassade__ och ange sedan hello __namn__ och __Bash skript URI__ för skriptet.

    ![Lägga till ett skript i hello väljer skriptform](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    hello i den följande tabellen beskrivs hello element i hello formulär:

    | Egenskap | Värde |
    | --- | --- |
    | Välj ett skript | toouse egna skript väljer __anpassade__. Välj annars en skriptet. |
    | Namn |Ange ett namn för hello skriptåtgärder. |
    | Bash-skript URI |Ange hello URI toohello skript som är anropade toocustomize hello-kluster. |
    | Zookeeper-HEAD/Worker |Ange hello noder (**Head**, **Worker**, eller **ZooKeeper**) på vilken hello anpassning skriptet körs. |
    | Parametrar |Ange hello parametrar, om det krävs av hello skript. |

    Använd hello __spara den här skriptåtgärden__ post toomake att hello skript används under skalning åtgärder.

5. Använd slutligen hello **skapa** knappen tooapply hello skriptet toohello klustret.

### <a name="apply-a-script-action-tooa-running-cluster-from-azure-powershell"></a>Tillämpa en skriptåtgärd tooa köra klustret från Azure PowerShell

Innan du fortsätter bör du kontrollera att du har installerat och konfigurerat Azure PowerShell. Information om hur du konfigurerar en arbetsstation toorun HDInsight PowerShell-cmdlets finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).

hello exemplet nedan visar hur tooapply ett skript åtgärd tooa kluster som körs:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

När hello-åtgärden har slutförts får du information liknande toohello följande text:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-cli"></a>Tillämpa en skriptåtgärd tooa kör kluster från hello Azure CLI

Innan du fortsätter bör du kontrollera att du har installerat och konfigurerat hello Azure CLI. Mer information finns i [installera hello Azure CLI](../cli-install-nodejs.md).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. tooswitch tooAzure Resource Manager-läge med följande kommando på kommandoraden för hello hello:

        azure config mode arm

2. Använd hello följande tooauthenticate tooyour Azure-prenumeration.

        azure login

3. Använd hello efter kommandot tooapply ett skript åtgärd tooa kör kluster

        azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>

    Om du utelämnar parametrar för kommandot uppmanas för dessa. Om hello skript som du anger med `-u` accepterar parametrar, kan du ange dem med hello `-p` parameter.

    Giltig nodtyper är `headnode`, `workernode`, och `zookeeper`. Om hello skript ska vara tillämpade toomultiple nodtyper, ange hello typer avgränsade med ett ';'. Till exempel `-n headnode;workernode`.

    toopersist hello skript, lägga till hello `--persistOnSuccess`. Du kan också spara skriptet hello senare med hjälp av `azure hdinsight script-action persisted set`.

    När hello jobbet har slutförts visas utdata liknande toohello följande text:

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-tooa-running-cluster-using-rest-api"></a>Tillämpa en skriptåtgärd tooa kör kluster med hjälp av REST API

Se [kör skriptåtgärder på ett kluster som körs](https://msdn.microsoft.com/library/azure/mt668441.aspx).

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-hdinsight-net-sdk"></a>Tillämpa en skriptåtgärd tooa kör kluster från hello HDInsight .NET SDK

Ett exempel på hur hello .NET SDK tooapply skript tooa klustret finns [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## <a name="view-history-promote-and-demote-script-actions"></a>Visa historik, uppgradera och degradera skriptåtgärder

### <a name="using-hello-azure-portal"></a>Med hjälp av hello Azure-portalen

1. Från hello [Azure-portalen](https://portal.azure.com), Välj ditt HDInsight-kluster.

2. Översikt för hello HDInsight-kluster, Välj hello **skriptåtgärder** panelen.

    ![Skriptet åtgärder sida vid sida](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > Du kan också välja **alla inställningar** och välj sedan **skriptåtgärder** från hello inställningar.

4. En historik över skript för det här klustret visas på hello skriptåtgärder avsnitt. Den här informationen innehåller en lista över bestående skript. I hello skärmbilden nedan ser du att hello Solr skriptet har körts på det här klustret. hello skärmbild visar inte alla beständiga skript.

    ![Script Actions-avsnitt](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. Om du väljer ett skript från hello historiken visas hello avsnittet för egenskaper för det här skriptet. Hello överkant hello-skärmen, kan du kör hello skript eller befordra.

    ![Åtgärder skriptegenskaper](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. Du kan också använda hello **...**  toohello höger på poster i hello skriptåtgärder avsnittet tooperform åtgärder.

    ![Script åtgärder... användning](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a>Använda Azure PowerShell

| Använd följande hello... | för... |
| --- | --- |
| Get-AzureRmHDInsightPersistedScriptAction |Hämta information om beständiga skriptåtgärder |
| Get-AzureRmHDInsightScriptActionHistory |Hämta en historik över skript åtgärder tillämpas toohello kluster eller information för ett visst skript |
| Set-AzureRmHDInsightPersistedScriptAction |Flyttar upp en ad hoc-skriptet åtgärd tooa beständiga skriptåtgärder |
| Remove-AzureRmHDInsightPersistedScriptAction |Flyttar ett bestående skript åtgärd tooan ad hoc-åtgärd |

> [!IMPORTANT]
> Med hjälp av `Remove-AzureRmHDInsightPersistedScriptAction` inte ångra hello-åtgärder som utförs av ett skript. Den här cmdleten tar bara bort hello beständiga flaggan.

Följande exempelskript hello visas hur du använder hello cmdlets toopromote sedan nedgradera ett skript.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-hello-azure-cli"></a>Med hjälp av hello Azure CLI

| Använd följande hello... | för... |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |Hämta en lista över beständiga skriptåtgärder |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |Hämta information om en specifik beständiga skriptåtgärder |
| `azure hdinsight script-action history list <clustername>` |Hämta en historik över skript åtgärder tillämpas toohello kluster |
| `azure hdinsight script-action history show <clustername> <scriptname>` |Hämta information om en specifik skriptåtgärd |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |Flyttar upp en ad hoc-skriptet åtgärd tooa beständiga skriptåtgärder |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |Flyttar ett bestående skript åtgärd tooan ad hoc-åtgärd |

> [!IMPORTANT]
> Med hjälp av `azure hdinsight script-action persisted delete` inte ångra hello-åtgärder som utförs av ett skript. Den här cmdleten tar bara bort hello beständiga flaggan.

### <a name="using-hello-hdinsight-net-sdk"></a>Med hjälp av hello HDInsight .NET SDK

Ett exempel på hur hello .NET SDK tooretrieve skript historik från ett kluster, uppgradera eller degradera skript, se [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [!NOTE]
> Det här exemplet visar även hur tooinstall ett HDInsight-program med hello .NET SDK.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Stöd för öppen källkod programvara som används i HDInsight-kluster

hello Microsoft Azure HDInsight-tjänsten använder ett ekosystem med öppen källkod tekniker formaterat runt Hadoop. Microsoft Azure tillhandahåller en allmän supportnivå för öppen källkod tekniker. Mer information finns i hello **stöd Scope** avsnitt i hello [Azure Support FAQ webbplats](https://azure.microsoft.com/support/faq/). Hej HDInsight-tjänst ger ytterligare en säkerhetsnivå för stöd för inbyggda komponenter.

Det finns två typer av komponenter som öppen källkod som är tillgängliga i hello HDInsight-tjänst:

* **Inbyggda komponenter** -komponenterna är förinstallerat på HDInsight-kluster och tillhandahåller huvudfunktionerna i hello klustret. Till exempel tillhör YARN resurshanteraren, hello Hive-frågespråket (HiveQL) och hello Mahout biblioteksresurser toothis kategori. En fullständig lista över komponenter i serverkluster finns i [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight](hdinsight-component-versioning.md).
* **Anpassade komponenter** -du som användare av hello klustret kan installera eller använda i din arbetsbelastning någon komponent som är tillgängliga i hello community eller skapades av du.

> [!WARNING]
> Komponenter som ingår i hello HDInsight-kluster stöds fullt ut. Microsoft Support hjälper tooisolate och lösa problem relaterade toothese komponenter.
>
> Anpassade komponenter får stöd för kommersiellt rimliga toohelp du toofurther hello felsökning. Microsoft-supporten kanske kan tooresolve hello problemet eller de begära du tooengage tillgängliga kanaler hello öppen källkod tekniker där djup expertis för att teknik finns. Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).

Hej HDInsight-tjänst finns flera sätt toouse anpassade komponenter. hello samma supportnivå gäller oavsett hur en komponent används eller installeras på hello-kluster. hello följande lista beskrivs hello vanligaste sätten att anpassade komponenter kan användas på HDInsight-kluster:

1. Jobbet - Hadoop och andra typer av jobb som kör eller använda anpassade komponenter kan vara skickade toohello.

2. Anpassning av kluster - när klustret skapas som du kan ange ytterligare inställningar och anpassade komponenter som är installerade på hello klusternoder.

3. Exempel - för populära anpassade komponenter, Microsoft och andra kan ge exempel på hur du kan använda de här komponenterna på hello HDInsight-kluster. De här exemplen tillhandahålls utan stöd.

## <a name="troubleshooting"></a>Felsökning

Du kan använda Ambari web UI tooview information som loggas av skriptåtgärder. Hello loggar är också tillgängliga i hello standardkontot för lagring som associerats med hello kluster om hello skriptet misslyckas när klustret skapas. Det här avsnittet innehåller information om hur tooretrieve hello loggar med båda alternativen.

### <a name="using-hello-ambari-web-ui"></a>Med hjälp av hello Ambari-Webbgränssnittet

1. Navigera toohttps://CLUSTERNAME.azurehdinsight.net i webbläsaren. Ersätt KLUSTERNAMN med hello namnet på ditt HDInsight-kluster.

    När du uppmanas ange hello admin kontonamn (admin) och lösenord för hello klustret. Du kan ha administratörsautentiseringsuppgifter för tooreenter hello i ett webbformulär.

2. Hello-fältet hello överst på hello sidan Välj hello **ops** post. En lista över aktuella och tidigare åtgärder som utförs på hello klustret via Ambari visas.

    ![Ambari web UI-fältet med ops som valts](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. Hitta hello poster som har **kör\_customscriptaction** i hello **Operations** kolumn. Dessa poster skapas när hello skriptåtgärder kör.

    ![Skärmbild av åtgärder](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    tooview hello STDOUT och STDERR utdata, Välj hello run\customscriptaction och detaljvisning hello länkar. Den här utdata skapas när hello skriptet körs, och kan innehålla användbar information.

### <a name="access-logs-from-hello-default-storage-account"></a>Åtkomstloggar från hello standardkontot för lagring

Om hello klustret skapas misslyckas på grund av tooa åtgärd skriptfel kan hello loggar nås från hello klustret storage-konto.

* hello lagring loggar finns på `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![Skärmbild av åtgärder](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    Under den här katalogen ordnas hello loggar separat för headnode, workernode och zookeeper-noder. Några exempel är:

    * **Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **Arbetsnoden** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Zookeeper-nod** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Alla stdout och stderr för hello motsvarande värden har överförts toohello storage-konto. Det finns en **utdata -\*.txt** och **fel -\*.txt** för varje skriptåtgärd. hello utdata *.txt filen innehåller information om hello URI för hello-skript som får köras på hello värden. Exempel

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Det är möjligt att du skapar ett skript åtgärd kluster flera gånger med hello samma namn. I så fall kan du skilja hello relevanta loggar baserat på hello datum mappnamn. Hello mappstrukturen för ett kluster (det här klustret) som skapats på olika datum visas till exempel liknande toohello följande loggposter:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Om du skapar ett skript åtgärd kluster med hello samma namn på hello samma dag, du kan använda hello unikt prefix tooidentify hello relevanta loggfiler ska kopieras.

* Om du skapar ett kluster nära 12:00:00 (midnatt), är det möjligt att fördela hello loggfiler mellan två dagar. I sådana fall måste du se två olika datum mappar för hello samma kluster.

* Överför loggen filer toohello standardbehållaren kan ta upp too5 minuter, särskilt för stora kluster. Så om du vill tooaccess hello loggar bör du inte omedelbart bort hello klustret om en skriptåtgärd misslyckas.

### <a name="ambari-watchdog"></a>Ambari watchdog

> [!WARNING]
> Ändra inte hello lösenordet för hello Ambari Watchdog (hdinsightwatchdog) på Linux-baserade HDInsight-kluster. Ändra hello lösenordet för detta konto bryts hello möjlighet toorun nya skriptåtgärder på hello HDInsight-kluster.

### <a name="cant-import-name-blobservice"></a>Det går inte att importera namn BlobService

__Symptom__: hello skript åtgärden misslyckas. Text liknande toohello följande fel visas när du visar hello åtgärden i Ambari:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Orsak__: det här felet uppstår om du uppgraderar hello Python Azure Storage-klienten som medföljer hello HDInsight-kluster. HDInsight förväntar Azure Storage client 0.20.0.

__Lösning__: tooresolve det här felet manuellt ansluta nod tooeach med `ssh` och Använd hello efter kommandot tooreinstall hello rätt lagring klientversion:

```
sudo pip install azure-storage==0.20.0
```

Mer information om anslutande toohello kluster med SSH finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a>Historik visar inte skript som ska användas när klustret skapas

Om klustret har skapats innan den 15 mars 2016 kan du inte se en post i historiken för skriptåtgärder. Om du ändrar storlek på hello kluster efter den 15 mars 2016 visas hello skript med hjälp av när klustret skapas i historiken som tillämpas de toonew noder i klustret hello som en del av hello att ändra storlek på.

Det finns två undantag:

* Om klustret har skapats före den 1 September 2015. Det här datumet är när skriptåtgärder introducerades. Ett kluster som skapats före det här datumet kunde inte har använt skriptåtgärder för klustret skapas.

* Om du använde flera skriptåtgärder när klustret skapas och används hello samma namn för flera skript eller hello namn samma, samma URI, men olika parametrar för flera skript. I dessa fall kan få du hello följande fel:

    Nya åtgärder kan vara skript kördes på det här klustret på grund av tooconflicting skript namnen i befintliga skript. Skriptnamn på klustret skapa måste vara alla unika. Befintliga skript kördes på ändring av storlek.

## <a name="next-steps"></a>Nästa steg

* [Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions-linux.md)
* [Installera och använda Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md)
* [Installera och använda Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md)
* [Lägga till ytterligare lagringsutrymme tooan HDInsight-kluster](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Steg när klustret skapas"
