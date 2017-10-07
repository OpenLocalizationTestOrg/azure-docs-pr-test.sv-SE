---
title: aaaHow toouse hello Azure slavserver plugin-program med Hudson kontinuerlig Integration | Microsoft Docs
description: Beskriver hur toouse hello Azure slavserver plugin-program med Hudson kontinuerlig Integration.
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
ms.openlocfilehash: cd6e67ad71c208aa56746aa8b70ba507da20bee9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-slave-plug-in-with-hudson-continuous-integration"></a><span data-ttu-id="59da9-103">Hur toouse hello Azure slavserver plugin-program med Hudson kontinuerlig Integration</span><span class="sxs-lookup"><span data-stu-id="59da9-103">How toouse hello Azure slave plug-in with Hudson Continuous Integration</span></span>
<span data-ttu-id="59da9-104">hello Azure slavserver plugin-program för Hudson kan tooprovision underordnade noder i Azure när distribuerade versioner.</span><span class="sxs-lookup"><span data-stu-id="59da9-104">hello Azure slave plug-in for Hudson enables you tooprovision slave nodes on Azure when running distributed builds.</span></span>

## <a name="install-hello-azure-slave-plug-in"></a><span data-ttu-id="59da9-105">Installera hello Azure slavserver plugin-program</span><span class="sxs-lookup"><span data-stu-id="59da9-105">Install hello Azure Slave plug-in</span></span>
1. <span data-ttu-id="59da9-106">I hello Hudson instrumentpanelen, klickar du på **hantera Hudson**.</span><span class="sxs-lookup"><span data-stu-id="59da9-106">In hello Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="59da9-107">I hello **hantera Hudson** klickar du på **hantera plugin-program**.</span><span class="sxs-lookup"><span data-stu-id="59da9-107">In hello **Manage Hudson** page, click on **Manage Plugins**.</span></span>
3. <span data-ttu-id="59da9-108">Klicka på hello **tillgänglig** fliken.</span><span class="sxs-lookup"><span data-stu-id="59da9-108">Click hello **Available** tab.</span></span>
4. <span data-ttu-id="59da9-109">Klicka på **Sök** och skriv **Azure** toolimit hello listan toorelevant plugin-program.</span><span class="sxs-lookup"><span data-stu-id="59da9-109">Click **Search** and type **Azure** toolimit hello list toorelevant plug-ins.</span></span>
   
    <span data-ttu-id="59da9-110">Om du väljer tooscroll hello listan med tillgängliga plugin-program, hittar du hello Azure slavserver plugin-programmet under hello **klusterhantering och distribuerade bygga** avsnitt i hello **andra** fliken.</span><span class="sxs-lookup"><span data-stu-id="59da9-110">If you opt tooscroll through hello list of available plug-ins, you will find hello Azure slave plug-in under hello **Cluster Management and Distributed Build** section in hello **Others** tab.</span></span>
5. <span data-ttu-id="59da9-111">Markera kryssrutan för hello för **Azure slavserver Plugin**.</span><span class="sxs-lookup"><span data-stu-id="59da9-111">Select hello checkbox for **Azure Slave Plugin**.</span></span>
6. <span data-ttu-id="59da9-112">Klicka på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="59da9-112">Click **Install**.</span></span>
7. <span data-ttu-id="59da9-113">Starta om Hudson.</span><span class="sxs-lookup"><span data-stu-id="59da9-113">Restart Hudson.</span></span>

<span data-ttu-id="59da9-114">Nu att plugin-programmet hello installeras vara hello nästa steg tooconfigure hello plugin-program med Azure-prenumeration profil och toocreate en mall som ska användas för att skapa hello VM för hello underordnade nod.</span><span class="sxs-lookup"><span data-stu-id="59da9-114">Now that hello plug-in is installed, hello next steps would be tooconfigure hello plug-in with your Azure subscription profile and toocreate a template that will be used in creating hello VM for hello slave node.</span></span>

## <a name="configure-hello-azure-slave-plug-in-with-your-subscription-profile"></a><span data-ttu-id="59da9-115">Konfigurera hello Azure slavserver plugin-program med din prenumeration profil</span><span class="sxs-lookup"><span data-stu-id="59da9-115">Configure hello Azure Slave plug-in with your subscription profile</span></span>
<span data-ttu-id="59da9-116">En prenumeration profil också hänvisade tooas Publiceringsinställningar, är en XML-fil som innehåller säkra referenser och ytterligare information som du behöver toowork med Azure i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="59da9-116">A subscription profile, also referred tooas publish settings, is an XML file that contains secure credentials and some additional information you'll need toowork with Azure in your development environment.</span></span> <span data-ttu-id="59da9-117">tooconfigure hello Azure slavserver plugin-program, behöver du:</span><span class="sxs-lookup"><span data-stu-id="59da9-117">tooconfigure hello Azure slave plug-in, you need:</span></span>

* <span data-ttu-id="59da9-118">Prenumerations-id</span><span class="sxs-lookup"><span data-stu-id="59da9-118">Your subscription id</span></span>
* <span data-ttu-id="59da9-119">Ett certifikat för din prenumeration</span><span class="sxs-lookup"><span data-stu-id="59da9-119">A management certificate for your subscription</span></span>

<span data-ttu-id="59da9-120">Dessa återfinns i din [prenumeration profil].</span><span class="sxs-lookup"><span data-stu-id="59da9-120">These can be found in your [subscription profile].</span></span> <span data-ttu-id="59da9-121">Nedan visas ett exempel på en profil för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="59da9-121">Below is an example of a subscription profile.</span></span>

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

<span data-ttu-id="59da9-122">Följ dessa steg tooconfigure hello Azure slavserver plugin-program när du har din prenumeration profil.</span><span class="sxs-lookup"><span data-stu-id="59da9-122">Once you have your subscription profile, follow these steps tooconfigure hello Azure slave plug-in.</span></span>

1. <span data-ttu-id="59da9-123">I hello Hudson instrumentpanelen, klickar du på **hantera Hudson**.</span><span class="sxs-lookup"><span data-stu-id="59da9-123">In hello Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="59da9-124">Klicka på **konfigurera systemet**.</span><span class="sxs-lookup"><span data-stu-id="59da9-124">Click **Configure System**.</span></span>
3. <span data-ttu-id="59da9-125">Bläddra nedåt hello sidan toofind hello **moln** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="59da9-125">Scroll down hello page toofind hello **Cloud** section.</span></span>
4. <span data-ttu-id="59da9-126">Klicka på **lägga till nya molntjänster > Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="59da9-126">Click **Add new cloud > Microsoft Azure**.</span></span>
   
    ![lägga till nya molntjänster][add new cloud]
   
    <span data-ttu-id="59da9-128">Detta visar hello fält där du behöver tooenter din prenumerationsinformation.</span><span class="sxs-lookup"><span data-stu-id="59da9-128">This will show hello fields where you need tooenter your subscription details.</span></span>
   
    ![Konfigurera profilen][configure profile]
5. <span data-ttu-id="59da9-130">Kopiera hello prenumerations-id och hantering av certifikat från profilen prenumeration och klistra in dem i hello relevanta fält.</span><span class="sxs-lookup"><span data-stu-id="59da9-130">Copy hello subscription id and management certificate from your subscription profile and paste them in hello appropriate fields.</span></span>
   
    <span data-ttu-id="59da9-131">När du kopierar hello prenumerations-id och hantering av certifikat, **inte** citattecken hello som omsluter hello värden.</span><span class="sxs-lookup"><span data-stu-id="59da9-131">When copying hello subscription id and management certificate, **do not** include hello quotes that enclose hello values.</span></span>
6. <span data-ttu-id="59da9-132">Klicka på **verifiera konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="59da9-132">Click on **Verify configuration**.</span></span>
7. <span data-ttu-id="59da9-133">När du har verifierats hello konfiguration, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="59da9-133">When hello configuration is verified successfully, click **Save**.</span></span>

## <a name="set-up-a-virtual-machine-template-for-hello-azure-slave-plug-in"></a><span data-ttu-id="59da9-134">Ställ in en mall för virtuell dator för hello Azure slavserver plugin-program</span><span class="sxs-lookup"><span data-stu-id="59da9-134">Set up a virtual machine template for hello Azure Slave plug-in</span></span>
<span data-ttu-id="59da9-135">En mall för virtuell dator definierar hello parametrar hello plugin-programmet använder toocreate en slav nod på Azure.</span><span class="sxs-lookup"><span data-stu-id="59da9-135">A virtual machine template defines hello parameters hello plug-in will use toocreate a slave node on Azure.</span></span> <span data-ttu-id="59da9-136">I följande steg hello ska vi skapa mall för en Ubuntu VM.</span><span class="sxs-lookup"><span data-stu-id="59da9-136">In hello following steps we'll be creating template for an Ubuntu VM.</span></span>

1. <span data-ttu-id="59da9-137">I hello Hudson instrumentpanelen, klickar du på **hantera Hudson**.</span><span class="sxs-lookup"><span data-stu-id="59da9-137">In hello Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="59da9-138">Klicka på **konfigurera systemet**.</span><span class="sxs-lookup"><span data-stu-id="59da9-138">Click on **Configure System**.</span></span>
3. <span data-ttu-id="59da9-139">Bläddra nedåt hello sidan toofind hello **moln** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="59da9-139">Scroll down hello page toofind hello **Cloud** section.</span></span>
4. <span data-ttu-id="59da9-140">Inom hello **moln** avsnittet, hitta **Lägg till mall för virtuell dator i Azure** och klicka på hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="59da9-140">Within hello **Cloud** section, find **Add Azure Virtual Machine Template** and click hello **Add** button.</span></span>
   
    ![Lägg till mall för virtuell dator][add vm template]
5. <span data-ttu-id="59da9-142">Ange ett tjänstnamn för molnet i hello **namn** fältet.</span><span class="sxs-lookup"><span data-stu-id="59da9-142">Specify a cloud service name in hello **Name** field.</span></span> <span data-ttu-id="59da9-143">Om hello namn du refererar tooan befintlig molntjänst, kommer att tillhandahållas hello VM i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59da9-143">If hello name you specify refers tooan existing cloud service, hello VM will be provisioned in that service.</span></span> <span data-ttu-id="59da9-144">Annars skapar Azure en ny.</span><span class="sxs-lookup"><span data-stu-id="59da9-144">Otherwise, Azure will create a new one.</span></span>
6. <span data-ttu-id="59da9-145">I hello **beskrivning** , ange text som beskriver hello-mall som du skapar.</span><span class="sxs-lookup"><span data-stu-id="59da9-145">In hello **Description** field, enter text that describes hello template you are creating.</span></span> <span data-ttu-id="59da9-146">Den här informationen är endast för dokumentation och används inte för att etablera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="59da9-146">This information is only for documentary purposes and is not used in provisioning a VM.</span></span>
7. <span data-ttu-id="59da9-147">I hello **etiketter** anger **linux**.</span><span class="sxs-lookup"><span data-stu-id="59da9-147">In hello **Labels** field, enter **linux**.</span></span> <span data-ttu-id="59da9-148">Den här etiketten är används tooidentify hello mall som du skapar och därefter används tooreference hello mallen när du skapar ett jobb för Hudson.</span><span class="sxs-lookup"><span data-stu-id="59da9-148">This label is used tooidentify hello template you are creating and is subsequently used tooreference hello template when creating a Hudson job.</span></span>
8. <span data-ttu-id="59da9-149">Välj en region där hello VM kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="59da9-149">Select a region where hello VM will be created.</span></span>
9. <span data-ttu-id="59da9-150">Välj hello lämpliga VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="59da9-150">Select hello appropriate VM size.</span></span>
10. <span data-ttu-id="59da9-151">Ange ett lagringskonto där hello VM kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="59da9-151">Specify a storage account where hello VM will be created.</span></span> <span data-ttu-id="59da9-152">Se till att den är i hello samma region som hello molntjänst som du ska använda.</span><span class="sxs-lookup"><span data-stu-id="59da9-152">Make sure that it is in hello same region as hello cloud service you'll be using.</span></span> <span data-ttu-id="59da9-153">Om du vill att den nya lagring toobe skapas kan du lämna fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="59da9-153">If you want new storage toobe created, you can leave this field blank.</span></span>
11. <span data-ttu-id="59da9-154">Tiden för datakvarhållning anger hello antalet minuter innan Hudson tar bort en inaktiv slavserver.</span><span class="sxs-lookup"><span data-stu-id="59da9-154">Retention time specifies hello number of minutes before Hudson deletes an idle slave.</span></span> <span data-ttu-id="59da9-155">Lämna detta på hello standardvärdet 60.</span><span class="sxs-lookup"><span data-stu-id="59da9-155">Leave this at hello default value of 60.</span></span>
12. <span data-ttu-id="59da9-156">I **användning**, Välj hello lämpliga villkor när den här sekundära noden kommer att användas.</span><span class="sxs-lookup"><span data-stu-id="59da9-156">In **Usage**, select hello appropriate condition when this slave node will be used.</span></span> <span data-ttu-id="59da9-157">Nu väljer **använda den här noden så mycket som möjligt**.</span><span class="sxs-lookup"><span data-stu-id="59da9-157">For now, select **Utilize this node as much as possible**.</span></span>
    
     <span data-ttu-id="59da9-158">Formuläret skulle nu se något liknande toothis:</span><span class="sxs-lookup"><span data-stu-id="59da9-158">At this point, your form would look somewhat similar toothis:</span></span>
    
     ![Mallkonfigurationen][template config]
13. <span data-ttu-id="59da9-160">I **Id eller bild-familjen** har toospecify vilka systemavbildning kommer att installeras på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="59da9-160">In **Image Family or Id** you have toospecify what system image will be installed on your VM.</span></span> <span data-ttu-id="59da9-161">Du kan välja från en lista över avbildningen familjer, eller så kan du ange en anpassad avbildning.</span><span class="sxs-lookup"><span data-stu-id="59da9-161">You can either select from a list of image families or specify a custom image.</span></span>
    
     <span data-ttu-id="59da9-162">Om du vill tooselect från en lista över avbildningen familjer, ange hello första tecknet (skiftlägeskänsligt) hello avbildningsnamn.</span><span class="sxs-lookup"><span data-stu-id="59da9-162">If you want tooselect from a list of image families, enter hello first character (case-sensitive) of hello image family name.</span></span> <span data-ttu-id="59da9-163">Till exempel skriva **U** visas en lista över Ubuntu Server familjer.</span><span class="sxs-lookup"><span data-stu-id="59da9-163">For instance, typing **U** will bring up a list of Ubuntu Server families.</span></span> <span data-ttu-id="59da9-164">När du har valt hello listan använder Jenkins hello senaste versionen av systemavbildningen från den familjen vid etablering av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="59da9-164">Once you select from hello list, Jenkins will use hello latest version of that system image from that family when provisioning your VM.</span></span>
    
     ![OS-familjen lista][OS family list]
    
     <span data-ttu-id="59da9-166">Om du har en anpassad avbildning som du vill toouse i stället ange hello namnet på den egna avbildningen.</span><span class="sxs-lookup"><span data-stu-id="59da9-166">If you have a custom image that you want toouse instead, enter hello name of that custom image.</span></span> <span data-ttu-id="59da9-167">Anpassad avbildningsnamn visas inte i en lista så att du har tooensure som hello namn har angetts korrekt.</span><span class="sxs-lookup"><span data-stu-id="59da9-167">Custom image names are not shown in a list so you have tooensure that hello name is entered correctly.</span></span>    
    
     <span data-ttu-id="59da9-168">Den här kursen skriver **U** toobring visas en lista över Ubuntu avbildningar och väljer **Ubuntu Server 14.04 LTS**.</span><span class="sxs-lookup"><span data-stu-id="59da9-168">For this tutorial, type **U** toobring up a list of Ubuntu images and select **Ubuntu Server 14.04 LTS**.</span></span>
14. <span data-ttu-id="59da9-169">För **starta metoden**väljer **SSH**.</span><span class="sxs-lookup"><span data-stu-id="59da9-169">For **Launch method**, select **SSH**.</span></span>
15. <span data-ttu-id="59da9-170">Kopiera hello skriptet nedan och klistra in i hello **Init skriptet** fältet.</span><span class="sxs-lookup"><span data-stu-id="59da9-170">Copy hello script below and paste in hello **Init script** field.</span></span>
    
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
    
     <span data-ttu-id="59da9-171">Hej **Init skriptet** ska köras efter hello virtuell dator skapas.</span><span class="sxs-lookup"><span data-stu-id="59da9-171">hello **Init script** will be executed after hello VM is created.</span></span> <span data-ttu-id="59da9-172">I det här exemplet installerar Java, git och ant hello skript.</span><span class="sxs-lookup"><span data-stu-id="59da9-172">In this example, hello script installs Java, git, and ant.</span></span>
16. <span data-ttu-id="59da9-173">I hello **användarnamn** och **lösenord** fält, ange din önskade värden för hello administratörskontot som kommer att skapas på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="59da9-173">In hello **Username** and **Password** fields, enter your preferred values for hello administrator account that will be created on your VM.</span></span>
17. <span data-ttu-id="59da9-174">Klicka på **Kontrollera mallen** toocheck om hello-parametrar som du angav är giltiga.</span><span class="sxs-lookup"><span data-stu-id="59da9-174">Click on **Verify Template** toocheck if hello parameters you specified are valid.</span></span>
18. <span data-ttu-id="59da9-175">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="59da9-175">Click on **Save**.</span></span>

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a><span data-ttu-id="59da9-176">Skapa ett Hudson jobb som körs på en slav nod på Azure</span><span class="sxs-lookup"><span data-stu-id="59da9-176">Create a Hudson job that runs on a slave node on Azure</span></span>
<span data-ttu-id="59da9-177">I det här avsnittet ska du skapa en Hudson aktivitet som körs på en slav nod på Azure.</span><span class="sxs-lookup"><span data-stu-id="59da9-177">In this section, you'll be creating a Hudson task that will run on a slave node on Azure.</span></span>

1. <span data-ttu-id="59da9-178">I hello Hudson instrumentpanelen, klickar du på **nytt jobb**.</span><span class="sxs-lookup"><span data-stu-id="59da9-178">In hello Hudson dashboard, click **New Job**.</span></span>
2. <span data-ttu-id="59da9-179">Ange ett namn för hello jobb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="59da9-179">Enter a name for hello job you are creating.</span></span>
3. <span data-ttu-id="59da9-180">Hello jobbtypen Välj **bygga ett ledigt format programvara jobb**.</span><span class="sxs-lookup"><span data-stu-id="59da9-180">For hello job type, select **Build a free-style software job**.</span></span>
4. <span data-ttu-id="59da9-181">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="59da9-181">Click **OK**.</span></span>
5. <span data-ttu-id="59da9-182">Hello jobbet konfiguration på sidan Välj **begränsa där du kan köra det här projektet**.</span><span class="sxs-lookup"><span data-stu-id="59da9-182">In hello job configuration page, select **Restrict where this project can be run**.</span></span>
6. <span data-ttu-id="59da9-183">Välj **nod och etikett menyn** och välj **linux** (vi angett den här etiketten när du skapar hello mall för virtuell dator i hello föregående avsnitt).</span><span class="sxs-lookup"><span data-stu-id="59da9-183">Select **Node and label menu** and select **linux** (we specified this label when creating hello virtual machine template in hello previous section).</span></span>
7. <span data-ttu-id="59da9-184">I hello **skapa** klickar du på **Lägg till build steg** och välj **köra shell**.</span><span class="sxs-lookup"><span data-stu-id="59da9-184">In hello **Build** section, click **Add build step** and select **Execute shell**.</span></span>
8. <span data-ttu-id="59da9-185">Redigera hello följande skript, ersätter **{github kontonamn}**, **{ditt projektnamn}**, och **{projektkatalogen}** med lämpliga värden och klistra in hello Redigera skriptet i område för hello text som visas.</span><span class="sxs-lookup"><span data-stu-id="59da9-185">Edit hello following script, replacing **{your github account name}**, **{your project name}**, and **{your project directory}** with appropriate values, and paste hello edited script in hello text area that appears.</span></span>
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory tooproject
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. <span data-ttu-id="59da9-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="59da9-186">Click on **Save**.</span></span>
10. <span data-ttu-id="59da9-187">I hello Hudson instrumentpanelen, hitta hello jobb som du just har skapat och klicka på hello **schemalägga en build** ikon.</span><span class="sxs-lookup"><span data-stu-id="59da9-187">In hello Hudson dashboard, find hello job you just created and click on hello **Schedule a build** icon.</span></span>

<span data-ttu-id="59da9-188">Hudson sedan skapa en slav nod med hjälp av hello mall hello föregående avsnitt och köra hello-skript som du angav i hello build steg för den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="59da9-188">Hudson will then create a slave node using hello template created in hello previous section and execute hello script you specified in hello build step for this task.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59da9-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59da9-189">Next Steps</span></span>
<span data-ttu-id="59da9-190">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="59da9-190">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[prenumeration profil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

