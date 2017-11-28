---
title: "Hantera Azure Key Vault med hjälp av CLI | Microsoft Docs"
description: "Använd den här självstudiekursen för att automatisera vanliga uppgifter i Nyckelvalvet med hjälp av CLI 2.0"
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
ms.openlocfilehash: 5da9f5eceda71ac85259193e0f183c72813e1679
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-key-vault-using-cli-20"></a><span data-ttu-id="f259d-103">Hantera Nyckelvalv med hjälp av CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f259d-103">Manage Key Vault using CLI 2.0</span></span>
<span data-ttu-id="f259d-104">Azure Key Vault är tillgängligt i de flesta regioner.</span><span class="sxs-lookup"><span data-stu-id="f259d-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="f259d-105">Mer information finns på sidan med [Key Vault-priser](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="f259d-105">For more information, see the [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="f259d-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="f259d-106">Introduction</span></span>
<span data-ttu-id="f259d-107">Den här självstudiekursen hjälper dig att komma igång med Azure Key Vault för att skapa en säker behållare (ett valv) i Azure för lagring och hantering av krypteringsnycklar och hemligheter i Azure.</span><span class="sxs-lookup"><span data-stu-id="f259d-107">Use this tutorial to help you get started with Azure Key Vault to create a hardened container (a vault) in Azure, to store and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="f259d-108">Den vägleder dig genom processen för att skapa ett valv som innehåller en nyckel eller ett lösenord som du sedan kan använda med ett Azure-program med Azure-kommandoradsgränssnittet för flera plattformar.</span><span class="sxs-lookup"><span data-stu-id="f259d-108">It walks you through the process of using Azure Cross-Platform Command-Line Interface to create a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="f259d-109">Den sedan visar hur ett program kan sedan använda denna nyckel eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="f259d-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="f259d-110">**Uppskattad tidsåtgång:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="f259d-110">**Estimated time to complete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="f259d-111">Den här självstudiekursen innehåller inte instruktioner om hur du skriver det Azure-programmet att något av steg innehåller, som visar hur du tillåter ett program att använda en nyckel eller hemlighet i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="f259d-111">This tutorial does not include instructions on how to write the Azure application that one of the steps includes, which shows how to authorize an application to use a key or secret in the key vault.</span></span>
>
> <span data-ttu-id="f259d-112">Den här kursen använder den senaste Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="f259d-112">This tutorial uses the latest Azure CLI 2.0.</span></span>
>
>

<span data-ttu-id="f259d-113">Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="f259d-113">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f259d-114">Krav</span><span class="sxs-lookup"><span data-stu-id="f259d-114">Prerequisites</span></span>
<span data-ttu-id="f259d-115">För att kunna slutföra den här självstudiekursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f259d-115">To complete this tutorial, you must have the following:</span></span>

* <span data-ttu-id="f259d-116">En prenumeration på Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f259d-116">A subscription to Microsoft Azure.</span></span> <span data-ttu-id="f259d-117">Om du inte har någon kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="f259d-117">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="f259d-118">Kommandoradsgränssnitt version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f259d-118">Command-Line Interface version 2.0 or later.</span></span> <span data-ttu-id="f259d-119">Om du vill installera den senaste versionen och ansluter till din Azure-prenumeration, se [installera och konfigurera Azure plattformsoberoende kommandoradsgränssnittet 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f259d-119">To install the latest version and connect to your Azure subscription, see [Install and Configure the Azure Cross-Platform Command-Line Interface 2.0](/cli/azure/install-azure-cli).</span></span>
* <span data-ttu-id="f259d-120">Ett program som ska konfigureras att använda nyckeln eller lösenordet som du skapar i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="f259d-120">An application that will be configured to use the key or password that you create in this tutorial.</span></span> <span data-ttu-id="f259d-121">Ett exempelprogram är tillgängligt från [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="f259d-121">A sample application is available from the [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="f259d-122">Anvisningar finns i den tillhörande Readme-filen.</span><span class="sxs-lookup"><span data-stu-id="f259d-122">For instructions, see the accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="f259d-123">Få hjälp med Azure plattformsoberoende-kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="f259d-123">Getting help with Azure Cross-Platform Command-Line Interface</span></span>
<span data-ttu-id="f259d-124">Den här kursen förutsätter att du är bekant med kommandoradsgränssnittet (Bash, Terminal, Kommandotolken)</span><span class="sxs-lookup"><span data-stu-id="f259d-124">This tutorial assumes that you are familiar with the command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="f259d-125">--Hjälp eller -h parametern kan användas för att visa hjälpen för specifika kommandon.</span><span class="sxs-lookup"><span data-stu-id="f259d-125">The --help or -h parameter can be used to view help for specific commands.</span></span> <span data-ttu-id="f259d-126">Alternativt kan hjälpa azure [kommando] [alternativ]-format kan också användas för att returnera samma information.</span><span class="sxs-lookup"><span data-stu-id="f259d-126">Alternately, The azure help [command] [options] format can also be used to return the same information.</span></span> <span data-ttu-id="f259d-127">Till exempel returnera alla följande kommandon samma information:</span><span class="sxs-lookup"><span data-stu-id="f259d-127">For example, the following commands all return the same information:</span></span>

```
az account set --help
az account set -h
```

<span data-ttu-id="f259d-128">När osäkra om de parametrar som krävs av ett kommando, finns i hjälpen med--hjälp, -h eller az help [kommando].</span><span class="sxs-lookup"><span data-stu-id="f259d-128">When in doubt about the parameters needed by a command, refer to help using --help, -h or az help [command].</span></span>

<span data-ttu-id="f259d-129">Du kan också läsa följande kurser att bekanta dig med Azure Resource Manager i Azure plattformsoberoende-kommandoradsgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="f259d-129">You can also read the following tutorials to get familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="f259d-130">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f259d-130">Install Azure CLI</span></span>](/cli/azure/install-azure-cli)
* [<span data-ttu-id="f259d-131">Kom igång med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f259d-131">Get started with Azure CLI 2.0</span></span>](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a><span data-ttu-id="f259d-132">Ansluta till dina prenumerationer</span><span class="sxs-lookup"><span data-stu-id="f259d-132">Connect to your subscriptions</span></span>
<span data-ttu-id="f259d-133">Om du vill logga in med ett organisationskonto, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f259d-133">To log in using an organizational account, use the following command:</span></span>

```
az login -u username@domain.com -p password
```

<span data-ttu-id="f259d-134">eller om du vill logga in genom att skriva interaktivt</span><span class="sxs-lookup"><span data-stu-id="f259d-134">or if you want to log in by typing interactively</span></span>

```
az login
```

<span data-ttu-id="f259d-135">Om du har flera prenumerationer och vill välja en specifik prenumeration för Azure Key Vault skriver du följande för att visa prenumerationerna för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="f259d-135">If you have multiple subscriptions and want to specify a specific one to use for Azure Key Vault, type the following to see the subscriptions for your account:</span></span>

```
az account list
```

<span data-ttu-id="f259d-136">Ange sedan den prenumeration som du vill använda genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="f259d-136">Then, to specify the subscription to use, type:</span></span>

```
az account set --subscription <subscription name or ID>
```

<span data-ttu-id="f259d-137">Mer information om hur du konfigurerar Azure plattformsoberoende kommandoradsgränssnittet finns [installera Azure CLI](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f259d-137">For more information about configuring Azure Cross-Platform Command-Line Interface, see [Install Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-new-resource-group"></a><span data-ttu-id="f259d-138">Skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="f259d-138">Create a new resource group</span></span>
<span data-ttu-id="f259d-139">När du använder Azure Resource Manager, skapas alla relaterade resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f259d-139">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="f259d-140">Vi skapar en ny resursgrupp 'ContosoResourceGroup' för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="f259d-140">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

<span data-ttu-id="f259d-141">Den första parametern är resursgruppens namn och den andra parametern är platsen.</span><span class="sxs-lookup"><span data-stu-id="f259d-141">The first parameter is resource group name and the second parameter is the location.</span></span> <span data-ttu-id="f259d-142">Använd kommandot för plats `az account list-locations` att identifiera hur du anger en alternativ plats till den i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="f259d-142">For location, use the command `az account list-locations` to identify how to specify an alternative location to the one in this example.</span></span> <span data-ttu-id="f259d-143">Om du behöver mer information skriver du: `az account list-locations -h`.</span><span class="sxs-lookup"><span data-stu-id="f259d-143">If you need more information, type: `az account list-locations -h`.</span></span>

## <a name="register-the-key-vault-resource-provider"></a><span data-ttu-id="f259d-144">Registerresursleverantören Key Vault</span><span class="sxs-lookup"><span data-stu-id="f259d-144">Register the Key Vault resource provider</span></span>
<span data-ttu-id="f259d-145">Kontrollera att Nyckelvalvet resursprovidern har registrerats i din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="f259d-145">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

```
az provider register -n Microsoft.KeyVault
```

<span data-ttu-id="f259d-146">Detta behöver bara göras en gång per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f259d-146">This only needs to be done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="f259d-147">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="f259d-147">Create a key vault</span></span>
<span data-ttu-id="f259d-148">Använd den `az keyvault create` kommando för att skapa en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="f259d-148">Use the `az keyvault create` command to create a key vault.</span></span> <span data-ttu-id="f259d-149">Det här skriptet har tre obligatoriska parametrar: en resursgruppens namn, ett nyckelvalv namn och den geografiska platsen.</span><span class="sxs-lookup"><span data-stu-id="f259d-149">This script has three mandatory parameters: a resource group name, a key vault name, and the geographic location.</span></span>

<span data-ttu-id="f259d-150">Skriv till exempel om du använder ContosoKeyVault valvet namn, resursgruppens namn för ContosoResourceGroup och platsen för Östasien:</span><span class="sxs-lookup"><span data-stu-id="f259d-150">For example, if you use the vault name of ContosoKeyVault, the resource group name of ContosoResourceGroup, and the location of East Asia, type:</span></span>
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

<span data-ttu-id="f259d-151">Kommandots utdata visar egenskaper för nyckelvalvet som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="f259d-151">The output of this command shows properties of the key vault that you've just created.</span></span> <span data-ttu-id="f259d-152">De två viktigaste egenskaperna är:</span><span class="sxs-lookup"><span data-stu-id="f259d-152">The two most important properties are:</span></span>

* <span data-ttu-id="f259d-153">**namnet**: I exemplet är ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="f259d-153">**name**: In the example this is ContosoKeyVault.</span></span> <span data-ttu-id="f259d-154">Du använder det här namnet för andra Key Vault-kommandon.</span><span class="sxs-lookup"><span data-stu-id="f259d-154">You will use this name for other Key Vault commands.</span></span>
* <span data-ttu-id="f259d-155">**vaultUri**: I exemplet är https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="f259d-155">**vaultUri**: In the example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="f259d-156">Program som använder ditt valv via dess REST-API måste använda denna URI.</span><span class="sxs-lookup"><span data-stu-id="f259d-156">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="f259d-157">Nu har ditt Azure-konto behörighet att utföra åtgärder i det här nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="f259d-157">Your Azure account is now authorized to perform any operations on this key vault.</span></span> <span data-ttu-id="f259d-158">Vilket ingen annan har ännu.</span><span class="sxs-lookup"><span data-stu-id="f259d-158">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-to-the-key-vault"></a><span data-ttu-id="f259d-159">Lägga till en nyckel eller hemlighet till nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="f259d-159">Add a key or secret to the key vault</span></span>
<span data-ttu-id="f259d-160">Om du vill Azure Key Vault för att skapa en programvaruskyddad nyckel som du använder den `az key create` kommando och skriver du följande:</span><span class="sxs-lookup"><span data-stu-id="f259d-160">If you want Azure Key Vault to create a software-protected key for you, use the `az key create` command, and type the following:</span></span>
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
<span data-ttu-id="f259d-161">Men om du har en befintlig nyckel i en PEM-filen sparas som en lokal fil i en fil med namnet softkey.pem som du vill överföra till Azure Key Vault, skriver du följande för att importera nyckeln från den. PEM-filen som skyddar nyckeln av programvara i Key Vault-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="f259d-161">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want to upload to Azure Key Vault, type the following to import the key from the .PEM file, which protects the key by software in the Key Vault service:</span></span>
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
<span data-ttu-id="f259d-162">Nu kan du referera den nyckel som du har skapat eller överföras till Azure Key Vault med hjälp av dess URI.</span><span class="sxs-lookup"><span data-stu-id="f259d-162">You can now reference the key that you created or uploaded to Azure Key Vault, by using its URI.</span></span> <span data-ttu-id="f259d-163">Använd **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** alltid hämta den aktuella versionen och använda **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** att hämta den här specifika versionen.</span><span class="sxs-lookup"><span data-stu-id="f259d-163">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** to always get the current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** to get this specific version.</span></span>

<span data-ttu-id="f259d-164">Om du vill lägga till en hemlighet i valvet, vilket är ett lösenord som heter SQLPassword och som har värdet för Pa$ $w0rd till Azure Key Vault, skriver du följande:</span><span class="sxs-lookup"><span data-stu-id="f259d-164">To add a secret to the vault, which is a password named SQLPassword and that has the value of Pa$$w0rd to Azure Key Vault, type the following:</span></span>
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
<span data-ttu-id="f259d-165">Nu kan du referera till det här lösenordet som du lagt till i Azure Key Vault med hjälp av dess URI.</span><span class="sxs-lookup"><span data-stu-id="f259d-165">You can now reference this password that you added to Azure Key Vault, by using its URI.</span></span> <span data-ttu-id="f259d-166">Använd **https://ContosoVault.vault.azure.net/secrets/SQLPassword** om du alltid vill hämta den senaste versionen och använd **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** om du vill hämta den här specifika versionen.</span><span class="sxs-lookup"><span data-stu-id="f259d-166">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** to always get the current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** to get this specific version.</span></span>

<span data-ttu-id="f259d-167">Nu ska vi visa nyckel eller hemlighet som du just skapat:</span><span class="sxs-lookup"><span data-stu-id="f259d-167">Let's view the key or secret that you just created:</span></span>

* <span data-ttu-id="f259d-168">Om du vill visa din nyckel skriver du: `az keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="f259d-168">To view your key, type: `az keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="f259d-169">Om du vill visa din hemlighet skriver du: `az keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="f259d-169">To view your secret, type: `az keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="f259d-170">Registrera ett program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f259d-170">Register an application with Azure Active Directory</span></span>
<span data-ttu-id="f259d-171">Det här steget utförs normalt av en utvecklare, på en separat dator.</span><span class="sxs-lookup"><span data-stu-id="f259d-171">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="f259d-172">Det är inte specifik för Azure Key Vault men ingår här, för fullständighetens skull.</span><span class="sxs-lookup"><span data-stu-id="f259d-172">It is not specific to Azure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f259d-173">För att slutföra självstudiekursen måste ditt konto, valvet och det program som du ska registrera i det här steget finnas i samma Azure-katalog.</span><span class="sxs-lookup"><span data-stu-id="f259d-173">To complete the tutorial, your account, the vault, and the application that you will register in this step must all be in the same Azure directory.</span></span>
>
>

<span data-ttu-id="f259d-174">Program som använder ett nyckelvalv måste autentiseras med hjälp av en token från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f259d-174">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="f259d-175">Programmets ägare måste då först registrera programmet i sin Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f259d-175">To do this, the owner of the application must first register the application in their Azure Active Directory.</span></span> <span data-ttu-id="f259d-176">I slutet av registreringen hämtar programmets ägare följande värden:</span><span class="sxs-lookup"><span data-stu-id="f259d-176">At the end of registration, the application owner gets the following values:</span></span>

* <span data-ttu-id="f259d-177">Ett **program-ID** (även kallat ett klient-ID) och en **autentiseringsnyckel** (även kallat den delade nyckeln).</span><span class="sxs-lookup"><span data-stu-id="f259d-177">An **Application ID** (also known as a Client ID) and **authentication key** (also known as the shared secret).</span></span> <span data-ttu-id="f259d-178">Programmet måste lämna båda dessa värden till Azure Active Directory att hämta en token.</span><span class="sxs-lookup"><span data-stu-id="f259d-178">The application must present both of these values to Azure Active Directory, to get a token.</span></span> <span data-ttu-id="f259d-179">Hur programmet är konfigurerat för att göra detta beror på programmet.</span><span class="sxs-lookup"><span data-stu-id="f259d-179">How the application is configured to do this depends on the application.</span></span> <span data-ttu-id="f259d-180">I Key Vault-exempelprogrammet anger programmets ägare dessa värden i filen app.config.</span><span class="sxs-lookup"><span data-stu-id="f259d-180">For the Key Vault sample application, the application owner sets these values in the app.config file.</span></span>

<span data-ttu-id="f259d-181">Så här registrerar du programmet i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="f259d-181">To register the application in Azure Active Directory:</span></span>

1. <span data-ttu-id="f259d-182">Logga in på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f259d-182">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="f259d-183">Klicka på vänster, **Azure Active Directory**, och välj sedan den katalog där du ska registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="f259d-183">On the left, click **Azure Active Directory**, and then select the directory in which you will register your application.</span></span> <br> <br> 

> [!Note] 
> <span data-ttu-id="f259d-184">Du måste välja samma katalog som innehåller Azure-prenumeration som du skapade nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="f259d-184">You must select the same directory that contains the Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="f259d-185">Om du inte vet vilken katalog det är klickar du på **Inställningar**, identifierar prenumerationen som du skapade nyckelvalvet med och noterar namnet på katalogen som visas i den sista kolumnen.</span><span class="sxs-lookup"><span data-stu-id="f259d-185">If you do not know which directory this is, click **Settings**, identify the subscription with which you created your key vault, and note the name of the directory displayed in the last column.</span></span>

3. <span data-ttu-id="f259d-186">Klicka på **Program**.</span><span class="sxs-lookup"><span data-stu-id="f259d-186">Click **APPLICATIONS**.</span></span> <span data-ttu-id="f259d-187">Om inga appar har lagts till din katalog kan den här sidan visar endast den **Lägg till en App** länk.</span><span class="sxs-lookup"><span data-stu-id="f259d-187">If no apps have been added to your directory, this page will show only the **Add an App** link.</span></span> <span data-ttu-id="f259d-188">Klicka på länken eller också kan du klicka på den **lägga till** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="f259d-188">Click the link, or alternatively, you can click the **ADD** on the command bar.</span></span>
4. <span data-ttu-id="f259d-189">I guiden **Lägg till program** på sidan **Vad vill du göra?** klickar du på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="f259d-189">In the **ADD APPLICATION** wizard, on the **What do you want to do?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="f259d-190">På den **berätta om tillämpningsprogrammet** , ange ett namn för ditt program och välja **WEB APPLICATION och/eller webb-API** (standard).</span><span class="sxs-lookup"><span data-stu-id="f259d-190">On the **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (the default).</span></span> <span data-ttu-id="f259d-191">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="f259d-191">Click the Next icon.</span></span>
6. <span data-ttu-id="f259d-192">På sidan **Appegenskaper** anger du webbappens **inloggnings-URL** och **appidentitets-URI**.</span><span class="sxs-lookup"><span data-stu-id="f259d-192">On the **App properties** page, specify the **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="f259d-193">Om programmet inte har dessa värden kan du hitta på dem för det här steget (du kan till exempel skriva http://test1.contoso.com i båda rutorna).</span><span class="sxs-lookup"><span data-stu-id="f259d-193">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="f259d-194">Det spelar ingen roll om dessa platser finns eller inte. Det viktiga är att appidentitets-URI:n för varje program är olika för varje program i katalogen.</span><span class="sxs-lookup"><span data-stu-id="f259d-194">It does not matter if these sites exist; what is important is that the app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="f259d-195">Katalogen använder den här strängen för att identifiera din app.</span><span class="sxs-lookup"><span data-stu-id="f259d-195">The directory uses this string to identify your app.</span></span>
7. <span data-ttu-id="f259d-196">Klicka på ikonen klar för att spara ändringarna i guiden.</span><span class="sxs-lookup"><span data-stu-id="f259d-196">Click the Complete icon to save your changes in the wizard.</span></span>
8. <span data-ttu-id="f259d-197">På sidan Snabbstart **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="f259d-197">On the Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="f259d-198">Bläddra till **nycklarna**, välj varaktighet och klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f259d-198">Scroll to the **keys** section, select the duration, and then click **SAVE**.</span></span> <span data-ttu-id="f259d-199">Sidan uppdateras och innehåller nu ett nyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="f259d-199">The page refreshes and now shows a key value.</span></span> <span data-ttu-id="f259d-200">Du måste konfigurera ditt program med det här nyckelvärdet och värdet **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="f259d-200">You must configure your application with this key value and the **CLIENT ID** value.</span></span> <span data-ttu-id="f259d-201">(Anvisningar för den här konfigurationen är programspecifika.)</span><span class="sxs-lookup"><span data-stu-id="f259d-201">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="f259d-202">Kopiera värdet för klient-ID:t från den här sidan, som du ska använda i nästa steg för att ange behörigheter för valvet.</span><span class="sxs-lookup"><span data-stu-id="f259d-202">Copy the client ID value from this page, which you will use in the next step to set permissions on your vault.</span></span>

## <a name="authorize-the-application-to-use-the-key-or-secret"></a><span data-ttu-id="f259d-203">Godkänna att programmet använder nyckeln eller hemligheten</span><span class="sxs-lookup"><span data-stu-id="f259d-203">Authorize the application to use the key or secret</span></span>
<span data-ttu-id="f259d-204">För att ge programmet åtkomst nyckel eller hemlighet i valvet, använda den `az keyvault set-policy` kommando.</span><span class="sxs-lookup"><span data-stu-id="f259d-204">To authorize the application to access the key or secret in the vault, use the `az keyvault set-policy` command.</span></span>

<span data-ttu-id="f259d-205">Till exempel om du valvet heter ContosoKeyVault och program som du vill verifiera har ett klient-ID för 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed och du vill tillåta program att dekryptera och logga in med nycklar i ditt valv, kör följande:</span><span class="sxs-lookup"><span data-stu-id="f259d-205">For example, if your vault name is ContosoKeyVault and the application you want to authorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want to authorize the application to decrypt and sign with keys in your vault, then run the following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

<span data-ttu-id="f259d-206">Om du vill att samma program ska kunna läsa hemligheter i valvet kör du följande:</span><span class="sxs-lookup"><span data-stu-id="f259d-206">If you want to authorize that same application to read secrets in your vault, run the following:</span></span>
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a><span data-ttu-id="f259d-207">Om du vill använda en maskinvarusäkerhetsmodul (HSM)</span><span class="sxs-lookup"><span data-stu-id="f259d-207">If you want to use a hardware security module (HSM)</span></span>
<span data-ttu-id="f259d-208">Om du vill öka säkerheten ytterligare kan du importera och generera nycklar i maskinvarusäkerhetsmoduler (HSM) som aldrig lämnar HSM-gränsen.</span><span class="sxs-lookup"><span data-stu-id="f259d-208">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave the HSM boundary.</span></span> <span data-ttu-id="f259d-209">HSM-modulerna är FIPS 140-2 Level 2-verifierade.</span><span class="sxs-lookup"><span data-stu-id="f259d-209">The HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="f259d-210">Om detta krav inte är nödvändigt för dig hoppar du över det här avsnittet och går vidare till [Ta bort nyckelvalvet och associerade nycklar och hemligheter](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="f259d-210">If this requirement doesn't apply to you, skip this section and go to [Delete the key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="f259d-211">Du måste ha en prenumeration för valvet som har stöd för HSM-skyddade nycklar för att skapa dessa HSM-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="f259d-211">To create these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="f259d-212">När du skapar keyvault lägger du till parametern ”sku”:</span><span class="sxs-lookup"><span data-stu-id="f259d-212">When you create the keyvault, add the 'sku' parameter:</span></span>

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
<span data-ttu-id="f259d-213">Du kan lägga till programvaruskyddade nycklar (som du såg tidigare) och HSM-skyddade nycklar till det här valvet.</span><span class="sxs-lookup"><span data-stu-id="f259d-213">You can add software-protected keys (as shown earlier) and HSM-protected keys to this vault.</span></span> <span data-ttu-id="f259d-214">Ange parametern Destination till 'HSM' om du vill skapa en HSM-skyddad nyckel:</span><span class="sxs-lookup"><span data-stu-id="f259d-214">To create an HSM-protected key, set the Destination parameter to 'HSM':</span></span>

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

<span data-ttu-id="f259d-215">Du kan använda följande kommando för att importera en nyckel från en PEM-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f259d-215">You can use the following command to import a key from a .pem file on your computer.</span></span> <span data-ttu-id="f259d-216">Det här kommandot importerar nyckeln till HSM-moduler i Key Vault-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="f259d-216">This command imports the key into HSMs in the Key Vault service:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
<span data-ttu-id="f259d-217">Nästa kommando importerar ett BYOK-paket (Bring Your Own Key).</span><span class="sxs-lookup"><span data-stu-id="f259d-217">The next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="f259d-218">På så sätt kan du generera nyckeln i din lokala HSM och överföra den till HSM-moduler i Key Vault-tjänsten utan att nyckeln lämnar HSM-gränsen:</span><span class="sxs-lookup"><span data-stu-id="f259d-218">This lets you generate your key in your local HSM, and transfer it to HSMs in the Key Vault service, without the key leaving the HSM boundary:</span></span>

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
<span data-ttu-id="f259d-219">Mer detaljerad information om hur du skapar den här BYOK-paket finns [använda HSM-Protected nycklar med Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="f259d-219">For more detailed instructions about how to generate this BYOK package, see [How to use HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="f259d-220">Ta bort nyckelvalvet och associerade nycklar och hemligheter</span><span class="sxs-lookup"><span data-stu-id="f259d-220">Delete the key vault and associated keys and secrets</span></span>
<span data-ttu-id="f259d-221">Om du inte längre behöver nyckelvalvet och nyckel eller hemlighet som den innehåller kan du ta bort nyckelvalvet med hjälp av den `az keyvault delete` kommando:</span><span class="sxs-lookup"><span data-stu-id="f259d-221">If you no longer need the key vault and the key or secret that it contains, you can delete the key vault by using the `az keyvault delete` command:</span></span>

```
az keyvault delete --name 'ContosoKeyVault'
```

<span data-ttu-id="f259d-222">Eller så kan du ta bort en hel Azure-resursgrupp, som innehåller nyckelvalvet och andra resurser som du har lagt till i gruppen:</span><span class="sxs-lookup"><span data-stu-id="f259d-222">Or, you can delete an entire Azure resource group, which includes the key vault and any other resources that you included in that group:</span></span>

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="f259d-223">Andra Azure plattformsoberoende kommandoradsgränssnittet kommandon</span><span class="sxs-lookup"><span data-stu-id="f259d-223">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="f259d-224">Andra kommandon som du kan användbart för att hantera Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f259d-224">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="f259d-225">Det här kommandot visas en tabell visning av alla nycklar och valda egenskaper:</span><span class="sxs-lookup"><span data-stu-id="f259d-225">This command lists a tabular display of all keys and selected properties:</span></span>

<span data-ttu-id="f259d-226">AZ keyvault Nyckellista--valvnamnet ContosoKeyVault</span><span class="sxs-lookup"><span data-stu-id="f259d-226">az keyvault key list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="f259d-227">Detta kommando visar en fullständig lista över egenskaper för den angivna nyckeln:</span><span class="sxs-lookup"><span data-stu-id="f259d-227">This command displays a full list of properties for the specified key:</span></span>

<span data-ttu-id="f259d-228">Visa AZ keyvault nyckel--valvnamnet ContosoKeyVault--name 'ContosoFirstKey'</span><span class="sxs-lookup"><span data-stu-id="f259d-228">az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="f259d-229">Det här kommandot visas en tabell visning av alla hemliga namn och egenskaper för valda:</span><span class="sxs-lookup"><span data-stu-id="f259d-229">This command lists a tabular display of all secret names and selected properties:</span></span>

<span data-ttu-id="f259d-230">AZ keyvault hemliga lista--valvnamnet ContosoKeyVault</span><span class="sxs-lookup"><span data-stu-id="f259d-230">az keyvault secret list --vault-name 'ContosoKeyVault'</span></span>

<span data-ttu-id="f259d-231">Här är ett exempel på hur du tar bort en viss nyckel:</span><span class="sxs-lookup"><span data-stu-id="f259d-231">Here's an example of how to remove a specific key:</span></span>

<span data-ttu-id="f259d-232">borttagning av AZ keyvault nyckel--valvnamnet ContosoKeyVault--name 'ContosoFirstKey'</span><span class="sxs-lookup"><span data-stu-id="f259d-232">az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'</span></span>

<span data-ttu-id="f259d-233">Här är ett exempel på hur du tar bort en specifik hemlighet:</span><span class="sxs-lookup"><span data-stu-id="f259d-233">Here's an example of how to remove a specific secret:</span></span>

<span data-ttu-id="f259d-234">AZ keyvault hemlighet ta bort--valvnamnet ContosoKeyVault--name 'SQLPassword'</span><span class="sxs-lookup"><span data-stu-id="f259d-234">az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'</span></span>


## <a name="next-steps"></a><span data-ttu-id="f259d-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f259d-235">Next steps</span></span>
<span data-ttu-id="f259d-236">Fullständiga Azure CLI-referens för nyckelvalvet kommandon, se [Key Vault CLI-referens](/cli/azure/keyvault)</span><span class="sxs-lookup"><span data-stu-id="f259d-236">For complete Azure CLI reference for key vault commands, see [Key Vault CLI reference](/cli/azure/keyvault)</span></span>

<span data-ttu-id="f259d-237">Programmeringsreferenser finns i [utvecklarguiden för Azure Key Vault](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="f259d-237">For programming references, see [the Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>
