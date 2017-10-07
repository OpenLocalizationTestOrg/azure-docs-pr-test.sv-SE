---
title: "aaaManage Azure nyckeln valvet med hjälp av CLI | Microsoft Docs"
description: "Använd den här självstudiekursen tooautomate vanliga uppgifter i Nyckelvalvet med hjälp av hello CLI"
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
ms.openlocfilehash: 9ef506faa67e1f0db5b9e303300d63b135ddd7b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-key-vault-using-cli"></a><span data-ttu-id="2e4bc-103">Hantera Nyckelvalv med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="2e4bc-103">Manage Key Vault using CLI</span></span>

<span data-ttu-id="2e4bc-104">Azure Key Vault är tillgängligt i de flesta regioner.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="2e4bc-105">Mer information finns i hello [Key Vault-priser](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-105">For more information, see hello [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="2e4bc-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="2e4bc-106">Introduction</span></span>

<span data-ttu-id="2e4bc-107">Använd den här självstudiekursen toohelp du få igång med Azure Key Vault toocreate en förstärkt behållare (ett valv) i Azure, toostore och hantera krypteringsnycklar och hemligheter i Azure.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-107">Use this tutorial toohelp you get started with Azure Key Vault toocreate a hardened container (a vault) in Azure, toostore and manage cryptographic keys and secrets in Azure.</span></span> <span data-ttu-id="2e4bc-108">Den vägleder dig genom hello processen med att använda Azure plattformsoberoende kommandoradsgränssnittet toocreate ett valv som innehåller en nyckel eller ett lösenord som du sedan kan använda med ett Azure-program.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-108">It walks you through hello process of using Azure Cross-Platform Command-Line Interface toocreate a vault that contains a key or password that you can then use with an Azure application.</span></span> <span data-ttu-id="2e4bc-109">Den sedan visar hur ett program kan sedan använda denna nyckel eller lösenord.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-109">It then shows you how an application can then use that key or password.</span></span>

<span data-ttu-id="2e4bc-110">**Uppskattad tid toocomplete:** 20 minuter</span><span class="sxs-lookup"><span data-stu-id="2e4bc-110">**Estimated time toocomplete:** 20 minutes</span></span>

> [!NOTE]
> <span data-ttu-id="2e4bc-111">Den här självstudiekursen innehåller inte instruktioner för hur toowrite hello Azure-program med något av hello steg, som visar hur tooauthorize ett program toouse en nyckel eller hemlighet i hello nyckeln valvet.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-111">This tutorial does not include instructions on how toowrite hello Azure application that one of hello steps includes, which shows how tooauthorize an application toouse a key or secret in hello key vault.</span></span>
> 
> <span data-ttu-id="2e4bc-112">För närvarande kan du konfigurera Azure Key Vault i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-112">Currently, you cannot configure Azure Key Vault in hello Azure portal.</span></span> <span data-ttu-id="2e4bc-113">Använd i stället instruktionerna plattformsoberoende kommandoradsgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-113">Instead, use these Cross-Platform Command-Line Interface  instructions.</span></span> <span data-ttu-id="2e4bc-114">Azure PowerShell anvisningar Se [självstudierna motsvarande](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-114">Or, for Azure PowerShell instructions, see [this equivalent tutorial](key-vault-get-started.md).</span></span>
> 
> 

<span data-ttu-id="2e4bc-115">Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="2e4bc-115">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e4bc-116">Krav</span><span class="sxs-lookup"><span data-stu-id="2e4bc-116">Prerequisites</span></span>

<span data-ttu-id="2e4bc-117">toocomplete den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-117">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="2e4bc-118">En prenumeration tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-118">A subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="2e4bc-119">Om du inte har någon kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-119">If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial).</span></span>
* <span data-ttu-id="2e4bc-120">Kommandoradsgränssnittet version 0.9.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-120">Command-Line Interface version 0.9.1 or later.</span></span> <span data-ttu-id="2e4bc-121">tooinstall hello senaste versionen och ansluta tooyour Azure-prenumeration, se [installera och konfigurera hello Azure plattformsoberoende kommandoradsgränssnittet](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-121">tooinstall hello latest version and connect tooyour Azure subscription, see [Install and Configure hello Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="2e4bc-122">Ett program som är konfigurerade toouse hello nyckel eller lösenord som du skapar i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-122">An application that will be configured toouse hello key or password that you create in this tutorial.</span></span> <span data-ttu-id="2e4bc-123">Ett exempelprogram som är tillgänglig från hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-123">A sample application is available from hello [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343).</span></span> <span data-ttu-id="2e4bc-124">Instruktioner finns i hello tillhörande Readme-filen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-124">For instructions, see hello accompanying Readme file.</span></span>

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a><span data-ttu-id="2e4bc-125">Få hjälp med Azure plattformsoberoende-kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="2e4bc-125">Getting help with Azure Cross-Platform Command-Line Interface</span></span>

<span data-ttu-id="2e4bc-126">Den här kursen förutsätter att du är bekant med hello-kommandoradsgränssnittet (Bash, Terminal, Kommandotolken)</span><span class="sxs-lookup"><span data-stu-id="2e4bc-126">This tutorial assumes that you are familiar with hello command-line interface (Bash, Terminal, Command prompt)</span></span>

<span data-ttu-id="2e4bc-127">hello--hjälp eller -h kan vara används tooview hjälpen för specifika kommandon.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-127">hello --help or -h parameter can be used tooview help for specific commands.</span></span> <span data-ttu-id="2e4bc-128">Alternativt kan hello hello azure hjälp [kommando] [alternativ] format kan också använda tooreturn samma information.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-128">Alternately, hello azure help [command] [options] format can also be used tooreturn hello same information.</span></span> <span data-ttu-id="2e4bc-129">Till exempel hello följande kommandon för alla returnera hello samma information:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-129">For example, hello following commands all return hello same information:</span></span>

    azure account set --help

    azure account set -h

    azure help account set

<span data-ttu-id="2e4bc-130">När osäkra om hello-parametrar som krävs av ett kommando, se toohelp med--hjälp, -h eller azure hjälp [kommando].</span><span class="sxs-lookup"><span data-stu-id="2e4bc-130">When in doubt about hello parameters needed by a command, refer toohelp using --help, -h or azure help [command].</span></span>

<span data-ttu-id="2e4bc-131">Du kan också läsa följande kurser tooget bekant med Azure Resource Manager i Azure plattformsoberoende-kommandoradsgränssnittet hello:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-131">You can also read hello following tutorials tooget familiar with Azure Resource Manager in Azure Cross-Platform Command-Line Interface:</span></span>

* [<span data-ttu-id="2e4bc-132">Hur tooinstall och konfigurera Azure plattformsoberoende kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="2e4bc-132">How tooinstall and configure Azure Cross-Platform Command Line Interface</span></span>](../cli-install-nodejs.md)
* [<span data-ttu-id="2e4bc-133">Med hjälp av Azure plattformsoberoende kommandoradsgränssnittet med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2e4bc-133">Using Azure Cross-Platform Command-Line Interface with Azure Resource Manager</span></span>](../xplat-cli-azure-resource-manager.md)

## <a name="connect-tooyour-subscriptions"></a><span data-ttu-id="2e4bc-134">Ansluta tooyour prenumerationer</span><span class="sxs-lookup"><span data-stu-id="2e4bc-134">Connect tooyour subscriptions</span></span>

<span data-ttu-id="2e4bc-135">toolog in med ett organisationskonto, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-135">toolog in using an organizational account, use hello following command:</span></span>

    azure login -u username -p password

<span data-ttu-id="2e4bc-136">eller om du vill toolog i genom att skriva interaktivt</span><span class="sxs-lookup"><span data-stu-id="2e4bc-136">or if you want toolog in by typing interactively</span></span>

    azure login

> [!NOTE]
> <span data-ttu-id="2e4bc-137">hello inloggningen metoden fungerar bara med organisationens konto.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-137">hello login method only works with organizational account.</span></span> <span data-ttu-id="2e4bc-138">Ett organisationskonto är en användare som hanteras av din organisation och som definierats i organisationens Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-138">An organizational account is a user that is managed by your organization, and defined in your organization's Azure Active Directory tenant.</span></span>
> 
> 

<span data-ttu-id="2e4bc-139">Om du för närvarande inte har ett organisationskonto och använder ett Microsoft-konto toolog i tooyour Azure-prenumeration, kan du enkelt skapa en med hjälp av hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-139">If you do not currently have an organizational account, and are using a Microsoft account toolog in tooyour Azure subscription, you can easily create one using hello following steps.</span></span>

1. <span data-ttu-id="2e4bc-140">Inloggningen toohello inloggning toohello [Azure-hanteringsportalen](https://manage.windowsazure.com/), och klicka på Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-140">Login toohello Login toohello [Azure Management Portal](https://manage.windowsazure.com/), and click on Active Directory.</span></span>
2. <span data-ttu-id="2e4bc-141">Om det finns ingen katalog, Välj Skapa din katalog och ange hello information som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-141">If no directory exists, select Create your directory and provide hello requested information.</span></span>
3. <span data-ttu-id="2e4bc-142">Välj din katalog och Lägg till en ny användare.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-142">Select your directory and add a new user.</span></span> <span data-ttu-id="2e4bc-143">Den nya användaren är ett organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-143">This new user is an organizational account.</span></span> <span data-ttu-id="2e4bc-144">Vid hello skapandet av hello användaren angav du med både en e-postadress för användaren hello och ett tillfälligt lösenord.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-144">During hello creation of hello user, you will be supplied with both an e-mail address for hello user and a temporary password.</span></span> <span data-ttu-id="2e4bc-145">Spara den här informationen eftersom den används i ett annat steg.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-145">Save this information as it is used in another step.</span></span>
4. <span data-ttu-id="2e4bc-146">Välj inställningar och välj sedan administratörer från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-146">From hello portal, select Settings and then select Administrators.</span></span> <span data-ttu-id="2e4bc-147">Välj Lägg till och Lägg till ny hello-användare som medadministratör.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-147">Select Add, and add hello new user as a co-administrator.</span></span> <span data-ttu-id="2e4bc-148">Detta gör hello organisationskonto toomanage din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-148">This allows hello organizational account toomanage your Azure subscription.</span></span>
5. <span data-ttu-id="2e4bc-149">Slutligen logga ut från hello Azure-portalen och sedan logga in igen med hello nya organisationskonto.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-149">Finally, log out of hello Azure portal and then log back in using hello new organizational account.</span></span> <span data-ttu-id="2e4bc-150">Om detta är hello första gången loggat in med det här kommer du att ange toochange hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-150">If this is hello first time logging in with this account, you will be prompted toochange hello password.</span></span>

<span data-ttu-id="2e4bc-151">Mer information om hur du använder ett organisationskonto med Microsoft Azure finns [registrera dig för Microsoft Azure som en organisation](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-151">For more information about using an organizational account with Microsoft Azure, see [Sign up for Microsoft Azure as an Organization](../active-directory/sign-up-organization.md).</span></span>

<span data-ttu-id="2e4bc-152">Om du har flera prenumerationer och vill toospecify en specifik en toouse för Azure Key Vault, skriver du hello följande toosee hello prenumerationer för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-152">If you have multiple subscriptions and want toospecify a specific one toouse for Azure Key Vault, type hello following toosee hello subscriptions for your account:</span></span>

    azure account list

<span data-ttu-id="2e4bc-153">Sedan toospecify hello prenumeration toouse, typ:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-153">Then, toospecify hello subscription toouse, type:</span></span>

    azure account set <subscription name>

<span data-ttu-id="2e4bc-154">Mer information om hur du konfigurerar Azure plattformsoberoende kommandoradsgränssnittet finns [hur tooInstall och konfigurera Azure plattformsoberoende-kommandoradsgränssnittet](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-154">For more information about configuring Azure Cross-Platform Command-Line Interface, see [How tooInstall and Configure Azure Cross-Platform Command-Line Interface](../cli-install-nodejs.md).</span></span>

## <a name="switch-toousing-azure-resource-manager"></a><span data-ttu-id="2e4bc-155">Växla toousing Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2e4bc-155">Switch toousing Azure Resource Manager</span></span>
<span data-ttu-id="2e4bc-156">hello Key Vault kräver Azure Resource Manager, så Skriv hello följande tooswitch tooAzure Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-156">hello Key Vault requires Azure Resource Manager, so type hello following tooswitch tooAzure Resource Manager mode:</span></span>

    azure config mode arm

## <a name="create-a-new-resource-group"></a><span data-ttu-id="2e4bc-157">Skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="2e4bc-157">Create a new resource group</span></span>
<span data-ttu-id="2e4bc-158">När du använder Azure Resource Manager, skapas alla relaterade resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-158">When using Azure Resource Manager, all related resources are created inside a resource group.</span></span> <span data-ttu-id="2e4bc-159">Vi skapar en ny resursgrupp 'ContosoResourceGroup' för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-159">We will create a new resource group 'ContosoResourceGroup' for this tutorial.</span></span>

    azure group create 'ContosoResourceGroup' 'East Asia'

<span data-ttu-id="2e4bc-160">hello första parameter resursgruppens namn och andra hello-parametern är hello plats.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-160">hello first parameter is resource group name and hello second parameter is hello location.</span></span> <span data-ttu-id="2e4bc-161">För plats kommandot hello `azure location list` tooidentify hur toospecify en alternativ plats toohello något i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-161">For location, use hello command `azure location list` tooidentify how toospecify an alternative location toohello one in this example.</span></span> <span data-ttu-id="2e4bc-162">Om du behöver mer information skriver du:`azure help location`</span><span class="sxs-lookup"><span data-stu-id="2e4bc-162">If you need more information, type: `azure help location`</span></span>

## <a name="register-hello-key-vault-resource-provider"></a><span data-ttu-id="2e4bc-163">Registerresursleverantören hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="2e4bc-163">Register hello Key Vault resource provider</span></span>
<span data-ttu-id="2e4bc-164">Kontrollera att Nyckelvalvet resursprovidern har registrerats i din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-164">Make sure that Key Vault resource provider is registered in your subscription:</span></span>

`azure provider register Microsoft.KeyVault`

<span data-ttu-id="2e4bc-165">På så sätt behöver bara toobe gjort en gång per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-165">This only needs toobe done once per subscription.</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="2e4bc-166">Skapa ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="2e4bc-166">Create a key vault</span></span>

<span data-ttu-id="2e4bc-167">Använd hello `azure keyvault create` kommandot toocreate en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-167">Use hello `azure keyvault create` command toocreate a key vault.</span></span> <span data-ttu-id="2e4bc-168">Det här skriptet har tre obligatoriska parametrar: en resursgruppens namn, ett nyckelvalv namn och hello geografisk plats.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-168">This script has three mandatory parameters: a resource group name, a key vault name, and hello geographic location.</span></span>

<span data-ttu-id="2e4bc-169">Skriv till exempel om du använder hello valvnamnet av ContosoKeyVault hello resursgruppens namn för ContosoResourceGroup och hello placeringen av Östasien:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-169">For example, if you use hello vault name of ContosoKeyVault, hello resource group name of ContosoResourceGroup, and hello location of East Asia, type:</span></span>

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

<span data-ttu-id="2e4bc-170">hello kommandots utdata visar egenskaperna för hello nyckelvalv som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-170">hello output of this command shows properties of hello key vault that you've just created.</span></span> <span data-ttu-id="2e4bc-171">hello två viktigaste egenskaper är:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-171">hello two most important properties are:</span></span>

* <span data-ttu-id="2e4bc-172">**Namnet**: I hello exempel är ContosoKeyVault.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-172">**Name**: In hello example this is ContosoKeyVault.</span></span> <span data-ttu-id="2e4bc-173">Du ska använda det här namnet för andra Key Vault-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-173">You will use this name for other Key Vault cmdlets.</span></span>
* <span data-ttu-id="2e4bc-174">**vaultUri**: I hello exempel är https://contosokeyvault.vault.azure.net.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-174">**vaultUri**: In hello example this is https://contosokeyvault.vault.azure.net.</span></span> <span data-ttu-id="2e4bc-175">Program som använder ditt valv via dess REST-API måste använda denna URI.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-175">Applications that use your vault through its REST API must use this URI.</span></span>

<span data-ttu-id="2e4bc-176">Azure-konto är nu behörig tooperform några åtgärder på den här nyckeln valvet.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-176">Your Azure account is now authorized tooperform any operations on this key vault.</span></span> <span data-ttu-id="2e4bc-177">Vilket ingen annan har ännu.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-177">As yet, nobody else is.</span></span>

## <a name="add-a-key-or-secret-toohello-key-vault"></a><span data-ttu-id="2e4bc-178">Lägga till en nyckel eller Hemlig toohello nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="2e4bc-178">Add a key or secret toohello key vault</span></span>

<span data-ttu-id="2e4bc-179">Om du vill Azure Key Vault toocreate programvaruskyddad nyckel som du kan använda hello `azure key create` kommando och skriver hello följande:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-179">If you want Azure Key Vault toocreate a software-protected key for you, use hello `azure key create` command, and type hello following:</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

<span data-ttu-id="2e4bc-180">Om du har en befintlig nyckel i en PEM-filen sparas som en lokal fil i en fil med namnet softkey.pem som du vill tooupload tooAzure Key Vault skriver du följande tooimport hello nyckeln från hello hello. PEM-filen som skyddar hello nyckeln av programvara i hello Key Vault-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-180">However, if you have an existing key in a .pem file saved as local file in a file named softkey.pem that you want tooupload tooAzure Key Vault, type hello following tooimport hello key from hello .PEM file, which protects hello key by software in hello Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

<span data-ttu-id="2e4bc-181">Nu kan du referera hello nyckel som du har skapat eller överföra tooAzure Key Vault med hjälp av dess URI.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-181">You can now reference hello key that you created or uploaded tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="2e4bc-182">Använd **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways hämta hello aktuella versionen och använda **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/ cgacf4f763ar42ffb0a1gca546aygd87** tooget den här specifika versionen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-182">Use  **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** tooalways get hello current version, and use **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** tooget this specific version.</span></span>

<span data-ttu-id="2e4bc-183">tooadd en hemlig toohello valvet, som är ett lösenord som heter SQLPassword och som har hello värdet för Pa$ w0rd tooAzure Key Vault typen hello följande:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-183">tooadd a secret toohello vault, which is a password named SQLPassword and that has hello value of Pa$$w0rd tooAzure Key Vault, type hello following:</span></span>

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

<span data-ttu-id="2e4bc-184">Nu kan du referera lösenordet du lagt till tooAzure Key Vault med dess URI.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-184">You can now reference this password that you added tooAzure Key Vault, by using its URI.</span></span> <span data-ttu-id="2e4bc-185">Använd **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways hämta hello aktuella versionen och använda **https://ContosoVault.vault.azure.net/secrets/SQLPassword/ 90018dbb96a84117a0d2847ef8e7189d** tooget den här specifika versionen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-185">Use **https://ContosoVault.vault.azure.net/secrets/SQLPassword** tooalways get hello current version, and use **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** tooget this specific version.</span></span>

<span data-ttu-id="2e4bc-186">Nu ska vi visa hello nyckel eller hemlighet som du just skapat:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-186">Let's view hello key or secret that you just created:</span></span>

* <span data-ttu-id="2e4bc-187">tooview nyckeltypen,:`azure keyvault key list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="2e4bc-187">tooview your key, type: `azure keyvault key list --vault-name 'ContosoKeyVault'`</span></span>
* <span data-ttu-id="2e4bc-188">tooview din hemliga, typ:`azure keyvault secret list --vault-name 'ContosoKeyVault'`</span><span class="sxs-lookup"><span data-stu-id="2e4bc-188">tooview your secret, type: `azure keyvault secret list --vault-name 'ContosoKeyVault'`</span></span>

## <a name="register-an-application-with-azure-active-directory"></a><span data-ttu-id="2e4bc-189">Registrera ett program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2e4bc-189">Register an application with Azure Active Directory</span></span>

<span data-ttu-id="2e4bc-190">Det här steget utförs normalt av en utvecklare, på en separat dator.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-190">This step would usually be done by a developer, on a separate computer.</span></span> <span data-ttu-id="2e4bc-191">Det är inte specifik tooAzure Key Vault men ingår här, för fullständighetens skull.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-191">It is not specific tooAzure Key Vault but is included here, for completeness.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2e4bc-192">toocomplete hello kursen, ditt konto, hello valvet och hello-program som du registrerar i det här steget måste vara i hello samma Azure-katalogen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-192">toocomplete hello tutorial, your account, hello vault, and hello application that you will register in this step must all be in hello same Azure directory.</span></span>
> 
> 

<span data-ttu-id="2e4bc-193">Program som använder ett nyckelvalv måste autentiseras med hjälp av en token från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-193">Applications that use a key vault must authenticate by using a token from Azure Active Directory.</span></span> <span data-ttu-id="2e4bc-194">toodo detta, hello ägare av programmet hello måste först registrera programmet hello i sina Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-194">toodo this, hello owner of hello application must first register hello application in their Azure Active Directory.</span></span> <span data-ttu-id="2e4bc-195">Hello slutet av registrering hämtar hello programägaren hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-195">At hello end of registration, hello application owner gets hello following values:</span></span>

* <span data-ttu-id="2e4bc-196">En **program-ID** (även kallat ett klient-ID) och **autentiseringsnyckel** (även kallat hello delad hemlighet).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-196">An **Application ID** (also known as a Client ID) and **authentication key** (also known as hello shared secret).</span></span> <span data-ttu-id="2e4bc-197">hello programmet måste visa båda dessa värden tooAzure Active Directory, tooget en token.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-197">hello application must present both of these values tooAzure Active Directory, tooget a token.</span></span> <span data-ttu-id="2e4bc-198">Hur konfigureras programmet hello toodo detta beror på programmet hello.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-198">How hello application is configured toodo this depends on hello application.</span></span> <span data-ttu-id="2e4bc-199">För hello Key Vault exempelprogrammet anger hello programägaren värdena i hello app.config-fil.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-199">For hello Key Vault sample application, hello application owner sets these values in hello app.config file.</span></span>

<span data-ttu-id="2e4bc-200">tooregister hello program i Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-200">tooregister hello application in Azure Active Directory:</span></span>

1. <span data-ttu-id="2e4bc-201">Logga in toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-201">Sign in toohello Azure portal.</span></span>
2. <span data-ttu-id="2e4bc-202">Klicka på vänster hello **Active Directory**, och välj sedan hello katalog där du ska registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-202">On hello left, click **Active Directory**, and then select hello directory in which you will register your application.</span></span> <br> <br> 

>[!NOTE] 
> <span data-ttu-id="2e4bc-203">Du måste välja hello samma katalog som innehåller hello Azure-prenumeration som du skapade nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-203">You must select hello same directory that contains hello Azure subscription with which you created your key vault.</span></span> <span data-ttu-id="2e4bc-204">Om du inte vet vilken katalog detta är klickar du på **inställningar**, identifiera hello prenumeration som du skapade ditt nyckelvalv och Observera hello namnet på hello katalog visas i hello sista kolumnen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-204">If you do not know which directory this is, click **Settings**, identify hello subscription with which you created your key vault, and note hello name of hello directory displayed in hello last column.</span></span>

3. <span data-ttu-id="2e4bc-205">Klicka på **Program**.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-205">Click **APPLICATIONS**.</span></span> <span data-ttu-id="2e4bc-206">Om inga appar har lagts till tooyour directory kan den här sidan visar endast hello **Lägg till en App** länk.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-206">If no apps have been added tooyour directory, this page will show only hello **Add an App** link.</span></span> <span data-ttu-id="2e4bc-207">På hello länk, eller också kan du klicka på hello **lägga till** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-207">Click hello link, or alternatively, you can click hello **ADD** on hello command bar.</span></span>
4. <span data-ttu-id="2e4bc-208">I hello **Lägg till program** på hello **vad vill du vill toodo?** klickar du på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-208">In hello **ADD APPLICATION** wizard, on hello **What do you want toodo?** page, click **Add an application my organization is developing**.</span></span>
5. <span data-ttu-id="2e4bc-209">På hello **berätta om tillämpningsprogrammet** , ange ett namn för ditt program och välja **WEB APPLICATION och/eller webb-API** (hello standard).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-209">On hello **Tell us about your application** page, specify a name for your application and select **WEB APPLICATION AND/OR WEB API** (hello default).</span></span> <span data-ttu-id="2e4bc-210">Klicka på nästa hello-ikonen.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-210">Click hello Next icon.</span></span>
6. <span data-ttu-id="2e4bc-211">På hello **appegenskaper** anger hello **SIGN-ON-URL** och **APP-ID URI** för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-211">On hello **App properties** page, specify hello **SIGN-ON URL** and **APP ID URI** for your web application.</span></span> <span data-ttu-id="2e4bc-212">Om programmet inte har dessa värden kan du hitta på dem för det här steget (du kan till exempel skriva http://test1.contoso.com i båda rutorna).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-212">If your application does not have these values, you can make them up for this step (for example, you could specify http://test1.contoso.com for both boxes).</span></span> <span data-ttu-id="2e4bc-213">Det spelar ingen roll om dessa webbplatser finns; Vad är viktigt är hello appen ID URI för varje program är olika för varje program i din katalog.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-213">It does not matter if these sites exist; what is important is that hello app ID URI for each application is different for every application in your directory.</span></span> <span data-ttu-id="2e4bc-214">hello directory använder denna sträng tooidentify din app.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-214">hello directory uses this string tooidentify your app.</span></span>
7. <span data-ttu-id="2e4bc-215">Klicka på hello fullständig ikonen toosave ändringarna i hello guiden.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-215">Click hello Complete icon toosave your changes in hello wizard.</span></span>
8. <span data-ttu-id="2e4bc-216">På hello Snabbstart klickar du på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-216">On hello Quick Start page, click **CONFIGURE**.</span></span>
9. <span data-ttu-id="2e4bc-217">Rulla toohello **nycklar** avsnittet väljer hello varaktighet och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-217">Scroll toohello **keys** section, select hello duration, and then click **SAVE**.</span></span> <span data-ttu-id="2e4bc-218">hello sidan uppdateras och innehåller nu ett nyckelvärde.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-218">hello page refreshes and now shows a key value.</span></span> <span data-ttu-id="2e4bc-219">Du måste konfigurera ditt program med det här värdet för nyckeln och hello **klient-ID** värde.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-219">You must configure your application with this key value and hello **CLIENT ID** value.</span></span> <span data-ttu-id="2e4bc-220">(Anvisningar för den här konfigurationen är programspecifika.)</span><span class="sxs-lookup"><span data-stu-id="2e4bc-220">(Instructions for this configuration will be application-specific.)</span></span>
10. <span data-ttu-id="2e4bc-221">Kopiera hello klient-ID-värde från den här sidan som du vill använda i hello nästa steg tooset behörigheter på ditt valv.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-221">Copy hello client ID value from this page, which you will use in hello next step tooset permissions on your vault.</span></span>

## <a name="authorize-hello-application-toouse-hello-key-or-secret"></a><span data-ttu-id="2e4bc-222">Auktorisera hello programmet toouse hello nyckel eller hemlighet.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-222">Authorize hello application toouse hello key or secret</span></span>
<span data-ttu-id="2e4bc-223">tooauthorize hello programmet tooaccess hello nyckel eller hemlighet i hello valvet, Använd hello `azure keyvault set-policy` kommando.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-223">tooauthorize hello application tooaccess hello key or secret in hello vault, use hello `azure keyvault set-policy` command.</span></span>

<span data-ttu-id="2e4bc-224">Till exempel om din valvnamnet är ContosoKeyVault och hello program som du vill tooauthorize har ett klient-ID för 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed och du vill tooauthorize hello programmet toodecrypt och logga med nycklar i ditt valv, kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-224">For example, if your vault name is ContosoKeyVault and hello application you want tooauthorize has a client ID of 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, and you want tooauthorize hello application toodecrypt and sign with keys in your vault, then run hello following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'

> [!NOTE]
> <span data-ttu-id="2e4bc-225">Om du kör på Windows kommandotolk, ska du ersätta enkla citattecken med dubbla citattecken och också undanta hello interna dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-225">If you are running on Windows command prompt, you should replace single quotes with double quotes, and also escape hello internal double quotes.</span></span> <span data-ttu-id="2e4bc-226">Till exempel ”: [\"dekryptera\",\"logga\"]”.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-226">For example: "[\"decrypt\",\"sign\"]".</span></span>
> 
> 

<span data-ttu-id="2e4bc-227">Om du vill tooauthorize att samma program tooread hemligheter i ditt valv, kör du hello följande:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-227">If you want tooauthorize that same application tooread secrets in your vault, run hello following:</span></span>

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'

## <a name="if-you-want-toouse-a-hardware-security-module-hsm"></a><span data-ttu-id="2e4bc-228">Om du vill toouse en maskinvarusäkerhetsmodul (HSM)</span><span class="sxs-lookup"><span data-stu-id="2e4bc-228">If you want toouse a hardware security module (HSM)</span></span>
<span data-ttu-id="2e4bc-229">För ytterligare säkerhet kan du importera eller generera nycklar i maskinvarusäkerhetsmoduler (HSM) som lämnar aldrig hello HSM gräns.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-229">For added assurance, you can import or generate keys in hardware security modules (HSMs) that never leave hello HSM boundary.</span></span> <span data-ttu-id="2e4bc-230">hello HSM är FIPS 140-2 Level 2-verifierade.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-230">hello HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="2e4bc-231">Om det här kravet inte gäller tooyou, hoppa över det här avsnittet och gå för[ta bort hello nyckelvalvet och associerade nycklar och hemligheter](#delete-the-key-vault-and-associated-keys-and-secrets).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-231">If this requirement doesn't apply tooyou, skip this section and go too[Delete hello key vault and associated keys and secrets](#delete-the-key-vault-and-associated-keys-and-secrets).</span></span>

<span data-ttu-id="2e4bc-232">toocreate nycklarna HSM-skyddad måste du ha en prenumeration för valvet som har stöd för HSM-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-232">toocreate these HSM-protected keys, you must have a vault subscription that supports HSM-protected keys.</span></span>

<span data-ttu-id="2e4bc-233">Lägg till hello ”sku”-parametern när du skapar hello keyvault:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-233">When you create hello keyvault, add hello 'sku' parameter:</span></span>

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

<span data-ttu-id="2e4bc-234">Du kan lägga till programvara-skyddade nycklar (som visas tidigare) och HSM-skyddade nycklar toothis valvet.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-234">You can add software-protected keys (as shown earlier) and HSM-protected keys toothis vault.</span></span> <span data-ttu-id="2e4bc-235">toocreate HSM-skyddad nyckel set hello mål parametern too'HSM':</span><span class="sxs-lookup"><span data-stu-id="2e4bc-235">toocreate an HSM-protected key, set hello Destination parameter too'HSM':</span></span>

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

<span data-ttu-id="2e4bc-236">Du kan använda hello efter kommandot tooimport en nyckel från en PEM-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-236">You can use hello following command tooimport a key from a .pem file on your computer.</span></span> <span data-ttu-id="2e4bc-237">Detta kommando importerar hello nyckel till HSM: er i hello Key Vault-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-237">This command imports hello key into HSMs in hello Key Vault service:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

<span data-ttu-id="2e4bc-238">hello nästa kommando importerar en ”bring your own key” paketet (BYOK).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-238">hello next command imports a “bring your own key" (BYOK) package.</span></span> <span data-ttu-id="2e4bc-239">På så sätt kan du skapa din nyckel i din lokala HSM och överföra den tooHSMs i hello Key Vault-tjänsten, utan hello nyckel lämnar hello HSM gräns:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-239">This lets you generate your key in your local HSM, and transfer it tooHSMs in hello Key Vault service, without hello key leaving hello HSM boundary:</span></span>

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

<span data-ttu-id="2e4bc-240">Detaljerade instruktioner om hur toogenerate BYOK paketet, se [hur toouse HSM-Protected nycklar med Azure Key Vault](key-vault-hsm-protected-keys.md).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-240">For more detailed instructions about how toogenerate this BYOK package, see [How toouse HSM-Protected Keys with Azure Key Vault](key-vault-hsm-protected-keys.md).</span></span>

## <a name="delete-hello-key-vault-and-associated-keys-and-secrets"></a><span data-ttu-id="2e4bc-241">Ta bort hello nyckelvalvet och associerade nycklar och hemligheter</span><span class="sxs-lookup"><span data-stu-id="2e4bc-241">Delete hello key vault and associated keys and secrets</span></span>
<span data-ttu-id="2e4bc-242">Om du behöver inte längre hello nyckelvalvet och hello nyckel eller hemlighet som den innehåller, kan du ta bort hello nyckelvalv med hello azure keyvault delete-kommandot:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-242">If you no longer need hello key vault and hello key or secret that it contains, you can delete hello key vault by using hello azure keyvault delete command:</span></span>

    azure keyvault delete --vault-name 'ContosoKeyVault'

<span data-ttu-id="2e4bc-243">Eller, du kan ta bort en hela Azure resursgrupp, vilket innefattar hello nyckelvalvet och andra resurser som du ingår i gruppen:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-243">Or, you can delete an entire Azure resource group, which includes hello key vault and any other resources that you included in that group:</span></span>

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a><span data-ttu-id="2e4bc-244">Andra Azure plattformsoberoende kommandoradsgränssnittet kommandon</span><span class="sxs-lookup"><span data-stu-id="2e4bc-244">Other Azure Cross-Platform Command-line Interface Commands</span></span>
<span data-ttu-id="2e4bc-245">Andra kommandon som du kan användbart för att hantera Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="2e4bc-245">Other commands that you might useful for managing Azure Key Vault.</span></span>

<span data-ttu-id="2e4bc-246">Det här kommandot visas en tabell visning av alla nycklar och valda egenskaper:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-246">This command lists a tabular display of all keys and selected properties:</span></span>

    azure keyvault key list --vault-name 'ContosoKeyVault'

<span data-ttu-id="2e4bc-247">Detta kommando visar en fullständig lista över egenskaper för hello angiven nyckel:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-247">This command displays a full list of properties for hello specified key:</span></span>

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="2e4bc-248">Det här kommandot visas en tabell visning av alla hemliga namn och egenskaper för valda:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-248">This command lists a tabular display of all secret names and selected properties:</span></span>

    azure keyvault secret list --vault-name 'ContosoKeyVault'

<span data-ttu-id="2e4bc-249">Här är ett exempel på hur tooremove en viss nyckel:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-249">Here's an example of how tooremove a specific key:</span></span>

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

<span data-ttu-id="2e4bc-250">Här är ett exempel på hur tooremove en specifik hemlighet:</span><span class="sxs-lookup"><span data-stu-id="2e4bc-250">Here's an example of how tooremove a specific secret:</span></span>

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a><span data-ttu-id="2e4bc-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e4bc-251">Next steps</span></span>
<span data-ttu-id="2e4bc-252">Programmering referenser finns [hello Azure Key Vault Utvecklarhandbok](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="2e4bc-252">For programming references, see [hello Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>

