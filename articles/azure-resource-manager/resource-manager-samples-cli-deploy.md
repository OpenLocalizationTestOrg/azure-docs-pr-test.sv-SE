---
title: aaaAzure CLI skriptexempel - distribuera mallen | Microsoft Docs
description: "Exempelskript för att distribuera en Azure Resource Manager-mall."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 5a94eedbd898ced29d67f8ce3023ca5c65f83af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-template-deployment---azure-cli-script"></a><span data-ttu-id="3aca3-103">Malldistribution Azure Resource Manager - Azure CLI-skript</span><span class="sxs-lookup"><span data-stu-id="3aca3-103">Azure Resource Manager template deployment - Azure CLI script</span></span>

<span data-ttu-id="3aca3-104">Det här skriptet distribuerar en Resource Manager mallen tooa resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3aca3-104">This script deploys a Resource Manager template tooa resource group in your subscription.</span></span>

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="3aca3-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="3aca3-105">Sample script</span></span>

```azurecli
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

# -e: immediately exit if any command has a non-zero exit status
# -o: prevents errors in a pipeline from being masked
# IFS new value is less likely toocause confusing bugs when looping arrays or arguments (e.g. $@)

usage() { echo "Usage: $0 -i <subscriptionId> -g <resourceGroupName> -n <deploymentName> -l <resourceGroupLocation>" 1>&2; exit 1; }

declare subscriptionId=""
declare resourceGroupName=""
declare deploymentName=""
declare resourceGroupLocation=""

# Initialize parameters specified from command line
while getopts ":i:g:n:l:" arg; do
    case "${arg}" in
        i)
            subscriptionId=${OPTARG}
            ;;
        g)
            resourceGroupName=${OPTARG}
            ;;
        n)
            deploymentName=${OPTARG}
            ;;
        l)
            resourceGroupLocation=${OPTARG}
            ;;
        esac
done
shift $((OPTIND-1))

#Prompt for parameters is some required parameters are missing
if [[ -z "$subscriptionId" ]]; then
    echo "Subscription Id:"
    read subscriptionId
    [[ "${subscriptionId:?}" ]]
fi

if [[ -z "$resourceGroupName" ]]; then
    echo "ResourceGroupName:"
    read resourceGroupName
    [[ "${resourceGroupName:?}" ]]
fi

if [[ -z "$deploymentName" ]]; then
    echo "DeploymentName:"
    read deploymentName
fi

if [[ -z "$resourceGroupLocation" ]]; then
    echo "Enter a location below toocreate a new resource group else skip this"
    echo "ResourceGroupLocation:"
    read resourceGroupLocation
fi

#templateFile Path - template file toobe used
templateFilePath="template.json"

if [ ! -f "$templateFilePath" ]; then
    echo "$templateFilePath not found"
    exit 1
fi

#parameter file path
parametersFilePath="parameters.json"

if [ ! -f "$parametersFilePath" ]; then
    echo "$parametersFilePath not found"
    exit 1
fi

if [ -z "$subscriptionId" ] || [ -z "$resourceGroupName" ] || [ -z "$deploymentName" ]; then
    echo "Either one of subscriptionId, resourceGroupName, deploymentName is empty"
    usage
fi

#login tooazure using your credentials
az account show 1> /dev/null

if [ $? != 0 ];
then
    az login
fi

#set hello default subscription id
az account set --subscription $subscriptionId

#Check for existing RG
if [ $(az group exists --name $resourceGroupName) == 'false' ]; then
    echo "Resource group with name" $resourceGroupName "could not be found. Creating new resource group.."
    (
        set -x
        az group create --name $resourceGroupName --location $resourceGroupLocation 1> /dev/null
    )
    else
    echo "Using existing resource group..."
fi

#Start deployment
echo "Starting deployment..."
(
    set -x
    az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-file $templateFilePath --parameters $parametersFilePath
)

if [ $?  == 0 ];
then
    echo "Template has been successfully deployed"
fi
```

## <a name="clean-up-deployment"></a><span data-ttu-id="3aca3-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="3aca3-106">Clean up deployment</span></span> 

<span data-ttu-id="3aca3-107">Hello kör följande kommando tooremove hello resursgruppen och alla dess resurser.</span><span class="sxs-lookup"><span data-stu-id="3aca3-107">Run hello following command tooremove hello resource group and all its resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="3aca3-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="3aca3-108">Script explanation</span></span>

<span data-ttu-id="3aca3-109">Det här skriptet använder hello följande kommandon toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="3aca3-109">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="3aca3-110">Varje objekt i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="3aca3-110">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="3aca3-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="3aca3-111">Command</span></span> | <span data-ttu-id="3aca3-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3aca3-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3aca3-113">AZ gruppen finns</span><span class="sxs-lookup"><span data-stu-id="3aca3-113">az group exists</span></span>](/cli/azure/group#exists) | <span data-ttu-id="3aca3-114">Kontrollerar om resursgruppen finns.</span><span class="sxs-lookup"><span data-stu-id="3aca3-114">Checks whether resource group exists.</span></span> |
| [<span data-ttu-id="3aca3-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="3aca3-115">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="3aca3-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="3aca3-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3aca3-117">Skapa AZ distribution</span><span class="sxs-lookup"><span data-stu-id="3aca3-117">az group deployment create</span></span>](/cli/azure/group/deployment#create) | <span data-ttu-id="3aca3-118">Starta en distribution.</span><span class="sxs-lookup"><span data-stu-id="3aca3-118">Start a deployment.</span></span>  |
| [<span data-ttu-id="3aca3-119">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="3aca3-119">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="3aca3-120">Tar bort en resursgrupp, inklusive alla dess resurser.</span><span class="sxs-lookup"><span data-stu-id="3aca3-120">Deletes a resource group including all its resources.</span></span> |



## <a name="next-steps"></a><span data-ttu-id="3aca3-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3aca3-121">Next steps</span></span>
* <span data-ttu-id="3aca3-122">En introduktion toodeploying mallar, se [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="3aca3-122">For an introduction toodeploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy-cli.md).</span></span>
* <span data-ttu-id="3aca3-123">Information om hur du distribuerar en mall som kräver en SAS-token finns [distribuera privata mallar med SAS-token](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="3aca3-123">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>
* <span data-ttu-id="3aca3-124">toodefine parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="3aca3-124">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="3aca3-125">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="3aca3-125">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

