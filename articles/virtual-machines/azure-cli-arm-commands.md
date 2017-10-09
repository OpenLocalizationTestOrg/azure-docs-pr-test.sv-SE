---
title: "aaaAzure CLI-kommandon i Resource Manager-läget | Microsoft Docs"
description: "Azure kommandoradsgränssnittet (CLI) kommandon toomanage resurser i hello Resource Manager-distributionsmodellen"
services: virtual-machines-linux,virtual-machines-windows,virtual-network,mobile-services,cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: be37da5b-72fe-41a1-9fa0-8937b69464ec
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: danlep
ms.openlocfilehash: 49539655f7b24511e219f982819bcb59c9305d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-commands-in-resource-manager-mode"></a>Azure CLI-kommandona i Resource Manager-läge
Den här artikeln innehåller syntax och alternativ för Azure-kommandoradsgränssnittet (CLI)-kommandon du kan ofta använda toocreate och hantera Azure-resurser i hello Azure Resource Manager-distributionsmodellen. Du har åtkomst till dessa kommandon genom att köra hello CLI i Resource Manager (arm)-läge. Detta är inte en fullständig referens och CLI-versionen kan indikera att något annorlunda kommandon eller parametrar. En allmän översikt över Azure-resurser och resursgrupper finns i [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).  

> [!NOTE]
> Detta kallas ibland artikeln visar Resource Manager kommandon läge i hello Azure CLI, Azure CLI 1.0. toowork i hello Resource Manager-modellen, du kan också försöka hello [Azure CLI 2.0](/cli/azure/install-az-cli2), våra nästa generation multiplattform CLI.
>Lär dig mer om hello [gamla och nya Azure CLIs](/cli/azure/old-and-new-clis).
>

tooget igång först [installera hello Azure CLI](../cli-install-nodejs.md) och [ansluta tooyour Azure-prenumeration](../xplat-cli-connect.md).

Skriv den aktuella kommandosyntax och alternativ på hello kommandoraden i Resource Manager-läget `azure help` eller toodisplay hjälp för ett specifikt kommando `azure help [command]`. Även kan hitta CLI-exempel i hello-dokumentationen för att skapa och hantera specifika Azure-tjänster.

Valfria parametrar visas inom hakparentes (till exempel `[parameter]`). Alla andra parametrar är obligatoriska.

Tillägg toocommand-specifika valfria parametrar dokumenteras här, finns det tre valfria parametrar som kan använda toodisplay detaljerade utdata, till exempel alternativen och statuskoder. Hej `-v` parametern innehåller utförliga data och hello `-vv` parametern innehåller även detaljerad utförlig utdata. Hej `--json` alternativet matar ut hello resultatet i raw json-format.

## <a name="setting-hello-resource-manager-mode"></a>Inställningen hello Resource Manager-läge
Använd följande kommandon för Azure CLI Resource Manager-läge i kommandot tooenable hello.

    azure config mode arm

> [!NOTE]
> hello CLI Azure Resource Manager och Azure Service Management-läge kan inte anges samtidigt. Det vill säga resurser som skapats i ett läge kan inte hanteras från hello andra läge.
> 
> 

## <a name="azure-account-manage-your-account-information"></a>Azure-konto: hantera din kontoinformation
Din Azure-prenumerationsinformation används av hello verktyget tooconnect tooyour konto.

**Lista hello importeras prenumerationer**

    account list [options]

**Visa information om en prenumeration**  

    account show [options] [subscriptionNameOrId]

**Ange hello aktuell prenumeration**

    account set [options] <subscriptionNameOrId>

**Ta bort en prenumeration eller miljö, eller avmarkera alla hello lagras information om kontot och miljö**  

    account clear [options]

**Kommandon toomanage konto miljön**  

    account env list [options]
    account env show [options] [environment]
    account env add [options] [environment]
    account env set [options] [environment]
    account env delete [options] [environment]

## <a name="azure-ad-commands-toodisplay-active-directory-objects"></a>Azure ad: kommandon toodisplay Active Directory-objekt
**Kommandon toodisplay active directory-program**

    ad app create [options]
    ad app delete [options] <object-id>

**Kommandon toodisplay active directory-grupper**

    ad group list [options]
    ad group show [options]

**Kommandon tooprovide en active directory sub gruppen eller medlemmen info**

    ad group member list [options] [objectId]

**Kommandon toodisplay active directory tjänstens huvudnamn**

    ad sp list [options]
    ad sp show [options]
    ad sp create [options] <application-id>
    ad sp delete [options] <object-id>

**Kommandon toodisplay active directory-användare**

    ad user list [options]
    ad user show [options]

## <a name="azure-availset-commands-toomanage-your-availability-sets"></a>Azure availset: kommandon toomanage din tillgänglighetsuppsättningar
**Skapar en tillgänglighetsuppsättning inom en resursgrupp**

    availset create [options] <resource-group> <name> <location> [tags]

**Visar hello tillgänglighetsuppsättningar inom en resursgrupp**

    availset list [options] <resource-group>

**Hämtar en tillgänglighetsuppsättning inom en resursgrupp**

    availset show [options] <resource-group> <name>

**Tar bort en tillgänglighetsuppsättning inom en resursgrupp**

    availset delete [options] <resource-group> <name>

## <a name="azure-config-commands-toomanage-your-local-settings"></a>Azure config: kommandon toomanage dina lokala inställningar
**Konfigurationsinställningar för listan Azure CLI**

    config list [options]

**Ta bort en konfigurationsinställning**

    config delete [options] <name>

**Uppdatera en konfigurationsinställning**

    config set <name> <value>

**Anger hello Azure CLI fungerar läge tooeither `arm` eller`asm`**

    config mode [options] <modename>


## <a name="azure-feature-commands-toomanage-account-features"></a>Azure-funktionen: kommandon toomanage kontofunktioner
**Visa en lista över alla funktioner som är tillgänglig för din prenumeration**

    feature list [options]

**Visar en funktion**

    feature show [options] <providerName> <featureName>

**Registrerar en förhandsgranskade funktion i en resursleverantör**

    feature register [options] <providerName> <featureName>

## <a name="azure-group-commands-toomanage-your-resource-groups"></a>Azure-grupp: kommandon toomanage resursgrupperna
**Skapar en resursgrupp**

    group create [options] <name> <location>

**Ange taggar tooa resursgruppen.**

    group set [options] <name> <tags>

**Tar bort en resursgrupp**

    group delete [options] <name>

**Visar hello resursgrupper för din prenumeration**

    group list [options]

**Visar en resursgrupp för din prenumeration**

    group show [options] <name>

**Kommandon toomanage resurs loggar**

    group log show [options] [name]

**Kommandon toomanage distributionen i en resursgrupp**

    group deployment create [options] [resource-group] [name]
    group deployment list [options] <resource-group> [state]
    group deployment show [options] <resource-group> [deployment-name]
    group deployment stop [options] <resource-group> [deployment-name]

**Kommandon toomanage mallen lokal eller gallery resource grupp**

    group template list [options]
    group template show [options] <name>
    group template download [options] [name] [file]
    group template validate [options] <resource-group>

## <a name="azure-hdinsight-commands-toomanage-your-hdinsight-clusters"></a>Azure hdinsight: kommandon toomanage HDInsight-kluster
**Kommandon toocreate eller lägga till tooa kluster-konfigurationsfil**

    hdinsight config create [options] <configFilePath> <overwrite>
    hdinsight config add-config-values [options] <configFilePath>
    hdinsight config add-script-action [options] <configFilePath>

Exempel: Skapa en konfigurationsfil som innehåller ett skript åtgärd toorun när du skapar ett kluster.

    hdinsight config create "C:\myFiles\configFile.config"
    hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

**Kommandot toocreate ett kluster i en resursgrupp**

    hdinsight cluster create [options] <clusterName>

Exempel: Skapa ett Storm på Linux-kluster

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Storm --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting hello request toocreate cluster...
    info:    hdinsight cluster create command OK

Exempel: Skapa ett kluster med en skriptåtgärd

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting hello request toocreate cluster...
    info:    hdinsight cluster create command OK

Parameteralternativ:

    -h, --help                                                 output usage information
    -v, --verbose                                              use verbose output
    -vv                                                        more verbose with debug output
    --json                                                     use json output
    -g --resource-group <resource-group>                       hello name of hello resource group
    -c, --clusterName <clusterName>                            HDInsight cluster name
    -l, --location <location>                                  Data center location for hello cluster
    -y, --osType <osType>                                      HDInsight cluster operating system
    'Windows' or 'Linux'
    --version <version>                                        HDInsight cluster version
    --clusterType <clusterType>                                HDInsight cluster type.
    Hadoop | HBase | Spark | Storm
    --defaultStorageAccountName <storageAccountName>           Storage account url toouse for default HDInsight storage
    --defaultStorageAccountKey <storageAccountKey>             Key toohello storage account toouse for default HDInsight storage
    --defaultStorageContainer <storageContainer>               Container in hello storage account toouse for HDInsight default storage
    --headNodeSize <headNodeSize>                              (Optional) Head node size for hello cluster
    --workerNodeCount <workerNodeCount>                        Number of worker nodes toouse for hello cluster
    --workerNodeSize <workerNodeSize>                          (Optional) Worker node size for hello cluster)
    --zookeeperNodeSize <zookeeperNodeSize>                    (Optional) Zookeeper node size for hello cluster
    --userName <userName>                                      Cluster username
    --password <password>                                      Cluster password
    --sshUserName <sshUserName>                                SSH username (only for Linux clusters)
    --sshPassword <sshPassword>                                SSH password (only for Linux clusters)
    --sshPublicKey <sshPublicKey>                              SSH public key (only for Linux clusters)
    --rdpUserName <rdpUserName>                                RDP username (only for Windows clusters)
    --rdpPassword <rdpPassword>                                RDP password (only for Windows clusters)
    --rdpAccessExpiry <rdpAccessExpiry>                        RDP access expiry.
    For example 12/12/2015 (only for Windows clusters)
    --virtualNetworkId <virtualNetworkId>                      (Optional) Virtual network ID for hello cluster.
    Value is a GUID for Windows cluster and ARM resource ID for Linux cluster)
    --subnetName <subnetName>                                  (Optional) Subnet for hello cluster
    --additionalStorageAccounts <additionalStorageAccounts>    (Optional) Additional storage accounts.
    Can be multiple.
    In hello format of 'accountName#accountKey'.
    For example, --additionalStorageAccounts "acc1#key1;acc2#key2"
    --hiveMetastoreServerName <hiveMetastoreServerName>        (Optional) SQL Server name for hello external metastore for Hive
    --hiveMetastoreDatabaseName <hiveMetastoreDatabaseName>    (Optional) Database name for hello external metastore for Hive
    --hiveMetastoreUserName <hiveMetastoreUserName>            (Optional) Database username for hello external metastore for Hive
    --hiveMetastorePassword <hiveMetastorePassword>            (Optional) Database password for hello external metastore for Hive
    --oozieMetastoreServerName <oozieMetastoreServerName>      (Optional) SQL Server name for hello external metastore for Oozie
    --oozieMetastoreDatabaseName <oozieMetastoreDatabaseName>  (Optional) Database name for hello external metastore for Oozie
    --oozieMetastoreUserName <oozieMetastoreUserName>          (Optional) Database username for hello external metastore for Oozie
    --oozieMetastorePassword <oozieMetastorePassword>          (Optional) Database password for hello external metastore for Oozie
    --configurationPath <configurationPath>                    (Optional) HDInsight cluster configuration file path
    -s, --subscription <id>                                    hello subscription id
    --tags <tags>                                              Tags tooset toohello cluster.
    Can be multiple.
    In hello format of 'name=value'.
    Name is required and value is optional.
    For example, --tags tag1=value1;tag2


**Kommandot toodelete ett kluster**

    hdinsight cluster delete [options] <clusterName>

**Information om kommandot tooshow kluster**

    hdinsight cluster show [options] <clusterName>

**Kommandot toolist alla kluster (i en viss resursgrupp, om angett)**

    hdinsight cluster list [options]

**Kommandot tooresize ett kluster**

    hdinsight cluster resize [options] <clusterName> <targetInstanceCount>

**Kommandot tooenable HTTP-åtkomst för ett kluster**

    hdinsight cluster enable-http-access [options] <clusterName> <userName> <password>

**Kommandot toodisable HTTP-åtkomst för ett kluster**

    hdinsight cluster disable-http-access [options] <clusterName>

**Kommandot tooenable RDP-åtkomst för ett kluster**

    hdinsight cluster enable-rdp-access [options] <clusterName> <rdpUserName> <rdpPassword> <rdpExpiryDate>

**Kommandot toodisable HTTP-åtkomst för ett kluster**

    hdinsight cluster disable-rdp-access [options] <clusterName>

## <a name="azure-insights-commands-related-toomonitoring-insights-events-alert-rules-autoscale-settings-metrics"></a>Azure insikter: kommandon relaterade toomonitoring insikter (händelser, Varningsregler, Autoskala inställningar, mått)
**Hämta åtgärdsloggar för en prenumeration, en correlationId, en resursgrupp, resurs eller resursprovidern**

    insights logs list [options]

## <a name="azure-location-commands-tooget-hello-available-locations-for-all-resource-types"></a>Azure-plats: kommandon tooget hello tillgängliga platser för alla typer av resurser
**Lista hello tillgängliga platser**

    location list [options]

## <a name="azure-network-commands-toomanage-network-resources"></a>Azure-nätverk: kommandon toomanage nätverksresurser
**Kommandon toomanage virtuella nätverk**

    network vnet create [options] <resource-group> <name> <location>
Skapar ett virtuellt nätverk. I hello namnet newvnet för resursen grupp myresourcegroup i hello västra USA region i följande exempel skapar vi ett virtuellt nätverk.

    azure network vnet create myresourcegroup newvnet "west us"
    info:    Executing command network vnet create
    + Looking up virtual network "newvnet"
    + Creating virtual network "newvnet"
     Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet create command OK


Parameteralternativ:

     -h, --help                                 output usage information
     -v, --verbose                              use verbose output
    --json                                     use json output
     -g, --resource-group <resource-group>      hello name of hello resource group
     -n, --name <name>                          hello name of hello virtual network
     -l, --location <location>                  hello location
     -a, --address-prefixes <address-prefixes>  hello comma separated list of address prefixes for this virtual network
      For example -a 10.0.0.0/24,10.0.1.0/24.
      Default value is 10.0.0.0/8

    -d, --dns-servers <dns-servers>            hello comma separated list of DNS servers IP addresses
     -t, --tags <tags>                          hello tags set on this virtual network.
      Can be multiple. In hello format of "name=value".
      Name is required and value is optional.
      For example, -t tag1=value1;tag2
     -s, --subscription <subscription>          hello subscription identifier
<BR>

    network vnet set [options] <resource-group> <name>

Uppdaterar en konfiguration av virtuellt nätverk inom en resursgrupp.

    azure network vnet set myresourcegroup newvnet

    info:    Executing command network vnet set
    + Looking up virtual network "newvnet"
    + Updating virtual network "newvnet"
    + Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet set command OK

Parameteralternativ:

       -h, --help                                 output usage information
       -v, --verbose                              use verbose output
       --json                                     use json output
       -g, --resource-group <resource-group>      hello name of hello resource group
       -n, --name <name>                          hello name of hello virtual network
       -a, --address-prefixes <address-prefixes>  hello comma separated list of address prefixes for this virtual network.
        For example -a 10.0.0.0/24,10.0.1.0/24.
        This list will be appended toohello current list of address prefixes.
        hello address prefixes in this list should not overlap between them.
        hello address prefixes in this list should not overlap with existing address prefixes in hello vnet.

       -d, --dns-servers [dns-servers]            hello comma separated list of DNS servers IP addresses.
        This list will be appended toohello current list of DNS server IP addresses.

       -t, --tags <tags>                          hello tags set on this virtual network.
        Can be multiple. In hello format of "name=value".
        Name is required and value is optional. For example, -t tag1=value1;tag2.
        This list will be appended toohello current list of tags

       --no-tags                                  remove all existing tags
       -s, --subscription <subscription>          hello subscription identifier
<BR>

    network vnet list [options] <resource-group>

hello kommando visar alla virtuella nätverk i en resursgrupp.

    C:\>azure network vnet list myresourcegroup

    info:    Executing command network vnet list
    + Listing virtual networks
        data:    ID
       Name      Location  Address prefixes  DNS servers
    data:    -------------------------------------------------------------------
    ------  --------  --------  ----------------  -----------
    data:    /subscriptions/###############################/resourceGroups/
    wvnet   newvnet   westus    10.0.0.0/8
    info:    network vnet list command OK

Parameteralternativ:

      -h, --help                             output usage information
      -v, --verbose                          use verbose output
      --json                                 use json output
      -g, --resource-group <resource-group>  hello name of hello resource group
      -s, --subscription <subscription>      hello subscription identifier

<BR>

    network vnet show [options] <resource-group> <name>
hello kommando visar hello egenskaper för virtuellt nätverk i en resursgrupp.

    azure network vnet show -g myresourcegroup -n newvnet

    info:    Executing command network vnet show
    + Looking up virtual network "newvnet"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet show command OK
<BR>

    network vnet delete [options] <resource-group> <name>
hello kommandot tar bort ett virtuellt nätverk.

    azure network vnet delete myresourcegroup newvnetX

    info:    Executing command network vnet delete
    + Looking up virtual network "newvnetX"
    Delete virtual network newvnetX? [y/n] y
    + Deleting virtual network "newvnetX"
    info:    network vnet delete command OK

Parameteralternativ:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -n, --name <name>                      hello name of hello virtual network
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      hello subscription identifier


**Kommandon toomanage virtuellt undernät**

    network vnet subnet create [options] <resource-group> <vnet-name> <name>

Lägger till ett annat undernät tooan befintligt virtuellt nätverk.

    azure network vnet subnet create -g myresourcegroup --vnet-name newvnet -n subnet --address-prefix 10.0.1.0/24

    info:    Executing command network vnet subnet create
    + Looking up hello subnet "subnet"
    + Creating subnet "subnet"
    + Looking up hello subnet "subnet"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Name:                      subnet
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet create command OK

Parameteralternativ:

     -h, --help                                                       output usage information
     -v, --verbose                                                    use verbose output
         --json                                                           use json output
     -g, --resource-group <resource-group>                            hello name of hello resource group
     -e, --vnet-name <vnet-name>                                      hello name of hello virtual network
     -n, --name <name>                                                hello name of hello subnet
     -a, --address-prefix <address-prefix>                            hello address prefix
     -w, --network-security-group-id <network-security-group-id>      hello network security group identifier.
           e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
     -o, --network-security-group-name <network-security-group-name>  hello network security group name
     -s, --subscription <subscription>                                hello subscription identifier

<BR>

    network vnet subnet set [options] <resource-group> <vnet-name> <name>

Anger ett specifikt virtuellt undernät inom en resursgrupp.

    C:\>azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up hello subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up hello subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet list [options] <resource-group> <vnet-name>

Listar alla hello virtuellt undernät för ett virtuellt nätverk inom en resursgrupp.

    azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up hello subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up hello subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet show [options] <resource-group> <vnet-name> <name>
Visar egenskaper för undernät för virtuellt nätverk

    azure network vnet subnet show -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet show
    + Looking up hello subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft
    .Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet show command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -e, --vnet-name <vnet-name>            hello name of hello virtual network
    -n, --name <name>                      hello name of hello subnet
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network vnet subnet delete [options] <resource-group> <vnet-name> <subnet-name>
Tar bort ett undernät från ett befintligt virtuellt nätverk.

    azure network vnet subnet delete -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet delete
    + Looking up hello subnet "subnet1"
    Delete subnet "subnet1"? [y/n] y
    + Deleting subnet "subnet1"
    info:    network vnet subnet delete command OK

Parameteralternativ:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -e, --vnet-name <vnet-name>            hello name of hello virtual network
     -n, --name <name>                      hello subnet name
     -s, --subscription <subscription>      hello subscription identifier
     -q, --quiet                            quiet mode, do not ask for delete confirmation

**Kommandon toomanage belastningsutjämnare**

    network lb create [options] <resource-group> <name> <location>
Skapar en belastningen belastningsutjämnaren mängd.

    azure network lb create -g myresourcegroup -n mylb -l westus

    info:    Executing command network lb create
    + Looking up hello load balancer "mylb"
    + Creating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb create command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello load balancer
    -l, --location <location>              hello location
    -t, --tags <tags>                      hello list of tags.
     Can be multiple. In hello format of "name=value".
     Name is required and value is optional. For example, -t tag1=value1;tag2
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb list [options] <resource-group>
Visar belastning belastningsutjämnaren resurser inom en resursgrupp.

    azure network lb list myresourcegroup

    info:    Executing command network lb list
    + Getting hello load balancers
    data:    Name  Location
    data:    ----  --------
    data:    mylb  westus
    info:    network lb list command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb show [options] <resource-group> <name>

Visar att läsa in information om belastningsutjämning för en särskild belastningsutjämnare inom en resursgrupp

    azure network lb show myresourcegroup mylb -v

    info:    Executing command network lb show
    verbose: Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb show command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier

<BR>

    network lb delete [options] <resource-group> <name>

Ta bort belastningen belastningsutjämnaren resurser.

    azure network lb delete  myresourcegroup mylb

    info:    Executing command network lb delete
    + Looking up hello load balancer "mylb"
    Delete load balancer "mylb"? [y/n] y
    + Deleting load balancer "mylb"
    info:    network lb delete command OK

Parameteralternativ:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -n, --name <name>                      hello name of hello load balancer
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      hello subscription identifier

**Kommandon toomanage avsökningar av en belastningsutjämnare**

    network lb probe create [options] <resource-group> <lb-name> <name>

Skapa hello avsökningen konfiguration för hälsostatus i hello belastningsutjämnaren. Kom ihåg toorun kommandot, din belastningsutjämnare kräver en frontend-ip-resurs (utcheckningen kommandot ”azure-nätverk klientdels-ip” tooassign en IP-adress tooload belastningsutjämnare).

    azure network lb probe create -g myresourcegroup --lb-name mylb -n mylbprobe --protocol tcp --port 80 -i 300

    info:    Executing command network lb probe create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe create command OK

Parameteralternativ:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello probe
    -p, --protocol <protocol>              hello probe protocol
    -o, --port <port>                      hello probe port
    -f, --path <path>                      hello probe path
    -i, --interval <interval>              hello probe interval in seconds
    -c, --count <count>                    hello number of probes
    -s, --subscription <subscription>      hello subscription identifier

<BR>

    network lb probe set [options] <resource-group> <lb-name> <name>

Uppdaterar en befintlig belastningsutjämningsavsökning med nya värden för den.

    azure network lb probe set -g myresourcegroup -l mylb -n mylbprobe -p mylbprobe1 -p TCP -o 443 -i 300

    info:    Executing command network lb probe set
        + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe set command OK

Parameteralternativ för

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello probe
    -e, --new-probe-name <new-probe-name>  hello new name of hello probe
    -p, --protocol <protocol>              hello new value for probe protocol
    -o, --port <port>                      hello new value for probe port
    -f, --path <path>                      hello new value for probe path
    -i, --interval <interval>              hello new value for probe interval in seconds
    -c, --count <count>                    hello new value for number of probes
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb probe list [options] <resource-group> <lb-name>

Hello avsökningen listegenskaper för belastningsutjämnaren belastningen.

    C:\>azure network lb probe list -g myresourcegroup -l mylb

    info:    Executing command network lb probe list
    + Looking up hello load balancer "mylb"
    data:    Name       Protocol  Port  Path  Interval  Count
    data:    ---------  --------  ----  ----  --------  -----
    data:    mylbprobe  Tcp       443         300       2
    info:    network lb probe list command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier


    network lb probe delete [options] <resource-group> <lb-name> <name>
Tar bort hello-avsökningen som skapats för hello belastningsutjämnaren.

    azure network lb probe delete -g myresourcegroup -l mylb -n mylbprobe

    info:    Executing command network lb probe delete
    + Looking up hello load balancer "mylb"
    Delete a probe "mylbprobe?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb probe delete command OK

**Kommandon toomanage klientdelens ip-konfigurationer för en belastningsutjämnare**

    network lb frontend-ip create [options] <resource-group> <lb-name> <name>
Skapar klientdelens IP-konfiguration tooan befintliga belastningen belastningsutjämnaren uppsättningen.

    azure network lb frontend-ip create -g myresourcegroup --lb-name mylb -n myfrontendip -o Dynamic -e subnet -m newvnet

    info:    Executing command network lb frontend-ip create
    + Looking up hello load balancer "mylb"
    + Looking up hello subnet "subnet"
    + Creating frontend IP configuration "myfrontendip"
    + Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/Myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    /frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:           10.0.1.4
    data:    Subnet:                       id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Public IP address:
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip create command OK

<BR>

    network lb frontend-ip set [options] <resource-group> <lb-name> <name>

Uppdateringar en befintlig konfiguration av en klientdel IP.hello kommandot nedan lägger till en offentlig IP-adress som kallas mypubip5 tooan befintliga belastningen belastningsutjämnarklientdel IP med namnet myfrontendip.

    azure network lb frontend-ip set -g myresourcegroup --lb-name mylb -n myfrontendip -i mypubip5

    info:    Executing command network lb frontend-ip set
    + Looking up hello load balancer "mylb"
    + Looking up hello public ip "mypubip5"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:
    data:    Subnet:
    data:    Public IP address:            id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypubip5
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip set command OK

Parameteralternativ:

    -h, --help                                                         output usage information
    -v, --verbose                                                      use verbose output
    --json                                                             use json output
    -g, --resource-group <resource-group>                              hello name of hello resource group
    -l, --lb-name <lb-name>                                            hello name of hello load balancer
    -n, --name <name>                                                  hello name of hello frontend ip configuration
    -a, --private-ip-address <private-ip-address>                      hello private ip address
    -o, --private-ip-allocation-method <private-ip-allocation-method>  hello private ip allocation method [Static, Dynamic]
    -u, --public-ip-id <public-ip-id>                                  hello public ip identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -i, --public-ip-name <public-ip-name>                              hello public ip name.
    This public ip must exist in hello same resource group as hello lb.
    Please use public-ip-id if that is not hello case.
    -b, --subnet-id <subnet-id>                                        hello subnet id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/VirtualNetworks/<vnet-name>/subnets/<subnet-name>
    -e, --subnet-name <subnet-name>                                    hello subnet name
    -m, --vnet-name <vnet-name>                                        hello virtual network name.
    This virtual network must exist in hello same resource group as hello lb.
    Please use subnet-id if that is not hello case.
    -s, --subscription <subscription>                                  hello subscription identifier

<BR>

    network lb frontend-ip list [options] <resource-group> <lb-name>

Visar alla hello klientdel IP-resurser som konfigurerats för hello belastningsutjämnaren.

    azure network lb frontend-ip list -g myresourcegroup -l mylb

    info:    Executing command network lb frontend-ip list
    + Looking up hello load balancer "mylb"
    data:    Name         Provisioning state  Private IP allocation method  Subnet
    data:    -----------  ------------------  ----------------------------  ------
    data:    myprivateip  Succeeded           Dynamic
    info:    network lb frontend-ip list command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb frontend-ip delete [options] <resource-group> <lb-name> <name>
Tar bort hello klientdelens IP-objektet som är associerat tooload belastningsutjämnare

    network lb frontend-ip delete -g myresourcegroup -l mylb -n myfrontendip
    info:    Executing command network lb frontend-ip delete
    + Looking up hello load balancer "mylb"
    Delete frontend ip configuration "myfrontendip"? [y/n] y
    + Updating load balancer "mylb"

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello frontend ip configuration
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

**Kommandon toomanage serverdelsadresspooler för belastningsutjämning**

    network lb address-pool create [options] <resource-group> <lb-name> <name>

Skapa en backend-adresspool för en belastningsutjämnare.

    azure network lb address-pool create -g myresourcegroup --lb-name mylb -n myaddresspool

    info:    Executing command network lb address-pool create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourgroup/providers/Microso.Network/loadBalancers/mylb/backendAddressPools/myaddresspool
    data:    Name:                      myaddresspool
    data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
    data:    Provisioning state:        Succeeded
    data:    Backend IP configurations:
    data:    Load balancing rules:
    data:
    info:    network lb address-pool create command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello backend address pool
    -s, --subscription <subscription>      hello subscription identifier

<BR>

    network lb address-pool list [options] <resource-group> <lb-name>

Lista backend IP-adressintervallet för en viss resursgrupp

    azure network lb address-pool list -g myresourcegroup -l mylb

    info:    Executing command network lb address-pool list
    + Looking up hello load balancer "mylb"
    data:    Name           Provisioning state
    data:    -------------  ------------------
    data:    mybackendpool  Succeeded
    info:    network lb address-pool list command OK

Parameteralternativ:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -l, --lb-name <lb-name>                hello name of hello load balancer
     -s, --subscription <subscription>      hello subscription identifier

<BR>
    lb-nätverksadresspool ta bort [alternativ] < resursgrupp >< lb-name ><name>

Tar bort hello backend IP-adresspool intervallet resursen från belastningsutjämnaren.

    azure network lb address-pool delete -g myresourcegroup -l mylb -n mybackendpool

    info:    Executing command network lb address-pool delete
    + Looking up hello load balancer "mylb"
    Delete backend address pool "mybackendpool"? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb address-pool delete command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello backend address pool
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

**Kommandon toomanage regler för inläsning av belastningsutjämnare**

    network lb rule create [options] <resource-group> <lb-name> <name>
Skapa regler för inläsning av belastningsutjämnaren.

Du kan skapa en regel för belastningsutjämnare konfigurera hello klientdel slutpunkt för hello belastningsutjämnare och hello backend adresspoolen intervallet tooreceive hello inkommande nätverkstrafik. Inställningarna omfattar också hello portar för klientdelens IP-slutpunkt och portar för intervall för hello backend-adresspool.

hello följande exempel visas hur toocreate belastningsutjämning regeln hello klientdel slutpunkt som lyssnade tooport 80 TCP och belastningsutjämning nätverkstrafik skickar tooport 8080 för intervall för hello backend-adresspool.

    azure network lb rule create -g myresourcegroup -l mylb -n mylbrule -p tcp -f 80 -b 8080 -i 10


    info:    Executing command network lb rule create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mylbrule
    data:    Name:                      mylbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule create command OK

<BR>

    network lb rule set [options] <resource-group> <lb-name> <name>

Uppdaterar en befintlig regel för belastningsutjämnare i en viss resursgrupp. I följande exempel hello, ändrat vi hello Regelnamn från mylbrule toomynewlbrule.

    azure network lb rule set -g myresourcegroup -l mylb -n mylbrule -r mynewlbrule -p tcp -f 80 -b 8080 -i 10 -t myfrontendip -o mybackendpool

    info:    Executing command network lb rule set
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mynewlbrule
    data:    Name:                      mynewlbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule set command OK

Parameteralternativ:

    -h, --help                                         output usage information
    -v, --verbose                                      use verbose output
    --json                                             use json output
    -g, --resource-group <resource-group>              hello name of hello resource group
    -l, --lb-name <lb-name>                            hello name of hello load balancer
    -n, --name <name>                                  hello name of hello rule
    -r, --new-rule-name <new-rule-name>                new rule name
    -p, --protocol <protocol>                          hello rule protocol
    -f, --frontend-port <frontend-port>                hello frontend port
    -b, --backend-port <backend-port>                  hello backend port
    -e, --enable-floating-ip <enable-floating-ip>      enable floating point ip
    -i, --idle-timeout <idle-timeout>                  hello idle timeout in minutes
    -a, --probe-name [probe-name]                      hello name of hello probe defined in hello same load balancer
    -t, --frontend-ip-name <frontend-ip-name>          hello name of hello frontend ip configuration in hello same load balancer
    -o, --backend-address-pool <backend-address-pool>  name of hello backend address pool defined in hello same load balancer
    -s, --subscription <subscription>                  hello subscription identifier


    network lb rule list [options] <resource-group> <lb-name>

Visar alla belastningsutjämningsreglerna som konfigurerats för en belastningsutjämnare i en viss resursgrupp.

    azure network lb rule list -g myresourcegroup -l mylb

    info:    Executing command network lb rule list
    + Looking up hello load balancer "mylb"
    data:    Name         Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend address pool  Probe data

    data:    mynewlbrule  Succeeded           Tcp       80             8080          false               10                       /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    info:    network lb rule list command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier

    network lb rule delete [options] <resource-group> <lb-name> <name>

Tar bort en regel för belastningsutjämnare.

    azure network lb rule delete -g myresourcegroup -l mylb -n mynewlbrule

    info:    Executing command network lb rule delete
    + Looking up hello load balancer "mylb"
    Delete load balancing rule mynewlbrule? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb rule delete command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

**Inkommande NAT-regler för belastningsutjämning för kommandon toomanage**

    network lb inbound-nat-rule create [options] <resource-group> <lb-name> <name>
Skapar en inkommande NAT-regel för belastningsutjämning.

I hello använder följande exempel som vi skapade en NAT-regel från klientdel IP (som definierades tidigare med hjälp av kommandot hello ”azure-nätverk klientdels-ip”) med en lyssningsport för inkommande och utgående port som hello belastningsutjämnare toosend hello-nätverkstrafik.

    azure network lb inbound-nat-rule create -g myresourcegroup -l mylb -n myinboundnat -p tcp -f 80 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              80
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule create command OK

Parameteralternativ:

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          hello name of hello resource group
    -l, --lb-name <lb-name>                        hello name of hello load balancer
    -n, --name <name>                              hello name of hello inbound NAT rule
    -p, --protocol <protocol>                      hello rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            hello frontend port [0-65535]
    -b, --backend-port <backend-port>              hello backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                hello name of hello frontend ip configuration
    -m, --vm-id <vm-id>                            hello VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        hello VM name.This VM must exist in hello same resource group as hello lb.
    Please use vm-id if that is not hello case.
    this parameter will be ignored if --vm-id is specified
    -s, --subscription <subscription>              hello subscription identifier
<BR>

    network lb inbound-nat-rule set [options] <resource-group> <lb-name> <name>
Uppdaterar en befintlig inkommande nat-regel. I följande exempel hello, vi ändrat hello inkommande lyssningsport från 80 too81.

    azure network lb inbound-nat-rule set -g group-1 -l mylb -n myinboundnat -p tcp -f 81 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule set
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              81
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule set command OK

Parameteralternativ:

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          hello name of hello resource group
    -l, --lb-name <lb-name>                        hello name of hello load balancer
    -n, --name <name>                              hello name of hello inbound NAT rule
    -p, --protocol <protocol>                      hello rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            hello frontend port [0-65535]
    -b, --backend-port <backend-port>              hello backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                hello name of hello frontend ip configuration
    -m, --vm-id [vm-id]                            hello VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        hello VM name.
    This virtual machine must exist in hello same resource group as hello lb.
    Please use vm-id if that is not hello case
    -s, --subscription <subscription>              hello subscription identifier
<BR>

    network lb inbound-nat-rule list [options] <resource-group> <lb-name>

Visar alla inkommande nat-regler för belastningsutjämning.

    azure network lb inbound-nat-rule list -g myresourcegroup -l mylb

    info:    Executing command network lb inbound-nat-rule list
    + Looking up hello load balancer "mylb"
    data:    Name          Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend IP configuration
    data:    ------------  ------------------  --------  -------------  ------------  ------------------  -----------------------  ---
    ---------------------
    data:    myinboundnat  Succeeded           Tcp       81             8080          false               4

    info:    network lb inbound-nat-rule list command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb inbound-nat-rule delete [options] <resource-group> <lb-name> <name>

Tar bort NAT-regel för hello belastningsutjämnare i en viss resursgrupp.

    azure network lb inbound-nat-rule delete -g myresourcegroup -l mylb -n myinboundnat

    info:    Executing command network lb inbound-nat-rule delete
    + Looking up hello load balancer "mylb"
    Delete inbound NAT rule "myinboundnat?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb inbound-nat-rule delete command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello inbound NAT rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

**Kommandon toomanage offentliga ip-adresser**

    network public-ip create [options] <resource-group> <name> <location>
Skapar en offentlig ip-resurs. Du skapar hello offentliga IP-resurs och associera tooa domännamn.

    azure network public-ip create -g myresourcegroup -n mytestpublicip1 -l eastus -d azureclitest -a "Dynamic"
    info:    Executing command network public-ip create
    + Looking up hello public ip "mytestpublicip1"
    + Creating public ip address "mytestpublicip1"
    + Looking up hello public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Dynamic
    data:    Idle timeout:         4
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip create command OK


Parameteralternativ:

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        hello name of hello resource group
    -n, --name <name>                            hello name of hello public ip
    -l, --location <location>                    hello location
    -d, --domain-name-label <domain-name-label>  hello domain name label.
    This set DNS too<domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  hello allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              hello idle timeout in minutes
    -f, --reverse-fqdn <reverse-fqdn>            hello reverse fqdn
    -t, --tags <tags>                            hello list of tags.
    Can be multiple. In hello format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>            hello subscription identifier
<br>

    network public-ip set [options] <resource-group> <name>
Uppdaterar hello egenskaperna för en befintlig offentlig ip-resurs. I följande exempel hello ändrat vi hello offentlig IP-adress från dynamisk tooStatic.

    azure network public-ip set -g group-1 -n mytestpublicip1 -d azureclitest -a "Static"
    info:    Executing command network public-ip set
    + Looking up hello public ip "mytestpublicip1"
    + Updating public ip address "mytestpublicip1"
    + Looking up hello public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip set command OK

Parameteralternativ:

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        hello name of hello resource group
    -n, --name <name>                            hello name of hello public ip
    -d, --domain-name-label [domain-name-label]  hello domain name label.
    This set DNS too<domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  hello allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              hello idle timeout in minutes
    -f, --reverse-fqdn [reverse-fqdn]            hello reverse fqdn
    -t, --tags <tags>                            hello list of tags.
    Can be multiple. In hello format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    --no-tags                                    remove all existing tags
    -s, --subscription <subscription>            hello subscription identifier

<br>
    listan över offentliga ip-nätverk [alternativ] < resursgrupp > listar alla offentliga IP-resurser inom en resursgrupp.

    azure network public-ip list -g myresourcegroup

    info:    Executing command network public-ip list
    + Getting hello public ip addresses
    data:    Name             Location  Allocation  IP Address    Idle timeout  DNS Name
    data:    ---------------  --------  ----------  ------------  ------------  -------------------------------------------
    data:    mypubip5         westus    Dynamic                   4             "domain name".westus.cloudapp.azure.com
    data:    myPublicIP       eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip   eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip1  eastus   Static (Static IP address) 4             azureclitest.eastus.cloudapp.azure.com

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -s, --subscription <subscription>      hello subscription identifier
<BR>
    nätverket offentliga IP-visa [alternativ] < resursgrupp ><name>

Visar offentliga ip-egenskaper för en offentlig ip-resurs i en resursgrupp.

    azure network public-ip show -g myresourcegroup -n mytestpublicip

    info:    Executing command network public-ip show
    + Looking up hello public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip
    data:    Name:                 mytestpublicip
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip show command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello public IP
    -s, --subscription <subscription>      hello subscription identifier


    network public-ip delete [options] <resource-group> <name>

Tar bort offentlig ip-resurs.

    azure network public-ip delete -g group-1 -n mypublicipname
    info:    Executing command network public-ip delete
    + Looking up hello public ip "mypublicipname"
    Delete public ip address "mypublicipname"? [y/n] y
    + Deleting public ip address "mypublicipname"
    info:    network public-ip delete command OK

Parameteralternativ:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello public IP
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier


**Kommandon toomanage nätverksgränssnitt**

    network nic create [options] <resource-group> <name> <location>
Skapar en resurs med namnet nätverksgränssnitt (NIC) som kan användas för belastningsutjämning eller associera tooa virtuella datorn.

    azure network nic create -g myresourcegroup -l eastus -n testnic1 --subnet-name subnet-1 --subnet-vnet-name myvnet

    info:    Executing command network nic create
    + Looking up hello network interface "testnic1"
    + Looking up hello subnet "subnet-1"
    + Creating network interface "testnic1"
    + Looking up hello network interface "testnic1"
    data:    Id:                     /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/networkInterfaces/testnic1
    data:    Name:                   testnic1
    data:    Type:                   Microsoft.Network/networkInterfaces
    data:    Location:               eastus
    data:    Provisioning state:     Succeeded
    data:    IP configurations:
    data:       Name:                         NIC-config
    data:       Provisioning state:           Succeeded
    data:       Private IP address:           10.0.0.5
    data:       Private IP Allocation Method: Dynamic
    data:       Subnet:                       /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/virtualNetworks/myVNET/subnets/Subnet-1

Parameteralternativ:

    -h, --help                                                       output usage information
    -v, --verbose                                                    use verbose output
    --json                                                           use json output
    -g, --resource-group <resource-group>                            hello name of hello resource group
    -n, --name <name>                                                hello name of hello network interface
    -l, --location <location>                                        hello location
    -w, --network-security-group-id <network-security-group-id>      hello network security group identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
    -o, --network-security-group-name <network-security-group-name>  hello network security group name.
    This network security group must exist in hello same resource group as hello nic.
    Please use network-security-group-id if that is not hello case.
    -i, --public-ip-id <public-ip-id>                                hello public IP identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -p, --public-ip-name <public-ip-name>                            hello public IP name.
    This public ip must exist in hello same resource group as hello nic.
    Please use public-ip-id if that is not hello case.
    -a, --private-ip-address <private-ip-address>                    hello private IP address
    -u, --subnet-id <subnet-id>                                      hello subnet identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>/subnets/<subnet-name>
    --subnet-name <subnet-name>                                  hello subnet name
    -m, --subnet-vnet-name <subnet-vnet-name>                        hello vnet name under which subnet-name exists
    -t, --tags <tags>                                                hello comma seperated list of tags.
    Can be multiple. In hello format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>                                hello subscription identifier
    data:
    info:    network nic create command OK

<BR>

    network nic set [options] <resource-group> <name>

    network nic list [options] <resource-group>
    network nic show [options] <resource-group> <name>
    network nic delete [options] <resource-group> <name>

**Kommandon toomanage nätverkssäkerhetsgrupper**

    network nsg create [options] <resource-group> <name> <location>
    network nsg set [options] <resource-group> <name>
    network nsg list [options] <resource-group>
    network nsg show [options] <resource-group> <name>
    network nsg delete [options] <resource-group> <name>

**Kommandon toomanage regler för nätverkssäkerhetsgrupper**

    network nsg rule create [options] <resource-group> <nsg-name> <name>
    network nsg rule set [options] <resource-group> <nsg-name> <name>
    network nsg rule list [options] <resource-group> <nsg-name>
    network nsg rule show [options] <resource-group> <nsg-name> <name>
    network nsg rule delete [options] <resource-group> <nsg-name> <name>

**Kommandon toomanage trafikhanterarprofilen**

    network traffic-manager profile create [options] <resource-group> <name>
    network traffic-manager profile set [options] <resource-group> <name>
    network traffic-manager profile list [options] <resource-group>
    network traffic-manager profile show [options] <resource-group> <name>
    network traffic-manager profile delete [options] <resource-group> <name>
    network traffic-manager profile is-dns-available [options] <resource-group> <relative-dns-name>

**Kommandon toomanage traffic manager-slutpunkter**

    network traffic-manager profile endpoint create [options] <resource-group> <profile-name> <name> <endpoint-location>
    network traffic-manager profile endpoint set [options] <resource-group> <profile-name> <name>
    network traffic-manager profile endpoint delete [options] <resource-group> <profile-name> <name>

**Kommandon toomanage virtuella nätverks-gateway**

    network gateway list [options] <resource-group>

## <a name="azure-provider-commands-toomanage-resource-provider-registrations"></a>Azure-providern: kommandon toomanage resource provider registreringar
**Visa en lista över providers som är registrerade i Resource Manager**

    provider list [options]

**Visa information om hello begärt providernamnrymden**

    provider show [options] <namespace>

**Registrera providern med hello prenumeration**

    provider register [options] <namespace>

**Avregistrera provider med hello prenumeration**

    provider unregister [options] <namespace>

## <a name="azure-resource-commands-toomanage-your-resources"></a>Azure-resurs: kommandon toomanage dina resurser
**Skapar en resurs i en resursgrupp**

    resource create [options] <resource-group> <name> <resource-type> <location> <api-version>

**Uppdaterar en resurs i en resursgrupp utan parametrar eller mallar**

    resource set [options] <resource-group> <name> <resource-type> <properties> <api-version>

**Visar hello resurser**

    resource list [options] [resource-group]

**Hämtar en resurs i en resursgrupp eller prenumeration**

    resource show [options] <resource-group> <name> <resource-type> <api-version>

**Tar bort en resurs i en resursgrupp**

    resource delete [options] <resource-group> <name> <resource-type> <api-version>

## <a name="azure-role-commands-toomanage-your-azure-roles"></a>Azure roll: kommandon toomanage dina Azure roller
**Hämta alla tillgängliga rolldefinitioner**

    role list [options]

**Hämta en tillgänglig rolldefinitionen**

    role show [options] [name]

**Kommandon toomanage din rolltilldelning**

    role assignment create [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment list [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment delete [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]

## <a name="azure-storage-commands-toomanage-your-storage-objects"></a>Azure storage: kommandon toomanage lagring-objekt
**Kommandon toomanage Storage-konton**

    storage account list [options]
    storage account show [options] <name>
    storage account create [options] <name>
    storage account set [options] <name>
    storage account delete [options] <name>

**Kommandon toomanage dina nycklar för Lagringskonto**

    storage account keys list [options] <name>
    storage account keys renew [options] <name>

**Kommandon tooshow anslutningssträngen för lagring**

    storage account connectionstring show [options] <name>

**Kommandon toomanage behållare för lagring**

    storage container list [options] [prefix]
    storage container show [options] [container]
    storage container create [options] [container]
    storage container delete [options] [container]
    storage container set [options] [container]

**Kommandon toomanage delade åtkomstsignaturer för Storage-behållare**

    storage container sas create [options] [container] [permissions] [expiry]

**Kommandon toomanage lagras åtkomstprinciper för Storage-behållare**

    storage container policy create [options] [container] [name]
    storage container policy show [options] [container] [name]
    storage container policy list [options] [container]
    storage container policy set [options] [container] [name]
    storage container policy delete [options] [container] [name]

**Kommandon toomanage Storage-blobbar**

    storage blob list [options] [container] [prefix]
    storage blob show [options] [container] [blob]
    storage blob delete [options] [container] [blob]
    storage blob upload [options] [file] [container] [blob]
    storage blob download [options] [container] [blob] [destination]

**Kommandon toomanage din blob kopieringsåtgärd**

    storage blob copy start [options] [sourceUri] [destContainer]
    storage blob copy show [options] [container] [blob]
    storage blob copy stop [options] [container] [blob] [copyid]

**Kommandon toomanage delad åtkomstsignatur för Storage-blob**

    storage blob sas create [options] [container] [blob] [permissions] [expiry]

**Kommandon toomanage din lagringsfilresurser**

    storage share create [options] [share]
    storage share show [options] [share]
    storage share delete [options] [share]
    storage share list [options] [prefix]

**Kommandon toomanage lagring-filer**

    storage file list [options] [share] [path]
    storage file delete [options] [share] [path]
    storage file upload [options] [source] [share] [path]
    storage file download [options] [share] [path] [destination]

**Kommandon toomanage din fil Arkivkatalog**

    storage directory create [options] [share] [path]
    storage directory delete [options] [share] [path]

**Kommandon toomanage Storage-köer**

    storage queue create [options] [queue]
    storage queue list [options] [prefix]
    storage queue show [options] [queue]
    storage queue delete [options] [queue]

**Kommandon toomanage delade åtkomstsignaturer för Storage-kö**

    storage queue sas create [options] [queue] [permissions] [expiry]

**Kommandon toomanage lagras åtkomstprinciper för Storage-kö**

    storage queue policy create [options] [queue] [name]
    storage queue policy show [options] [queue] [name]
    storage queue policy list [options] [queue]
    storage queue policy set [options] [queue] [name]
    storage queue policy delete [options] [queue] [name]

**Kommandon toomanage din Loggningsegenskaper för lagring**

    storage logging show [options]
    storage logging set [options]

**Kommandon toomanage din mått lagringsegenskaper**

    storage metrics show [options]
    storage metrics set [options]

**Kommandon toomanage Storage-tabeller**

    storage table create [options] [table]
    storage table list [options] [prefix]
    storage table show [options] [table]
    storage table delete [options] [table]

**Kommandon toomanage delade åtkomstsignaturer din tabell för lagring**

    storage table sas create [options] [table] [permissions] [expiry]

**Kommandon toomanage lagras åtkomstprinciper din tabell för lagring**

    storage table policy create [options] [table] [name]
    storage table policy show [options] [table] [name]
    storage table policy list [options] [table]
    storage table policy set [options] [table] [name]
    storage table policy delete [options] [table] [name]

## <a name="azure-tag-commands-toomanage-your-resource-manager-tag"></a>Azure-taggen: kommandon toomanage din resource manager-tagg
**Lägga till en tagg**

    tag create [options] <name> <value>

**Ta bort en hel tagg eller ett Taggvärde**

    tag delete [options] <name> <value>

**Visar hello tagginformation**

    tag list [options]

**Hämta en tagg**

    tag show [options] [name]

## <a name="azure-vm-commands-toomanage-your-azure-virtual-machines"></a>Azure vm: kommandon toomanage Azure Virtual Machines
**Skapa en virtuell dator**

    vm create [options] <resource-group> <name> <location> <os-type>

**Skapa en virtuell dator med standardresurser**

    vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password

> [!TIP]
> Från och med CLI version 0.10 kan du ange ett kort alias, till exempel ”UbuntuLTS” eller ”Win2012R2Datacenter” som hello `image-urn` för vissa populära Marketplace-bilder. Kör `azure help vm quick-create` för alternativ. Dessutom från och med version 0.10, `azure vm quick-create` använder premium-lagring som standard om den är tillgänglig i hello valda regionen.
> 
> 

**Lista hello virtuella datorer i ett konto**

    vm list [options]

**Hämta en virtuell dator i en resursgrupp**

    vm show [options] <resource-group> <name>

**Ta bort en virtuell dator i en resursgrupp**

    vm delete [options] <resource-group> <name>

**Avsluta en virtuell dator i en resursgrupp**

    vm stop [options] <resource-group> <name>

**Starta om en virtuell dator i en resursgrupp**

    vm restart [options] <resource-group> <name>

**Starta en virtuell dator i en resursgrupp**

    vm start [options] <resource-group> <name>

**Avsluta en virtuell dator i en resurs grupp och versioner hello beräkningsresurser**

    vm deallocate [options] <resource-group> <name>

**Lista över tillgängliga virtuella storlekar**

    vm sizes [options]

**Avbilda hello VM som OS-avbildningen eller VM-avbildning**

    vm capture [options] <resource-group> <name> <vhd-name-prefix>

**Ange hello tillstånd för hello VM tooGeneralized**

    vm generalize [options] <resource-group> <name>

**Hämta Instansvy för hello VM**

    vm get-instance-view [options] <resource-group> <name>

**Aktivera tooreset fjärråtkomst till skrivbordet eller SSH-inställningar för en virtuell dator och tooreset hello lösenord för hello-konto som har administratör eller sudo-behörighet**

    vm reset-access [options] <resource-group> <name>

**Uppdatera virtuell dator med nya data**

    vm set [options] <resource-group> <name>

**Kommandon toomanage data virtuella diskar**

    vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]
    vm disk detach [options] <resource-group> <vm-name> <lun>
    vm disk attach [options] <resource-group> <vm-name> [vhd-url]

**Resurstillägg för kommandon toomanage VM**

    vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>
    vm extension get [options] <resource-group> <vm-name>

**Kommandon toomanage Docker virtuella datorn**

    vm docker create [options] <resource-group> <name> <location> <os-type>

**Kommandon toomanage VM-avbildningar**

    vm image list-publishers [options] <location>
    vm image list-offers [options] <location> <publisher>
    vm image list-skus [options] <location> <publisher> <offer>
    vm image list [options] <location> <publisher> [offer] [sku]
