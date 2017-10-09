---
title: "aaaLog i tooAzure från hello CLI | Microsoft Docs"
description: "Ansluta tooyour Azure-prenumeration från hello Azure-kommandoradsgränssnittet (Azure CLI) för Mac, Linux och Windows"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 42682c00c8dea78b2c624e640379716d1d4d7a2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-in-tooazure-from-hello-azure-cli"></a><span data-ttu-id="e9448-103">Logga in tooAzure från hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e9448-103">Log in tooAzure from hello Azure CLI</span></span>
<span data-ttu-id="e9448-104">hello Azure CLI är öppen källkod, plattformsoberoende kommandon för att arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e9448-104">hello Azure CLI is a set of open-source, cross-platform commands for working with Azure resources.</span></span> <span data-ttu-id="e9448-105">Den här artikeln beskriver hello olika sätt tooprovide din Azure-konto autentiseringsuppgifter tooconnect hello Azure CLI tooyour Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="e9448-105">This article describes hello different ways tooprovide your Azure account credentials tooconnect hello Azure CLI tooyour Azure subscription:</span></span>

* <span data-ttu-id="e9448-106">Kör hello `azure login` CLI kommandot tooauthenticate via Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e9448-106">Run hello `azure login` CLI command tooauthenticate through Azure Active Directory.</span></span> <span data-ttu-id="e9448-107">Den här metoden ger du åtkomst till tooCLI kommandon i både [kommandot lägen](#cli-command-modes).</span><span class="sxs-lookup"><span data-stu-id="e9448-107">This method gives you access tooCLI commands in both [command modes](#cli-command-modes).</span></span> <span data-ttu-id="e9448-108">När du kör kommandot hello utan ytterligare alternativ `azure login` efterfrågar toocontinue logga in interaktivt i en webbportal.</span><span class="sxs-lookup"><span data-stu-id="e9448-108">When you run hello command without additional options, `azure login` prompts you toocontinue logging in interactively through a web portal.</span></span> <span data-ttu-id="e9448-109">För ytterligare `azure login` kommandot Alternativ, se hello scenarier i den här artikeln eller Skriv `azure login --help`.</span><span class="sxs-lookup"><span data-stu-id="e9448-109">For additional `azure login` command options, see hello scenarios in this article, or type `azure login --help`.</span></span>
* <span data-ttu-id="e9448-110">Om du bara behöver toouse Azure Service Management läge CLI-kommandona (rekommenderas inte för de flesta nya distributioner) kan du hämta och installera en publicera inställningsfil på datorn.</span><span class="sxs-lookup"><span data-stu-id="e9448-110">If you only need toouse Azure Service Management mode CLI commands (not recommended for most new deployments), you can download and install a publish settings file on your computer.</span></span>

<span data-ttu-id="e9448-111">Om du inte redan har installerat hello CLI, se [installera hello Azure CLI](cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="e9448-111">If you haven't already installed hello CLI, see [Install hello Azure CLI](cli-install-nodejs.md).</span></span> <span data-ttu-id="e9448-112">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](http://azure.microsoft.com/free/) på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="e9448-112">If you don't have an Azure subscription, you can create a [free account](http://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="e9448-113">Bakgrundsinformation om annat konto identiteter och Azure-prenumerationer finns [hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="e9448-113">For background about different account identities and Azure subscriptions, see [How Azure subscriptions are associated with Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).</span></span>

## <a name="scenario-1-azure-login-with-interactive-login"></a><span data-ttu-id="e9448-114">Scenario 1: azure-inloggning med interaktiv inloggning</span><span class="sxs-lookup"><span data-stu-id="e9448-114">Scenario 1: azure login with interactive login</span></span>
<span data-ttu-id="e9448-115">Med vissa konton hello CLI kräver att du toorun `azure login` och fortsätt sedan hello inloggningen med en webbläsare via en webbportal, en process som kallas *interaktiv inloggning*.</span><span class="sxs-lookup"><span data-stu-id="e9448-115">With certain accounts, hello CLI requires you toorun `azure login` and then continue hello login process with a web browser through a web portal, a process called *interactive login*.</span></span> <span data-ttu-id="e9448-116">En vanlig orsak är när du har ett arbets- eller skolkonto konto (kallas även en *organisationskonto*) som har ställts in toorequire flerfunktionsautentisering.</span><span class="sxs-lookup"><span data-stu-id="e9448-116">A common reason is when you have a work or school account (also called an *organizational account*) that is set up toorequire multifactor authentication.</span></span> <span data-ttu-id="e9448-117">Använd också interaktiv inloggning med ditt Microsoft-konto när du vill toouse kommandon för Resource Manager-läge.</span><span class="sxs-lookup"><span data-stu-id="e9448-117">Also use interactive login with your Microsoft account, when you want toouse Resource Manager mode commands.</span></span>

<span data-ttu-id="e9448-118">Interaktiv inloggning är enkel: typen `azure login` --utan alternativ--som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e9448-118">Interactive login is easy: type `azure login` -- without any options -- as shown in hello following example:</span></span>

```
azure login
```                                                                                             

<span data-ttu-id="e9448-119">hello utdata visas ungefär hello följande:</span><span class="sxs-lookup"><span data-stu-id="e9448-119">hello output appears something like hello following:</span></span>

```         
info:    Executing command login
info:    toosign in, use a web browser tooopen hello page http://aka.ms/devicelogin. Enter hello code XXXXXXXXX tooauthenticate.
```
<span data-ttu-id="e9448-120">Kopiera hello kod erbjuds tooyou i hello kommandoutdata och öppna en webbläsare toohttp://aka.ms/devicelogin eller andra sidan om anges.</span><span class="sxs-lookup"><span data-stu-id="e9448-120">Copy hello code offered tooyou in hello command output, and open a browser toohttp://aka.ms/devicelogin, or other page if specified.</span></span> <span data-ttu-id="e9448-121">(Du kan öppna en webbläsare på hello samma dator eller på en annan dator eller enhet.) Ange hello kod och är du tillfrågas tooenter hello användarnamn och lösenord för hello identitet du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="e9448-121">(You can open a browser on hello same computer, or on a different computer or device.) Enter hello code, and then you are prompted tooenter hello username and password for hello identity you want toouse.</span></span> <span data-ttu-id="e9448-122">När den här processen är klar slutför hello-kommandogränssnittet hello inloggningen.</span><span class="sxs-lookup"><span data-stu-id="e9448-122">When that process completes, hello command shell completes hello login.</span></span> <span data-ttu-id="e9448-123">Det kan se ut ungefär så:</span><span class="sxs-lookup"><span data-stu-id="e9448-123">It might look something like:</span></span>

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> <span data-ttu-id="e9448-124">Med interaktiv inloggning utförs autentisering och auktorisering med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e9448-124">With interactive login, authentication and authorization are performed using Azure Active Directory.</span></span> <span data-ttu-id="e9448-125">Om du använder en Microsoft-kontoidentitet hello inloggningen har åtkomst till din Azure Active Directory-standarddomän.</span><span class="sxs-lookup"><span data-stu-id="e9448-125">If you use a Microsoft account identity, hello login process accesses your Azure Active Directory default domain.</span></span> <span data-ttu-id="e9448-126">(Om du har registrerat dig för ett kostnadsfritt Azure-konto, Azure Active Directory skapas automatiskt en standarddomän för ditt konto.)</span><span class="sxs-lookup"><span data-stu-id="e9448-126">(If you signed up for a free Azure account, Azure Active Directory automatically created a default domain for your account.)</span></span>
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a><span data-ttu-id="e9448-127">Scenario 2: azure logga in med ett användarnamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="e9448-127">Scenario 2: azure login with a username and password</span></span>
<span data-ttu-id="e9448-128">Använd hello `azure login` med hello användarnamn (`-u`) parametern tooauthenticate när du vill toouse ett arbets- eller Skol-konto som inte kräver flerfunktionsautentisering.</span><span class="sxs-lookup"><span data-stu-id="e9448-128">Use hello `azure login` command with hello username (`-u`) parameter tooauthenticate when you want toouse a work or school account that doesn't require multifactor authentication.</span></span> <span data-ttu-id="e9448-129">Du uppmanas kommandoraden hello hello lösenord (eller alternativt kan du skicka hello lösenord som en ytterligare hello-parameter `azure login` kommandot).</span><span class="sxs-lookup"><span data-stu-id="e9448-129">You are prompted at hello command line for hello password (or you can optionally pass hello password as an additional parameter of hello `azure login` command).</span></span> <span data-ttu-id="e9448-130">hello skickas följande exempel hello användarnamnet för ett organisationskonto:</span><span class="sxs-lookup"><span data-stu-id="e9448-130">hello following example passes hello username of an organizational account:</span></span>

    azure login -u myUserName@contoso.onmicrosoft.com

<span data-ttu-id="e9448-131">Du kan sedan uppmanas tooenter ditt lösenord:</span><span class="sxs-lookup"><span data-stu-id="e9448-131">You are then prompted tooenter your password:</span></span>

    info:    Executing command login
    Password: *********

<span data-ttu-id="e9448-132">hello inloggningen sedan är klar.</span><span class="sxs-lookup"><span data-stu-id="e9448-132">hello login process then completes.</span></span>

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

<span data-ttu-id="e9448-133">Om det här är din första gången logga in med autentiseringsuppgifterna du uppmanas tooverify du vill toocache en autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="e9448-133">If this is your first time logging in with these credentials, you are asked tooverify that you wish toocache an authentication token.</span></span> <span data-ttu-id="e9448-134">Det här meddelandet kan också uppstå om du tidigare använt hello `azure logout` kommando (beskrivs senare i artikeln hello).</span><span class="sxs-lookup"><span data-stu-id="e9448-134">This prompt also occurs if you previously used hello `azure logout` command (described later in hello article).</span></span> <span data-ttu-id="e9448-135">toobypass varningen för automatiseringsscenarier kör `azure login` med hello `-q` parameter.</span><span class="sxs-lookup"><span data-stu-id="e9448-135">toobypass this prompt for automation scenarios, run `azure login` with hello `-q` parameter.</span></span>

## <a name="scenario-3-azure-login-with-a-service-principal"></a><span data-ttu-id="e9448-136">Scenario 3: azure logga in med ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="e9448-136">Scenario 3: azure login with a service principal</span></span>
<span data-ttu-id="e9448-137">Om du skapar ett huvudnamn för tjänsten för ett Active Directory-program och hello tjänstens huvudnamn har behörighet för din prenumeration, kan du använda hello `azure login` kommandot tooauthenticate hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="e9448-137">If you create a service principal for an Active Directory application, and hello service principal has permissions on your subscription, you can use hello `azure login` command tooauthenticate hello service principal.</span></span> <span data-ttu-id="e9448-138">Beroende på ditt scenario kan du ange autentiseringsuppgifter för hello av hello tjänstens huvudnamn som explicit parametrar av hello `azure login` kommando.</span><span class="sxs-lookup"><span data-stu-id="e9448-138">Depending on your scenario, you could provide hello credentials of hello service principal as explicit parameters of hello `azure login` command.</span></span> <span data-ttu-id="e9448-139">Till exempel skickar hello följande kommando hello tjänstens huvudnamn och Active Directory klient-ID:</span><span class="sxs-lookup"><span data-stu-id="e9448-139">For example, hello following command passes hello service principal name and Active Directory tenant ID:</span></span>

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

<span data-ttu-id="e9448-140">Du kan sedan ange tooprovide hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="e9448-140">You are then prompted tooprovide hello password.</span></span> <span data-ttu-id="e9448-141">Du kan också ange hello autentiseringsuppgifter via en CLI-kod i skript eller ett program, eller använda ett certifikat tooauthenticate hello tjänstens huvudnamn icke-interaktivt för automatiseringsscenarier.</span><span class="sxs-lookup"><span data-stu-id="e9448-141">You can also provide hello credentials through a CLI script or application code, or use a certificate tooauthenticate hello service principal non-interactively for automation scenarios.</span></span> <span data-ttu-id="e9448-142">Mer information och exempel finns [autentiserar ett huvudnamn för tjänsten med Azure Resource Manager](resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e9448-142">For details and examples, see [Authenticating a service principal with Azure Resource Manager](resource-group-authenticate-service-principal-cli.md).</span></span>

## <a name="scenario-4-use-a-publish-settings-file"></a><span data-ttu-id="e9448-143">Scenario 4: Använda en publicera inställningsfil</span><span class="sxs-lookup"><span data-stu-id="e9448-143">Scenario 4: Use a publish settings file</span></span>
<span data-ttu-id="e9448-144">Om du bara behöver toouse hello Azure Service Management läge CLI-kommandona (till exempel toodeploy virtuella Azure-datorer i hello klassiska distributionsmodellen) kan du ansluta med en fil med inställningar i Publicera.</span><span class="sxs-lookup"><span data-stu-id="e9448-144">If you only need toouse hello Azure Service Management mode CLI commands (for example, toodeploy Azure VMs in hello classic deployment model), you can connect using a publish settings file.</span></span> <span data-ttu-id="e9448-145">Den här metoden installerar ett certifikat på den lokala datorn som du kan använda tooperform hanteringsaktiviteter för så länge hello prenumeration och hello certifikat är giltiga.</span><span class="sxs-lookup"><span data-stu-id="e9448-145">This method installs a certificate on your local computer that allows you tooperform management tasks for as long as hello subscription and hello certificate are valid.</span></span>

* <span data-ttu-id="e9448-146">**inställningsfilen för publicering av toodownload hello** för ditt konto, se till att hello CLI är i Service Management-läge genom att skriva `azure config mode asm`.</span><span class="sxs-lookup"><span data-stu-id="e9448-146">**toodownload hello publish settings file** for your account, ensure that hello CLI is in Service Management mode by typing `azure config mode asm`.</span></span> <span data-ttu-id="e9448-147">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="e9448-147">Then run hello following command:</span></span>

        azure account download

<span data-ttu-id="e9448-148">Detta öppnar din standardwebbläsare och du uppmanas toosign i toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="e9448-148">This opens your default browser and prompts you toosign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="e9448-149">När du loggar in, en `.publishsettings` filhämtningar.</span><span class="sxs-lookup"><span data-stu-id="e9448-149">After you sign in, a `.publishsettings` file downloads.</span></span> <span data-ttu-id="e9448-150">Notera var filen sparas.</span><span class="sxs-lookup"><span data-stu-id="e9448-150">Make note of where this file is saved.</span></span>

> [!NOTE]
> <span data-ttu-id="e9448-151">Om ditt konto är kopplat till flera Azure Active Directory-klienter måste du ange tooselect som Active Directory som du vill toodownload en publiceringsinställningarna filen för.</span><span class="sxs-lookup"><span data-stu-id="e9448-151">If your account is associated with multiple Azure Active Directory tenants, you may be prompted tooselect which Active Directory you wish toodownload a publish settings file for.</span></span>
>
>

<span data-ttu-id="e9448-152">När valts med hjälp av hello hämtningssidan, eller genom att besöka hello klassiska Azure-portalen, blir hello valda Active Directory hello-standard som används av hello klassiska portalen och hämtningssidan.</span><span class="sxs-lookup"><span data-stu-id="e9448-152">Once selected using hello download page, or by visiting hello Azure classic portal, hello selected Active Directory becomes hello default used by hello classic portal and download page.</span></span> <span data-ttu-id="e9448-153">När ett standardvärde har upprättats kan du se hello text '**Klicka här tooreturn toohello sidan**' hello överst i hello hämtningssidan.</span><span class="sxs-lookup"><span data-stu-id="e9448-153">Once a default has been established, you see hello text '**click here tooreturn toohello selection page**' at hello top of hello download page.</span></span> <span data-ttu-id="e9448-154">Använd hello som länk tooreturn toohello på sidan.</span><span class="sxs-lookup"><span data-stu-id="e9448-154">Use hello provided link tooreturn toohello selection page.</span></span>

* <span data-ttu-id="e9448-155">**inställningsfilen för publicering av tooimport hello**kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e9448-155">**tooimport hello publish settings file**, run hello following command:</span></span>

        azure account import <path tooyour .publishsettings file>

> [!IMPORTANT]
> <span data-ttu-id="e9448-156">När du har importerat dina Publiceringsinställningar, bör du ta bort hello `.publishsettings` fil.</span><span class="sxs-lookup"><span data-stu-id="e9448-156">After importing your publish settings, you should delete hello `.publishsettings` file.</span></span> <span data-ttu-id="e9448-157">Det krävs inte längre av hello Azure CLI och utgör en säkerhetsrisk eftersom det skulle kunna använda toogain åtkomst tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e9448-157">It is no longer required by hello Azure CLI and presents a security risk as it could be used toogain access tooyour subscription.</span></span>
>
>

## <a name="cli-command-modes"></a><span data-ttu-id="e9448-158">CLI kommandolägen</span><span class="sxs-lookup"><span data-stu-id="e9448-158">CLI command modes</span></span>
<span data-ttu-id="e9448-159">hello Azure CLI har två kommandolägen för att arbeta med Azure-resurser med olika uppsättningar:</span><span class="sxs-lookup"><span data-stu-id="e9448-159">hello Azure CLI provides two command modes for working with Azure resources, with different command sets:</span></span>

* <span data-ttu-id="e9448-160">**Resource Manager-läget** – för att arbeta med Azure-resurser i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e9448-160">**Resource Manager mode** - for working with Azure resources in hello Resource Manager deployment model.</span></span> <span data-ttu-id="e9448-161">tooset det här läget kör `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="e9448-161">tooset this mode, run `azure config mode arm`.</span></span>
* <span data-ttu-id="e9448-162">**Service Management-läge** – för att arbeta med Azure-resurser i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e9448-162">**Service Management mode** - for working with Azure resources in hello classic deployment model.</span></span> <span data-ttu-id="e9448-163">tooset det här läget kör `azure config mode asm`.</span><span class="sxs-lookup"><span data-stu-id="e9448-163">tooset this mode, run `azure config mode asm`.</span></span>

<span data-ttu-id="e9448-164">När installeras är hello aktuella versionen av hello CLI finns i Resource Manager-läget.</span><span class="sxs-lookup"><span data-stu-id="e9448-164">When first installed, hello current release of hello CLI is in Resource Manager mode.</span></span>

> [!NOTE]
> <span data-ttu-id="e9448-165">hello Resource Manager och Service Management-läge kan inte anges samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e9448-165">hello Resource Manager mode and Service Management mode are mutually exclusive.</span></span> <span data-ttu-id="e9448-166">Det vill säga resurser som skapats i ett läge kan inte hanteras från hello andra läge.</span><span class="sxs-lookup"><span data-stu-id="e9448-166">That is, resources created in one mode cannot be managed from hello other mode.</span></span>
>
>

## <a name="multiple-subscriptions"></a><span data-ttu-id="e9448-167">Flera prenumerationer</span><span class="sxs-lookup"><span data-stu-id="e9448-167">Multiple subscriptions</span></span>
<span data-ttu-id="e9448-168">Om du har flera Azure-prenumerationer, beviljar ansluta tooAzure åtkomst tooall prenumerationer som är kopplade till dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e9448-168">If you have multiple Azure subscriptions, connecting tooAzure grants access tooall subscriptions associated with your credentials.</span></span> <span data-ttu-id="e9448-169">En prenumeration är valt som standard hello och används av hello Azure CLI när du utför åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e9448-169">One subscription is selected as hello default, and used by hello Azure CLI when performing operations.</span></span> <span data-ttu-id="e9448-170">Du kan visa hello prenumerationer, inklusive hello aktuell standard prenumeration, med hello `azure account list` kommando.</span><span class="sxs-lookup"><span data-stu-id="e9448-170">You can view hello subscriptions, including hello current default subscription, using hello `azure account list` command.</span></span> <span data-ttu-id="e9448-171">Det här kommandot returnerar informationen liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="e9448-171">This command returns information similar toohello following:</span></span>

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

<span data-ttu-id="e9448-172">I föregående lista hello, hello **aktuella** visar hello aktuell standard prenumeration som Azure-sub-1.</span><span class="sxs-lookup"><span data-stu-id="e9448-172">In hello preceding list, hello **Current** column indicates hello current default subscription as Azure-sub-1.</span></span> <span data-ttu-id="e9448-173">toochange hello standard prenumeration, Använd hello `azure account set` kommando och ange hello-prenumeration som du vill toobe hello standard.</span><span class="sxs-lookup"><span data-stu-id="e9448-173">toochange hello default subscription, use hello `azure account set` command, and specify hello subscription that you wish toobe hello default.</span></span> <span data-ttu-id="e9448-174">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e9448-174">For example:</span></span>

    azure account set Azure-sub-2

<span data-ttu-id="e9448-175">Detta ändrar hello standard prenumeration tooAzure-sub-2.</span><span class="sxs-lookup"><span data-stu-id="e9448-175">This changes hello default subscription tooAzure-sub-2.</span></span>

> [!NOTE]
> <span data-ttu-id="e9448-176">Ändra hello standardabonnemang träder i kraft omedelbart och är en global ändring; nya Azure CLI-kommandona om du kör dem från hello samma kommandoradsverktyget instans eller en annan instans använder hello standard prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e9448-176">Changing hello default subscription takes effect immediately, and is a global change; new Azure CLI commands, whether you run them from hello same command-line instance or a different instance, use hello new default subscription.</span></span>
>
>

<span data-ttu-id="e9448-177">Om du vill toouse en icke-standard-prenumeration med hello Azure CLI, men inte vill toochange hello aktuell standard, kan du använda hello `--subscription` alternativet för hello kommando och ange hello namn för hello prenumeration gärna toouse för hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e9448-177">If you wish toouse a non-default subscription with hello Azure CLI, but don't want toochange hello current default, you can use hello `--subscription` option for hello command and provide hello name of hello subscription you wish toouse for hello operation.</span></span>

<span data-ttu-id="e9448-178">När du är ansluten tooyour Azure-prenumeration kan börja du använda hello Azure CLI-kommandon toowork med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e9448-178">Once you are connected tooyour Azure subscription, you can start using hello Azure CLI commands toowork with Azure resources.</span></span>

## <a name="storage-of-cli-settings"></a><span data-ttu-id="e9448-179">Lagring av CLI-inställningar</span><span class="sxs-lookup"><span data-stu-id="e9448-179">Storage of CLI settings</span></span>
<span data-ttu-id="e9448-180">Om du loggar in med hello `azure login` kommando eller importera Publiceringsinställningar, din CLI profil och loggar lagras i en `.azure` directory finns i din `user` directory.</span><span class="sxs-lookup"><span data-stu-id="e9448-180">Whether you log in with hello `azure login` command or import publish settings, your CLI profile and logs are stored in a `.azure` directory located in your `user` directory.</span></span> <span data-ttu-id="e9448-181">Din `user` directory skyddas av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="e9448-181">Your `user` directory is protected by your operating system.</span></span> <span data-ttu-id="e9448-182">Vi rekommenderar dock att du vidta ytterligare åtgärder tooencrypt din `user` directory.</span><span class="sxs-lookup"><span data-stu-id="e9448-182">However, we recommend that you take additional steps tooencrypt your `user` directory.</span></span> <span data-ttu-id="e9448-183">Du kan göra det i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="e9448-183">You can do so in hello following ways:</span></span>

* <span data-ttu-id="e9448-184">Ändra hello katalogegenskaper eller använda BitLocker för Windows.</span><span class="sxs-lookup"><span data-stu-id="e9448-184">On Windows, modify hello directory properties or use BitLocker.</span></span>
* <span data-ttu-id="e9448-185">På Mac aktivera FileVault för hello directory.</span><span class="sxs-lookup"><span data-stu-id="e9448-185">On Mac, turn on FileVault for hello directory.</span></span>
* <span data-ttu-id="e9448-186">På Ubuntu, funktionen hello krypterade Home directory.</span><span class="sxs-lookup"><span data-stu-id="e9448-186">On Ubuntu, use hello Encrypted Home directory feature.</span></span> <span data-ttu-id="e9448-187">Andra Linux-distributioner erbjuder liknande funktioner.</span><span class="sxs-lookup"><span data-stu-id="e9448-187">Other Linux distributions offer similar features.</span></span>

## <a name="logging-out"></a><span data-ttu-id="e9448-188">Logga ut</span><span class="sxs-lookup"><span data-stu-id="e9448-188">Logging out</span></span>
<span data-ttu-id="e9448-189">toolog ut Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e9448-189">toolog out, use hello following command:</span></span>

    azure logout -u <username>

<span data-ttu-id="e9448-190">Om hello prenumerationer som är kopplade till autentiseras endast hello kontot med Active Directory, logga ut borttagningar hello prenumerationsinformation från hello lokal profil.</span><span class="sxs-lookup"><span data-stu-id="e9448-190">If hello subscriptions associated with hello account are only authenticated with Active Directory, logging out deletes hello subscription information from hello local profile.</span></span> <span data-ttu-id="e9448-191">Men om även en publicera inställningsfil importerades för hello prenumerationer loggar endast tar bort Active Directory relaterad information från hello lokal profil.</span><span class="sxs-lookup"><span data-stu-id="e9448-191">However, if a publish settings file was also imported for hello subscriptions, logging out only deletes Active Directory related information from hello local profile.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9448-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9448-192">Next steps</span></span>
* <span data-ttu-id="e9448-193">toouse Azure CLI-kommandon finns i [Azure CLI-kommandona i Resource Manager-läget](virtual-machines/azure-cli-arm-commands.md) och [Azure CLI-kommandon i Service Management-läge](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="e9448-193">toouse Azure CLI commands, see [Azure CLI commands in Resource Manager mode](virtual-machines/azure-cli-arm-commands.md) and [Azure CLI commands in Service Management mode](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>
* <span data-ttu-id="e9448-194">toolearn mer om hello Azure CLI, ladda ned källkoden, rapportera problem, eller bidra toohello projekt, besök hello [GitHub-lagringsplatsen för hello Azure CLI](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="e9448-194">toolearn more about hello Azure CLI, download source code, report problems, or contribute toohello project, visit hello [GitHub repository for hello Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="e9448-195">Om du får problem med att använda hello Azure CLI eller Azure, gå hello [Azure forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="e9448-195">If you encounter problems using hello Azure CLI, or Azure, visit hello [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>
