---
title: aaaInstall och konfigurera Terraform tooprovision virtuella datorer och annan infrastruktur i Azure | Microsoft Docs
description: "Lär dig hur tooinstall och konfigurera Terraform toocreate Azure resurser"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: b6706f53b21347442def05a592c30a2d22718984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-terraform-tooprovision-vms-and-other-infrastructure-into-azure"></a><span data-ttu-id="6fd4a-103">Installera och konfigurera Terraform tooprovision virtuella datorer och annan infrastruktur till Azure</span><span class="sxs-lookup"><span data-stu-id="6fd4a-103">Install and configure Terraform tooprovision VMs and other infrastructure into Azure</span></span> 
<span data-ttu-id="6fd4a-104">Den här artikeln beskriver hello nödvändiga åtgärder tooinstall och konfigurera Terraform tooprovision resurser, t.ex virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-104">This article describes hello necessary steps tooinstall and configure Terraform tooprovision resources such as virtual machines into Azure.</span></span> <span data-ttu-id="6fd4a-105">Får du lära dig hur toocreate och använda Azure autentiseringsuppgifter tooenable Terraform tooprovision molnresurser att på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-105">You will learn how toocreate and use Azure credentials tooenable Terraform tooprovision cloud resources in a secure manner.</span></span>

<span data-ttu-id="6fd4a-106">HashiCorp Terraform ger ett enkelt sätt toodefine och distribuera moln-infrastruktur med hjälp av en anpassad templating språk som kallas HashiCorp configuration språk (LISTAN).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-106">HashiCorp Terraform provides an easy way toodefine and deploy cloud infrastructure by using a custom templating language called HashiCorp configuration language (HCL).</span></span> <span data-ttu-id="6fd4a-107">Den här anpassade språket är [enkelt toowrite och enkelt toounderstand](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-107">This custom language is [easy toowrite and easy toounderstand](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="6fd4a-108">Dessutom med hello `terraform plan` kommandot kan du visualisera hello ändringar tooyour infrastruktur innan du utför dem.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-108">Additionally, by using hello `terraform plan` command, you can visualize hello changes tooyour infrastructure before you commit them.</span></span> <span data-ttu-id="6fd4a-109">Följ dessa steg toostart Terraform med Azure.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-109">Follow these steps toostart using Terraform with Azure.</span></span>

## <a name="install-terraform"></a><span data-ttu-id="6fd4a-110">Installera Terraform</span><span class="sxs-lookup"><span data-stu-id="6fd4a-110">Install Terraform</span></span>
<span data-ttu-id="6fd4a-111">tooinstall Terraform, [hämta](https://www.terraform.io/downloads.html) hello-paket som är lämpliga för ditt operativsystem i en separat installationskatalogen.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-111">tooinstall Terraform, [download](https://www.terraform.io/downloads.html) hello package appropriate for your operating system into a separate install directory.</span></span> <span data-ttu-id="6fd4a-112">hello innehåller en enda körbar fil som bör du också definiera en global sökväg.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-112">hello download contains a single executable file, for which you should also define a global path.</span></span> <span data-ttu-id="6fd4a-113">För instruktioner om hur tooset hello sökväg för Linux och Mac, gå för[den här webbsidan](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-113">For instructions on how tooset hello path on Linux and Mac, go too[this webpage](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux).</span></span> <span data-ttu-id="6fd4a-114">För instruktioner om hur tooset hello sökväg på Windows, gå för[den här webbsidan](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-114">For instructions on how tooset hello path on Windows, go too[this webpage](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).</span></span> <span data-ttu-id="6fd4a-115">tooverify installationen kör hello `terraform` kommando.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-115">tooverify your installation, run hello `terraform` command.</span></span> <span data-ttu-id="6fd4a-116">Du bör se en lista över tillgängliga alternativ för Terraform som utdata.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-116">You should see a list of available Terraform options as output.</span></span>

<span data-ttu-id="6fd4a-117">Du måste sedan tooallow Terraform åtkomst tooyour Azure-prenumeration tooperform infrastruktur etablering.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-117">Next, you need tooallow Terraform access tooyour Azure subscription tooperform infrastructure provisioning.</span></span>

## <a name="set-up-terraform-access-tooazure"></a><span data-ttu-id="6fd4a-118">Ställ in Terraform åtkomst tooAzure</span><span class="sxs-lookup"><span data-stu-id="6fd4a-118">Set up Terraform access tooAzure</span></span>
<span data-ttu-id="6fd4a-119">tooenable Terraform tooprovision resurser i Azure, behöver du toocreate två entiteter i Azure Active Directory (AD Azure): en Azure AD-program och en Azure AD-tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-119">tooenable Terraform tooprovision resources into Azure, you need toocreate two entities in Azure Active Directory (Azure AD): an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="6fd4a-120">Sedan kan du använda dessa enheter identifierare i Terraform skript.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-120">Then, you use these entities' identifiers in your Terraform scripts.</span></span> <span data-ttu-id="6fd4a-121">Ett huvudnamn för tjänsten är en lokal instans av en global Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-121">A service principal is a local instance of a global Azure AD application.</span></span> <span data-ttu-id="6fd4a-122">Ett huvudnamn för tjänsten kan detaljerade lokal åtkomstkontroll tooglobal resurser.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-122">A service principal allows granular local access control tooglobal resources.</span></span>

<span data-ttu-id="6fd4a-123">Det finns flera sätt toocreate en Azure AD-program och en Azure AD-tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-123">There are several ways toocreate an Azure AD application and an Azure AD service principal.</span></span> <span data-ttu-id="6fd4a-124">hello enklaste och snabbaste vägen idag är toouse Azure CLI 2.0 som [du kan hämta och installera på Windows, Linux eller en Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-124">hello easiest and fastest way today is toouse Azure CLI 2.0, which [you can download and install on Windows, Linux, or a Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="6fd4a-125">Du kan också använda PowerShell eller Azure CLI 1.0 toocreate hello nödvändiga säkerhetsinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-125">You also can use PowerShell or Azure CLI 1.0 toocreate hello necessary security infrastructure.</span></span> <span data-ttu-id="6fd4a-126">hello-instruktionerna som följer visar hur tooconfigure Terraform för Azure med hjälp av alla dessa metoder.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-126">hello instructions that follow show you how tooconfigure Terraform for Azure by using all of these approaches.</span></span>

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a><span data-ttu-id="6fd4a-127">Använda Azure CLI 2.0 (för Windows, Linux och Mac-användare)</span><span class="sxs-lookup"><span data-stu-id="6fd4a-127">Use Azure CLI 2.0 (for Windows, Linux, or Mac users)</span></span> 
<span data-ttu-id="6fd4a-128">När du hämtar och installerar hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), logga in tooadminister din Azure-prenumeration genom att utfärda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6fd4a-128">After you download and install hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), sign in tooadminister your Azure subscription by issuing hello following command:</span></span>

```
az login
```

>[!NOTE]
><span data-ttu-id="6fd4a-129">Om du använder hello Kina, Azure Tyskland eller Azure Government-moln, måste toofirst konfigurera hello Azure CLI toowork med det molnet.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-129">If you use hello China, Azure Germany, or Azure Government clouds, you need toofirst configure hello Azure CLI toowork with that cloud.</span></span> <span data-ttu-id="6fd4a-130">Du kan göra detta genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="6fd4a-130">You can do this by running hello following:</span></span>

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

<span data-ttu-id="6fd4a-131">Om du har flera Azure-prenumerationer, deras information returneras av hello `az login` kommando.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-131">If you have multiple Azure subscriptions, their details are returned by hello `az login` command.</span></span> <span data-ttu-id="6fd4a-132">Ange hello `SUBSCRIPTION_ID` returneras miljö variabeln toohold hello värdet för hello `id` från hello prenumeration du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-132">Set hello `SUBSCRIPTION_ID` environment variable toohold hello value of hello returned `id` field from hello subscription you want toouse.</span></span> 

<span data-ttu-id="6fd4a-133">Ange hello prenumeration som du vill toouse för den här sessionen.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-133">Set hello subscription that you want toouse for this session.</span></span>

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

<span data-ttu-id="6fd4a-134">Fråga hello konto tooget hello prenumerations-ID och klient-ID-värden.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-134">Query hello account tooget hello subscription ID and tenant ID values.</span></span>

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

<span data-ttu-id="6fd4a-135">Därefter måste du skapa separata autentiseringsuppgifter för Terraform.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-135">Next, create separate credentials for Terraform.</span></span>

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

<span data-ttu-id="6fd4a-136">AppId, lösenordet, sp_name och klient returneras.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-136">Your appId, password, sp_name, and tenant are returned.</span></span> <span data-ttu-id="6fd4a-137">Anteckna hello appId och lösenord.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-137">Make a note of hello appId and password.</span></span>

<span data-ttu-id="6fd4a-138">tooconfirm dina autentiseringsuppgifter (tjänstens huvudnamn), öppna ett nytt gränssnitt och kör följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-138">tooconfirm your credentials (service principal), open a new shell and run hello following commands.</span></span> <span data-ttu-id="6fd4a-139">Ersätt hello returnerade värden för sp_name, lösenord och klientorganisations:</span><span class="sxs-lookup"><span data-stu-id="6fd4a-139">Substitute hello returned values for sp_name, password, and tenant:</span></span>

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a><span data-ttu-id="6fd4a-140">Använd PowerShell (för Windows-användare)</span><span class="sxs-lookup"><span data-stu-id="6fd4a-140">Use PowerShell (for Windows users)</span></span> 
<span data-ttu-id="6fd4a-141">toouse Windows datorn toowrite och kör din Terraform skript och toouse PowerShell för konfigurationsåtgärder konfigurera din dator med hello rätt PowerShell-verktyg.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-141">toouse a Windows machine toowrite and execute your Terraform scripts and toouse PowerShell for configuration tasks, configure your machine with hello right PowerShell tools.</span></span> 

1. <span data-ttu-id="6fd4a-142">Installera PowerShell verktygen genom att följa stegen hello i [installera och konfigurera Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-142">Install PowerShell tools by following hello steps in [Install and configure Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span> 

2. <span data-ttu-id="6fd4a-143">Hämta och köra hello [azure setup.ps1 skriptet](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) från hello PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-143">Download and execute hello [azure-setup.ps1 script](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) from hello PowerShell console.</span></span>

3. <span data-ttu-id="6fd4a-144">toorun hello azure setup.ps1 skript, hämta och köra hello `./azure-setup.ps1 setup` från hello PowerShell-konsolen.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-144">toorun hello azure-setup.ps1 script, download it and execute hello `./azure-setup.ps1 setup` command from hello PowerShell console.</span></span> <span data-ttu-id="6fd4a-145">Logga sedan in tooyour Azure-prenumeration med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-145">Then sign in tooyour Azure subscription with administrative privileges.</span></span>

4. <span data-ttu-id="6fd4a-146">Ange ett programnamn (godtycklig sträng, krävs) när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-146">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="6fd4a-147">Du kan också ange ett starkt lösenord när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-147">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="6fd4a-148">Om du inte anger ett lösenord, genereras ett starkt lösenord med hjälp av .NET-bibliotek för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-148">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a><span data-ttu-id="6fd4a-149">Använda Azure CLI 1.0 (för Linux- eller Mac-användare)</span><span class="sxs-lookup"><span data-stu-id="6fd4a-149">Use Azure CLI 1.0 (for Linux or Mac users)</span></span>
<span data-ttu-id="6fd4a-150">tooget igång med Terraform på Linux-datorer eller Mac-datorer med Azure CLI 1.0, installera hello rätt bibliotek på din dator.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-150">tooget started with Terraform on Linux machines or Macs with Azure CLI 1.0, install hello proper libraries on your machine.</span></span>  

1. <span data-ttu-id="6fd4a-151">Installera Azure xPlat CLI-verktygen genom att följa stegen hello i [installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-151">Install Azure xPlat CLI tools by following hello steps in [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> 

2. <span data-ttu-id="6fd4a-152">Hämta och installera en JSON-processor genom att följa instruktionerna hello i [hämta jq](https://stedolan.github.io/jq/download/).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-152">Download and install a JSON processor by following hello instructions in [Download jq](https://stedolan.github.io/jq/download/).</span></span>

3. <span data-ttu-id="6fd4a-153">Hämta och köra hello [azure setup.sh skriptet](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash-skript från hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-153">Download and execute hello [azure-setup.sh script](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash script from hello console.</span></span>

4. <span data-ttu-id="6fd4a-154">toorun hello azure setup.sh skript, hämta och köra hello `./azure-setup setup` från hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-154">toorun hello azure-setup.sh script, download it and execute hello `./azure-setup setup` command from hello console.</span></span> <span data-ttu-id="6fd4a-155">Logga sedan in tooyour Azure-prenumeration med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-155">Then sign in tooyour Azure subscription with administrative privileges.</span></span>
 
5. <span data-ttu-id="6fd4a-156">Ange ett programnamn (godtycklig sträng, krävs) när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-156">Provide an application name (arbitrary string, required) when prompted.</span></span> <span data-ttu-id="6fd4a-157">Du kan också ange ett starkt lösenord när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-157">Optionally, supply a strong password when prompted.</span></span> <span data-ttu-id="6fd4a-158">Om du inte anger ett lösenord, genereras ett starkt lösenord med hjälp av .NET-bibliotek för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-158">If you don't provide a password, a strong password is generated for you by using .NET security libraries.</span></span>

<span data-ttu-id="6fd4a-159">Alla hello föregående skript skapa en Azure AD-program och tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-159">All hello previous scripts create an Azure AD application and service principal.</span></span> <span data-ttu-id="6fd4a-160">hello tjänstens huvudnamn hämtar deltagare eller ägare behörighet på hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-160">hello service principal gets a contributor or owner-level access on hello subscription.</span></span> <span data-ttu-id="6fd4a-161">På grund av hello hög nivå av åtkomst, bör du alltid skydda hello säkerhetsinformation som genereras av dessa skript.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-161">Because of hello high level of access granted, you should always protect hello security information generated by those scripts.</span></span> <span data-ttu-id="6fd4a-162">Anteckna alla fyra typer av säkerhetsinformation som tillhandahålls av dessa skript: appId, lösenord och PRENUMERATIONSID tenant_id.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-162">Make a note of all four pieces of security information provided by those scripts: appId, password, subscription_id, and tenant_id.</span></span>

## <a name="set-environment-variables"></a><span data-ttu-id="6fd4a-163">Uppsättning miljövariabler</span><span class="sxs-lookup"><span data-stu-id="6fd4a-163">Set environment variables</span></span>
<span data-ttu-id="6fd4a-164">När du skapar och konfigurerar en Azure AD-tjänstens huvudnamn måste toolet Terraform vet hello klient-ID, prenumerations-ID, klient-ID och klientens hemliga toouse.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-164">After you create and configure an Azure AD service principal, you need toolet Terraform know hello tenant ID, subscription ID, client ID, and client secret toouse.</span></span> <span data-ttu-id="6fd4a-165">Du kan göra det genom att bädda in dessa värden i skripten Terraform, enligt beskrivningen i [skapa grundläggande infrastruktur med hjälp av Terraform](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-165">You can do it by embedding those values in your Terraform scripts, as described in [Create basic infrastructure by using Terraform](terraform-create-complete-vm.md).</span></span> <span data-ttu-id="6fd4a-166">Alternativt kan kan du ange hello följande miljövariabler (och därmed undvika incheckning av misstag eller dela dina autentiseringsuppgifter):</span><span class="sxs-lookup"><span data-stu-id="6fd4a-166">Alternately, you can set hello following environment variables (and thus avoid accidentally checking in or sharing your credentials):</span></span>

- <span data-ttu-id="6fd4a-167">ARM_SUBSCRIPTION_ID</span><span class="sxs-lookup"><span data-stu-id="6fd4a-167">ARM_SUBSCRIPTION_ID</span></span>
- <span data-ttu-id="6fd4a-168">ARM_CLIENT_ID</span><span class="sxs-lookup"><span data-stu-id="6fd4a-168">ARM_CLIENT_ID</span></span>
- <span data-ttu-id="6fd4a-169">ARM_CLIENT_SECRET</span><span class="sxs-lookup"><span data-stu-id="6fd4a-169">ARM_CLIENT_SECRET</span></span>
- <span data-ttu-id="6fd4a-170">ARM_TENANT_ID</span><span class="sxs-lookup"><span data-stu-id="6fd4a-170">ARM_TENANT_ID</span></span>

<span data-ttu-id="6fd4a-171">Du kan använda det här exemplet shell-skript tooset dessa variabler:</span><span class="sxs-lookup"><span data-stu-id="6fd4a-171">You can use this sample shell script tooset those variables:</span></span>

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

<span data-ttu-id="6fd4a-172">Dessutom, om du använder Terraform med Azure i Kina eller antingen Azure Government eller Azure Tyskland måste tooset hello miljövariabeln korrekt.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-172">Additionally, if you use Terraform with Azure in China or either Azure Government or Azure Germany, you need tooset hello ENVIRONMENT variable appropriately.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6fd4a-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6fd4a-173">Next steps</span></span>
<span data-ttu-id="6fd4a-174">Du har nu installerats Terraform och konfigurerat Azure autentiseringsuppgifter så att du kan starta distribution av infrastrukturen i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6fd4a-174">You have now installed Terraform and configured Azure credentials so that you can start deploying infrastructure into your Azure subscription.</span></span> <span data-ttu-id="6fd4a-175">Lär dig sedan hur för[skapa infrastruktur med Terraform](terraform-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="6fd4a-175">Next, learn how too[create infrastructure with Terraform](terraform-create-complete-vm.md).</span></span>
