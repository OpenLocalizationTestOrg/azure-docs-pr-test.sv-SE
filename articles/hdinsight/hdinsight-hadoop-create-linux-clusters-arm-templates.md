---
title: "aaaCreate Hadoop-kluster med hjälp av mallar - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate kluster för HDInsight med hjälp av Resource Manager-mallar"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: jgao
ms.openlocfilehash: 92a6c1d888e401a11537dba34f188245ac17f448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Skapa Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

I den här artikeln får du lära dig på flera olika sätt toocreate Azure HDInsight-kluster med Azure Resource Manager-mallar. Mer information finns i [distribuera ett program med Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md). toolearn om andra verktyg för att skapa klustret och funktioner, klickar du på flikväljaren för hello hello längst upp i den här sidan eller se [kluster metoder för att skapa](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Krav
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

toofollow hello anvisningarna i den här artikeln, behöver du:

* En [Azure-prenumeration](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell och/eller Azure CLI.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a>Mallar för Resurshanteraren
En mall för Resource Manager gör det enkelt toocreate hello följande för ditt program i en enda, samordnad åtgärd:
* HDInsight-kluster och deras beroende resurser (till exempel hello standardkontot för lagring)
* Andra resurser (till exempel Azure SQL Database toouse Apache Sqoop)

I hello mallen definierar du hello-resurser som behövs för hello program. Du kan också ange distribution parametrar tooinput värden för olika miljöer. hello mallen består av JSON och uttryck som du använder tooconstruct värden för din distribution.

Du kan hitta HDInsight mall prov på [Azure Quickstart mallar](https://azure.microsoft.com/resources/templates/?term=hdinsight). Använda plattformsoberoende [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) med hello [Resource Manager tillägget](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) eller en text editor toosave hello-mall till en fil på din arbetsstation. Du lär dig hur toocall hello mallen med hjälp av olika metoder.

Mer information om Resource Manager-mallar finns i hello följande artiklar:

* [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)
* [Distribuera ett program med Azure Resource Manager-mallar](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Skapa mallar

Med hjälp av hello Azure-portalen kan du konfigurera alla hello egenskaper i ett kluster och sedan spara hello mallen innan du distribuerar den. Du kan sedan återanvända hello mallen.

**toogenerate en mall med hjälp av hello Azure-portalen**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **ny** på hello vänstra menyn **Intelligence + analys**, och klicka sedan på **HDInsight**.
3. Följ hello instruktioner tooenter egenskaper. Du kan använda antingen hello **Snabbregistrering** eller hello **anpassad** alternativet.
4. På hello **sammanfattning** klickar du på **ladda ned mall och parametrar**:

    ![HDInsight Hadoop skapa klustret Resource Manager hämtar en mall](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    Du kan se en lista över hello mallfil, parameterfilen och koden exempel som används för toodeploy hello mall:

    ![HDInsight Hadoop Skapa kluster Hämtningsalternativ för Resource Manager-mall](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    Från den här du hämta hello mall, spara den tooyour mallbibliotek eller distribuera hello-mallen.

    tooaccess en mall i biblioteket, klicka på **fler tjänster** hello vänstra menyn och klicka sedan på **mallar** (under hello **andra** kategori).

    > [!Note]
    > hello mall och parametrar filen måste användas tillsammans. Annars kan du få oväntade resultat. Till exempel hello standard **clusterKind** egenskapsvärdet är alltid **hadoop**, trots vad du ange innan du hämtar hello mall.



## <a name="deploy-with-powershell"></a>Distribuera med PowerShell

Den här proceduren skapar ett Hadoop-kluster i HDInsight.

1. Spara hello JSON-fil i hello [bilaga](#appx-a-arm-template) tooyour arbetsstation. I hello PowerShell-skript, hello filnamnet är `C:\HDITutorials-ARM\hdinsight-arm-template.json`.
2. Ange hello parametrar och variabler vid behov.
3. Kör hello mallen med hjälp av hello följande PowerShell-skript:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect tooAzure
        ####################################
        #region - Connect tooAzure subscription
        Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and hello dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    hello PowerShell-skript som konfigurerar bara hello klusternamnet. Hej lagringskontonamnet är hårdkodat i hello mallen. Du kan ange tooenter hello klustret användarens lösenord. (hello Standardanvändarnamnet är **admin**.) Du kan också ange tooenter hello SSH användarens lösenord. (hello SSH Standardanvändarnamnet är **sshuser**.)  

Mer information finns i [distribuera med PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).

## <a name="deploy-with-cli"></a>Distribuera med CLI
följande exempel hello använder Azure-kommandoradsgränssnittet (CLI). Den skapar ett kluster och dess beroende lagringskontot och behållare genom att anropa en Resource Manager-mall:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

Du kommer att tillfrågas tooenter:
* hello klusternamnet.
* hello klustret användarens lösenord. (hello Standardanvändarnamnet är **admin**.)
* hello SSH användarens lösenord. (hello SSH Standardanvändarnamnet är **sshuser**.)

hello följande kod innehåller infogade parametrar:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-hello-rest-api"></a>Distribuera med hello REST API
Se [distribuera med hello REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Distribuera med Visual Studio
 Använda Visual Studio toocreate en resursgruppsprojektet och distribuera det tooAzure via hello användargränssnitt. Du kan välja hello typ av resurser tooinclude i projektet. Resurserna läggs automatiskt toohello Resource Manager-mall. hello projektet innehåller också en PowerShell-skriptet toodeploy hello mall.

En introduktion toousing Visual Studio med resursgrupper, se [skapa och distribuera Azure-resursgrupper via Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig flera olika sätt toocreate ett HDInsight-kluster. toolearn se fler hello följande artiklar:

* Ett exempel för att distribuera resurser via hello .NET-klientbibliotek finns [distribuera resurser med hjälp av .NET-bibliotek och en mall](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* En detaljerad exempel för att distribuera ett program finns i [etablera och distribuera mikrotjänster förutsägbart i Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
* Anvisningar om hur du distribuerar din lösning toodifferent miljöer finns [utvecklings- och testmiljöer i Microsoft Azure](../solution-dev-test-environments.md).
* toolearn om hello avsnitt i hello Azure Resource Manager-mallen finns [Webbsidemallar](../azure-resource-manager/resource-group-authoring-templates.md).
* En lista över hello-funktioner som kan användas i en Azure Resource Manager-mallen finns [Mallfunktioner](../azure-resource-manager/resource-group-template-functions.md).

## <a name="appendix-resource-manager-template-toocreate-a-hadoop-cluster"></a>Bilaga: Resource Manager mallen toocreate ett Hadoop-kluster
hello skapar följande mall för Azure Resource Manager ett Linux-baserade Hadoop-kluster med hello beroende Azure storage-konto.

> [!NOTE]
> Det här exemplet innehåller konfigurationsinformation för Hive metastore och Oozie metastore. Ta bort hello avsnittet eller konfigurera hello avsnitt innan du använder hello mallen.
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "hello name of hello HDInsight cluster toocreate."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used tooremotely access hello cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "hello location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "hello type of hello HDInsight cluster toocreate."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "hello number of nodes in hello HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

## <a name="appendix-resource-manager-template-toocreate-a-spark-cluster"></a>Bilaga: Resource Manager mallen toocreate ett Spark-kluster

Det här avsnittet innehåller en Resource Manager-mall som du kan använda toocreate ett HDInsight Spark-kluster. Den här mallen innehåller konfigurationer för `spark-defaults` och `spark-thrift-sparkconf` (för 1.6 Spark-kluster) och `spark2-defaults` och `spark2-thrift-sparkconf` (för 2 Spark-kluster). Dessutom toothis, HDInsight beräknas och anger konfigurationer som `spark.executor.instances`, `spark.executor.memory`, och `spark.executor.cores` utifrån hello klusterstorleken. 

Om du ställer in alla en parameter i ett avsnitt som en del av själva hello mallen HDInsight inte beräkna och ange hello andra parametrar av hello samma avsnitt. Till exempel parametern `spark.executor.instances` i hello `spark-defaults` konfiguration. Om du anger en annan parametern (till exempel `spark.yarn.exector.memoryOverhead`) i hello `spark-defaults` konfiguration, HDInsight inte beräkna och ange hello `spark.executor.instances` samt parametern.

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "hello name of hello HDInsight cluster toocreate."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
            "metadata": {
                "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
            }
        },
        "clusterLoginPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "hello location where all azure resources will be deployed."
            }
        },
        "clusterVersion": {
            "type": "string",
            "defaultValue": "3.5",
            "metadata": {
                "description": "HDInsight cluster version."
            }
        },
        "clusterWorkerNodeCount": {
            "type": "int",
            "defaultValue": 4,
            "metadata": {
                "description": "hello number of nodes in hello HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "hello type of hello HDInsight cluster toocreate."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used tooremotely access hello cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        }
    },
    "variables": {
        "defaultApiVersion": "2017-06-01",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
    {
            "apiVersion": "2015-03-01-preview",
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "dependsOn": [],
            "properties": {
                "clusterVersion": "[parameters('clusterVersion')]",
                "osType": "Linux",
                "tier": "standard",
                "clusterDefinition": {
                    "kind": "[parameters('clusterKind')]",
                    "configurations": {
                        "gateway": {
                            "restAuthCredential.isEnabled": true,
                            "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                            "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                        },
                        "spark-defaults": {
                            "spark.executor.cores": "2"
                        },
                        "spark-thrift-sparkconf": {
                            "spark.yarn.executor.memoryOverhead": "896"
                        }
                    }
                },
                "storageProfile": {
                    "storageaccounts": [
                        {
                            "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                            "isDefault": true,
                            "container": "[parameters('clusterName')]",
                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                        }
                    ]
                },
                "computeProfile": {
                    "roles": [
                        {
                            "name": "headnode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 2,
                            "hardwareProfile": {
                                "vmSize": "Standard_D12"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                }
                            },
                            "virtualNetworkProfile": null,
                            "scriptActions": []
                        },
                        {
                            "name": "workernode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 4,
                            "hardwareProfile": {
                                "vmSize": "Standard_D4"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                    }
                                },
                                "virtualNetworkProfile": null,
                                "scriptActions": []
                            }
                        ]
                    }
                }
            }
        ]
    }
