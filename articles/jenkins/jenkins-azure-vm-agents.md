---
title: "aaaUse Virtuella Azure-agenter för kontinuerlig integrering med Jenkins."
description: Azure VM-agenter som Jenkins slaves.
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 2388e6919d0280372166fbd325d80dafb00d7550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a><span data-ttu-id="fbccc-103">Använda virtuella Azure-agenter för kontinuerlig integrering med Jenkins.</span><span class="sxs-lookup"><span data-stu-id="fbccc-103">Use Azure VM agents for continuous integration with Jenkins.</span></span>

<span data-ttu-id="fbccc-104">Den här snabbstarten visar hur hello toouse Jenkins Azure VM agenter plugin-programmet toocreate en agent för Linux (Ubuntu) av på begäran i Azure.</span><span class="sxs-lookup"><span data-stu-id="fbccc-104">This quickstart shows how toouse hello Jenkins Azure VM Agents plugin toocreate an on-demand Linux (Ubuntu) agent in Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbccc-105">Krav</span><span class="sxs-lookup"><span data-stu-id="fbccc-105">Prerequisites</span></span>

<span data-ttu-id="fbccc-106">toocomplete denna Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="fbccc-106">toocomplete this quickstart:</span></span>

* <span data-ttu-id="fbccc-107">Om du inte redan har en Jenkins master, du kan starta med hello [Lösningsmall](install-jenkins-solution-template.md)</span><span class="sxs-lookup"><span data-stu-id="fbccc-107">If you do not already have a Jenkins master, you can start with hello [Solution Template](install-jenkins-solution-template.md)</span></span> 
* <span data-ttu-id="fbccc-108">Se för[skapa ett Azure-tjänstens huvudnamn med Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) om du inte redan har en Azure-tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="fbccc-108">Refer too[Create an Azure Service principal with Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) if you do not already have an Azure service principal.</span></span>

## <a name="install-azure-vm-agents-plugin"></a><span data-ttu-id="fbccc-109">Installera Plugin-program för Azure VM-agenter</span><span class="sxs-lookup"><span data-stu-id="fbccc-109">Install Azure VM Agents plugin</span></span>

<span data-ttu-id="fbccc-110">Om du startar från hello [Lösningsmall](install-jenkins-solution-template.md), hello Azure VM-agenten plugin-programmet är installerat i hello Jenkins master.</span><span class="sxs-lookup"><span data-stu-id="fbccc-110">If you start from hello [Solution Template](install-jenkins-solution-template.md), hello Azure VM Agent plugin is installed in hello Jenkins master.</span></span>

<span data-ttu-id="fbccc-111">I annat fall installerar hello **Azure VM agenter** plugin-programmet från inom hello Jenkins instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="fbccc-111">Otherwise, install hello **Azure VM Agents** plugin from within hello Jenkins dashboard.</span></span>

## <a name="configure-hello-plugin"></a><span data-ttu-id="fbccc-112">Konfigurera hello plugin-program</span><span class="sxs-lookup"><span data-stu-id="fbccc-112">Configure hello plugin</span></span>

* <span data-ttu-id="fbccc-113">Inom hello Jenkins instrumentpanelen, klickar du på **Jenkins hantera -> Konfigurera System ->**.</span><span class="sxs-lookup"><span data-stu-id="fbccc-113">Within hello Jenkins dashboard, click **Manage Jenkins -> Configure System ->**.</span></span> <span data-ttu-id="fbccc-114">Rulla toohello längst ned på sidan för hello och hitta hello avsnitt med hello nedrullningsbara **lägga till nya molntjänster**.</span><span class="sxs-lookup"><span data-stu-id="fbccc-114">Scroll toohello bottom of hello page and find hello section with hello dropdown **Add new cloud**.</span></span> <span data-ttu-id="fbccc-115">Hello-menyn, Välj **Microsoft Azure VM agenter**</span><span class="sxs-lookup"><span data-stu-id="fbccc-115">From hello menu, select **Microsoft Azure VM Agents**</span></span>
* <span data-ttu-id="fbccc-116">Välj ett befintligt konto hello Azure-autentiseringsuppgifterna listrutan.</span><span class="sxs-lookup"><span data-stu-id="fbccc-116">Select an existing account from hello Azure Credentials dropdown.</span></span>  <span data-ttu-id="fbccc-117">tooadd en ny **Microsoft Azure Service Principal** ange hello följande värden: prenumerations-ID, klient-ID, Klienthemligheten och OAuth 2.0-Token för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="fbccc-117">tooadd a new **Microsoft Azure Service Principal,** enter hello following values: Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span>

![Azure-autentiseringsuppgifter](./media/jenkins-azure-vm-agents/service-principal.png)

* <span data-ttu-id="fbccc-119">Klicka på **verifiera konfigurationen** toomake att hello profilkonfigurationen är korrekt.</span><span class="sxs-lookup"><span data-stu-id="fbccc-119">Click **Verify configuration** toomake sure that hello profile configuration is correct.</span></span>
* <span data-ttu-id="fbccc-120">Spara hello konfigurationen och fortsätta toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="fbccc-120">Save hello configuration, and continue toohello next step.</span></span>

## <a name="template-configuration"></a><span data-ttu-id="fbccc-121">Mallkonfiguration</span><span class="sxs-lookup"><span data-stu-id="fbccc-121">Template configuration</span></span>

### <a name="general-configuration"></a><span data-ttu-id="fbccc-122">Allmän konfiguration</span><span class="sxs-lookup"><span data-stu-id="fbccc-122">General configuration</span></span>
<span data-ttu-id="fbccc-123">Konfigurera en mall för Använd toodefine Virtuella Azure-agenten.</span><span class="sxs-lookup"><span data-stu-id="fbccc-123">Next, configure a template for use toodefine an Azure VM agent.</span></span> 

* <span data-ttu-id="fbccc-124">Klicka på **Lägg till** tooadd en mall.</span><span class="sxs-lookup"><span data-stu-id="fbccc-124">Click **Add** tooadd a template.</span></span> 
* <span data-ttu-id="fbccc-125">Ge din nya mall ett nytt namn.</span><span class="sxs-lookup"><span data-stu-id="fbccc-125">Provide a name for your new template.</span></span> 
* <span data-ttu-id="fbccc-126">Ange hello etiketten ”ubuntu”.</span><span class="sxs-lookup"><span data-stu-id="fbccc-126">For hello label, enter  "ubuntu."</span></span> <span data-ttu-id="fbccc-127">Den här etiketten används under konfigurationen av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="fbccc-127">This label is used during hello job configuration.</span></span>
* <span data-ttu-id="fbccc-128">Välj önskad region för hello hello kombinationsruta.</span><span class="sxs-lookup"><span data-stu-id="fbccc-128">Select hello desired region from hello combo box.</span></span>
* <span data-ttu-id="fbccc-129">Välj hello önskade VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="fbccc-129">Select hello desired VM size.</span></span>
* <span data-ttu-id="fbccc-130">Ange kontonamn för hello Azure Storage eller lämna den tom toouse hello standardnamnet ”jenkinsarmst”.</span><span class="sxs-lookup"><span data-stu-id="fbccc-130">Specify hello Azure Storage account name or leave it blank toouse hello default name "jenkinsarmst."</span></span>
* <span data-ttu-id="fbccc-131">Ange hello kvarhållningstiden i minuter.</span><span class="sxs-lookup"><span data-stu-id="fbccc-131">Specify hello retention time in minutes.</span></span> <span data-ttu-id="fbccc-132">Den här inställningen definierar hello antal minuter som Jenkins kan vänta innan du tar bort en inaktiv agent automatiskt.</span><span class="sxs-lookup"><span data-stu-id="fbccc-132">This setting defines hello number of minutes Jenkins can wait before automatically deleting an idle agent.</span></span> <span data-ttu-id="fbccc-133">Ange 0 om du inte vill att inaktiva agenter toobe tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="fbccc-133">Specify 0 if you do not want idle agents toobe deleted automatically.</span></span>

![Allmän konfiguration](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a><span data-ttu-id="fbccc-135">Bildkonfiguration</span><span class="sxs-lookup"><span data-stu-id="fbccc-135">Image configuration</span></span>

<span data-ttu-id="fbccc-136">toocreate linuxagenten (Ubuntu), Välj **bild referens** och Använd hello efter konfigurationen som exempel.</span><span class="sxs-lookup"><span data-stu-id="fbccc-136">toocreate a Linux (Ubuntu) agent, select **Image reference** and use hello following configuration as an example.</span></span> <span data-ttu-id="fbccc-137">Se för[Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) för hello senaste Azure stöds bilder.</span><span class="sxs-lookup"><span data-stu-id="fbccc-137">Refer too[Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) for hello latest Azure supported images.</span></span>

* <span data-ttu-id="fbccc-138">Bildutgivare: Canonical</span><span class="sxs-lookup"><span data-stu-id="fbccc-138">Image Publisher: Canonical</span></span>
* <span data-ttu-id="fbccc-139">Bild-erbjudande: UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="fbccc-139">Image Offer: UbuntuServer</span></span>
* <span data-ttu-id="fbccc-140">Bild-Sku: 14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="fbccc-140">Image Sku: 14.04.5-LTS</span></span>
* <span data-ttu-id="fbccc-141">Bildversion: senaste</span><span class="sxs-lookup"><span data-stu-id="fbccc-141">Image version: latest</span></span>
* <span data-ttu-id="fbccc-142">OS-typ: Linux</span><span class="sxs-lookup"><span data-stu-id="fbccc-142">OS Type: Linux</span></span>
* <span data-ttu-id="fbccc-143">Startmetod: SSH</span><span class="sxs-lookup"><span data-stu-id="fbccc-143">Launch method: SSH</span></span>
* <span data-ttu-id="fbccc-144">Uppge administratörsautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="fbccc-144">Provide an admin credentials</span></span>
* <span data-ttu-id="fbccc-145">Ange följande för initieringsskriptet för virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="fbccc-145">For VM initialization script, enter:</span></span>
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Bildkonfiguration](./media/jenkins-azure-vm-agents/image-config.png)

* <span data-ttu-id="fbccc-147">Klicka på **Kontrollera mallen** tooverify hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fbccc-147">Click **Verify Template** tooverify hello configuration.</span></span>
* <span data-ttu-id="fbccc-148">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="fbccc-148">Click **Save**.</span></span>

## <a name="create-a-job-in-jenkins"></a><span data-ttu-id="fbccc-149">Skapa ett jobb i Jenkins</span><span class="sxs-lookup"><span data-stu-id="fbccc-149">Create a job in Jenkins</span></span>

* <span data-ttu-id="fbccc-150">Inom hello Jenkins instrumentpanelen, klickar du på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="fbccc-150">Within hello Jenkins dashboard, click **New Item**.</span></span> 
* <span data-ttu-id="fbccc-151">Ange ett namn och välj **Freestyle-projekt** och sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fbccc-151">Enter a name and select **Freestyle project** and click **OK**.</span></span>
* <span data-ttu-id="fbccc-152">I hello **allmänna** fliken, markerar ”begränsa där du kan köra project” och typ ”ubuntu” i etikettuttrycket.</span><span class="sxs-lookup"><span data-stu-id="fbccc-152">In hello **General** tab, select "Restrict where project can be run" and type "ubuntu" in Label Expression.</span></span> <span data-ttu-id="fbccc-153">Nu kan du se ”ubuntu” i hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="fbccc-153">You now see "ubuntu" in hello dropdown.</span></span>
* <span data-ttu-id="fbccc-154">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="fbccc-154">Click **Save**.</span></span>

![Ställ in jobb](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a><span data-ttu-id="fbccc-156">Skapa det nya projektet</span><span class="sxs-lookup"><span data-stu-id="fbccc-156">Build your new project</span></span>

* <span data-ttu-id="fbccc-157">Gå tillbaka toohello Jenkins instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="fbccc-157">Go back toohello Jenkins dashboard.</span></span>
* <span data-ttu-id="fbccc-158">Högerklicka på hello nytt jobb som du har skapat, klicka på **skapa nu**.</span><span class="sxs-lookup"><span data-stu-id="fbccc-158">Right-click hello new job you created, then click **Build now**.</span></span> <span data-ttu-id="fbccc-159">En version har startats.</span><span class="sxs-lookup"><span data-stu-id="fbccc-159">A build is kicked off.</span></span> 
* <span data-ttu-id="fbccc-160">När hello build är klar, går för**konsolen utdata**.</span><span class="sxs-lookup"><span data-stu-id="fbccc-160">Once hello build is complete, go too**Console output**.</span></span> <span data-ttu-id="fbccc-161">Du ser att hello build utfördes via fjärranslutning på Azure.</span><span class="sxs-lookup"><span data-stu-id="fbccc-161">You see that hello build was performed remotely on Azure.</span></span>

![Konsolutdata](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a><span data-ttu-id="fbccc-163">Referens</span><span class="sxs-lookup"><span data-stu-id="fbccc-163">Reference</span></span>

* <span data-ttu-id="fbccc-164">Azure Friday-video: [kontinuerlig integration med Jenkins med virtuella Azure-agenter](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span><span class="sxs-lookup"><span data-stu-id="fbccc-164">Azure Friday video: [Continuous Integration with Jenkins using Azure VM agents](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span></span>
* <span data-ttu-id="fbccc-165">Supportalternativ och konfigurationsinformation: [Azure VM-agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span><span class="sxs-lookup"><span data-stu-id="fbccc-165">Support information and configuration options:  [Azure VM Agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span></span> 

