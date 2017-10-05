---
title: "Hantera Azure Key Vault med hjälp av CLI | Microsoft Docs"
description: "Använd den här självstudiekursen för att automatisera vanliga uppgifter i Nyckelvalvet med hjälp av CLI"
services: key-vault
documentationcenter: 
author: BrucePerlerMS
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: bruceper
ms.openlocfilehash: c2565a742ce4f6ab5f7639a54c4a475f00cbc260
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-key-vault-using-cli"></a><span data-ttu-id="a71b9-103">Hantera Nyckelvalv med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="a71b9-103">Manage Key Vault using CLI</span></span>

<span data-ttu-id="a71b9-104">Azure Key Vault är tillgängligt i de flesta regioner.</span><span class="sxs-lookup"><span data-stu-id="a71b9-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="a71b9-105">Mer information finns på sidan med [Key Vault-priser](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="a71b9-105">For more information, see the [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="a71b9-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="a71b9-106">Introduction</span></span>

<span data-ttu-id="a71b9-107">Den här självstudiekursen hjälper dig att komma igång med Azure Key Vault för att skapa en säker behållare (ett valv) i Azure för lagring och hantering av krypteringsnycklar och hemligheter i Azure.</span><span class="sxs-lookup"><span data-stu-id="a71b9-107">Use this tutorial to help you get started with Azure Key Vault to create a hardened container (a vault) in Azure, to store and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="a71b9-108">Den vägleder dig genom processen för att skapa ett valv som innehåller en nyckel eller ett lösenord som du sedan kan använda med ett Azure-program med Azure-kommandoradsgränssnittet för flera plattformar.</span><span class="sxs-lookup"><span data-stu-id="a71b9-108">It walks you through the process of using Azure Cross-Platform Command-Line Interface to create a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="a71b9-109">Den sedan visar hur ett program kan sedan använda denna nyckel eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="a71b9-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="a71b9-110">**Uppskattad tidsåtgång:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="a71b9-110">**Estimated time to complete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="a71b9-111">Den här självstudiekursen innehåller inte instruktioner om hur du skriver det Azure-programmet att något av steg innehåller, som visar hur du tillåter ett program att använda en nyckel eller hemlighet i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-111">This tutorial does not include instructions on how to write the Azure application that one of the steps includes, which shows how to authorize an application to use a key or secret in the key vault.</span></span>
> 
> <span data-ttu-id="a71b9-112">För närvarande kan du inte konfigurera Azure Key Vault på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-112">Currently, you cannot configure Azure Key Vault in the Azure portal.</span></span> <span data-ttu-id="a71b9-113">Använd i stället instruktionerna plattformsoberoende kommandoradsgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-113">Instead, use these Cross-Platform Command-Line Interface  instructions.</span></span> <span data-ttu-id="a71b9-114">Azure PowerShell anvisningar Se [självstudierna motsvarande](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a71b9-114">Or, for Azure PowerShell instructions, see [this equivalent tutorial](key-vault-get-started.md).</span></span>
> 
> 

<span data-ttu-id="a71b9-115">Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="a71b9-115">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a71b9-116">Krav</span><span class="sxs-lookup"><span data-stu-id="a71b9-116">Prerequisites</span></span>

<span data-ttu-id="a71b9-117">För att kunna slutföra den här självstudiekursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a71b9-117">To complete this tutorial, you must have the following:</span></span>

* <span data-ttu-id="a71b9-118">En prenumeration på Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="a71b9-118">A subscription to Microsoft Azure.</span></span> <span data-ttu-id="a71b9-119">Om du inte har någon kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="a71b9-119">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="a71b9-120">Kommandoradsgränssnittet version 0.9.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a71b9-120">Command-Line Interface version 0.9.1 or later.</span></span> <span data-ttu-id="a71b9-121">Om du vill installera den senaste versionen och ansluter till din Azure-prenumeration, se [installera och konfigurera Azure-plattformsoberoende kommandoradsgränssnittet](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a71b9-121">To install the latest version and connect to your Azure subscription, see [Install and Configure the Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="a71b9-122">Ett program som ska konfigureras att använda nyckeln eller lösenordet som du skapar i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-122">An application that will be configured to use the key or password that you create in this tutorial.</span></span> <span data-ttu-id="a71b9-123">Ett exempelprogram är tillgängligt från [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="a71b9-123">A sample application is available from the [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="a71b9-124">Anvisningar finns i den tillhörande Readme-filen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-124">For instructions, see the accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="a71b9-125">Få hjälp med Azure plattformsoberoende-kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="a71b9-125">Getting help with Azure Cross-Platform Command-Line Interface</span></span>

<span data-ttu-id="a71b9-126">Den här kursen förutsätter att du är bekant med kommandoradsgränssnittet (Bash, Terminal, Kommandotolken)</span><span class="sxs-lookup"><span data-stu-id="a71b9-126">This tutorial assumes that you are familiar with the command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="a71b9-127">--Hjälp eller -h parametern kan användas för att visa hjälpen för specifika kommandon.</span><span class="sxs-lookup"><span data-stu-id="a71b9-127">The --help or -h parameter can be used to view help for specific commands.</span></span> <span data-ttu-id="a71b9-128">Alternativt kan hjälpa azure [kommando] [alternativ]-format kan också användas för att returnera samma information.</span><span class="sxs-lookup"><span data-stu-id="a71b9-128">Alternately, The azure help [command] [options] format can also be used to return the same information.</span></span> <span data-ttu-id="a71b9-129">Till exempel returnera alla följande kommandon samma information:</span><span class="sxs-lookup"><span data-stu-id="a71b9-129">For example, the following commands all return the same information:</span></span>

    azure account set --help

    azure account set -h

    azure help account set

<span data-ttu-id="a71b9-130">När osäkra om de parametrar som krävs av ett kommando, finns i hjälpen med--hjälp, -h eller azure hjälp [kommando].</span><span class="sxs-lookup"><span data-stu-id="a71b9-130">When in doubt about the parameters needed by a command, refer to help using --help, -h or azure help [command].</span></span>

<span data-ttu-id="a71b9-131">Du kan också läsa följande kurser att bekanta dig med Azure Resource Manager i Azure plattformsoberoende-kommandoradsgränssnittet:</span><span class="sxs-lookup"><span data-stu-id="a71b9-131">You can also read the following tutorials to get familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="a71b9-132">Installera och konfigurera Azure plattformsoberoende kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="a71b9-132">How to install and configure Azure Cross-Platform Command Line Interface</span></span>](../cli-install-nodejs.md)
* [<span data-ttu-id="a71b9-133">Med hjälp av Azure plattformsoberoende kommandoradsgränssnittet med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a71b9-133">Using Azure Cross-Platform Command-Line Interface with Azure Resource Manager</span></span>](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a><span data-ttu-id="a71b9-134">Ansluta till dina prenumerationer</span><span class="sxs-lookup"><span data-stu-id="a71b9-134">Connect to your subscriptions</span></span>

<span data-ttu-id="a71b9-135">Om du vill logga in med ett organisationskonto, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a71b9-135">To log in using an organizational account, use the following command:</span></span>

    azure login -u username -p password

<span data-ttu-id="a71b9-136">eller om du vill logga in genom att skriva interaktivt</span><span class="sxs-lookup"><span data-stu-id="a71b9-136">or if you want to log in by typing interactively</span></span>

    azure login

> [!NOTE]
> <span data-ttu-id="a71b9-137">Inloggnings-metoden fungerar bara med organisationens konto.</span><span class="sxs-lookup"><span data-stu-id="a71b9-137">The login method only works with organizational account.</span></span> <span data-ttu-id="a71b9-138">Ett organisationskonto är en användare som hanteras av din organisation och som definierats i organisationens Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="a71b9-138">An organizational account is a user that is managed by your organization, and defined in your organization's Azure Active Directory tenant.</span></span>
> 
> 

<span data-ttu-id="a71b9-139">Om du för närvarande inte har ett organisationskonto och använder ett Microsoft-konto för att logga in på Azure-prenumerationen, kan du enkelt skapa ett med hjälp av följande steg.</span><span class="sxs-lookup"><span data-stu-id="a71b9-139">If you do not currently have an organizational account, and are using a Microsoft account to log in to your Azure subscription, you can easily create one using the following steps.</span></span>

1. <span data-ttu-id="a71b9-140">Logga in för att logga in på den [Azure-hanteringsportalen](https://manage.windowsazure.com/), och klicka på Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a71b9-140">Login to the Login to the [Azure Management Portal](https://manage.windowsazure.com/), and click on Active Directory.</span></span>
2. <span data-ttu-id="a71b9-141">Om det finns ingen katalog, Välj Skapa din katalog och ange den begärda informationen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-141">If no directory exists, select Create your directory and provide the requested information.</span></span>
3. <span data-ttu-id="a71b9-142">Välj din katalog och Lägg till en ny användare.</span><span class="sxs-lookup"><span data-stu-id="a71b9-142">Select your directory and add a new user.</span></span> <span data-ttu-id="a71b9-143">Den nya användaren är ett organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="a71b9-143">This new user is an organizational account.</span></span> <span data-ttu-id="a71b9-144">Under genereringen av användaren angav du med både en e-postadress för användaren och ett tillfälligt lösenord.</span><span class="sxs-lookup"><span data-stu-id="a71b9-144">During the creation of the user, you will be supplied with both an e-mail address for the user and a temporary password.</span></span> <span data-ttu-id="a71b9-145">Spara den här informationen eftersom den används i ett annat steg.</span><span class="sxs-lookup"><span data-stu-id="a71b9-145">Save this information as it is used in another step.</span></span>
4. <span data-ttu-id="a71b9-146">Välj inställningar och välj sedan administratörer från portalen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-146">From the portal, select Settings and then select Administrators.</span></span> <span data-ttu-id="a71b9-147">Välj Lägg till och Lägg till den nya användaren som medadministratör.</span><span class="sxs-lookup"><span data-stu-id="a71b9-147">Select Add, and add the new user as a co-administrator.</span></span> <span data-ttu-id="a71b9-148">På så sätt kan organisationskonto för att hantera Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-148">This allows the organizational account to manage your Azure subscription.</span></span>
5. <span data-ttu-id="a71b9-149">Slutligen logga ut från Azure-portalen och sedan logga in igen med den nya organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="a71b9-149">Finally, log out of the Azure portal and then log back in using the new organizational account.</span></span> <span data-ttu-id="a71b9-150">Om det är första gången loggning med det här kontot, uppmanas du att ändra lösenordet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-150">If this is the first time logging in with this account, you will be prompted to change the password.</span></span>

<span data-ttu-id="a71b9-151">Mer information om hur du använder ett organisationskonto med Microsoft Azure finns [registrera dig för Microsoft Azure som en organisation](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="a71b9-151">For more information about using an organizational account with Microsoft Azure, see [Sign up for Microsoft Azure as an Organization](../active-directory/sign-up-organization.md).</span></span>

<span data-ttu-id="a71b9-152">Om du har flera prenumerationer och vill välja en specifik prenumeration för Azure Key Vault skriver du följande för att visa prenumerationerna för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="a71b9-152">If you have multiple subscriptions and want to specify a specific one to use for Azure Key Vault, type the following to see the subscriptions for your account:</span></span>

    azure account list

<span data-ttu-id="a71b9-153">Ange sedan den prenumeration som du vill använda genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="a71b9-153">Then, to specify the subscription to use, type:</span></span>

    azure account set <subscription name>

<span data-ttu-id="a71b9-154">Mer information om hur du konfigurerar Azure plattformsoberoende kommandoradsgränssnittet finns [installera och konfigurera Azure plattformsoberoende-kommandoradsgränssnittet](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a71b9-154">For more information about configuring Azure Cross-Platform Command-Line Interface, see [How to Install and Configure Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>

## <a name="switch-to-using-azure-resource-manager"></a><span data-ttu-id="a71b9-155">Växla till med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a71b9-155">Switch to using Azure Resource Manager</span></span>
<span data-ttu-id="a71b9-156">Key Vault kräver Azure Resource Manager, så Skriv följande för att växla till läget Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="a71b9-156">The Key Vault requires Azure Resource Manager, so type the following to switch to Azure Resource Manager mode:</span></span>

    azure config mode arm

## <a name="create-a-new-resource-group"></a><span data-ttu-id="a71b9-157">Skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a71b9-157">Create a new resource group</span></span>
<span data-ttu-id="a71b9-158">När du använder Azure Resource Manager, skapas alla relaterade resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a71b9-158">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="a71b9-159">Vi skapar en ny resursgrupp 'ContosoResourceGroup' för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-159">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

    azure group create 'ContosoResourceGroup' 'East Asia'

<span data-ttu-id="a71b9-160">Den första parametern är resursgruppens namn och den andra parametern är platsen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-160">The first parameter is resource group name and the second parameter is the location.</span></span> <span data-ttu-id="a71b9-161">Använd kommandot för plats `azure location list` att identifiera hur du anger en alternativ plats till den i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-161">For location, use the command `azure location list` to identify how to specify an alternative location to the one in this example.</span></span> <span data-ttu-id="a71b9-162">Om du behöver mer information skriver du:`azure help location`</span><span class="sxs-lookup"><span data-stu-id="a71b9-162">If you need more information, type: `azure help location`</span></span>

## <a name="register-the-key-vault-resource-provider"></a><span data-ttu-id="a71b9-163">Registerresursleverantören Key Vault</span><span class="sxs-lookup"><span data-stu-id="a71b9-163">Register the Key Vault resource provider</span></span>
<span data-ttu-id="a71b9-164">Kontrollera att Nyckelvalvet resursprovidern har registrerats i din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="a71b9-164">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

`azure provider register Microsoft.KeyVault`

<span data-ttu-id="a71b9-165">Detta behöver bara göras en gång per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a71b9-165">This only needs to be done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="a71b9-166">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="a71b9-166">Create a key vault</span></span>

<span data-ttu-id="a71b9-167">Använd den `azure keyvault create` kommando för att skapa en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-167">Use the `azure keyvault create` command to create a key vault.</span></span> <span data-ttu-id="a71b9-168">Det här skriptet har tre obligatoriska parametrar: en resursgruppens namn, ett nyckelvalv namn och den geografiska platsen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-168">This script has three mandatory parameters: a resource group name, a key vault name, and the geographic location.</span></span>

<span data-ttu-id="a71b9-169">Skriv till exempel om du använder ContosoKeyVault valvet namn, resursgruppens namn för ContosoResourceGroup och platsen för Östasien:</span><span class="sxs-lookup"><span data-stu-id="a71b9-169">For example, if you use the vault name of ContosoKeyVault, the resource group name of ContosoResourceGroup, and the location of East Asia, type:</span></span>

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

<span data-ttu-id="a71b9-170">Kommandots utdata visar egenskaper för nyckelvalvet som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="a71b9-170">The output of this command shows properties of the key vault that you've just created.</span></span> <span data-ttu-id="a71b9-171">De två viktigaste egenskaperna är:</span><span class="sxs-lookup"><span data-stu-id="a71b9-171">The two most important properties are:</span></span>

* <span data-ttu-id="a71b9-172">**Namnet**: I exemplet är ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="a71b9-172">**Name**: In the example this is ContosoKeyVault.</span></span> <span data-ttu-id="a71b9-173">Du ska använda det här namnet för andra Key Vault-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="a71b9-173">You will use this name for other Key Vault cmdlets.</span></span>
* <span data-ttu-id="a71b9-174">**vaultUri**: I exemplet är https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="a71b9-174">**vaultUri**: In the example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="a71b9-175">Program som använder ditt valv via dess REST-API måste använda denna URI.</span><span class="sxs-lookup"><span data-stu-id="a71b9-175">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="a71b9-176">Nu har ditt Azure-konto behörighet att utföra åtgärder i det här nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-176">Your Azure account is now authorized to perform any operations on this key vault.</span></span> <span data-ttu-id="a71b9-177">Vilket ingen annan har ännu.</span><span class="sxs-lookup"><span data-stu-id="a71b9-177">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-to-the-key-vault"></a><span data-ttu-id="a71b9-178">Lägga till en nyckel eller hemlighet till nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="a71b9-178">Add a key or secret to the key vault</span></span>

<span data-ttu-id="a71b9-179">Om du vill Azure Key Vault för att skapa en programvaruskyddad nyckel som du använder den `azure key create` kommando och skriver du följande:</span><span class="sxs-lookup"><span data-stu-id="a71b9-179">If you want Azure Key Vault to create a software-protected key for you, use the `azure key create` command, and type the following:</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

<span data-ttu-id="a71b9-180">Men om du har en befintlig nyckel i en PEM-filen sparas som en lokal fil i en fil med namnet softkey.pem som du vill överföra till Azure Key Vault, skriver du följande för att importera nyckeln från den. PEM-filen som skyddar nyckeln av programvara i Key Vault-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="a71b9-180">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want to upload to Azure Key Vault, type the following to import the key from the .PEM file, which protects the key by software in the Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

<span data-ttu-id="a71b9-181">Nu kan du referera den nyckel som du har skapat eller överföras till Azure Key Vault med hjälp av dess URI.</span><span class="sxs-lookup"><span data-stu-id="a71b9-181">You can now reference the key that you created or uploaded to Azure Key Vault, by using its URI.</span></span> <span data-ttu-id="a71b9-182">Använd **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** alltid hämta den aktuella versionen och använda **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** att hämta den här specifika versionen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-182">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** to always get the current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** to get this specific version.</span></span>

<span data-ttu-id="a71b9-183">Om du vill lägga till en hemlighet i valvet, vilket är ett lösenord som heter SQLPassword och som har värdet för Pa$ $w0rd till Azure Key Vault, skriver du följande:</span><span class="sxs-lookup"><span data-stu-id="a71b9-183">To add a secret to the vault, which is a password named SQLPassword and that has the value of Pa$$w0rd to Azure Key Vault, type the following:</span></span>

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

<span data-ttu-id="a71b9-184">Nu kan du referera till det här lösenordet som du lagt till i Azure Key Vault med hjälp av dess URI.</span><span class="sxs-lookup"><span data-stu-id="a71b9-184">You can now reference this password that you added to Azure Key Vault, by using its URI.</span></span> <span data-ttu-id="a71b9-185">Använd **https://ContosoVault.vault.azure.net/secrets/SQLPassword** om du alltid vill hämta den senaste versionen och använd **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** om du vill hämta den här specifika versionen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-185">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** to always get the current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** to get this specific version.</span></span>

<span data-ttu-id="a71b9-186">Nu ska vi visa nyckel eller hemlighet som du just skapat:</span><span class="sxs-lookup"><span data-stu-id="a71b9-186">Let's view the key or secret that you just created:</span></span>

* <span data-ttu-id="a71b9-187">Om du vill visa din nyckel skriver du: `azure keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="a71b9-187">To view your key, type: `azure keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="a71b9-188">Om du vill visa din hemlighet skriver du: `azure keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="a71b9-188">To view your secret, type: `azure keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="a71b9-189">Registrera ett program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a71b9-189">Register an application with Azure Active Directory</span></span>

<span data-ttu-id="a71b9-190">Det här steget utförs normalt av en utvecklare, på en separat dator.</span><span class="sxs-lookup"><span data-stu-id="a71b9-190">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="a71b9-191">Det är inte specifik för Azure Key Vault men ingår här, för fullständighetens skull.</span><span class="sxs-lookup"><span data-stu-id="a71b9-191">It is not specific to Azure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a71b9-192">För att slutföra självstudiekursen måste ditt konto, valvet och det program som du ska registrera i det här steget finnas i samma Azure-katalog.</span><span class="sxs-lookup"><span data-stu-id="a71b9-192">To complete the tutorial, your account, the vault, and the application that you will register in this step must all be in the same Azure directory.</span></span>
> 
> 

<span data-ttu-id="a71b9-193">Program som använder ett nyckelvalv måste autentiseras med hjälp av en token från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a71b9-193">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="a71b9-194">Programmets ägare måste då först registrera programmet i sin Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a71b9-194">To do this, the owner of the application must first register the application in their Azure Active Directory.</span></span> <span data-ttu-id="a71b9-195">I slutet av registreringen hämtar programmets ägare följande värden:</span><span class="sxs-lookup"><span data-stu-id="a71b9-195">At the end of registration, the application owner gets the following values:</span></span>

* <span data-ttu-id="a71b9-196">Ett **program-ID** (även kallat ett klient-ID) och en **autentiseringsnyckel** (även kallat den delade nyckeln).</span><span class="sxs-lookup"><span data-stu-id="a71b9-196">An **Application ID** (also known as a Client ID) and **authentication key** (also known as the shared secret).</span></span> <span data-ttu-id="a71b9-197">Programmet måste lämna båda dessa värden till Azure Active Directory att hämta en token.</span><span class="sxs-lookup"><span data-stu-id="a71b9-197">The application must present both of these values to Azure Active Directory, to get a token.</span></span> <span data-ttu-id="a71b9-198">Hur programmet är konfigurerat för att göra detta beror på programmet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-198">How the application is configured to do this depends on the application.</span></span> <span data-ttu-id="a71b9-199">I Key Vault-exempelprogrammet anger programmets ägare dessa värden i filen app.config.</span><span class="sxs-lookup"><span data-stu-id="a71b9-199">For the Key Vault sample application, the application owner sets these values in the app.config file.</span></span>

<span data-ttu-id="a71b9-200">Så här registrerar du programmet i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="a71b9-200">To register the application in Azure Active Directory:</span></span>

1. <span data-ttu-id="a71b9-201">Logga in på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a71b9-201">Sign in to the Azure portal.</span></span>
2. <span data-ttu-id="a71b9-202">Klicka på **Active Directory** till vänster och välj sedan den katalog som du ska registrera programmet i.</span><span class="sxs-lookup"><span data-stu-id="a71b9-202">On the left, click **Active Directory**, and then select the directory in which you will register your application.</span></span> <br> <br> 

>[!NOTE] 
> <span data-ttu-id="a71b9-203">Du måste välja samma katalog som innehåller Azure-prenumeration som du skapade nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-203">You must select the same directory that contains the Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="a71b9-204">Om du inte vet vilken katalog det är klickar du på **Inställningar**, identifierar prenumerationen som du skapade nyckelvalvet med och noterar namnet på katalogen som visas i den sista kolumnen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-204">If you do not know which directory this is, click **Settings**, identify the subscription with which you created your key vault, and note the name of the directory displayed in the last column.</span></span>

3. <span data-ttu-id="a71b9-205">Klicka på **Program**.</span><span class="sxs-lookup"><span data-stu-id="a71b9-205">Click **APPLICATIONS**.</span></span> <span data-ttu-id="a71b9-206">Om inga appar har lagts till din katalog kan den här sidan visar endast den **Lägg till en App** länk.</span><span class="sxs-lookup"><span data-stu-id="a71b9-206">If no apps have been added to your directory, this page will show only the **Add an App** link.</span></span> <span data-ttu-id="a71b9-207">Klicka på länken eller också kan du klicka på den **lägga till** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-207">Click the link, or alternatively, you can click the **ADD** on the command bar.</span></span>
4. <span data-ttu-id="a71b9-208">I guiden **Lägg till program** på sidan **Vad vill du göra?** klickar du på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="a71b9-208">In the **ADD APPLICATION** wizard, on the **What do you want to do?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="a71b9-209">På den **berätta om tillämpningsprogrammet** , ange ett namn för ditt program och välja **WEB APPLICATION och/eller webb-API** (standard).</span><span class="sxs-lookup"><span data-stu-id="a71b9-209">On the **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (the default).</span></span> <span data-ttu-id="a71b9-210">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="a71b9-210">Click the Next icon.</span></span>
6. <span data-ttu-id="a71b9-211">På sidan **Appegenskaper** anger du webbappens **inloggnings-URL** och **appidentitets-URI**.</span><span class="sxs-lookup"><span data-stu-id="a71b9-211">On the **App properties** page, specify the **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="a71b9-212">Om programmet inte har dessa värden kan du hitta på dem för det här steget (du kan till exempel skriva http://test1.contoso.com i båda rutorna).</span><span class="sxs-lookup"><span data-stu-id="a71b9-212">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="a71b9-213">Det spelar ingen roll om dessa platser finns eller inte. Det viktiga är att appidentitets-URI:n för varje program är olika för varje program i katalogen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-213">It does not matter if these sites exist; what is important is that the app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="a71b9-214">Katalogen använder den här strängen för att identifiera din app.</span><span class="sxs-lookup"><span data-stu-id="a71b9-214">The directory uses this string to identify your app.</span></span>
7. <span data-ttu-id="a71b9-215">Klicka på ikonen klar för att spara ändringarna i guiden.</span><span class="sxs-lookup"><span data-stu-id="a71b9-215">Click the Complete icon to save your changes in the wizard.</span></span>
8. <span data-ttu-id="a71b9-216">På sidan Snabbstart **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="a71b9-216">On the Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="a71b9-217">Bläddra till **nycklarna**, välj varaktighet och klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a71b9-217">Scroll to the **keys** section, select the duration, and then click **SAVE**.</span></span> <span data-ttu-id="a71b9-218">Sidan uppdateras och innehåller nu ett nyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="a71b9-218">The page refreshes and now shows a key value.</span></span> <span data-ttu-id="a71b9-219">Du måste konfigurera ditt program med det här nyckelvärdet och värdet **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="a71b9-219">You must configure your application with this key value and the **CLIENT ID** value.</span></span> <span data-ttu-id="a71b9-220">(Anvisningar för den här konfigurationen är programspecifika.)</span><span class="sxs-lookup"><span data-stu-id="a71b9-220">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="a71b9-221">Kopiera värdet för klient-ID:t från den här sidan, som du ska använda i nästa steg för att ange behörigheter för valvet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-221">Copy the client ID value from this page, which you will use in the next step to set permissions on your vault.</span></span>

## <a name="authorize-the-application-to-use-the-key-or-secret"></a><span data-ttu-id="a71b9-222">Godkänna att programmet använder nyckeln eller hemligheten</span><span class="sxs-lookup"><span data-stu-id="a71b9-222">Authorize the application to use the key or secret</span></span>
<span data-ttu-id="a71b9-223">För att ge programmet åtkomst nyckel eller hemlighet i valvet, använda den `azure keyvault set-policy` kommando.</span><span class="sxs-lookup"><span data-stu-id="a71b9-223">To authorize the application to access the key or secret in the vault, use the `azure keyvault set-policy` command.</span></span>

<span data-ttu-id="a71b9-224">Till exempel om du valvet heter ContosoKeyVault och program som du vill verifiera har ett klient-ID för 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed och du vill tillåta program att dekryptera och logga in med nycklar i ditt valv, kör följande:</span><span class="sxs-lookup"><span data-stu-id="a71b9-224">For example, if your vault name is ContosoKeyVault and the application you want to authorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want to authorize the application to decrypt and sign with keys in your vault, then run the following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'

> [!NOTE]
> <span data-ttu-id="a71b9-225">Om du kör på Windows kommandotolk, ska du ersätta enkla citattecken med dubbla citattecken och också undanta interna dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="a71b9-225">If you are running on Windows command prompt, you should replace single quotes with double quotes, and also escape the internal double quotes.</span></span> <span data-ttu-id="a71b9-226">Till exempel ”: [\"dekryptera\",\"logga\"]”.</span><span class="sxs-lookup"><span data-stu-id="a71b9-226">For example: "[\"decrypt\",\"sign\"]".</span></span>
> 
> 

<span data-ttu-id="a71b9-227">Om du vill att samma program ska kunna läsa hemligheter i valvet kör du följande:</span><span class="sxs-lookup"><span data-stu-id="a71b9-227">If you want to authorize that same application to read secrets in your vault, run the following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a><span data-ttu-id="a71b9-228">Om du vill använda en maskinvarusäkerhetsmodul (HSM)</span><span class="sxs-lookup"><span data-stu-id="a71b9-228">If you want to use a hardware security module (HSM)</span></span>
<span data-ttu-id="a71b9-229">Om du vill öka säkerheten ytterligare kan du importera och generera nycklar i maskinvarusäkerhetsmoduler (HSM) som aldrig lämnar HSM-gränsen.</span><span class="sxs-lookup"><span data-stu-id="a71b9-229">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave the HSM boundary.</span></span> <span data-ttu-id="a71b9-230">HSM-modulerna är FIPS 140-2 Level 2-verifierade.</span><span class="sxs-lookup"><span data-stu-id="a71b9-230">The HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="a71b9-231">Om detta krav inte är nödvändigt för dig hoppar du över det här avsnittet och går vidare till [Ta bort nyckelvalvet och associerade nycklar och hemligheter](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="a71b9-231">If this requirement doesn't apply to you, skip this section and go to [Delete the key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="a71b9-232">Du måste ha en prenumeration för valvet som har stöd för HSM-skyddade nycklar för att skapa dessa HSM-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="a71b9-232">To create these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="a71b9-233">När du skapar keyvault lägger du till parametern ”sku”:</span><span class="sxs-lookup"><span data-stu-id="a71b9-233">When you create the keyvault, add the 'sku' parameter:</span></span>

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

<span data-ttu-id="a71b9-234">Du kan lägga till programvaruskyddade nycklar (som du såg tidigare) och HSM-skyddade nycklar till det här valvet.</span><span class="sxs-lookup"><span data-stu-id="a71b9-234">You can add software-protected keys (as shown earlier) and HSM-protected keys to this vault.</span></span> <span data-ttu-id="a71b9-235">Ange parametern Destination till 'HSM' om du vill skapa en HSM-skyddad nyckel:</span><span class="sxs-lookup"><span data-stu-id="a71b9-235">To create an HSM-protected key, set the Destination parameter to 'HSM':</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

<span data-ttu-id="a71b9-236">Du kan använda följande kommando för att importera en nyckel från en PEM-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a71b9-236">You can use the following command to import a key from a .pem file on your computer.</span></span> <span data-ttu-id="a71b9-237">Det här kommandot importerar nyckeln till HSM-moduler i Key Vault-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="a71b9-237">This command imports the key into HSMs in the Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

<span data-ttu-id="a71b9-238">Nästa kommando importerar ett BYOK-paket (Bring Your Own Key).</span><span class="sxs-lookup"><span data-stu-id="a71b9-238">The next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="a71b9-239">På så sätt kan du generera nyckeln i din lokala HSM och överföra den till HSM-moduler i Key Vault-tjänsten utan att nyckeln lämnar HSM-gränsen:</span><span class="sxs-lookup"><span data-stu-id="a71b9-239">This lets you generate your key in your local HSM, and transfer it to HSMs in the Key Vault service, without the key leaving the HSM boundary:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

<span data-ttu-id="a71b9-240">Mer detaljerad information om hur du skapar den här BYOK-paket finns [använda HSM-Protected nycklar med Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="a71b9-240">For more detailed instructions about how to generate this BYOK package, see [How to use HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="a71b9-241">Ta bort nyckelvalvet och associerade nycklar och hemligheter</span><span class="sxs-lookup"><span data-stu-id="a71b9-241">Delete the key vault and associated keys and secrets</span></span>
<span data-ttu-id="a71b9-242">Om du behöver inte längre nyckelvalvet och nyckel eller hemlighet som den innehåller, kan du ta bort nyckelvalvet med hjälp av azure keyvault delete-kommandot:</span><span class="sxs-lookup"><span data-stu-id="a71b9-242">If you no longer need the key vault and the key or secret that it contains, you can delete the key vault by using the azure keyvault delete command:</span></span>

    azure keyvault delete --vault-name 'ContosoKeyVault'

<span data-ttu-id="a71b9-243">Eller så kan du ta bort en hel Azure-resursgrupp, som innehåller nyckelvalvet och andra resurser som du har lagt till i gruppen:</span><span class="sxs-lookup"><span data-stu-id="a71b9-243">Or, you can delete an entire Azure resource group, which includes the key vault and any other resources that you included in that group:</span></span>

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="a71b9-244">Andra Azure plattformsoberoende kommandoradsgränssnittet kommandon</span><span class="sxs-lookup"><span data-stu-id="a71b9-244">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="a71b9-245">Andra kommandon som du kan användbart för att hantera Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a71b9-245">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="a71b9-246">Det här kommandot visas en tabell visning av alla nycklar och valda egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a71b9-246">This command lists a tabular display of all keys and selected properties:</span></span>

    azure keyvault key list --vault-name 'ContosoKeyVault'

<span data-ttu-id="a71b9-247">Detta kommando visar en fullständig lista över egenskaper för den angivna nyckeln:</span><span class="sxs-lookup"><span data-stu-id="a71b9-247">This command displays a full list of properties for the specified key:</span></span>

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="a71b9-248">Det här kommandot visas en tabell visning av alla hemliga namn och egenskaper för valda:</span><span class="sxs-lookup"><span data-stu-id="a71b9-248">This command lists a tabular display of all secret names and selected properties:</span></span>

    azure keyvault secret list --vault-name 'ContosoKeyVault'

<span data-ttu-id="a71b9-249">Här är ett exempel på hur du tar bort en viss nyckel:</span><span class="sxs-lookup"><span data-stu-id="a71b9-249">Here's an example of how to remove a specific key:</span></span>

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="a71b9-250">Här är ett exempel på hur du tar bort en specifik hemlighet:</span><span class="sxs-lookup"><span data-stu-id="a71b9-250">Here's an example of how to remove a specific secret:</span></span>

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a><span data-ttu-id="a71b9-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a71b9-251">Next steps</span></span>
<span data-ttu-id="a71b9-252">Programmeringsreferenser finns i [utvecklarguiden för Azure Key Vault](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="a71b9-252">For programming references, see [the Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>

