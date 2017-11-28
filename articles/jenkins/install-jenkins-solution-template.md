---
title: "aaaCreate en Jenkins server på Azure"
description: "Installera Jenkins på en virtuell dator i Azure Linux från hello Jenkins lösningsmall och skapa ett exempelprogram för Java."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 82ab2ac52594acba131414b449b608978591d4b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-hello-azure-portal"></a><span data-ttu-id="dedf7-103">Skapa en Jenkins-server på en Azure Linux-VM från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dedf7-103">Create a Jenkins server on an Azure Linux VM from hello Azure portal</span></span>

<span data-ttu-id="dedf7-104">Den här snabbstarten visar hur tooinstall [Jenkins](https://jenkins.io) på en Ubuntu Linux VM hello verktyg och toowork plugin-program som har konfigurerats med Azure.</span><span class="sxs-lookup"><span data-stu-id="dedf7-104">This quickstart shows how tooinstall [Jenkins](https://jenkins.io) on an Ubuntu Linux VM with hello tools and plug-ins configured toowork with Azure.</span></span> <span data-ttu-id="dedf7-105">När du är klar körs en Jenkins-server i Azure och skapar en Java-exempelapp från [GitHub](https://github.com).</span><span class="sxs-lookup"><span data-stu-id="dedf7-105">When you're finished, you have a Jenkins server running in Azure building a sample Java app from [GitHub](https://github.com).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dedf7-106">Krav</span><span class="sxs-lookup"><span data-stu-id="dedf7-106">Prerequisites</span></span>

* <span data-ttu-id="dedf7-107">En Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dedf7-107">An Azure subscription</span></span>
* <span data-ttu-id="dedf7-108">Åtkomst tooSSH på kommandoraden för din dator (till exempel hello Bash shell eller [PuTTY](http://www.putty.org/))</span><span class="sxs-lookup"><span data-stu-id="dedf7-108">Access tooSSH on your computer's command line (such as hello Bash shell or [PuTTY](http://www.putty.org/))</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-jenkins-vm-from-hello-solution-template"></a><span data-ttu-id="dedf7-109">Skapa hello Jenkins VM från hello lösningsmall</span><span class="sxs-lookup"><span data-stu-id="dedf7-109">Create hello Jenkins VM from hello solution template</span></span>

<span data-ttu-id="dedf7-110">Öppna hello [marketplace-avbildning för Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) i webbläsaren och välj **hämta IT nu** från hello vänster sida av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="dedf7-110">Open hello [marketplace image for Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) in your web browser and select  **GET IT NOW** from hello left-hand side of hello page.</span></span> <span data-ttu-id="dedf7-111">Granska hello priser information och välj **Fortsätt**och välj **skapa** tooconfigure hello Jenkins server i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dedf7-111">Review hello pricing details and select **Continue**, then select **Create** tooconfigure hello Jenkins server in hello Azure portal.</span></span> 
   
![Dialogruta i Azure Portal](./media/install-jenkins-solution-template/ap-create.png)

<span data-ttu-id="dedf7-113">I hello **Konfigurera grundläggande inställningar** fliken, Fyll i hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="dedf7-113">In hello **Configure basic settings** tab, fill in hello following fields:</span></span>

![Konfigurera grundläggande inställningar](./media/install-jenkins-solution-template/ap-basic.png)

* <span data-ttu-id="dedf7-115">Använd **Jenkins** som **namn**.</span><span class="sxs-lookup"><span data-stu-id="dedf7-115">Use **Jenkins** for **Name**.</span></span>
* <span data-ttu-id="dedf7-116">Ange ett **användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="dedf7-116">Enter a **User name**.</span></span> <span data-ttu-id="dedf7-117">hello användarnamn måste uppfylla [specifika krav](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="dedf7-117">hello user name must meet [specific requirements](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).</span></span>
* <span data-ttu-id="dedf7-118">Välj **lösenord** som hello **autentiseringstyp** och ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="dedf7-118">Select **Password** as hello **Authentication type** and enter a password.</span></span> <span data-ttu-id="dedf7-119">hello lösenordet måste innehålla en versal bokstav, en siffra och ett specialtecken.</span><span class="sxs-lookup"><span data-stu-id="dedf7-119">hello password must have an upper case character, a number, and one special character.</span></span>
* <span data-ttu-id="dedf7-120">Använd **myJenkinsResourceGroup** för hello **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="dedf7-120">Use **myJenkinsResourceGroup** for hello **Resource Group**.</span></span>
* <span data-ttu-id="dedf7-121">Välj hello **östra USA** [Azure-region](https://azure.microsoft.com/regions/) från hello **plats** listrutan.</span><span class="sxs-lookup"><span data-stu-id="dedf7-121">Choose hello **East US** [Azure region](https://azure.microsoft.com/regions/) from hello **Location** drop-down.</span></span>

<span data-ttu-id="dedf7-122">Välj **OK** tooproceed toohello **konfigurera ytterligare alternativ** fliken. Ange ett unikt namn tooidentify hello Jenkins domänserver och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="dedf7-122">Select **OK** tooproceed toohello **Configure additional options** tab. Enter a unique domain name tooidentify hello Jenkins server and select **OK**.</span></span>

![Ställ in ytterligare alternativ](./media/install-jenkins-solution-template/ap-addtional.png)  

 <span data-ttu-id="dedf7-124">När valideringen lyckas, välja **OK** igen från hello **sammanfattning** fliken. Välj slutligen **inköp** toocreate hello Jenkins VM.</span><span class="sxs-lookup"><span data-stu-id="dedf7-124">Once validation passes, select **OK** again from hello **Summary** tab. Finally, select **Purchase** toocreate hello Jenkins VM.</span></span> <span data-ttu-id="dedf7-125">När servern är klar kan få du ett meddelande i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="dedf7-125">When your server is ready, you get a notification in hello Azure portal:</span></span>   

![Meddelande om att Jenkins är klar](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-toojenkins"></a><span data-ttu-id="dedf7-127">Ansluta tooJenkins</span><span class="sxs-lookup"><span data-stu-id="dedf7-127">Connect tooJenkins</span></span>

<span data-ttu-id="dedf7-128">Navigera tooyour virtuella datorn (till exempel http://jenkins2517454.eastus.cloudapp.azure.com/) i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="dedf7-128">Navigate tooyour virtual machine (for example, http://jenkins2517454.eastus.cloudapp.azure.com/) in  your web browser.</span></span> <span data-ttu-id="dedf7-129">Hej Jenkins konsolen inte är tillgänglig via osäker HTTP så anvisningar finns på hello sidan tooaccess hello Jenkins konsolen på ett säkert sätt från datorn med hjälp av en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="dedf7-129">hello Jenkins console is inaccessible through unsecured HTTP so instructions are provided on hello page tooaccess hello Jenkins console securely from your computer using an SSH tunnel.</span></span>

![Låsa upp Jenkins](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

<span data-ttu-id="dedf7-131">Ställ in hello tunneln med hello `ssh` på hello sida från kommandoraden hello ersätter `username` med hello namnet hello virtuella administratörsanvändare valt tidigare när du ställer in hello virtuell dator från hello lösningsmall.</span><span class="sxs-lookup"><span data-stu-id="dedf7-131">Set up hello tunnel using hello `ssh` command on hello page from hello command line, replacing `username` with hello name of hello virtual machine admin user chosen earlier when setting up hello virtual machine from hello solution template.</span></span>

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

<span data-ttu-id="dedf7-132">När du har startat hello tunnel, navigera toohttp://localhost:8080 / på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="dedf7-132">After you have started hello tunnel, navigate toohttp://localhost:8080/ on your local machine.</span></span> 

<span data-ttu-id="dedf7-133">Hämta hello initialt lösenord genom att köra följande kommando på kommandoraden hello medan du är ansluten via SSH toohello Jenkins VM hello.</span><span class="sxs-lookup"><span data-stu-id="dedf7-133">Get hello initial password by running hello following command in hello command line while connected through SSH toohello Jenkins VM.</span></span>

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

<span data-ttu-id="dedf7-134">Låsa upp hello Jenkins instrumentpanel för hello första gången du använder den här initialt lösenord.</span><span class="sxs-lookup"><span data-stu-id="dedf7-134">Unlock hello Jenkins dashboard for hello first time using this initial password.</span></span>

![Låsa upp Jenkins](./media/install-jenkins-solution-template/jenkins-unlock.png)

<span data-ttu-id="dedf7-136">Välj **installera plugin-program för föreslagna** på hello nästa sida och sedan skapa en Jenkins admin används tooaccess hello Jenkins instrumentpanel för användare.</span><span class="sxs-lookup"><span data-stu-id="dedf7-136">Select **Install suggested plugins** on hello next page and then create a Jenkins admin user used tooaccess hello Jenkins dashboard.</span></span>

![Jenkins är redo!](./media/install-jenkins-solution-template/jenkins-welcome.png)

<span data-ttu-id="dedf7-138">Hej Jenkins servern är nu redo toobuild kod.</span><span class="sxs-lookup"><span data-stu-id="dedf7-138">hello Jenkins server is now ready toobuild code.</span></span>

## <a name="create-your-first-job"></a><span data-ttu-id="dedf7-139">Skapa ditt första virtuella jobb</span><span class="sxs-lookup"><span data-stu-id="dedf7-139">Create your first job</span></span>

<span data-ttu-id="dedf7-140">Välj **skapa nya jobb** hello Jenkins konsolen namnge den **mySampleApp** och välj **Freestyle projektet**och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="dedf7-140">Select **Create new jobs** from hello Jenkins console, then name it **mySampleApp** and select **Freestyle project**, then select **OK**.</span></span>

![Skapa ett nytt jobb](./media/install-jenkins-solution-template/jenkins-new-job.png) 

<span data-ttu-id="dedf7-142">Välj hello **källa kod Management** fliken, aktivera **Git**, och ange följande URL i hello **databasen URL** fält:`https://github.com/spring-guides/gs-spring-boot.git`</span><span class="sxs-lookup"><span data-stu-id="dedf7-142">Select hello **Source Code Management** tab, enable **Git**, and enter hello following URL in **Repository URL**  field: `https://github.com/spring-guides/gs-spring-boot.git`</span></span>

![Definiera hello Git repo](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

<span data-ttu-id="dedf7-144">Välj hello **skapa** fliken och markera sedan **Lägg till build steg**, **anropa Gradle skriptet**.</span><span class="sxs-lookup"><span data-stu-id="dedf7-144">Select hello **Build** tab, then select **Add build step**, **Invoke Gradle script**.</span></span> <span data-ttu-id="dedf7-145">Välj **Use Gradle Wrapper** (Använd Gradle-omslutning) och ange `complete` i **Wrapper location** (Omslutningsplats) och `build` för **Tasks** (Uppgifter).</span><span class="sxs-lookup"><span data-stu-id="dedf7-145">Select **Use Gradle Wrapper**, then enter `complete` in **Wrapper location** and `build` for **Tasks**.</span></span>

![Använd hello Gradle wrapper toobuild](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

<span data-ttu-id="dedf7-147">Välj **Avancerat..**</span><span class="sxs-lookup"><span data-stu-id="dedf7-147">Select **Advanced..**</span></span> <span data-ttu-id="dedf7-148">och ange sedan `complete` i hello **rot skapa skript** fältet.</span><span class="sxs-lookup"><span data-stu-id="dedf7-148">and then enter `complete` in hello **Root Build script** field.</span></span> <span data-ttu-id="dedf7-149">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dedf7-149">Select **Save**.</span></span>

![Ange avancerade inställningar i hello Gradle wrapper build steg](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-hello-code"></a><span data-ttu-id="dedf7-151">Skapa hello kod</span><span class="sxs-lookup"><span data-stu-id="dedf7-151">Build hello code</span></span>

<span data-ttu-id="dedf7-152">Välj **skapa nu** toocompile hello kod och paketet hello sample-appen.</span><span class="sxs-lookup"><span data-stu-id="dedf7-152">Select **Build Now** toocompile hello code and package hello sample app.</span></span> <span data-ttu-id="dedf7-153">När din build är klar väljer du hello **arbetsytan** länk för hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="dedf7-153">When your build completes, select hello **Workspace** link for hello project.</span></span>

![Bläddra toohello arbetsytan tooget hello JAR-filen från hello build](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

<span data-ttu-id="dedf7-155">Navigera för`complete/build/libs` och kontrollera hello `gs-spring-boot-0.1.0.jar` finns det tooverify att din build lyckades.</span><span class="sxs-lookup"><span data-stu-id="dedf7-155">Navigate too`complete/build/libs` and ensure hello `gs-spring-boot-0.1.0.jar` is there tooverify that your build was successful.</span></span> <span data-ttu-id="dedf7-156">Din Jenkins servern är nu redo toobuild projekt i Azure.</span><span class="sxs-lookup"><span data-stu-id="dedf7-156">Your Jenkins server is now ready toobuild your own projects in Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dedf7-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dedf7-157">Next Steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dedf7-158">Lägga till virtuella Azure-datorer som Jenkins-agenter</span><span class="sxs-lookup"><span data-stu-id="dedf7-158">Add Azure VMs as Jenkins agents</span></span>](jenkins-azure-vm-agents.md)
