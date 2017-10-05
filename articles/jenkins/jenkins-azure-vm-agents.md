---
title: "Använda virtuella Azure-agenter för kontinuerlig integrering med Jenkins."
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
ms.openlocfilehash: 0b22a559fbc03158a6d4398603d1a7d2874d7b67
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a><span data-ttu-id="e3dc0-103">Använda virtuella Azure-agenter för kontinuerlig integrering med Jenkins.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-103">Use Azure VM agents for continuous integration with Jenkins.</span></span>

<span data-ttu-id="e3dc0-104">Den här snabbstarten visar hur du använder plugin-programmet Jenkins Azure VM-agenter för att skapa en agent för Linux (Ubuntu) på begäran i Azure.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-104">This quickstart shows how to use the Jenkins Azure VM Agents plugin to create an on-demand Linux (Ubuntu) agent in Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3dc0-105">Krav</span><span class="sxs-lookup"><span data-stu-id="e3dc0-105">Prerequisites</span></span>

<span data-ttu-id="e3dc0-106">För att slutföra den här snabbstarten behöver du:</span><span class="sxs-lookup"><span data-stu-id="e3dc0-106">To complete this quickstart:</span></span>

* <span data-ttu-id="e3dc0-107">Om du inte redan har en Jenkins-master, kan du starta börja med [Lösningsmallen](install-jenkins-solution-template.md)</span><span class="sxs-lookup"><span data-stu-id="e3dc0-107">If you do not already have a Jenkins master, you can start with the [Solution Template](install-jenkins-solution-template.md)</span></span> 
* <span data-ttu-id="e3dc0-108">Se [Skapa huvudsaklig Azure-tjänst med Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) om du inte redan har en huvudsaklig Azure-tjänst.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-108">Refer to [Create an Azure Service principal with Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) if you do not already have an Azure service principal.</span></span>

## <a name="install-azure-vm-agents-plugin"></a><span data-ttu-id="e3dc0-109">Installera Plugin-program för Azure VM-agenter</span><span class="sxs-lookup"><span data-stu-id="e3dc0-109">Install Azure VM Agents plugin</span></span>

<span data-ttu-id="e3dc0-110">Om du börjar från [Lösningsmallen](install-jenkins-solution-template.md) är plugin-programmet för Azure VM-agenten installerad i Jenkins-master.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-110">If you start from the [Solution Template](install-jenkins-solution-template.md), the Azure VM Agent plugin is installed in the Jenkins master.</span></span>

<span data-ttu-id="e3dc0-111">I annat fall installerar du plugin-programmet **Azure VM agenter** från Jenkins instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-111">Otherwise, install the **Azure VM Agents** plugin from within the Jenkins dashboard.</span></span>

## <a name="configure-the-plugin"></a><span data-ttu-id="e3dc0-112">Konfigurera plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="e3dc0-112">Configure the plugin</span></span>

* <span data-ttu-id="e3dc0-113">I Jenkins-instrumentpanelen klickar du på **Hantera Jenkins -> Konfigurera System ->**.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-113">Within the Jenkins dashboard, click **Manage Jenkins -> Configure System ->**.</span></span> <span data-ttu-id="e3dc0-114">Bläddra till längst ned på sidan och leta reda på avsnittet i listrutan **Lägga till nya molntjänster**.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-114">Scroll to the bottom of the page and find the section with the dropdown **Add new cloud**.</span></span> <span data-ttu-id="e3dc0-115">Välj **Microsoft Azure VM agenter** på menyn</span><span class="sxs-lookup"><span data-stu-id="e3dc0-115">From the menu, select **Microsoft Azure VM Agents**</span></span>
* <span data-ttu-id="e3dc0-116">Välj ett befintligt konto i listrutan Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-116">Select an existing account from the Azure Credentials dropdown.</span></span>  <span data-ttu-id="e3dc0-117">För att lägga till en ny **Microsoft Azure Service Principal** anger du följande värden: prenumerations-ID, klient-ID, Klienthemligheten och OAuth 2.0-Token för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-117">To add a new **Microsoft Azure Service Principal,** enter the following values: Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span>

![Azure-autentiseringsuppgifter](./media/jenkins-azure-vm-agents/service-principal.png)

* <span data-ttu-id="e3dc0-119">Klicka på **verifiera konfigurationen** och kontrollera att profilkonfigurationen är korrekt.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-119">Click **Verify configuration** to make sure that the profile configuration is correct.</span></span>
* <span data-ttu-id="e3dc0-120">Spara konfigurationen och fortsätt till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-120">Save the configuration, and continue to the next step.</span></span>

## <a name="template-configuration"></a><span data-ttu-id="e3dc0-121">Mallkonfiguration</span><span class="sxs-lookup"><span data-stu-id="e3dc0-121">Template configuration</span></span>

### <a name="general-configuration"></a><span data-ttu-id="e3dc0-122">Allmän konfiguration</span><span class="sxs-lookup"><span data-stu-id="e3dc0-122">General configuration</span></span>
<span data-ttu-id="e3dc0-123">Konfigurera en mall som ska användas för att definiera en Azure VM-agent.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-123">Next, configure a template for use to define an Azure VM agent.</span></span> 

* <span data-ttu-id="e3dc0-124">Klicka på **Lägg till** att lägga till en mall.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-124">Click **Add** to add a template.</span></span> 
* <span data-ttu-id="e3dc0-125">Ge din nya mall ett nytt namn.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-125">Provide a name for your new template.</span></span> 
* <span data-ttu-id="e3dc0-126">För etiketten, anger du ”ubuntu”.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-126">For the label, enter  "ubuntu."</span></span> <span data-ttu-id="e3dc0-127">Den här etiketten används under jobbkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-127">This label is used during the job configuration.</span></span>
* <span data-ttu-id="e3dc0-128">Välj önskad region från kombinationsrutan.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-128">Select the desired region from the combo box.</span></span>
* <span data-ttu-id="e3dc0-129">Välj önskad storlek för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-129">Select the desired VM size.</span></span>
* <span data-ttu-id="e3dc0-130">Ange namnet på Azure Storage-kontot eller lämna det tomt om du vill använda standardnamnet ”jenkinsarmst”.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-130">Specify the Azure Storage account name or leave it blank to use the default name "jenkinsarmst."</span></span>
* <span data-ttu-id="e3dc0-131">Ange tiden för datakvarhållning i minuter.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-131">Specify the retention time in minutes.</span></span> <span data-ttu-id="e3dc0-132">Den här inställningen anger hur många minuter Jenkins kan vänta innan du tar bort en inaktiv agent automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-132">This setting defines the number of minutes Jenkins can wait before automatically deleting an idle agent.</span></span> <span data-ttu-id="e3dc0-133">Ange 0 om du inte vill att inaktiva agenter ska tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-133">Specify 0 if you do not want idle agents to be deleted automatically.</span></span>

![Allmän konfiguration](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a><span data-ttu-id="e3dc0-135">Bildkonfiguration</span><span class="sxs-lookup"><span data-stu-id="e3dc0-135">Image configuration</span></span>

<span data-ttu-id="e3dc0-136">Välj **Bildreferens** och använd följande konfiguration som exempel om du vill skapa en agent för Linux (Ubuntu).</span><span class="sxs-lookup"><span data-stu-id="e3dc0-136">To create a Linux (Ubuntu) agent, select **Image reference** and use the following configuration as an example.</span></span> <span data-ttu-id="e3dc0-137">Se [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) för den senaste bilderna som stöds i Azure.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-137">Refer to [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) for the latest Azure supported images.</span></span>

* <span data-ttu-id="e3dc0-138">Bildutgivare: Canonical</span><span class="sxs-lookup"><span data-stu-id="e3dc0-138">Image Publisher: Canonical</span></span>
* <span data-ttu-id="e3dc0-139">Bild-erbjudande: UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="e3dc0-139">Image Offer: UbuntuServer</span></span>
* <span data-ttu-id="e3dc0-140">Bild-Sku: 14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="e3dc0-140">Image Sku: 14.04.5-LTS</span></span>
* <span data-ttu-id="e3dc0-141">Bildversion: senaste</span><span class="sxs-lookup"><span data-stu-id="e3dc0-141">Image version: latest</span></span>
* <span data-ttu-id="e3dc0-142">OS-typ: Linux</span><span class="sxs-lookup"><span data-stu-id="e3dc0-142">OS Type: Linux</span></span>
* <span data-ttu-id="e3dc0-143">Startmetod: SSH</span><span class="sxs-lookup"><span data-stu-id="e3dc0-143">Launch method: SSH</span></span>
* <span data-ttu-id="e3dc0-144">Uppge administratörsautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="e3dc0-144">Provide an admin credentials</span></span>
* <span data-ttu-id="e3dc0-145">Ange följande för initieringsskriptet för virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="e3dc0-145">For VM initialization script, enter:</span></span>
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Bildkonfiguration](./media/jenkins-azure-vm-agents/image-config.png)

* <span data-ttu-id="e3dc0-147">Klicka på **Kontrollera mallen** att kontrollera konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-147">Click **Verify Template** to verify the configuration.</span></span>
* <span data-ttu-id="e3dc0-148">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-148">Click **Save**.</span></span>

## <a name="create-a-job-in-jenkins"></a><span data-ttu-id="e3dc0-149">Skapa ett jobb i Jenkins</span><span class="sxs-lookup"><span data-stu-id="e3dc0-149">Create a job in Jenkins</span></span>

* <span data-ttu-id="e3dc0-150">I instrumentpanelen för Jenkins klickar du på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-150">Within the Jenkins dashboard, click **New Item**.</span></span> 
* <span data-ttu-id="e3dc0-151">Ange ett namn och välj **Freestyle-projekt** och sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-151">Enter a name and select **Freestyle project** and click **OK**.</span></span>
* <span data-ttu-id="e3dc0-152">På fliken **Allmänt** markerar du ”Begränsa var projekt kan köras” och typen ”ubuntu” i etikettuttrycket.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-152">In the **General** tab, select "Restrict where project can be run" and type "ubuntu" in Label Expression.</span></span> <span data-ttu-id="e3dc0-153">Nu kan du se ”ubuntu” i listrutan.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-153">You now see "ubuntu" in the dropdown.</span></span>
* <span data-ttu-id="e3dc0-154">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-154">Click **Save**.</span></span>

![Ställ in jobb](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a><span data-ttu-id="e3dc0-156">Skapa det nya projektet</span><span class="sxs-lookup"><span data-stu-id="e3dc0-156">Build your new project</span></span>

* <span data-ttu-id="e3dc0-157">Gå tillbaka till Jenkins-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-157">Go back to the Jenkins dashboard.</span></span>
* <span data-ttu-id="e3dc0-158">Högerklicka på det nya jobbet du skapade och klicka sedan på **Skapa nu**.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-158">Right-click the new job you created, then click **Build now**.</span></span> <span data-ttu-id="e3dc0-159">En version har startats.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-159">A build is kicked off.</span></span> 
* <span data-ttu-id="e3dc0-160">När versionen har slutförts, går du till **Konsolutdata**.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-160">Once the build is complete, go to **Console output**.</span></span> <span data-ttu-id="e3dc0-161">Du ser att versionen skapades via en fjärranslutning på Azure.</span><span class="sxs-lookup"><span data-stu-id="e3dc0-161">You see that the build was performed remotely on Azure.</span></span>

![Konsolutdata](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a><span data-ttu-id="e3dc0-163">Referens</span><span class="sxs-lookup"><span data-stu-id="e3dc0-163">Reference</span></span>

* <span data-ttu-id="e3dc0-164">Azure Friday-video: [kontinuerlig integration med Jenkins med virtuella Azure-agenter](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span><span class="sxs-lookup"><span data-stu-id="e3dc0-164">Azure Friday video: [Continuous Integration with Jenkins using Azure VM agents](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span></span>
* <span data-ttu-id="e3dc0-165">Supportalternativ och konfigurationsinformation: [Azure VM-agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span><span class="sxs-lookup"><span data-stu-id="e3dc0-165">Support information and configuration options:  [Azure VM Agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span></span> 

