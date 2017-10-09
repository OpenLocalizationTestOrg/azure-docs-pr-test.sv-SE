---
title: "aaaRun Java-programservern på en klassisk Azure-VM | Microsoft Docs"
description: "Den här kursen använder resurser som har skapats med hello klassiska distributionsmodellen och visar hur toocreate Windows virtuell dator och konfigurera den toorun Apache Tomcat-programserver."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d627aa09-f7d6-4239-8110-f8fc5111b939
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 03/16/2017
ms.author: robmcm
ms.openlocfilehash: 2d9f586c9f628d3738522b320996b95b078d7454
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-java-application-server-on-a-virtual-machine-created-with-hello-classic-deployment-model"></a><span data-ttu-id="43ad3-103">Hur toorun Java-programservern på en virtuell dator skapas med hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="43ad3-103">How toorun a Java application server on a virtual machine created with hello classic deployment model</span></span>
> [!IMPORTANT]
> <span data-ttu-id="43ad3-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="43ad3-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="43ad3-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="43ad3-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="43ad3-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="43ad3-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="43ad3-107">En Resource Manager mallen toodeploy en webapp med Java 8 och Tomcat, se [här](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span><span class="sxs-lookup"><span data-stu-id="43ad3-107">For a Resource Manager template toodeploy a webapp with Java 8 and Tomcat, see [here](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span></span>

<span data-ttu-id="43ad3-108">Du kan använda en virtuell dator tooprovide serverfunktioner med Azure.</span><span class="sxs-lookup"><span data-stu-id="43ad3-108">With Azure, you can use a virtual machine tooprovide server capabilities.</span></span> <span data-ttu-id="43ad3-109">Exempelvis kan en virtuell dator som körs i Azure vara konfigurerade toohost Java-programservern, till exempel Apache Tomcat.</span><span class="sxs-lookup"><span data-stu-id="43ad3-109">As an example, a virtual machine running on Azure can be configured toohost a Java application server, such as Apache Tomcat.</span></span>

<span data-ttu-id="43ad3-110">När du har slutfört den här guiden kommer du ha en förståelse av hur toocreate en virtuell dator körs på Azure och konfigurera den toorun Java-programservern.</span><span class="sxs-lookup"><span data-stu-id="43ad3-110">After completing this guide, you will have an understanding of how toocreate a virtual machine running on Azure and configure it toorun a Java application server.</span></span> <span data-ttu-id="43ad3-111">Du lär dig och utföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="43ad3-111">You will learn and perform hello following tasks:</span></span>

* <span data-ttu-id="43ad3-112">Hur toocreate en virtuell dator som har en Java Development Kit (JDK) redan har installerats.</span><span class="sxs-lookup"><span data-stu-id="43ad3-112">How toocreate a virtual machine that has a Java Development Kit (JDK) already installed.</span></span>
* <span data-ttu-id="43ad3-113">Hur tooremotely inloggningen tooyour virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="43ad3-113">How tooremotely sign in tooyour virtual machine.</span></span>
* <span data-ttu-id="43ad3-114">Hur tooinstall Java-programservern--Apache Tomcat--på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="43ad3-114">How tooinstall a Java application server--Apache Tomcat--on your virtual machine.</span></span>
* <span data-ttu-id="43ad3-115">Hur toocreate en slutpunkt för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="43ad3-115">How toocreate an endpoint for your virtual machine.</span></span>
* <span data-ttu-id="43ad3-116">Hur tooopen en hello-port i brandväggen för programservern.</span><span class="sxs-lookup"><span data-stu-id="43ad3-116">How tooopen a port in hello firewall for your application server.</span></span>

<span data-ttu-id="43ad3-117">hello slutföra Installationsresultat i Tomcat som körs på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="43ad3-117">hello completed installation results in Tomcat running on a virtual machine.</span></span>

![Virtuell dator som kör Apache Tomcat][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a><span data-ttu-id="43ad3-119">toocreate en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="43ad3-119">toocreate a virtual machine</span></span>
1. <span data-ttu-id="43ad3-120">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="43ad3-120">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="43ad3-121">Klicka på **ny**, klickar du på **Compute**, klicka på **se alla** i hello **aktuella appar**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-121">Click **New**, click **Compute**, then click **See all** in hello **Featured apps**.</span></span>
3. <span data-ttu-id="43ad3-122">Klicka på **JDK**, klickar du på **JDK 8** i hello **JDK** fönstret.</span><span class="sxs-lookup"><span data-stu-id="43ad3-122">Click **JDK**, click **JDK 8** in hello **JDK** pane.</span></span>  
   <span data-ttu-id="43ad3-123">Virtuella bilder som stöder **JDK 6** och **JDK 7** är tillgängliga om du har äldre program som inte är redo toorun i JDK 8.</span><span class="sxs-lookup"><span data-stu-id="43ad3-123">Virtual machine images that support **JDK 6** and **JDK 7** are available if you have legacy applications that are not ready toorun in JDK 8.</span></span>
4. <span data-ttu-id="43ad3-124">Välj i rutan hello JDK 8 **klassiska**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-124">In hello JDK 8 pane, select **Classic**, then click **Create**.</span></span>
5. <span data-ttu-id="43ad3-125">I hello **grunderna** bladet:</span><span class="sxs-lookup"><span data-stu-id="43ad3-125">In hello **Basics** blade:</span></span>
   1. <span data-ttu-id="43ad3-126">Ange ett namn för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="43ad3-126">Specify a name for hello virtual machine.</span></span>
   2. <span data-ttu-id="43ad3-127">Ange ett namn för Hej administratör i hello **användarnamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="43ad3-127">Enter a name for hello administrator in hello **User Name** field.</span></span> <span data-ttu-id="43ad3-128">Kom ihåg detta namn och hello tillhörande lösenord som följer i hello nästa fält.</span><span class="sxs-lookup"><span data-stu-id="43ad3-128">Remember this name and hello associated password that follows in hello next field.</span></span> <span data-ttu-id="43ad3-129">Du behöver dem när du loggar in via fjärranslutning toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="43ad3-129">You need them when you remotely sign in toohello virtual machine.</span></span>
   3. <span data-ttu-id="43ad3-130">Ange ett lösenord i hello **nytt lösenord** fältet och igen i hello **Bekräfta lösenord** fältet.</span><span class="sxs-lookup"><span data-stu-id="43ad3-130">Enter a password in hello **New password** field, and reenter it in hello **Confirm password** field.</span></span> <span data-ttu-id="43ad3-131">Lösenordet är för hello administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="43ad3-131">This password is for hello Administrator account.</span></span>
   4. <span data-ttu-id="43ad3-132">Välj lämplig hello **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-132">Select hello appropriate **Subscription**.</span></span>
   5. <span data-ttu-id="43ad3-133">För hello **resursgruppen**, klickar du på **Skapa nytt** och ange hello namnet på en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="43ad3-133">For hello **Resource group**, click **Create new** and enter hello name of a new resource group.</span></span> <span data-ttu-id="43ad3-134">Klicka på **använda befintliga** och välj en av hello tillgängliga resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="43ad3-134">Or, click **Use existing** and select one of hello available resource groups.</span></span>
   6. <span data-ttu-id="43ad3-135">Välj en plats där hello virtuella dator finns, som **södra centrala USA**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-135">Select a location where hello virtual machine resides, such as **South Central US**.</span></span>
6. <span data-ttu-id="43ad3-136">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-136">Click **Next**.</span></span>
7. <span data-ttu-id="43ad3-137">I hello **virtuella bildstorleken** bladet väljer **A1 Standard** eller en annan lämplig bild.</span><span class="sxs-lookup"><span data-stu-id="43ad3-137">In hello **Virtual machine image size** blade, select **A1 Standard** or another appropriate image.</span></span>
8. <span data-ttu-id="43ad3-138">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-138">Click **Select**.</span></span>

9. <span data-ttu-id="43ad3-139">I hello **inställningar** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-139">In hello **Settings** blade, click **OK**.</span></span> <span data-ttu-id="43ad3-140">Du kan använda hello standardvärden som tillhandahålls av Azure.</span><span class="sxs-lookup"><span data-stu-id="43ad3-140">You can use hello default values provided by Azure.</span></span>  
10. <span data-ttu-id="43ad3-141">I hello **sammanfattning** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-141">In hello **Summary** blade, click **OK**.</span></span>

## <a name="tooremotely-sign-in-tooyour-virtual-machine"></a><span data-ttu-id="43ad3-142">tooremotely inloggning tooyour virtuell dator</span><span class="sxs-lookup"><span data-stu-id="43ad3-142">tooremotely sign in tooyour virtual machine</span></span>
1. <span data-ttu-id="43ad3-143">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="43ad3-143">Log on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="43ad3-144">Klicka på **virtuella datorer (klassisk)**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-144">Click **Virtual machines (classic)**.</span></span> <span data-ttu-id="43ad3-145">Om det behövs klickar du på **fler tjänster** i hello nedre vänstra hörnet under hello servicekategorier.</span><span class="sxs-lookup"><span data-stu-id="43ad3-145">If needed, click **More services** at hello bottom left corner under hello service categories.</span></span> <span data-ttu-id="43ad3-146">Hej **virtuella datorer (klassisk)** posten visas i hello **Compute** grupp.</span><span class="sxs-lookup"><span data-stu-id="43ad3-146">hello **Virtual machines (classic)** entry is listed in hello **Compute** group.</span></span>
3. <span data-ttu-id="43ad3-147">Klicka på hello virtuell dator som du vill toosign i hello namn.</span><span class="sxs-lookup"><span data-stu-id="43ad3-147">Click hello name of hello virtual machine that you want toosign in to.</span></span>
4. <span data-ttu-id="43ad3-148">När hello virtuella datorn har startat tillåter en meny hello överst i fönstret hello anslutningar.</span><span class="sxs-lookup"><span data-stu-id="43ad3-148">After hello virtual machine has started, a menu at hello top of hello pane allows connections.</span></span>
5. <span data-ttu-id="43ad3-149">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-149">Click **Connect**.</span></span>
6. <span data-ttu-id="43ad3-150">Svara toohello prompter som behövs tooconnect toohello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="43ad3-150">Respond toohello prompts as needed tooconnect toohello virtual machine.</span></span> <span data-ttu-id="43ad3-151">Normalt spara eller öppna hello RDP-fil som innehåller hello anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="43ad3-151">Typically, you save or open hello .rdp file that contains hello connection details.</span></span> <span data-ttu-id="43ad3-152">Du kan ha toocopy hello URL-adress: port som hello sista delen av hello första raden i hello RDP-filen och klistra in den i inloggning fjärrprogram.</span><span class="sxs-lookup"><span data-stu-id="43ad3-152">You might have toocopy hello url:port as hello last part of hello first line of hello .rdp file and paste it in a remote sign-in application.</span></span>

## <a name="tooinstall-a-java-application-server-on-your-virtual-machine"></a><span data-ttu-id="43ad3-153">tooinstall Java-programservern på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="43ad3-153">tooinstall a Java application server on your virtual machine</span></span>
<span data-ttu-id="43ad3-154">Du kan kopiera en Java application server tooyour virtuell dator eller installera en Java-programservern via ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="43ad3-154">You can copy a Java application server tooyour virtual machine, or you can install a Java application server through an installer.</span></span>

<span data-ttu-id="43ad3-155">Den här kursen använder Tomcat som hello Java application server tooinstall.</span><span class="sxs-lookup"><span data-stu-id="43ad3-155">This tutorial uses Tomcat as hello Java application server tooinstall.</span></span>

1. <span data-ttu-id="43ad3-156">När du är inloggad tooyour virtuella datorn, öppna en webbläsarsession för[Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span><span class="sxs-lookup"><span data-stu-id="43ad3-156">When you are signed in tooyour virtual machine, open a browser session too[Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span></span>
2. <span data-ttu-id="43ad3-157">Dubbelklicka på hello länk för **32-bitars/64-bitars Windows installationsprogram**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-157">Double-click hello link for **32-bit/64-bit Windows Service Installer**.</span></span> <span data-ttu-id="43ad3-158">Med den här tekniken kan installeras Tomcat som en Windows-tjänst.</span><span class="sxs-lookup"><span data-stu-id="43ad3-158">By using this technique, Tomcat installs as a Windows service.</span></span>
3. <span data-ttu-id="43ad3-159">Välj toorun hello installer.</span><span class="sxs-lookup"><span data-stu-id="43ad3-159">When prompted, choose toorun hello installer.</span></span>
4. <span data-ttu-id="43ad3-160">Inom hello **Apache Tomcat installationsprogrammet** , följ hello uppmanas tooinstall Tomcat.</span><span class="sxs-lookup"><span data-stu-id="43ad3-160">Within hello **Apache Tomcat Setup** wizard, follow hello prompts tooinstall Tomcat.</span></span> <span data-ttu-id="43ad3-161">Hello enligt den här kursen räcker det bra att acceptera hello standard.</span><span class="sxs-lookup"><span data-stu-id="43ad3-161">For hello purposes of this tutorial, accepting hello defaults is fine.</span></span> <span data-ttu-id="43ad3-162">När du når hello **hello Slutför installationsguiden för Apache Tomcat** dialogrutan kan du om du vill kontrollera **köra Apache Tomcat** toohave Tomcat start nu.</span><span class="sxs-lookup"><span data-stu-id="43ad3-162">When you reach hello **Completing hello Apache Tomcat Setup Wizard** dialog box, you can optionally check **Run Apache Tomcat** toohave Tomcat start now.</span></span> <span data-ttu-id="43ad3-163">Klicka på **Slutför** toocomplete hello Tomcat installationsprocessen.</span><span class="sxs-lookup"><span data-stu-id="43ad3-163">Click **Finish** toocomplete hello Tomcat setup process.</span></span>

## <a name="toostart-tomcat"></a><span data-ttu-id="43ad3-164">toostart Tomcat</span><span class="sxs-lookup"><span data-stu-id="43ad3-164">toostart Tomcat</span></span>

<span data-ttu-id="43ad3-165">Du kan starta Tomcat manuellt genom att öppna en kommandotolk på den virtuella datorn körs hello-kommandot **net&nbsp;starta&nbsp;Tomcat8**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-165">You can manually start Tomcat by opening a command prompt on your virtual machine and running hello command **net&nbsp;start&nbsp;Tomcat8**.</span></span>

<span data-ttu-id="43ad3-166">När Tomcat körs kan du komma åt Tomcat genom att ange hello URL <http://localhost: 8080> i hello virtuell dators webbläsare.</span><span class="sxs-lookup"><span data-stu-id="43ad3-166">Once Tomcat is running, you can access Tomcat by entering hello URL <http://localhost:8080> in hello virtual machine's browser.</span></span>

<span data-ttu-id="43ad3-167">toosee Tomcat körs från externa datorer kan du behöver toocreate en slutpunkt och öppnar en port.</span><span class="sxs-lookup"><span data-stu-id="43ad3-167">toosee Tomcat running from external machines, you need toocreate an endpoint and open a port.</span></span>

## <a name="toocreate-an-endpoint-for-your-virtual-machine"></a><span data-ttu-id="43ad3-168">toocreate en slutpunkt för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="43ad3-168">toocreate an endpoint for your virtual machine</span></span>
1. <span data-ttu-id="43ad3-169">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="43ad3-169">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="43ad3-170">Klicka på **virtuella datorer (klassisk)**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-170">Click **Virtual machines (classic)**.</span></span>
3. <span data-ttu-id="43ad3-171">Klicka på hello virtuell dator som körs på Java-programservern hello namn.</span><span class="sxs-lookup"><span data-stu-id="43ad3-171">Click hello name of hello virtual machine that is running your Java application server.</span></span>
4. <span data-ttu-id="43ad3-172">Klicka på **Slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-172">Click **Endpoints**.</span></span>
5. <span data-ttu-id="43ad3-173">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-173">Click **Add**.</span></span>
6. <span data-ttu-id="43ad3-174">I hello **lägga till slutpunkten** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="43ad3-174">In hello **Add endpoint** dialog box:</span></span>
   1. <span data-ttu-id="43ad3-175">Ange ett namn för slutpunkten hello; till exempel **HttpIn**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-175">Specify a name for hello endpoint; for example, **HttpIn**.</span></span>
   2. <span data-ttu-id="43ad3-176">Välj **TCP** för hello-protokollet.</span><span class="sxs-lookup"><span data-stu-id="43ad3-176">Select **TCP** for hello protocol.</span></span>
   3. <span data-ttu-id="43ad3-177">Ange **80** för hello offentlig port.</span><span class="sxs-lookup"><span data-stu-id="43ad3-177">Specify **80** for hello public port.</span></span>
   4. <span data-ttu-id="43ad3-178">Ange **8080** för hello privat port.</span><span class="sxs-lookup"><span data-stu-id="43ad3-178">Specify **8080** for hello private port.</span></span>
   5. <span data-ttu-id="43ad3-179">Välj **inaktiverad** för hello flytande IP-adress.</span><span class="sxs-lookup"><span data-stu-id="43ad3-179">Select **Disabled** for hello floating IP address.</span></span>
   6. <span data-ttu-id="43ad3-180">Lämna hello åtkomstkontrollistan är.</span><span class="sxs-lookup"><span data-stu-id="43ad3-180">Leave hello access control list as is.</span></span>
   7. <span data-ttu-id="43ad3-181">Klicka på hello **OK** knappen tooclose hello dialogrutan och skapa hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="43ad3-181">Click hello **OK** button tooclose hello dialog box and create hello endpoint.</span></span>

## <a name="tooopen-a-port-in-hello-firewall-for-your-virtual-machine"></a><span data-ttu-id="43ad3-182">tooopen en port i brandväggen hello för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="43ad3-182">tooopen a port in hello firewall for your virtual machine</span></span>
1. <span data-ttu-id="43ad3-183">Logga in tooyour virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="43ad3-183">Sign in tooyour virtual machine.</span></span>
2. <span data-ttu-id="43ad3-184">Klicka på **starta Windows**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-184">Click **Windows Start**.</span></span>
3. <span data-ttu-id="43ad3-185">Klicka på **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-185">Click **Control Panel**.</span></span>
4. <span data-ttu-id="43ad3-186">Klicka på **System och säkerhet**, klickar du på **Windows-brandväggen**, och klicka sedan på **avancerade inställningar**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-186">Click **System and Security**, click **Windows Firewall**, and then click **Advanced Settings**.</span></span>
5. <span data-ttu-id="43ad3-187">Klicka på **regler för inkommande trafik**, och klicka sedan på **ny regel**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-187">Click **Inbound Rules**, and then click **New Rule**.</span></span>  
   <span data-ttu-id="43ad3-188">![Ny inkommande regel][NewIBRule]</span><span class="sxs-lookup"><span data-stu-id="43ad3-188">![New inbound rule][NewIBRule]</span></span>
6. <span data-ttu-id="43ad3-189">För hello **regeltyp**väljer **Port**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-189">For hello **Rule Type**, select **Port**, and then click **Next**.</span></span>  
   <span data-ttu-id="43ad3-190">![Ny inkommande regel port][NewRulePort]</span><span class="sxs-lookup"><span data-stu-id="43ad3-190">![New inbound rule port][NewRulePort]</span></span>
7. <span data-ttu-id="43ad3-191">På hello **protokoll och portar** väljer **TCP**, ange **8080** som hello **specifika lokala portar**, och klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-191">On hello **Protocol and Ports** screen, select **TCP**, specify **8080** as hello **Specific local port**, and then click **Next**.</span></span>  
  <span data-ttu-id="43ad3-192">![Ny inkommande regel][NewRuleProtocol]</span><span class="sxs-lookup"><span data-stu-id="43ad3-192">![New inbound rule ][NewRuleProtocol]</span></span>
8. <span data-ttu-id="43ad3-193">På hello **åtgärd** väljer **Tillåt hello anslutning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-193">On hello **Action** screen, select **Allow hello connection**, and then click **Next**.</span></span>
   <span data-ttu-id="43ad3-194">![Ny inkommande Regelåtgärd][NewRuleAction]</span><span class="sxs-lookup"><span data-stu-id="43ad3-194">![New inbound rule action][NewRuleAction]</span></span>
9. <span data-ttu-id="43ad3-195">På hello **profil** kontrollerar du att **domän**, **privata**, och **offentliga** är markerade och klickar sedan på **nästa** .</span><span class="sxs-lookup"><span data-stu-id="43ad3-195">On hello **Profile** screen, ensure that **Domain**, **Private**, and **Public** are selected, and then click **Next**.</span></span>
   <span data-ttu-id="43ad3-196">![Ny inkommande regel profil][NewRuleProfile]</span><span class="sxs-lookup"><span data-stu-id="43ad3-196">![New inbound rule profile][NewRuleProfile]</span></span>
10. <span data-ttu-id="43ad3-197">På hello **namn** skärmen, ange ett namn för hello regel som **HttpIn** (hello Regelnamnet är inte obligatoriska toomatch hello slutpunktsnamn, men), och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-197">On hello **Name** screen, specify a name for hello rule, such as **HttpIn** (hello rule name is not required toomatch hello endpoint name, however), and then click **Finish**.</span></span>  
    <span data-ttu-id="43ad3-198">![Namn på ny inkommande regel][NewRuleName]</span><span class="sxs-lookup"><span data-stu-id="43ad3-198">![New inbound rule name][NewRuleName]</span></span>

<span data-ttu-id="43ad3-199">Tomcat webbplatsen ska nu visas på en extern webbläsare.</span><span class="sxs-lookup"><span data-stu-id="43ad3-199">At this point, your Tomcat website should be viewable from an external browser.</span></span> <span data-ttu-id="43ad3-200">Ange en URL i formatet hello i hello webbläsarens adressfönstret  **http://*din\_DNS\_namn*. cloudapp.net**, där ***din\_DNS\_namn*** är hello DNS-namn du angav när du skapade hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="43ad3-200">In hello browser's address window, type a URL of hello form **http://*your\_DNS\_name*.cloudapp.net**, where ***your\_DNS\_name*** is hello DNS name you specified when you created hello virtual machine.</span></span>

## <a name="application-lifecycle-considerations"></a><span data-ttu-id="43ad3-201">Programmet livscykel överväganden</span><span class="sxs-lookup"><span data-stu-id="43ad3-201">Application lifecycle considerations</span></span>
* <span data-ttu-id="43ad3-202">Du kan skapa egna exempelwebbappens Arkiv (WAR) och lägga till den toohello **webbappar** mapp.</span><span class="sxs-lookup"><span data-stu-id="43ad3-202">You could create your own web application archive (WAR) and add it toohello **webapps** folder.</span></span> <span data-ttu-id="43ad3-203">Till exempel skapa ett grundläggande Service (Java JSP sidan) dynamiskt webbprojekt och exportera det som en WAR-fil.</span><span class="sxs-lookup"><span data-stu-id="43ad3-203">For example, create a basic Java Service Page (JSP) dynamic web project and export it as a WAR file.</span></span> <span data-ttu-id="43ad3-204">Kopiera hello WAR toohello Apache Tomcat **webbappar** mapp på hello virtuell dator, kör det i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="43ad3-204">Next, copy hello WAR toohello Apache Tomcat **webapps** folder on hello virtual machine, then run it in a browser.</span></span>
* <span data-ttu-id="43ad3-205">Som standard när hello Tomcat-tjänsten är installerad, är den inställd toostart manuellt.</span><span class="sxs-lookup"><span data-stu-id="43ad3-205">By default when hello Tomcat service is installed, it is set toostart manually.</span></span> <span data-ttu-id="43ad3-206">Du kan växla den toostart automatiskt med hjälp av hello snapin-modulen tjänster.</span><span class="sxs-lookup"><span data-stu-id="43ad3-206">You can switch it toostart automatically by using hello Services snap-in.</span></span> <span data-ttu-id="43ad3-207">Starta hello snapin-modulen Tjänster genom att klicka på **Windows starta**, **Administrationsverktyg**, och sedan **Services**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-207">Start hello Services snap-in by clicking **Windows Start**, **Administrative Tools**, and then **Services**.</span></span> <span data-ttu-id="43ad3-208">Dubbelklicka på hello **Apache Tomcat** och ange **starttyp** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="43ad3-208">Double-click hello **Apache Tomcat** service and set **Startup type** too**Automatic**.</span></span>

    ![Ange en service toostart automatiskt][service_automatic_startup]

    <span data-ttu-id="43ad3-210">hello är fördelen med Tomcat startar automatiskt att den börjar köras när hello virtuella datorn startas (till exempel efter programuppdateringar som kräver en omstart har installerats).</span><span class="sxs-lookup"><span data-stu-id="43ad3-210">hello benefit of having Tomcat start automatically is that it starts running when hello virtual machine is rebooted (for example, after software updates that require a reboot are installed).</span></span>

## <a name="next-steps"></a><span data-ttu-id="43ad3-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43ad3-211">Next steps</span></span>
<span data-ttu-id="43ad3-212">Du kan lära dig om andra tjänster (t.ex Azure Storage, service bus och SQL-databas) som du vill tooinclude med Java-program.</span><span class="sxs-lookup"><span data-stu-id="43ad3-212">You can learn about other services (such as Azure Storage, service bus, and SQL Database) that you may want tooinclude with your Java applications.</span></span> <span data-ttu-id="43ad3-213">Visa information om hello tillgänglig på hello [Java-Utvecklingscenter](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="43ad3-213">View hello information available at hello [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from hello "toocreate an ednpoint for your virtual mache" 3/17/2017,
     toouse hello new portal.
6. In hello **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In hello **New endpoint details** dialog box:
-->
