---
title: "aaaGet igång med Azure CLI för Batch | Microsoft Docs"
description: "Få en snabb introduktion toohello Batch-kommandon i Azure CLI för att hantera Azure Batch-tjänsten resurser"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 14f28311ecb16c8097d0d304a4ad89de282a2e9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-azure-cli"></a><span data-ttu-id="f269b-103">Hantera Batch-resurser med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f269b-103">Manage Batch resources with Azure CLI</span></span>

<span data-ttu-id="f269b-104">hello Azure CLI 2.0 är Azures nya kommandoradsverktyget upplevelsen för att hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="f269b-104">hello Azure CLI 2.0 is Azure's new command-line experience for managing Azure resources.</span></span> <span data-ttu-id="f269b-105">Den kan användas i Mac OS, Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="f269b-105">It can be used on macOS, Linux, and Windows.</span></span> <span data-ttu-id="f269b-106">Azure CLI 2.0 är optimerad för att hantera och administrera Azure-resurser från hello Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="f269b-106">Azure CLI 2.0 is optimized for managing and administering Azure resources from hello command line.</span></span> <span data-ttu-id="f269b-107">Du kan använda hello Azure CLI toomanage Azure Batch-konton och toomanage resurser, till exempel pooler, jobb och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="f269b-107">You can use hello Azure CLI toomanage your Azure Batch accounts and toomanage resources such as pools, jobs, and tasks.</span></span> <span data-ttu-id="f269b-108">Med hello Azure CLI, du kan skapa ett skript många hello samma uppgifter som du vill utföra med hello Batch-API: er, Azure-portalen och Batch-PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="f269b-108">With hello Azure CLI, you can script many of hello same tasks you carry out with hello Batch APIs, Azure portal, and Batch PowerShell cmdlets.</span></span>

<span data-ttu-id="f269b-109">Den här artikeln innehåller en översikt över hur du använder [Azure CLI version 2.0](https://docs.microsoft.com/cli/azure/overview) med Batch.</span><span class="sxs-lookup"><span data-stu-id="f269b-109">This article provides an overview of using [Azure CLI version 2.0](https://docs.microsoft.com/cli/azure/overview) with Batch.</span></span> <span data-ttu-id="f269b-110">Se [Kom igång med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) en översikt över hello CLI med Azure.</span><span class="sxs-lookup"><span data-stu-id="f269b-110">See [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) for an overview of using hello CLI with Azure.</span></span>

<span data-ttu-id="f269b-111">Microsoft rekommenderar att du använder hello senaste versionen av hello Azure CLI version 2.0.</span><span class="sxs-lookup"><span data-stu-id="f269b-111">Microsoft recommends using hello latest version of hello Azure CLI, version 2.0.</span></span> <span data-ttu-id="f269b-112">Mer information om version 2.0 finns i [Azure Command Line 2.0 now generally available](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/) (på engelska).</span><span class="sxs-lookup"><span data-stu-id="f269b-112">For more information about version 2.0, see [Azure Command Line 2.0 now generally available](https://azure.microsoft.com/blog/announcing-general-availability-of-vm-storage-and-network-azure-cli-2-0/).</span></span>

## <a name="set-up-hello-azure-cli"></a><span data-ttu-id="f269b-113">Ställ in hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f269b-113">Set up hello Azure CLI</span></span>

<span data-ttu-id="f269b-114">tooinstall hello Azure CLI, följ hello steg som beskrivs i [installera hello Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f269b-114">tooinstall hello Azure CLI, follow hello steps outlined in [Install hello Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli.md).</span></span>

> [!TIP]
> <span data-ttu-id="f269b-115">Vi rekommenderar att du uppdaterar installationen av Azure CLI ofta tootake nytta av tjänsteuppdateringar och förbättringar.</span><span class="sxs-lookup"><span data-stu-id="f269b-115">We recommend that you update your Azure CLI installation frequently tootake advantage of service updates and enhancements.</span></span>
> 
> 

## <a name="command-help"></a><span data-ttu-id="f269b-116">Kommandohjälp</span><span class="sxs-lookup"><span data-stu-id="f269b-116">Command help</span></span>

<span data-ttu-id="f269b-117">Du kan visa hjälptext som visas för alla kommandon i hello Azure CLI genom att lägga till `-h` toohello kommando.</span><span class="sxs-lookup"><span data-stu-id="f269b-117">You can display help text for every command in hello Azure CLI by appending `-h` toohello command.</span></span> <span data-ttu-id="f269b-118">Utelämna andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="f269b-118">Omit any other options.</span></span> <span data-ttu-id="f269b-119">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f269b-119">For example:</span></span>

* <span data-ttu-id="f269b-120">tooget hjälp för hello `az` kommandot, ange:`az -h`</span><span class="sxs-lookup"><span data-stu-id="f269b-120">tooget help for hello `az` command, enter: `az -h`</span></span>
* <span data-ttu-id="f269b-121">tooget en lista över alla Batch-kommandon i hello CLI, använder du:`az batch -h`</span><span class="sxs-lookup"><span data-stu-id="f269b-121">tooget a list of all Batch commands in hello CLI, use: `az batch -h`</span></span>
* <span data-ttu-id="f269b-122">tooget hjälp om hur du skapar ett Batch-konto, ange:`az batch account create -h`</span><span class="sxs-lookup"><span data-stu-id="f269b-122">tooget help on creating a Batch account, enter: `az batch account create -h`</span></span>

<span data-ttu-id="f269b-123">När osäkra, använda hello `-h` kommandoradsalternativet tooget hjälp med några Azure CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="f269b-123">When in doubt, use hello `-h` command-line option tooget help on any Azure CLI command.</span></span>

> [!NOTE]
> <span data-ttu-id="f269b-124">Tidigare versioner av hello Azure CLI används `azure` toopreface kommandot CLI.</span><span class="sxs-lookup"><span data-stu-id="f269b-124">Earlier versions of hello Azure CLI used `azure` toopreface a CLI command.</span></span> <span data-ttu-id="f269b-125">I version 2.0 har alla kommandon nu `az` som prefix.</span><span class="sxs-lookup"><span data-stu-id="f269b-125">In version 2.0, all commands are now prefaced with `az`.</span></span> <span data-ttu-id="f269b-126">Vara säker på att tooupdate skript toouse hello nya syntaxen med version 2.0.</span><span class="sxs-lookup"><span data-stu-id="f269b-126">Be sure tooupdate your scripts toouse hello new syntax with version 2.0.</span></span>
>
>  

<span data-ttu-id="f269b-127">Dessutom finns toohello Azure CLI-referensdokumentationen för information om [Azure CLI-kommandona för Batch](https://docs.microsoft.com/cli/azure/batch).</span><span class="sxs-lookup"><span data-stu-id="f269b-127">Additionally, refer toohello Azure CLI reference documentation for details about [Azure CLI commands for Batch](https://docs.microsoft.com/cli/azure/batch).</span></span> 

## <a name="log-in-and-authenticate"></a><span data-ttu-id="f269b-128">Logga in och autentisera</span><span class="sxs-lookup"><span data-stu-id="f269b-128">Log in and authenticate</span></span>

<span data-ttu-id="f269b-129">toouse hello Azure CLI med Batch, du behöver toolog i och autentisera.</span><span class="sxs-lookup"><span data-stu-id="f269b-129">toouse hello Azure CLI with Batch, you need toolog in and authenticate.</span></span> <span data-ttu-id="f269b-130">Det finns två enkla steg toofollow:</span><span class="sxs-lookup"><span data-stu-id="f269b-130">There are two simple steps toofollow:</span></span>

1. <span data-ttu-id="f269b-131">**Logga in på Azure.**</span><span class="sxs-lookup"><span data-stu-id="f269b-131">**Log into Azure.**</span></span> <span data-ttu-id="f269b-132">Logga in på Azure ger du åtkomst till tooAzure Resource Manager-kommandon, inklusive [Batch Management-tjänsten](batch-management-dotnet.md) kommandon.</span><span class="sxs-lookup"><span data-stu-id="f269b-132">Logging into Azure gives you access tooAzure Resource Manager commands, including [Batch Management service](batch-management-dotnet.md) commands.</span></span>  
2. <span data-ttu-id="f269b-133">**Logga in på Batch-kontot**.</span><span class="sxs-lookup"><span data-stu-id="f269b-133">**Log into your Batch account.**</span></span> <span data-ttu-id="f269b-134">Logga in på din Batch-konto får du åtkomst till tooBatch kommandon.</span><span class="sxs-lookup"><span data-stu-id="f269b-134">Logging into your Batch account gives you access tooBatch service commands.</span></span>   

### <a name="log-in-tooazure"></a><span data-ttu-id="f269b-135">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="f269b-135">Log in tooAzure</span></span>

<span data-ttu-id="f269b-136">Det finns några olika sätt toolog i Azure, beskrivs i detalj i [logga in med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):</span><span class="sxs-lookup"><span data-stu-id="f269b-136">There are a few different ways toolog into Azure, described in detail in [Log in with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli):</span></span>

1. <span data-ttu-id="f269b-137">[Logga in interaktivt](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in).</span><span class="sxs-lookup"><span data-stu-id="f269b-137">[Log in interactively](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#interactive-log-in).</span></span> <span data-ttu-id="f269b-138">Logga in interaktivt när du använder Azure CLI-kommandona själv hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="f269b-138">Log in interactively when you are running Azure CLI commands yourself from hello command line.</span></span>
2. <span data-ttu-id="f269b-139">[Logga in med ett huvudnamn för tjänsten](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal).</span><span class="sxs-lookup"><span data-stu-id="f269b-139">[Log in with a service principal](https://docs.microsoft.com/cli/azure/authenticate-azure-cli#logging-in-with-a-service-principal).</span></span> <span data-ttu-id="f269b-140">Logga in med ett huvudnamn för tjänsten när du använder Azure CLI-kommandon från ett skript eller ett program.</span><span class="sxs-lookup"><span data-stu-id="f269b-140">Log in with a service principal when you are running Azure CLI commands from a script or an application.</span></span>

<span data-ttu-id="f269b-141">Hello enligt den här artikeln visar vi hur toolog till Azure interaktivt.</span><span class="sxs-lookup"><span data-stu-id="f269b-141">For hello purposes of this article, we show how toolog into Azure interactively.</span></span> <span data-ttu-id="f269b-142">Typen [az inloggningen](https://docs.microsoft.com/cli/azure/#login) på hello kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="f269b-142">Type [az login](https://docs.microsoft.com/cli/azure/#login) on hello command line:</span></span>

```azurecli
# Log in tooAzure and authenticate interactively.
az login
```

<span data-ttu-id="f269b-143">Hej `az login` kommandot returnerar en token som du kan använda tooauthenticate, som visas här.</span><span class="sxs-lookup"><span data-stu-id="f269b-143">hello `az login` command returns a token that you can use tooauthenticate, as shown here.</span></span> <span data-ttu-id="f269b-144">Följ hello anvisningarna tooopen en webbsida och skicka hello token tooAzure:</span><span class="sxs-lookup"><span data-stu-id="f269b-144">Follow hello instructions provided tooopen a web page and submit hello token tooAzure:</span></span>

![Logga in tooAzure](./media/batch-cli-get-started/az-login.png)

<span data-ttu-id="f269b-146">hello-exempel som anges i hello [exempel kommandoskript](#sample-shell-scripts) avsnittet också visa hur toostart Azure CLI-session genom att logga in på Azure interaktivt.</span><span class="sxs-lookup"><span data-stu-id="f269b-146">hello examples listed in hello [Sample shell scripts](#sample-shell-scripts) section also show how toostart your Azure CLI session by logging into Azure interactively.</span></span> <span data-ttu-id="f269b-147">När du har loggat in kan du anropa kommandon toowork med Batch Management-resurser, inklusive Batch-konton, nycklar, programpaket och kvoter.</span><span class="sxs-lookup"><span data-stu-id="f269b-147">Once you have logged in, you can call commands toowork with Batch Management resources, including Batch accounts, keys, application packages, and quotas.</span></span>  

### <a name="log-in-tooyour-batch-account"></a><span data-ttu-id="f269b-148">Logga in tooyour Batch-kontot</span><span class="sxs-lookup"><span data-stu-id="f269b-148">Log in tooyour Batch account</span></span>

<span data-ttu-id="f269b-149">toouse hello Azure CLI toomanage Batch resurser, till exempel pooler, jobb och uppgifter, du behöver toolog i Batch-kontot och autentisera.</span><span class="sxs-lookup"><span data-stu-id="f269b-149">toouse hello Azure CLI toomanage Batch resources, such as pools, jobs, and tasks, you need toolog into your Batch account and authenticate.</span></span> <span data-ttu-id="f269b-150">toolog i toohello Batch-tjänsten använder hello [az batch kontoinloggning](https://docs.microsoft.com/cli/azure/batch/account#login) kommando.</span><span class="sxs-lookup"><span data-stu-id="f269b-150">toolog in toohello Batch service, use hello [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login) command.</span></span> 

<span data-ttu-id="f269b-151">Det finns två alternativ för att autentisera mot Batch-kontot:</span><span class="sxs-lookup"><span data-stu-id="f269b-151">You have two options for authenticating against your Batch account:</span></span>

- <span data-ttu-id="f269b-152">**Med autentisering via Azure Active Directory (Azure AD).**</span><span class="sxs-lookup"><span data-stu-id="f269b-152">**By using Azure Active Directory (Azure AD) authentication.**</span></span> 

    <span data-ttu-id="f269b-153">Autentisera med Azure AD är hello standard när du använder hello Azure CLI med Batch och rekommenderas för de flesta scenarier.</span><span class="sxs-lookup"><span data-stu-id="f269b-153">Authenticating with Azure AD is hello default when you use hello Azure CLI with Batch, and recommended for most scenarios.</span></span> 
    
    <span data-ttu-id="f269b-154">När du loggar in tooAzure interaktivt, enligt beskrivningen i föregående avsnitt i hello cachelagras dina autentiseringsuppgifter så hello Azure CLI kan logga in dig tooyour Batch-kontot med dessa samma autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f269b-154">When you log in tooAzure interactively, as described in hello previous section, your credentials are cached, so hello Azure CLI can log you in tooyour Batch account using those same credentials.</span></span> <span data-ttu-id="f269b-155">Om du loggar in med ett huvudnamn för tjänsten tooAzure är också dessa autentiseringsuppgifter används toolog i tooyour Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="f269b-155">If you log in tooAzure using a service principal, those credentials are also used toolog in tooyour Batch account.</span></span>

    <span data-ttu-id="f269b-156">En fördel med Azure AD är att den innehåller rollbaserad åtkomstkontroll (RBAC).</span><span class="sxs-lookup"><span data-stu-id="f269b-156">An advantage of Azure AD is that it offers role-based access control (RBAC).</span></span> <span data-ttu-id="f269b-157">Med RBAC beror en användares behörighet på den tilldelade rollen snarare än om de har hello nycklar.</span><span class="sxs-lookup"><span data-stu-id="f269b-157">With RBAC, a user's access depends on their assigned role, rather than whether or not they possess hello account keys.</span></span> <span data-ttu-id="f269b-158">Du kan hantera RBAC-roller och låta Azure AD hantera åtkomst och autentisering i stället för att hantera nycklar.</span><span class="sxs-lookup"><span data-stu-id="f269b-158">Instead of managing account keys, you can manage RBAC roles, and let Azure AD handle access and authentication.</span></span>  

    <span data-ttu-id="f269b-159">Autentisera med Azure AD krävs om du har skapat ditt Azure Batch-konto med dess poolen allokering läge ange too'User prenumeration '.</span><span class="sxs-lookup"><span data-stu-id="f269b-159">Authenticating with Azure AD is required if you created your Azure Batch account with its pool allocation mode set too'User Subscription'.</span></span> 

    <span data-ttu-id="f269b-160">toolog i tooyour Batch-konto med Azure AD, anropa hello [az batch kontoinloggning](https://docs.microsoft.com/cli/azure/batch/account#login) kommando:</span><span class="sxs-lookup"><span data-stu-id="f269b-160">toolog in tooyour Batch account using Azure AD, call hello [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#login) command:</span></span> 

    ```azurecli
    az batch account login -g myresource group -n mybatchaccount
    ```

- <span data-ttu-id="f269b-161">**Autentisering med delad nyckel.**</span><span class="sxs-lookup"><span data-stu-id="f269b-161">**By using Shared Key authentication.**</span></span>

    <span data-ttu-id="f269b-162">[Autentisering med delad nyckel](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) använder ditt konto åtkomstnycklar tooauthenticate Azure CLI-kommandona för hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f269b-162">[Shared Key authentication](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service#authentication-via-shared-key) uses your account access keys tooauthenticate Azure CLI commands for hello Batch service.</span></span>

    <span data-ttu-id="f269b-163">Om du skapar Azure CLI skript tooautomate anropa Batch-kommandon, kan du använda autentisering med delad nyckel eller en Azure AD-tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="f269b-163">If you are creating Azure CLI scripts tooautomate calling Batch commands, you can use either Shared Key authentication, or an Azure AD service principal.</span></span> <span data-ttu-id="f269b-164">I vissa situationer kan det vara enklare att använda autentisering med delad nyckel än att skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f269b-164">In some scenarios, using Shared Key authentication may be simpler than creating a service principal.</span></span>  

    <span data-ttu-id="f269b-165">toolog i med hjälp av autentisering med delad nyckel, inkludera hello `--shared-key-auth` alternativ på kommandoraden för hello:</span><span class="sxs-lookup"><span data-stu-id="f269b-165">toolog in using Shared Key authentication, include hello `--shared-key-auth` option on hello command line:</span></span>

    ```azurecli
    az batch account login -g myresourcegroup -n mybatchaccount --shared-key-auth
    ```

<span data-ttu-id="f269b-166">hello-exempel som anges i hello [exempel kommandoskript](#sample-shell-scripts) avsnittet visar hur toolog i Batch-kontot med hello Azure CLI med Azure AD och delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="f269b-166">hello examples listed in hello [Sample shell scripts](#sample-shell-scripts) section show how toolog into your Batch account with hello Azure CLI using both Azure AD and Shared Key.</span></span>

## <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a><span data-ttu-id="f269b-167">Använda Azure Batch CLI-mallar och filöverföring (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="f269b-167">Use Azure Batch CLI Templates and File Transfer (Preview)</span></span>

<span data-ttu-id="f269b-168">Du kan använda hello Azure CLI toorun Batch jobb slutpunkt till slutpunkt utan att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="f269b-168">You can use hello Azure CLI toorun Batch jobs end-to-end without writing code.</span></span> <span data-ttu-id="f269b-169">Mallen kommandofiler stöd för att skapa pooler, jobb och aktiviteter med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f269b-169">Batch template files support creating pools, jobs, and tasks with hello Azure CLI.</span></span> <span data-ttu-id="f269b-170">Du kan också använda hello Azure CLI tooupload jobbet indatafiler toohello Azure Storage-konto som är associerade med hello Batch-kontot och hämta jobb utgående filer från den.</span><span class="sxs-lookup"><span data-stu-id="f269b-170">You can also use hello Azure CLI tooupload job input files toohello Azure Storage account associated with hello Batch account, and download job output files from it.</span></span> <span data-ttu-id="f269b-171">Mer information finns i [Använda Azure Batch CLI-mallar och filöverföring (förhandsversion)](batch-cli-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f269b-171">For more information, see [Use Azure Batch CLI Templates and File Transfer (Preview)](batch-cli-templates.md).</span></span>

## <a name="sample-shell-scripts"></a><span data-ttu-id="f269b-172">Exempel på kommandoskript</span><span class="sxs-lookup"><span data-stu-id="f269b-172">Sample shell scripts</span></span>

<span data-ttu-id="f269b-173">hello exempelskript som anges i följande tabell visar hello hur toouse Azure CLI-kommandon med hello Batch-tjänsten och Batch-tjänsten tooaccomplish vanliga hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f269b-173">hello sample scripts listed in hello following table show how toouse Azure CLI commands with hello Batch service and Batch Management service tooaccomplish common tasks.</span></span> <span data-ttu-id="f269b-174">Dessa exempel på skript omfattar många av hello kommandon i hello Azure CLI för Batch.</span><span class="sxs-lookup"><span data-stu-id="f269b-174">These sample scripts cover many of hello commands available in hello Azure CLI for Batch.</span></span> 

| <span data-ttu-id="f269b-175">Skript</span><span class="sxs-lookup"><span data-stu-id="f269b-175">Script</span></span> | <span data-ttu-id="f269b-176">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="f269b-176">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f269b-177">Skapa ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="f269b-177">Create a Batch account</span></span>](./scripts/batch-cli-sample-create-account.md) | <span data-ttu-id="f269b-178">Skapar ett Batch-konto och kopplar det till ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f269b-178">Creates a Batch account and associates it with a storage account.</span></span> |
| [<span data-ttu-id="f269b-179">Lägga till ett program</span><span class="sxs-lookup"><span data-stu-id="f269b-179">Add an application</span></span>](./scripts/batch-cli-sample-add-application.md) | <span data-ttu-id="f269b-180">Lägger till ett program och laddar upp paketerade binärfiler.</span><span class="sxs-lookup"><span data-stu-id="f269b-180">Adds an application and uploads packaged binaries.</span></span>|
| [<span data-ttu-id="f269b-181">Hantera Batch-pooler</span><span class="sxs-lookup"><span data-stu-id="f269b-181">Manage Batch pools</span></span>](./scripts/batch-cli-sample-manage-pool.md) | <span data-ttu-id="f269b-182">Visar hur du skapar, ändrar storlek på och hanterar pooler.</span><span class="sxs-lookup"><span data-stu-id="f269b-182">Demonstrates creating, resizing, and managing pools.</span></span> |
| [<span data-ttu-id="f269b-183">Köra ett jobb och aktiviteter med Batch</span><span class="sxs-lookup"><span data-stu-id="f269b-183">Run a job and tasks with Batch</span></span>](./scripts/batch-cli-sample-run-job.md) | <span data-ttu-id="f269b-184">Visar hur du kör ett jobb och lägger till aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f269b-184">Demonstrates running a job and adding tasks.</span></span> |

## <a name="json-files-for-resource-creation"></a><span data-ttu-id="f269b-185">JSON-filer för resursskapande</span><span class="sxs-lookup"><span data-stu-id="f269b-185">JSON files for resource creation</span></span>

<span data-ttu-id="f269b-186">När du skapar Batch-resurser som pooler och jobb kan ange du en JSON-fil som innehåller hello ny resurs konfiguration i stället för att ange dess parametrar som kommandoradsalternativ.</span><span class="sxs-lookup"><span data-stu-id="f269b-186">When you create Batch resources like pools and jobs, you can specify a JSON file containing hello new resource's configuration instead of passing its parameters as command-line options.</span></span> <span data-ttu-id="f269b-187">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f269b-187">For example:</span></span>

```azurecli
az batch pool create my_batch_pool.json
```

<span data-ttu-id="f269b-188">Du kan skapa de flesta Batch-resurser med hjälp av endast kommandoradsalternativ, vissa funktioner som kräver att du anger en JSON-formaterad fil som innehåller information om hello resursen.</span><span class="sxs-lookup"><span data-stu-id="f269b-188">While you can create most Batch resources using only command-line options, some features require that you specify a JSON-formatted file containing hello resource details.</span></span> <span data-ttu-id="f269b-189">Du måste till exempel använda en JSON-fil om du vill toospecify resursfiler för en start-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f269b-189">For example, you must use a JSON file if you want toospecify resource files for a start task.</span></span>

<span data-ttu-id="f269b-190">toosee hello JSON syntax krävs toocreate en resurs finns toohello [Batch REST API-referensen] [ rest_api] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f269b-190">toosee hello JSON syntax required toocreate a resource, refer toohello [Batch REST API reference][rest_api] documentation.</span></span> <span data-ttu-id="f269b-191">Varje ”Lägg till *resurstypen*” hello REST API-referensen innehåller exempel på JSON-skript för att skapa den här resursen.</span><span class="sxs-lookup"><span data-stu-id="f269b-191">Each "Add *resource type*" topic in hello REST API reference contains sample JSON scripts for creating that resource.</span></span> <span data-ttu-id="f269b-192">Du kan använda dessa exempel JSON-skript som en mall för JSON-filer toouse med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f269b-192">You can use those sample JSON scripts as templates for JSON files toouse with hello Azure CLI.</span></span> <span data-ttu-id="f269b-193">Till exempel toosee hello JSON-syntax för att skapa en pool, referera för[Lägg till ett poolen tooan][rest_add_pool].</span><span class="sxs-lookup"><span data-stu-id="f269b-193">For example, toosee hello JSON syntax for pool creation, refer too[Add a pool tooan account][rest_add_pool].</span></span>

<span data-ttu-id="f269b-194">Ett exempelskript som anger en JSON-fil finns i [Köra ett jobb och aktiviteter med Batch](./scripts/batch-cli-sample-run-job.md).</span><span class="sxs-lookup"><span data-stu-id="f269b-194">For a sample script that specifies a JSON file, see [Run a job and tasks with Batch](./scripts/batch-cli-sample-run-job.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f269b-195">Om du anger en JSON-fil när du skapar en resurs, ignoreras andra parametrar som du anger på hello kommandoraden för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="f269b-195">If you specify a JSON file when you create a resource, any other parameters that you specify on hello command line for that resource are ignored.</span></span>
> 
> 

## <a name="efficient-queries-for-batch-resources"></a><span data-ttu-id="f269b-196">Effektiva frågor för Batch-resurser</span><span class="sxs-lookup"><span data-stu-id="f269b-196">Efficient queries for Batch resources</span></span>

<span data-ttu-id="f269b-197">Varje Batch-resurstyp stöder ett `list`-kommando som frågar Batch-kontot och visar en lista över resurser av den typen.</span><span class="sxs-lookup"><span data-stu-id="f269b-197">Each Batch resource type supports a `list` command that queries your Batch account and lists resources of that type.</span></span> <span data-ttu-id="f269b-198">Du kan till exempel ange hello pooler i ditt konto och hello aktiviteter i ett projekt:</span><span class="sxs-lookup"><span data-stu-id="f269b-198">For example, you can list hello pools in your account and hello tasks in a job:</span></span>

```azurecli
az batch pool list
az batch task list --job-id job001
```

<span data-ttu-id="f269b-199">När du frågar hello Batch-tjänsten med en `list` åtgärd, som du kan ange en OData-satsen toolimit hello mängden data som returneras.</span><span class="sxs-lookup"><span data-stu-id="f269b-199">When you query hello Batch service with a `list` operation, you can specify an OData clause toolimit hello amount of data returned.</span></span> <span data-ttu-id="f269b-200">Eftersom alla filtrering sker serversidan, korsar endast hello data som du begär hello-överföring.</span><span class="sxs-lookup"><span data-stu-id="f269b-200">Because all filtering occurs server-side, only hello data you request crosses hello wire.</span></span> <span data-ttu-id="f269b-201">Använd dessa satser toosave bandbredd (och därför tid) när du utför Liståtgärder.</span><span class="sxs-lookup"><span data-stu-id="f269b-201">Use these clauses toosave bandwidth (and therefore time) when you perform list operations.</span></span>

<span data-ttu-id="f269b-202">hello beskrivs följande tabell hello OData-satser stöds av hello Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="f269b-202">hello following table describes hello OData clauses supported by hello Batch service:</span></span>

| <span data-ttu-id="f269b-203">Sats</span><span class="sxs-lookup"><span data-stu-id="f269b-203">Clause</span></span> | <span data-ttu-id="f269b-204">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f269b-204">Description</span></span> |
|---|---|
| `--select-clause [select-clause]` | <span data-ttu-id="f269b-205">Returnerar en delmängd av egenskaperna för varje entitet.</span><span class="sxs-lookup"><span data-stu-id="f269b-205">Returns a subset of properties for each entity.</span></span> |
| `--filter-clause [filter-clause]` | <span data-ttu-id="f269b-206">Returnerar endast enheter som matchar hello angivna OData-uttrycket.</span><span class="sxs-lookup"><span data-stu-id="f269b-206">Returns only entities that match hello specified OData expression.</span></span> |
| `--expand-clause [expand-clause]` | <span data-ttu-id="f269b-207">Hämtar hello entitet informationen i ett enda underliggande REST-anrop.</span><span class="sxs-lookup"><span data-stu-id="f269b-207">Obtains hello entity information in a single underlying REST call.</span></span> <span data-ttu-id="f269b-208">hello Expandera satsen för närvarande stöder endast hello `stats` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="f269b-208">hello expand clause currently supports only hello `stats` property.</span></span> |

<span data-ttu-id="f269b-209">För ett exempel på skript som visar hur toouse en OData-sats, se [köra ett jobb och aktiviteter med Batch](./scripts/batch-cli-sample-run-job.md).</span><span class="sxs-lookup"><span data-stu-id="f269b-209">For a sample script that shows how toouse an OData clause, see [Run a job and tasks with Batch](./scripts/batch-cli-sample-run-job.md).</span></span>

<span data-ttu-id="f269b-210">Mer information om hur du utför effektiv listan frågor med OData-satser finns [fråga hello Azure Batch-tjänsten effektivt](batch-efficient-list-queries.md).</span><span class="sxs-lookup"><span data-stu-id="f269b-210">For more information on performing efficient list queries with OData clauses, see [Query hello Azure Batch service efficiently](batch-efficient-list-queries.md).</span></span>

## <a name="troubleshooting-tips"></a><span data-ttu-id="f269b-211">Felsökningstips</span><span class="sxs-lookup"><span data-stu-id="f269b-211">Troubleshooting tips</span></span>

<span data-ttu-id="f269b-212">hello följande tips kan hjälpa dig när du felsöker problem med Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="f269b-212">hello following tips may help when you are troubleshooting Azure CLI issues:</span></span>

* <span data-ttu-id="f269b-213">Använd `-h` tooget **hjälptext** för alla CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="f269b-213">Use `-h` tooget **help text** for any CLI command</span></span>
* <span data-ttu-id="f269b-214">Använd `-v` och `-vv` toodisplay **utförlig** kommandot utdata.</span><span class="sxs-lookup"><span data-stu-id="f269b-214">Use `-v` and `-vv` toodisplay **verbose** command output.</span></span> <span data-ttu-id="f269b-215">När hello `-vv` flaggan ingår, hello Azure CLI visar hello faktiska REST-begäranden och -svar.</span><span class="sxs-lookup"><span data-stu-id="f269b-215">When hello `-vv` flag is included, hello Azure CLI displays hello actual REST requests and responses.</span></span> <span data-ttu-id="f269b-216">Växlarna är användbara för att visa fullständiga utdata vid fel.</span><span class="sxs-lookup"><span data-stu-id="f269b-216">These switches are handy for displaying full error output.</span></span>
* <span data-ttu-id="f269b-217">Du kan visa **kommandot utdata som JSON** med hello `--json` alternativet.</span><span class="sxs-lookup"><span data-stu-id="f269b-217">You can view **command output as JSON** with hello `--json` option.</span></span> <span data-ttu-id="f269b-218">Till exempel visar `az batch pool show pool001 --json` pool001:s egenskaper i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="f269b-218">For example, `az batch pool show pool001 --json` displays pool001's properties in JSON format.</span></span> <span data-ttu-id="f269b-219">Du kan sedan kopiera och ändra den här utdata toouse i en `--json-file` (se [JSON-filer](#json-files) tidigare i den här artikeln).</span><span class="sxs-lookup"><span data-stu-id="f269b-219">You can then copy and modify this output toouse in a `--json-file` (see [JSON files](#json-files) earlier in this article).</span></span>
<!---Loc Comment: Please, check link [JSON files] since it's not redirecting tooany location.--->
* <span data-ttu-id="f269b-220">Hej [Batch-forum] [ batch_forum] övervakas av Batch-gruppmedlemmar.</span><span class="sxs-lookup"><span data-stu-id="f269b-220">hello [Batch forum][batch_forum] is monitored by Batch team members.</span></span> <span data-ttu-id="f269b-221">Du kan ställa frågor där om du får problem eller behöver hjälp med en viss åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f269b-221">You can post your questions there if you run into issues or would like help with a specific operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f269b-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f269b-222">Next steps</span></span>

* <span data-ttu-id="f269b-223">Mer information om hello Azure CLI finns hello [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f269b-223">For more information about hello Azure CLI, see hello [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>
* <span data-ttu-id="f269b-224">Mer information om Batch-resurser finns i [Utveckla storskaliga parallella beräkningslösningar med Batch](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="f269b-224">For more information about Batch resources, see [Overview of Azure Batch for developers](batch-api-basics.md).</span></span>
* <span data-ttu-id="f269b-225">Mer information om hur du använder Batch mallar toocreate pooler, jobb och uppgifter utan att skriva kod finns [Azure Batch CLI mallar och filöverföring (förhandsgranskning)](batch-cli-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f269b-225">For more information about using Batch templates toocreate pools, jobs, and tasks without writing code, see [Use Azure Batch CLI Templates and File Transfer (Preview)](batch-cli-templates.md).</span></span>

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
