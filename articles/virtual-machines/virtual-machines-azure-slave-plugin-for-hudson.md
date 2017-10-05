---
title: "Hur du använder den Azure underordnade plugin-program med Hudson kontinuerlig Integration | Microsoft Docs"
description: "Beskriver hur du använder den Azure underordnade plugin-program med Hudson kontinuerlig Integration."
services: virtual-machines-linux
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: c11b59f8ea432075b147a391de4b7bd3331e639e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a><span data-ttu-id="58d14-103">Hur du använder den Azure underordnade plugin-program med Hudson kontinuerlig Integration</span><span class="sxs-lookup"><span data-stu-id="58d14-103">How to use the Azure slave plug-in with Hudson Continuous Integration</span></span>
<span data-ttu-id="58d14-104">Den Azure underordnade plugin-program för Hudson kan du etablera underordnade noder i Azure när distribuerade versioner.</span><span class="sxs-lookup"><span data-stu-id="58d14-104">The Azure slave plug-in for Hudson enables you to provision slave nodes on Azure when running distributed builds.</span></span>

## <a name="install-the-azure-slave-plug-in"></a><span data-ttu-id="58d14-105">Installera Azure-slavserver plugin-program</span><span class="sxs-lookup"><span data-stu-id="58d14-105">Install the Azure Slave plug-in</span></span>
1. <span data-ttu-id="58d14-106">I instrumentpanelen Hudson klickar du på **hantera Hudson**.</span><span class="sxs-lookup"><span data-stu-id="58d14-106">In the Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="58d14-107">I den **hantera Hudson** klickar du på **hantera plugin-program**.</span><span class="sxs-lookup"><span data-stu-id="58d14-107">In the **Manage Hudson** page, click on **Manage Plugins**.</span></span>
3. <span data-ttu-id="58d14-108">Klicka på den **tillgänglig** fliken.</span><span class="sxs-lookup"><span data-stu-id="58d14-108">Click the **Available** tab.</span></span>
4. <span data-ttu-id="58d14-109">Klicka på **Sök** och skriv **Azure** att begränsa listan till relevanta plugin-program.</span><span class="sxs-lookup"><span data-stu-id="58d14-109">Click **Search** and type **Azure** to limit the list to relevant plug-ins.</span></span>
   
    <span data-ttu-id="58d14-110">Om du väljer för att gå igenom listan med tillgängliga plugin-program, hittar du den Azure underordnade plugin-program under den **klusterhantering och distribuerade bygga** under den **andra** fliken.</span><span class="sxs-lookup"><span data-stu-id="58d14-110">If you opt to scroll through the list of available plug-ins, you will find the Azure slave plug-in under the **Cluster Management and Distributed Build** section in the **Others** tab.</span></span>
5. <span data-ttu-id="58d14-111">Markera kryssrutan för **Azure slavserver Plugin**.</span><span class="sxs-lookup"><span data-stu-id="58d14-111">Select the checkbox for **Azure Slave Plugin**.</span></span>
6. <span data-ttu-id="58d14-112">Klicka på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="58d14-112">Click **Install**.</span></span>
7. <span data-ttu-id="58d14-113">Starta om Hudson.</span><span class="sxs-lookup"><span data-stu-id="58d14-113">Restart Hudson.</span></span>

<span data-ttu-id="58d14-114">Plugin-programmet är installerat, är nästa steg att konfigurera plugin-programmet med din Azure-prenumeration och skapa en mall som ska användas för att skapa den virtuella datorn för den underordnade noden.</span><span class="sxs-lookup"><span data-stu-id="58d14-114">Now that the plug-in is installed, the next steps would be to configure the plug-in with your Azure subscription profile and to create a template that will be used in creating the VM for the slave node.</span></span>

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a><span data-ttu-id="58d14-115">Konfigurera Azure-slavserver plugin-program med din prenumeration profil</span><span class="sxs-lookup"><span data-stu-id="58d14-115">Configure the Azure Slave plug-in with your subscription profile</span></span>
<span data-ttu-id="58d14-116">En profil för prenumerationen, även kallat Publiceringsinställningar, är en XML-fil som innehåller säkra referenser och ytterligare information som du behöver arbeta med Azure i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="58d14-116">A subscription profile, also referred to as publish settings, is an XML file that contains secure credentials and some additional information you'll need to work with Azure in your development environment.</span></span> <span data-ttu-id="58d14-117">Om du vill konfigurera den Azure underordnade plugin-program behöver du:</span><span class="sxs-lookup"><span data-stu-id="58d14-117">To configure the Azure slave plug-in, you need:</span></span>

* <span data-ttu-id="58d14-118">Prenumerations-id</span><span class="sxs-lookup"><span data-stu-id="58d14-118">Your subscription id</span></span>
* <span data-ttu-id="58d14-119">Ett certifikat för din prenumeration</span><span class="sxs-lookup"><span data-stu-id="58d14-119">A management certificate for your subscription</span></span>

<span data-ttu-id="58d14-120">Dessa återfinns i din [prenumeration profil].</span><span class="sxs-lookup"><span data-stu-id="58d14-120">These can be found in your [subscription profile].</span></span> <span data-ttu-id="58d14-121">Nedan visas ett exempel på en profil för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="58d14-121">Below is an example of a subscription profile.</span></span>

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

<span data-ttu-id="58d14-122">När du har din prenumeration profil, Följ dessa steg om du vill konfigurera den Azure underordnade plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="58d14-122">Once you have your subscription profile, follow these steps to configure the Azure slave plug-in.</span></span>

1. <span data-ttu-id="58d14-123">I instrumentpanelen Hudson klickar du på **hantera Hudson**.</span><span class="sxs-lookup"><span data-stu-id="58d14-123">In the Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="58d14-124">Klicka på **konfigurera systemet**.</span><span class="sxs-lookup"><span data-stu-id="58d14-124">Click **Configure System**.</span></span>
3. <span data-ttu-id="58d14-125">Rulla ned på sidan för att hitta den **moln** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="58d14-125">Scroll down the page to find the **Cloud** section.</span></span>
4. <span data-ttu-id="58d14-126">Klicka på **lägga till nya molntjänster > Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="58d14-126">Click **Add new cloud > Microsoft Azure**.</span></span>
   
    ![lägga till nya molntjänster][add new cloud]
   
    <span data-ttu-id="58d14-128">Det här alternativet visas fält där du måste ange information om din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="58d14-128">This will show the fields where you need to enter your subscription details.</span></span>
   
    ![Konfigurera profilen][configure profile]
5. <span data-ttu-id="58d14-130">Kopiera prenumerations-id och hantering av certifikat från profilen prenumeration och klistra in dem i relevanta fält.</span><span class="sxs-lookup"><span data-stu-id="58d14-130">Copy the subscription id and management certificate from your subscription profile and paste them in the appropriate fields.</span></span>
   
    <span data-ttu-id="58d14-131">När du kopierar prenumerations-id och hantering av certifikat, **inte** citattecknen som omsluter värdena.</span><span class="sxs-lookup"><span data-stu-id="58d14-131">When copying the subscription id and management certificate, **do not** include the quotes that enclose the values.</span></span>
6. <span data-ttu-id="58d14-132">Klicka på **verifiera konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="58d14-132">Click on **Verify configuration**.</span></span>
7. <span data-ttu-id="58d14-133">När konfigurationen är har verifierats klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="58d14-133">When the configuration is verified successfully, click **Save**.</span></span>

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a><span data-ttu-id="58d14-134">Ställ in en mall för virtuell dator för Azure-slavserver plugin-program</span><span class="sxs-lookup"><span data-stu-id="58d14-134">Set up a virtual machine template for the Azure Slave plug-in</span></span>
<span data-ttu-id="58d14-135">En mall för virtuell dator definierar de parametrar som plugin-programmet använder för att skapa en slav nod på Azure.</span><span class="sxs-lookup"><span data-stu-id="58d14-135">A virtual machine template defines the parameters the plug-in will use to create a slave node on Azure.</span></span> <span data-ttu-id="58d14-136">I följande steg ska vi skapa mall för en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="58d14-136">In the following steps we'll be creating template for an Ubuntu VM.</span></span>

1. <span data-ttu-id="58d14-137">I instrumentpanelen Hudson klickar du på **hantera Hudson**.</span><span class="sxs-lookup"><span data-stu-id="58d14-137">In the Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="58d14-138">Klicka på **konfigurera systemet**.</span><span class="sxs-lookup"><span data-stu-id="58d14-138">Click on **Configure System**.</span></span>
3. <span data-ttu-id="58d14-139">Rulla ned på sidan för att hitta den **moln** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="58d14-139">Scroll down the page to find the **Cloud** section.</span></span>
4. <span data-ttu-id="58d14-140">I den **moln** avsnittet, hitta **Lägg till mall för virtuell dator i Azure** och klicka på den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="58d14-140">Within the **Cloud** section, find **Add Azure Virtual Machine Template** and click the **Add** button.</span></span>
   
    ![Lägg till mall för virtuell dator][add vm template]
5. <span data-ttu-id="58d14-142">Ange ett tjänstnamn för molnet i den **namn** fältet.</span><span class="sxs-lookup"><span data-stu-id="58d14-142">Specify a cloud service name in the **Name** field.</span></span> <span data-ttu-id="58d14-143">Om det namn du refererar till en befintlig molntjänst, kommer den virtuella datorn att tillhandahållas i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="58d14-143">If the name you specify refers to an existing cloud service, the VM will be provisioned in that service.</span></span> <span data-ttu-id="58d14-144">Annars skapar Azure en ny.</span><span class="sxs-lookup"><span data-stu-id="58d14-144">Otherwise, Azure will create a new one.</span></span>
6. <span data-ttu-id="58d14-145">I den **beskrivning** , ange text som beskriver den mall som du skapar.</span><span class="sxs-lookup"><span data-stu-id="58d14-145">In the **Description** field, enter text that describes the template you are creating.</span></span> <span data-ttu-id="58d14-146">Den här informationen är endast för dokumentation och används inte för att etablera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="58d14-146">This information is only for documentary purposes and is not used in provisioning a VM.</span></span>
7. <span data-ttu-id="58d14-147">I den **etiketter** anger **linux**.</span><span class="sxs-lookup"><span data-stu-id="58d14-147">In the **Labels** field, enter **linux**.</span></span> <span data-ttu-id="58d14-148">Den här etiketten används för att identifiera den mall som du skapar och används sedan för att referera till mallen när du skapar ett jobb för Hudson.</span><span class="sxs-lookup"><span data-stu-id="58d14-148">This label is used to identify the template you are creating and is subsequently used to reference the template when creating a Hudson job.</span></span>
8. <span data-ttu-id="58d14-149">Välj en region där den virtuella datorn kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="58d14-149">Select a region where the VM will be created.</span></span>
9. <span data-ttu-id="58d14-150">Välj lämplig VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="58d14-150">Select the appropriate VM size.</span></span>
10. <span data-ttu-id="58d14-151">Ange ett lagringskonto där den virtuella datorn kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="58d14-151">Specify a storage account where the VM will be created.</span></span> <span data-ttu-id="58d14-152">Kontrollera att den är i samma region som den molntjänst som du ska använda.</span><span class="sxs-lookup"><span data-stu-id="58d14-152">Make sure that it is in the same region as the cloud service you'll be using.</span></span> <span data-ttu-id="58d14-153">Om du vill ny lagring som ska skapas, kan du lämna fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="58d14-153">If you want new storage to be created, you can leave this field blank.</span></span>
11. <span data-ttu-id="58d14-154">Tiden för datakvarhållning anger antalet minuter innan Hudson tar bort en inaktiv slavserver.</span><span class="sxs-lookup"><span data-stu-id="58d14-154">Retention time specifies the number of minutes before Hudson deletes an idle slave.</span></span> <span data-ttu-id="58d14-155">Lämna det här standardvärdet 60.</span><span class="sxs-lookup"><span data-stu-id="58d14-155">Leave this at the default value of 60.</span></span>
12. <span data-ttu-id="58d14-156">I **användning**, väljer du den lämpliga när den här sekundära noden kommer att användas.</span><span class="sxs-lookup"><span data-stu-id="58d14-156">In **Usage**, select the appropriate condition when this slave node will be used.</span></span> <span data-ttu-id="58d14-157">Nu väljer **använda den här noden så mycket som möjligt**.</span><span class="sxs-lookup"><span data-stu-id="58d14-157">For now, select **Utilize this node as much as possible**.</span></span>
    
     <span data-ttu-id="58d14-158">Nu ser formuläret något ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="58d14-158">At this point, your form would look somewhat similar to this:</span></span>
    
     ![Mallkonfigurationen][template config]
13. <span data-ttu-id="58d14-160">I **Id eller bild-familjen** måste du ange vilka systemavbildning kommer att installeras på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="58d14-160">In **Image Family or Id** you have to specify what system image will be installed on your VM.</span></span> <span data-ttu-id="58d14-161">Du kan välja från en lista över avbildningen familjer, eller så kan du ange en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="58d14-161">You can either select from a list of image families or specify a custom image.</span></span>
    
     <span data-ttu-id="58d14-162">Om du vill välja från en lista över avbildningen familjer ange det första tecknet (skiftlägeskänsligt) familj avbildningsnamn.</span><span class="sxs-lookup"><span data-stu-id="58d14-162">If you want to select from a list of image families, enter the first character (case-sensitive) of the image family name.</span></span> <span data-ttu-id="58d14-163">Till exempel skriva **U** visas en lista över Ubuntu Server familjer.</span><span class="sxs-lookup"><span data-stu-id="58d14-163">For instance, typing **U** will bring up a list of Ubuntu Server families.</span></span> <span data-ttu-id="58d14-164">När du har valt i listan använder Jenkins den senaste versionen av systemavbildningen från den familjen vid etablering av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="58d14-164">Once you select from the list, Jenkins will use the latest version of that system image from that family when provisioning your VM.</span></span>
    
     ![OS-familjen lista][OS family list]
    
     <span data-ttu-id="58d14-166">Om du har en anpassad avbildning som du vill använda i stället anger du namnet på den egna avbildningen.</span><span class="sxs-lookup"><span data-stu-id="58d14-166">If you have a custom image that you want to use instead, enter the name of that custom image.</span></span> <span data-ttu-id="58d14-167">Anpassad avbildningsnamn visas inte i en lista, så du måste se till att namnet är rätt angivet.</span><span class="sxs-lookup"><span data-stu-id="58d14-167">Custom image names are not shown in a list so you have to ensure that the name is entered correctly.</span></span>    
    
     <span data-ttu-id="58d14-168">Den här kursen skriver **U** att få fram en lista över Ubuntu avbildningar och väljer **Ubuntu Server 14.04 LTS**.</span><span class="sxs-lookup"><span data-stu-id="58d14-168">For this tutorial, type **U** to bring up a list of Ubuntu images and select **Ubuntu Server 14.04 LTS**.</span></span>
14. <span data-ttu-id="58d14-169">För **starta metoden**väljer **SSH**.</span><span class="sxs-lookup"><span data-stu-id="58d14-169">For **Launch method**, select **SSH**.</span></span>
15. <span data-ttu-id="58d14-170">Kopiera skriptet nedan och klistra in i den **Init skriptet** fältet.</span><span class="sxs-lookup"><span data-stu-id="58d14-170">Copy the script below and paste in the **Init script** field.</span></span>
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     <span data-ttu-id="58d14-171">Den **Init skriptet** ska utföras efter att den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="58d14-171">The **Init script** will be executed after the VM is created.</span></span> <span data-ttu-id="58d14-172">I det här exemplet installerar skriptet Java, git och ant.</span><span class="sxs-lookup"><span data-stu-id="58d14-172">In this example, the script installs Java, git, and ant.</span></span>
16. <span data-ttu-id="58d14-173">I den **användarnamn** och **lösenord** fält, ange din önskade värden för administratörskontot som kommer att skapas på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="58d14-173">In the **Username** and **Password** fields, enter your preferred values for the administrator account that will be created on your VM.</span></span>
17. <span data-ttu-id="58d14-174">Klicka på **Kontrollera mallen** att kontrollera om de parametrar som du angav är giltiga.</span><span class="sxs-lookup"><span data-stu-id="58d14-174">Click on **Verify Template** to check if the parameters you specified are valid.</span></span>
18. <span data-ttu-id="58d14-175">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="58d14-175">Click on **Save**.</span></span>

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a><span data-ttu-id="58d14-176">Skapa ett Hudson jobb som körs på en slav nod på Azure</span><span class="sxs-lookup"><span data-stu-id="58d14-176">Create a Hudson job that runs on a slave node on Azure</span></span>
<span data-ttu-id="58d14-177">I det här avsnittet ska du skapa en Hudson aktivitet som körs på en slav nod på Azure.</span><span class="sxs-lookup"><span data-stu-id="58d14-177">In this section, you'll be creating a Hudson task that will run on a slave node on Azure.</span></span>

1. <span data-ttu-id="58d14-178">I instrumentpanelen Hudson klickar du på **nytt jobb**.</span><span class="sxs-lookup"><span data-stu-id="58d14-178">In the Hudson dashboard, click **New Job**.</span></span>
2. <span data-ttu-id="58d14-179">Ange ett namn för det jobb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="58d14-179">Enter a name for the job you are creating.</span></span>
3. <span data-ttu-id="58d14-180">Jobbtypen, Välj **bygga ett ledigt format programvara jobb**.</span><span class="sxs-lookup"><span data-stu-id="58d14-180">For the job type, select **Build a free-style software job**.</span></span>
4. <span data-ttu-id="58d14-181">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="58d14-181">Click **OK**.</span></span>
5. <span data-ttu-id="58d14-182">Välj i konfigurationssidan **begränsa där du kan köra det här projektet**.</span><span class="sxs-lookup"><span data-stu-id="58d14-182">In the job configuration page, select **Restrict where this project can be run**.</span></span>
6. <span data-ttu-id="58d14-183">Välj **nod och etikett menyn** och välj **linux** (vi angett den här etiketten när du skapar mallen för virtuella datorer i föregående avsnitt).</span><span class="sxs-lookup"><span data-stu-id="58d14-183">Select **Node and label menu** and select **linux** (we specified this label when creating the virtual machine template in the previous section).</span></span>
7. <span data-ttu-id="58d14-184">I den **skapa** klickar du på **Lägg till build steg** och välj **köra shell**.</span><span class="sxs-lookup"><span data-stu-id="58d14-184">In the **Build** section, click **Add build step** and select **Execute shell**.</span></span>
8. <span data-ttu-id="58d14-185">Redigera följande skript, ersätter **{github kontonamn}**, **{ditt projektnamn}**, och **{projektkatalogen}** med lämpliga värden och klistra in den redigerade skript i textområdet som visas.</span><span class="sxs-lookup"><span data-stu-id="58d14-185">Edit the following script, replacing **{your github account name}**, **{your project name}**, and **{your project directory}** with appropriate values, and paste the edited script in the text area that appears.</span></span>
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory to project
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. <span data-ttu-id="58d14-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="58d14-186">Click on **Save**.</span></span>
10. <span data-ttu-id="58d14-187">I instrumentpanelen för Hudson att hitta jobbet som du just har skapat och klicka på den **schemalägga en build** ikon.</span><span class="sxs-lookup"><span data-stu-id="58d14-187">In the Hudson dashboard, find the job you just created and click on the **Schedule a build** icon.</span></span>

<span data-ttu-id="58d14-188">Hudson sedan skapa en slav nod med hjälp av mallen skapas i föregående avsnitt och kör skriptet som du angav i steg build för den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="58d14-188">Hudson will then create a slave node using the template created in the previous section and execute the script you specified in the build step for this task.</span></span>

## <a name="next-steps"></a><span data-ttu-id="58d14-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="58d14-189">Next Steps</span></span>
<span data-ttu-id="58d14-190">Mer information om hur du använder Java i Azure finns i [Java-utvecklingscenter].</span><span class="sxs-lookup"><span data-stu-id="58d14-190">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<!-- URL List -->

<span data-ttu-id="58d14-191">[Java-utvecklingscenter]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="58d14-191">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="58d14-192">[prenumeration profil]: http://go.microsoft.com/fwlink/?LinkID=396395</span><span class="sxs-lookup"><span data-stu-id="58d14-192">[subscription profile]: http://go.microsoft.com/fwlink/?LinkID=396395</span></span>

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

