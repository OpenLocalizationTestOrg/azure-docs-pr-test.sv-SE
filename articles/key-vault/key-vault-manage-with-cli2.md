---
title: "aaaManage Azure nyckeln valvet med hjälp av CLI | Microsoft Docs"
description: "Använd den här självstudiekursen tooautomate vanliga uppgifter i Nyckelvalvet med hjälp av hello CLI 2.0"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ambapat
ms.openlocfilehash: 76855c0ea09b6b307159468382a6a63627205556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli-20"></a><span data-ttu-id="842fd-103">Hantera Nyckelvalv med hjälp av CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="842fd-103">Manage Key Vault using CLI 2.0</span></span>
<span data-ttu-id="842fd-104">Azure Key Vault är tillgängligt i de flesta regioner.</span><span class="sxs-lookup"><span data-stu-id="842fd-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="842fd-105">Mer information finns i hello [Key Vault-priser](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="842fd-105">For more information, see hello [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="842fd-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="842fd-106">Introduction</span></span>
<span data-ttu-id="842fd-107">Använd den här självstudiekursen toohelp du få igång med Azure Key Vault toocreate en förstärkt behållare (ett valv) i Azure, toostore och hantera krypteringsnycklar och hemligheter i Azure.</span><span class="sxs-lookup"><span data-stu-id="842fd-107">Use this tutorial toohelp you get started with Azure Key Vault toocreate a hardened container (a vault) in Azure, toostore and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="842fd-108">Den vägleder dig genom hello processen med att använda Azure plattformsoberoende kommandoradsgränssnittet toocreate ett valv som innehåller en nyckel eller ett lösenord som du sedan kan använda med ett Azure-program.</span><span class="sxs-lookup"><span data-stu-id="842fd-108">It walks you through hello process of using Azure Cross-Platform Command-Line Interface toocreate a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="842fd-109">Den sedan visar hur ett program kan sedan använda denna nyckel eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="842fd-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="842fd-110">**Uppskattad tid toocomplete:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="842fd-110">**Estimated time toocomplete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="842fd-111">Den här självstudiekursen innehåller inte instruktioner för hur toowrite hello Azure-program med något av hello steg, som visar hur tooauthorize ett program toouse en nyckel eller hemlighet i hello nyckeln valvet.</span><span class="sxs-lookup"><span data-stu-id="842fd-111">This tutorial does not include instructions on how toowrite hello Azure application that one of hello steps includes, which shows how tooauthorize an application toouse a key or secret in hello key vault.</span></span>
>
> <span data-ttu-id="842fd-112">Den här självstudiekursen använder hello senaste Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="842fd-112">This tutorial uses hello latest Azure CLI 2.0.</span></span>
>
>

<span data-ttu-id="842fd-113">Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="842fd-113">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="842fd-114">Krav</span><span class="sxs-lookup"><span data-stu-id="842fd-114">Prerequisites</span></span>
<span data-ttu-id="842fd-115">toocomplete den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="842fd-115">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="842fd-116">En prenumeration tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="842fd-116">A subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="842fd-117">Om du inte har någon kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="842fd-117">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="842fd-118">Kommandoradsgränssnitt version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="842fd-118">Command-Line Interface version 2.0 or later.</span></span> <span data-ttu-id="842fd-119">tooinstall hello senaste versionen och ansluta tooyour Azure-prenumeration, se [installera och konfigurera hello Azure plattformsoberoende kommandoradsgränssnittet 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="842fd-119">tooinstall hello latest version and connect tooyour Azure subscription, see [Install and Configure hello Azure Cross-Platform Command-Line Interface 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="842fd-120">Ett program som är konfigurerade toouse hello nyckel eller lösenord som du skapar i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="842fd-120">An application that will be configured toouse hello key or password that you create in this tutorial.</span></span> <span data-ttu-id="842fd-121">Ett exempelprogram som är tillgänglig från hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="842fd-121">A sample application is available from hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="842fd-122">Instruktioner finns i hello tillhörande Readme-filen.</span><span class="sxs-lookup"><span data-stu-id="842fd-122">For instructions, see hello accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="842fd-123">Få hjälp med Azure plattformsoberoende-kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="842fd-123">Getting help with Azure Cross-Platform Command-Line Interface</span></span>
<span data-ttu-id="842fd-124">Den här kursen förutsätter att du är bekant med hello-kommandoradsgränssnittet (Bash, Terminal, Kommandotolken)</span><span class="sxs-lookup"><span data-stu-id="842fd-124">This tutorial assumes that you are familiar with hello command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="842fd-125">hello--hjälp eller -h kan vara används tooview hjälpen för specifika kommandon.</span><span class="sxs-lookup"><span data-stu-id="842fd-125">hello --help or -h parameter can be used tooview help for specific commands.</span></span> <span data-ttu-id="842fd-126">Alternativt kan hello hello azure hjälp [kommando] [alternativ] format kan också använda tooreturn samma information.</span><span class="sxs-lookup"><span data-stu-id="842fd-126">Alternately, hello azure help [command] [options] format can also be used tooreturn hello same information.</span></span> <span data-ttu-id="842fd-127">Till exempel hello följande kommandon för alla returnera hello samma information:</span><span class="sxs-lookup"><span data-stu-id="842fd-127">For example, hello following commands all return hello same information:</span></span>

```
az account set --help
az account set -h
```

<span data-ttu-id="842fd-128">När osäkra om hello-parametrar som krävs av ett kommando, se toohelp med--hjälp, -h eller az help [kommando].</span><span class="sxs-lookup"><span data-stu-id="842fd-128">When in doubt about hello parameters needed by a command, refer toohelp using --help, -h or az help [command].</span></span>

<span data-ttu-id="842fd-129">Du kan också läsa följande kurser tooget bekant med Azure Resource Manager i Azure plattformsoberoende-kommandoradsgränssnittet hello:</span><span class="sxs-lookup"><span data-stu-id="842fd-129">You can also read hello following tutorials tooget familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="842fd-130">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="842fd-130">Install Azure CLI</span></span>](/cli/azure/install-azure-cli)
* [<span data-ttu-id="842fd-131">Kom igång med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="842fd-131">Get started with Azure CLI 2.0</span></span>](/cli/azure/get-started-with-azure-cli)

## <a name="connect-tooyour-subscriptions"></a><span data-ttu-id="842fd-132">Ansluta tooyour prenumerationer</span><span class="sxs-lookup"><span data-stu-id="842fd-132">Connect tooyour subscriptions</span></span>
<span data-ttu-id="842fd-133">toolog in med ett organisationskonto, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="842fd-133">toolog in using an organizational account, use hello following command:</span></span>

```
az login -u username@domain.com -p password
```

<span data-ttu-id="842fd-134">eller om du vill toolog i genom att skriva interaktivt</span><span class="sxs-lookup"><span data-stu-id="842fd-134">or if you want toolog in by typing interactively</span></span>

```
az login
```

<span data-ttu-id="842fd-135">Om du har flera prenumerationer och vill toospecify en specifik en toouse för Azure Key Vault, skriver du hello följande toosee hello prenumerationer för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="842fd-135">If you have multiple subscriptions and want toospecify a specific one toouse for Azure Key Vault, type hello following toosee hello subscriptions for your account:</span></span>

```
az account list
```

<span data-ttu-id="842fd-136">Sedan toospecify hello prenumeration toouse, typ:</span><span class="sxs-lookup"><span data-stu-id="842fd-136">Then, toospecify hello subscription toouse, type:</span></span>

```
az account set --subscription <subscription name or ID>
```

<span data-ttu-id="842fd-137">Mer information om hur du konfigurerar Azure plattformsoberoende kommandoradsgränssnittet finns [installera Azure CLI](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="842fd-137">For more information about configuring Azure Cross-Platform Command-Line Interface, see [Install Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-new-resource-group"></a><span data-ttu-id="842fd-138">Skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="842fd-138">Create a new resource group</span></span>
<span data-ttu-id="842fd-139">När du använder Azure Resource Manager, skapas alla relaterade resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="842fd-139">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="842fd-140">Vi skapar en ny resursgrupp 'ContosoResourceGroup' för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="842fd-140">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

<span data-ttu-id="842fd-141">hello första parameter resursgruppens namn och andra hello-parametern är hello plats.</span><span class="sxs-lookup"><span data-stu-id="842fd-141">hello first parameter is resource group name and hello second parameter is hello location.</span></span> <span data-ttu-id="842fd-142">För plats kommandot hello `az account list-locations` tooidentify hur toospecify en alternativ plats toohello något i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="842fd-142">For location, use hello command `az account list-locations` tooidentify how toospecify an alternative location toohello one in this example.</span></span> <span data-ttu-id="842fd-143">Om du behöver mer information skriver du: `az account list-locations -h`.</span><span class="sxs-lookup"><span data-stu-id="842fd-143">If you need more information, type: `az account list-locations -h`.</span></span>

## <a name="register-hello-key-vault-resource-provider"></a><span data-ttu-id="842fd-144">Registerresursleverantören hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="842fd-144">Register hello Key Vault resource provider</span></span>
<span data-ttu-id="842fd-145">Kontrollera att Nyckelvalvet resursprovidern har registrerats i din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="842fd-145">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

```
az provider register -n Microsoft.KeyVault
```

<span data-ttu-id="842fd-146">På så sätt behöver bara toobe gjort en gång per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="842fd-146">This only needs toobe done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="842fd-147">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="842fd-147">Create a key vault</span></span>
<span data-ttu-id="842fd-148">Använd hello `az keyvault create` kommandot toocreate en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="842fd-148">Use hello `az keyvault create` command toocreate a key vault.</span></span> <span data-ttu-id="842fd-149">Det här skriptet har tre obligatoriska parametrar: en resursgruppens namn, ett nyckelvalv namn och hello geografisk plats.</span><span class="sxs-lookup"><span data-stu-id="842fd-149">This script has three mandatory parameters: a resource group name, a key vault name, and hello geographic location.</span></span>

<span data-ttu-id="842fd-150">Skriv till exempel om du använder hello valvnamnet av ContosoKeyVault hello resursgruppens namn för ContosoResourceGroup och hello placeringen av Östasien:</span><span class="sxs-lookup"><span data-stu-id="842fd-150">For example, if you use hello vault name of ContosoKeyVault, hello resource group name of ContosoResourceGroup, and hello location of East Asia, type:</span></span>
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

<span data-ttu-id="842fd-151">hello kommandots utdata visar egenskaperna för hello nyckelvalv som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="842fd-151">hello output of this command shows properties of hello key vault that you've just created.</span></span> <span data-ttu-id="842fd-152">hello två viktigaste egenskaper är:</span><span class="sxs-lookup"><span data-stu-id="842fd-152">hello two most important properties are:</span></span>

* <span data-ttu-id="842fd-153">**namnet**: I hello exempel är ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="842fd-153">**name**: In hello example this is ContosoKeyVault.</span></span> <span data-ttu-id="842fd-154">Du använder det här namnet för andra Key Vault-kommandon.</span><span class="sxs-lookup"><span data-stu-id="842fd-154">You will use this name for other Key Vault commands.</span></span>
* <span data-ttu-id="842fd-155">**vaultUri**: I hello exempel är https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="842fd-155">**vaultUri**: In hello example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="842fd-156">Program som använder ditt valv via dess REST-API måste använda denna URI.</span><span class="sxs-lookup"><span data-stu-id="842fd-156">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="842fd-157">Azure-konto är nu behörig tooperform några åtgärder på den här nyckeln valvet.</span><span class="sxs-lookup"><span data-stu-id="842fd-157">Your Azure account is now authorized tooperform any operations on this key vault.</span></span> <span data-ttu-id="842fd-158">Vilket ingen annan har ännu.</span><span class="sxs-lookup"><span data-stu-id="842fd-158">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-toohello-key-vault"></a><span data-ttu-id="842fd-159">Lägga till en nyckel eller Hemlig toohello nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="842fd-159">Add a key or secret toohello key vault</span></span>
<span data-ttu-id="842fd-160">Om du vill Azure Key Vault toocreate programvaruskyddad nyckel som du kan använda hello `az key create` kommando och skriver hello följande:</span><span class="sxs-lookup"><span data-stu-id="842fd-160">If you want Azure Key Vault toocreate a software-protected key for you, use hello `az key create` command, and type hello following:</span></span>
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
<span data-ttu-id="842fd-161">Om du har en befintlig nyckel i en PEM-filen sparas som en lokal fil i en fil med namnet softkey.pem som du vill tooupload tooAzure Key Vault skriver du följande tooimport hello nyckeln från hello hello. PEM-filen som skyddar hello nyckeln av programvara i hello Key Vault-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="842fd-161">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want tooupload tooAzure Key Vault, type hello following tooimport hello key from hello .PEM file, which protects hello key by software in hello Key Vault service:</span></span>
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
<span data-ttu-id="842fd-162">Nu kan du referera hello nyckel som du har skapat eller överföra tooAzure Key Vault med hjälp av dess URI.</span><span class="sxs-lookup"><span data-stu-id="842fd-162">You can now reference hello key that you created or uploaded tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="842fd-163">Använd **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways hämta hello aktuella versionen och använda **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget den här specifika versionen.</span><span class="sxs-lookup"><span data-stu-id="842fd-163">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways get hello current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** tooget this specific version.</span></span>

<span data-ttu-id="842fd-164">tooadd en hemlig toohello valvet, som är ett lösenord som heter SQLPassword och som har hello värdet för Pa$ w0rd tooAzure Key Vault typen hello följande:</span><span class="sxs-lookup"><span data-stu-id="842fd-164">tooadd a secret toohello vault, which is a password named SQLPassword and that has hello value of Pa$$w0rd tooAzure Key Vault, type hello following:</span></span>
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
<span data-ttu-id="842fd-165">Nu kan du referera lösenordet du lagt till tooAzure Key Vault med dess URI.</span><span class="sxs-lookup"><span data-stu-id="842fd-165">You can now reference this password that you added tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="842fd-166">Använd **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways hämta hello aktuella versionen och använda **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget den här specifika versionen.</span><span class="sxs-lookup"><span data-stu-id="842fd-166">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways get hello current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** tooget this specific version.</span></span>

<span data-ttu-id="842fd-167">Nu ska vi visa hello nyckel eller hemlighet som du just skapat:</span><span class="sxs-lookup"><span data-stu-id="842fd-167">Let's view hello key or secret that you just created:</span></span>

* <span data-ttu-id="842fd-168">tooview nyckeltypen,:`az keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="842fd-168">tooview your key, type: `az keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="842fd-169">tooview din hemliga, typ:`az keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="842fd-169">tooview your secret, type: `az keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="842fd-170">Registrera ett program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="842fd-170">Register an application with Azure Active Directory</span></span>
<span data-ttu-id="842fd-171">Det här steget utförs normalt av en utvecklare, på en separat dator.</span><span class="sxs-lookup"><span data-stu-id="842fd-171">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="842fd-172">Det är inte specifik tooAzure Key Vault men ingår här, för fullständighetens skull.</span><span class="sxs-lookup"><span data-stu-id="842fd-172">It is not specific tooAzure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="842fd-173">toocomplete hello kursen, ditt konto, hello valvet och hello-program som du registrerar i det här steget måste vara i hello samma Azure-katalogen.</span><span class="sxs-lookup"><span data-stu-id="842fd-173">toocomplete hello tutorial, your account, hello vault, and hello application that you will register in this step must all be in hello same Azure directory.</span></span>
>
>

<span data-ttu-id="842fd-174">Program som använder ett nyckelvalv måste autentiseras med hjälp av en token från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="842fd-174">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="842fd-175">toodo detta, hello ägare av programmet hello måste först registrera programmet hello i sina Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="842fd-175">toodo this, hello owner of hello application must first register hello application in their Azure Active Directory.</span></span> <span data-ttu-id="842fd-176">Hello slutet av registrering hämtar hello programägaren hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="842fd-176">At hello end of registration, hello application owner gets hello following values:</span></span>

* <span data-ttu-id="842fd-177">En **program-ID** (även kallat ett klient-ID) och **autentiseringsnyckel** (även kallat hello delad hemlighet).</span><span class="sxs-lookup"><span data-stu-id="842fd-177">An **Application ID** (also known as a Client ID) and **authentication key** (also known as hello shared secret).</span></span> <span data-ttu-id="842fd-178">hello programmet måste visa båda dessa värden tooAzure Active Directory, tooget en token.</span><span class="sxs-lookup"><span data-stu-id="842fd-178">hello application must present both of these values tooAzure Active Directory, tooget a token.</span></span> <span data-ttu-id="842fd-179">Hur konfigureras programmet hello toodo detta beror på programmet hello.</span><span class="sxs-lookup"><span data-stu-id="842fd-179">How hello application is configured toodo this depends on hello application.</span></span> <span data-ttu-id="842fd-180">För hello Key Vault exempelprogrammet anger hello programägaren värdena i hello app.config-fil.</span><span class="sxs-lookup"><span data-stu-id="842fd-180">For hello Key Vault sample application, hello application owner sets these values in hello app.config file.</span></span>

<span data-ttu-id="842fd-181">tooregister hello program i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="842fd-181">tooregister hello application in Azure Active Directory:</span></span>

1. <span data-ttu-id="842fd-182">Logga in toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="842fd-182">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="842fd-183">Klicka på vänster hello **Azure Active Directory**, och välj sedan hello katalog där du ska registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="842fd-183">On hello left, click **Azure Active Directory**, and then select hello directory in which you will register your application.</span></span> <br> <br> 

> [!Note] 
> <span data-ttu-id="842fd-184">Du måste välja hello samma katalog som innehåller hello Azure-prenumeration som du skapade nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="842fd-184">You must select hello same directory that contains hello Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="842fd-185">Om du inte vet vilken katalog detta är klickar du på **inställningar**, identifiera hello prenumeration som du skapade ditt nyckelvalv och Observera hello namnet på hello katalog visas i hello sista kolumnen.</span><span class="sxs-lookup"><span data-stu-id="842fd-185">If you do not know which directory this is, click **Settings**, identify hello subscription with which you created your key vault, and note hello name of hello directory displayed in hello last column.</span></span>

3. <span data-ttu-id="842fd-186">Klicka på **Program**.</span><span class="sxs-lookup"><span data-stu-id="842fd-186">Click **APPLICATIONS**.</span></span> <span data-ttu-id="842fd-187">Om inga appar har lagts till tooyour directory kan den här sidan visar endast hello **Lägg till en App** länk.</span><span class="sxs-lookup"><span data-stu-id="842fd-187">If no apps have been added tooyour directory, this page will show only hello **Add an App** link.</span></span> <span data-ttu-id="842fd-188">På hello länk, eller också kan du klicka på hello **lägga till** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="842fd-188">Click hello link, or alternatively, you can click hello **ADD** on hello command bar.</span></span>
4. <span data-ttu-id="842fd-189">I hello **Lägg till program** på hello **vad vill du vill toodo?** klickar du på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="842fd-189">In hello **ADD APPLICATION** wizard, on hello **What do you want toodo?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="842fd-190">På hello **berätta om tillämpningsprogrammet** , ange ett namn för ditt program och välja **WEB APPLICATION och/eller webb-API** (hello standard).</span><span class="sxs-lookup"><span data-stu-id="842fd-190">On hello **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (hello default).</span></span> <span data-ttu-id="842fd-191">Klicka på nästa hello-ikonen.</span><span class="sxs-lookup"><span data-stu-id="842fd-191">Click hello Next icon.</span></span>
6. <span data-ttu-id="842fd-192">På hello **appegenskaper** anger hello **SIGN-ON-URL** och **APP-ID URI** för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="842fd-192">On hello **App properties** page, specify hello **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="842fd-193">Om programmet inte har dessa värden kan du hitta på dem för det här steget (du kan till exempel skriva http://test1.contoso.com i båda rutorna).</span><span class="sxs-lookup"><span data-stu-id="842fd-193">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="842fd-194">Det spelar ingen roll om dessa webbplatser finns; Vad är viktigt är hello appen ID URI för varje program är olika för varje program i din katalog.</span><span class="sxs-lookup"><span data-stu-id="842fd-194">It does not matter if these sites exist; what is important is that hello app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="842fd-195">hello directory använder denna sträng tooidentify din app.</span><span class="sxs-lookup"><span data-stu-id="842fd-195">hello directory uses this string tooidentify your app.</span></span>
7. <span data-ttu-id="842fd-196">Klicka på hello fullständig ikonen toosave ändringarna i hello guiden.</span><span class="sxs-lookup"><span data-stu-id="842fd-196">Click hello Complete icon toosave your changes in hello wizard.</span></span>
8. <span data-ttu-id="842fd-197">På hello Snabbstart klickar du på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="842fd-197">On hello Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="842fd-198">Rulla toohello **nycklar** avsnittet väljer hello varaktighet och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="842fd-198">Scroll toohello **keys** section, select hello duration, and then click **SAVE**.</span></span> <span data-ttu-id="842fd-199">hello sidan uppdateras och innehåller nu ett nyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="842fd-199">hello page refreshes and now shows a key value.</span></span> <span data-ttu-id="842fd-200">Du måste konfigurera ditt program med det här värdet för nyckeln och hello **klient-ID** värde.</span><span class="sxs-lookup"><span data-stu-id="842fd-200">You must configure your application with this key value and hello **CLIENT ID** value.</span></span> <span data-ttu-id="842fd-201">(Anvisningar för den här konfigurationen är programspecifika.)</span><span class="sxs-lookup"><span data-stu-id="842fd-201">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="842fd-202">Kopiera hello klient-ID-värde från den här sidan som du vill använda i hello nästa steg tooset behörigheter på ditt valv.</span><span class="sxs-lookup"><span data-stu-id="842fd-202">Copy hello client ID value from this page, which you will use in hello next step tooset permissions on your vault.</span></span>

## <a name="authorize-hello-application-toouse-hello-key-or-secret"></a><span data-ttu-id="842fd-203">Auktorisera hello programmet toouse hello nyckel eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="842fd-203">Authorize hello application toouse hello key or secret</span></span>
<span data-ttu-id="842fd-204">tooauthorize hello programmet tooaccess hello nyckel eller hemlighet i hello valvet, Använd hello `az keyvault set-policy` kommando.</span><span class="sxs-lookup"><span data-stu-id="842fd-204">tooauthorize hello application tooaccess hello key or secret in hello vault, use hello `az keyvault set-policy` command.</span></span>

<span data-ttu-id="842fd-205">Till exempel om din valvnamnet är ContosoKeyVault och hello program som du vill tooauthorize har ett klient-ID för 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed och du vill tooauthorize hello programmet toodecrypt och logga med nycklar i ditt valv, kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="842fd-205">For example, if your vault name is ContosoKeyVault and hello application you want tooauthorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want tooauthorize hello application toodecrypt and sign with keys in your vault, then run hello following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

<span data-ttu-id="842fd-206">Om du vill tooauthorize att samma program tooread hemligheter i ditt valv, kör du hello följande:</span><span class="sxs-lookup"><span data-stu-id="842fd-206">If you want tooauthorize that same application tooread secrets in your vault, run hello following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a><span data-ttu-id="842fd-207">Om du vill toouse en maskinvarusäkerhetsmodul (HSM)</span><span class="sxs-lookup"><span data-stu-id="842fd-207">If you want toouse a hardware security module (HSM)</span></span>
<span data-ttu-id="842fd-208">För ytterligare säkerhet kan du importera eller generera nycklar i maskinvarusäkerhetsmoduler (HSM) som lämnar aldrig hello HSM gräns.</span><span class="sxs-lookup"><span data-stu-id="842fd-208">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave hello HSM boundary.</span></span> <span data-ttu-id="842fd-209">hello HSM är FIPS 140-2 Level 2-verifierade.</span><span class="sxs-lookup"><span data-stu-id="842fd-209">hello HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="842fd-210">Om det här kravet inte gäller tooyou, hoppa över det här avsnittet och gå för[ta bort hello nyckelvalvet och associerade nycklar och hemligheter](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="842fd-210">If this requirement doesn't apply tooyou, skip this section and go too[Delete hello key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="842fd-211">toocreate nycklarna HSM-skyddad måste du ha en prenumeration för valvet som har stöd för HSM-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="842fd-211">toocreate these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="842fd-212">Lägg till hello ”sku”-parametern när du skapar hello keyvault:</span><span class="sxs-lookup"><span data-stu-id="842fd-212">When you create hello keyvault, add hello 'sku' parameter:</span></span>

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
<span data-ttu-id="842fd-213">Du kan lägga till programvara-skyddade nycklar (som visas tidigare) och HSM-skyddade nycklar toothis valvet.</span><span class="sxs-lookup"><span data-stu-id="842fd-213">You can add software-protected keys (as shown earlier) and HSM-protected keys toothis vault.</span></span> <span data-ttu-id="842fd-214">toocreate HSM-skyddad nyckel set hello mål parametern too'HSM':</span><span class="sxs-lookup"><span data-stu-id="842fd-214">toocreate an HSM-protected key, set hello Destination parameter too'HSM':</span></span>

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

<span data-ttu-id="842fd-215">Du kan använda hello efter kommandot tooimport en nyckel från en PEM-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="842fd-215">You can use hello following command tooimport a key from a .pem file on your computer.</span></span> <span data-ttu-id="842fd-216">Detta kommando importerar hello nyckel till HSM: er i hello Key Vault-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="842fd-216">This command imports hello key into HSMs in hello Key Vault service:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
<span data-ttu-id="842fd-217">hello nästa kommando importerar en ”bring your own key” paketet (BYOK).</span><span class="sxs-lookup"><span data-stu-id="842fd-217">hello next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="842fd-218">På så sätt kan du skapa din nyckel i din lokala HSM och överföra den tooHSMs i hello Key Vault-tjänsten, utan hello nyckel lämnar hello HSM gräns:</span><span class="sxs-lookup"><span data-stu-id="842fd-218">This lets you generate your key in your local HSM, and transfer it tooHSMs in hello Key Vault service, without hello key leaving hello HSM boundary:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
<span data-ttu-id="842fd-219">Detaljerade instruktioner om hur toogenerate BYOK paketet, se [hur toouse HSM-Protected nycklar med Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="842fd-219">For more detailed instructions about how toogenerate this BYOK package, see [How toouse HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="842fd-220">Ta bort hello nyckelvalvet och associerade nycklar och hemligheter</span><span class="sxs-lookup"><span data-stu-id="842fd-220">Delete hello key vault and associated keys and secrets</span></span>
<span data-ttu-id="842fd-221">Om du behöver inte längre hello nyckelvalvet och hello nyckel eller hemlighet som den innehåller, kan du ta bort hello nyckelvalv med hello `az keyvault delete` kommando:</span><span class="sxs-lookup"><span data-stu-id="842fd-221">If you no longer need hello key vault and hello key or secret that it contains, you can delete hello key vault by using hello `az keyvault delete` command:</span></span>

```
az keyvault delete --name 'ContosoKeyVault'
```

<span data-ttu-id="842fd-222">Eller, du kan ta bort en hela Azure resursgrupp, vilket innefattar hello nyckelvalvet och andra resurser som du ingår i gruppen:</span><span class="sxs-lookup"><span data-stu-id="842fd-222">Or, you can delete an entire Azure resource group, which includes hello key vault and any other resources that you included in that group:</span></span>

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="842fd-223">Andra Azure plattformsoberoende kommandoradsgränssnittet kommandon</span><span class="sxs-lookup"><span data-stu-id="842fd-223">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="842fd-224">Andra kommandon som du kan användbart för att hantera Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="842fd-224">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="842fd-225">Det här kommandot visas en tabell visning av alla nycklar och valda egenskaper:</span><span class="sxs-lookup"><span data-stu-id="842fd-225">This command lists a tabular display of all keys and selected properties:</span></span>

<span data-ttu-id="842fd-226">AZ keyvault Nyckellista--valvnamnet ContosoKeyVault</span><span class="sxs-lookup"><span data-stu-id="842fd-226">az keyvault key list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="842fd-227">Detta kommando visar en fullständig lista över egenskaper för hello angiven nyckel:</span><span class="sxs-lookup"><span data-stu-id="842fd-227">This command displays a full list of properties for hello specified key:</span></span>

<span data-ttu-id="842fd-228">Visa AZ keyvault nyckel--valvnamnet ContosoKeyVault--name 'ContosoFirstKey'</span><span class="sxs-lookup"><span data-stu-id="842fd-228">az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="842fd-229">Det här kommandot visas en tabell visning av alla hemliga namn och egenskaper för valda:</span><span class="sxs-lookup"><span data-stu-id="842fd-229">This command lists a tabular display of all secret names and selected properties:</span></span>

<span data-ttu-id="842fd-230">AZ keyvault hemliga lista--valvnamnet ContosoKeyVault</span><span class="sxs-lookup"><span data-stu-id="842fd-230">az keyvault secret list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="842fd-231">Här är ett exempel på hur tooremove en viss nyckel:</span><span class="sxs-lookup"><span data-stu-id="842fd-231">Here's an example of how tooremove a specific key:</span></span>

<span data-ttu-id="842fd-232">borttagning av AZ keyvault nyckel--valvnamnet ContosoKeyVault--name 'ContosoFirstKey'</span><span class="sxs-lookup"><span data-stu-id="842fd-232">az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="842fd-233">Här är ett exempel på hur tooremove en specifik hemlighet:</span><span class="sxs-lookup"><span data-stu-id="842fd-233">Here's an example of how tooremove a specific secret:</span></span>

<span data-ttu-id="842fd-234">AZ keyvault hemlighet ta bort--valvnamnet ContosoKeyVault--name 'SQLPassword'</span><span class="sxs-lookup"><span data-stu-id="842fd-234">az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'</span></span>


## <a name="next-steps"></a><span data-ttu-id="842fd-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="842fd-235">Next steps</span></span>
<span data-ttu-id="842fd-236">Fullständiga Azure CLI-referens för nyckelvalvet kommandon, se [Key Vault CLI-referens](/cli/azure/keyvault)</span><span class="sxs-lookup"><span data-stu-id="842fd-236">For complete Azure CLI reference for key vault commands, see [Key Vault CLI reference](/cli/azure/keyvault)</span></span>

<span data-ttu-id="842fd-237">Programmering referenser finns [hello Azure Key Vault Utvecklarhandbok](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="842fd-237">For programming references, see [hello Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>
