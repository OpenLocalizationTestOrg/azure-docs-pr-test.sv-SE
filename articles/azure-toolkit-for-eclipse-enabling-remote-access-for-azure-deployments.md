---
title: "Aktivera fjärråtkomst för Azure-distributioner i Eclipse"
description: "Lär dig mer om att aktivera fjärråtkomst för Azure med Azure-verktygen för Eclipse-distributioner."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: b6150006-9a7f-4d83-be18-d35ec780c7c5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 654d511bd5a62341f87569317e97360c94a6f26c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a><span data-ttu-id="b42af-103">Aktivera fjärråtkomst för Azure-distributioner i Eclipse</span><span class="sxs-lookup"><span data-stu-id="b42af-103">Enabling Remote Access for Azure Deployments in Eclipse</span></span>
<span data-ttu-id="b42af-104">För att felsöka dina distributioner, kan du aktivera och använda fjärråtkomst för att ansluta till den virtuella datorn som är värd för din distribution.</span><span class="sxs-lookup"><span data-stu-id="b42af-104">To help troubleshoot your deployments, you may enable and use Remote Access to connect to the virtual machine hosting your deployment.</span></span> <span data-ttu-id="b42af-105">Funktioner i fjärråtkomst förlitar sig på Remote Desktop Protocol (RDP).</span><span class="sxs-lookup"><span data-stu-id="b42af-105">The Remote Access functionality relies on the Remote Desktop Protocol (RDP).</span></span> <span data-ttu-id="b42af-106">Du kan konfigurera fjärråtkomst för din distribution när du har publicerat den till Azure eller om du använder Eclipse med ett Windows-operativsystem, kan du konfigurera fjärråtkomst innan du publicerar till Azure.</span><span class="sxs-lookup"><span data-stu-id="b42af-106">You can configure Remote Access for your deployment after you have published it to Azure, or if you are using Eclipse with a Windows operating system, you can configure Remote Access before you publish to Azure.</span></span> <span data-ttu-id="b42af-107">Observera att du behöver en fjärrskrivbordsklient som är kompatibel med operativsystemet för att ansluta till din distribution virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="b42af-107">Note that you will need a remote desktop client that is compatible with your operating system in order to connect to your deployment's virtual machine in Azure.</span></span>

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a><span data-ttu-id="b42af-108">Så här aktiverar du fjärråtkomst innan du distribuerar till Azure</span><span class="sxs-lookup"><span data-stu-id="b42af-108">How to enable Remote Access before you deploy to Azure</span></span>
> [!NOTE]
> <span data-ttu-id="b42af-109">Om du vill aktivera fjärråtkomst innan du distribuerar programmet till Azure måste du köra Eclipse i Windows.</span><span class="sxs-lookup"><span data-stu-id="b42af-109">To enable Remote Access before you deploy your application to Azure, you need to be running Eclipse on Windows.</span></span>
> 
> 

<span data-ttu-id="b42af-110">Följande bild visar den **fjärråtkomst** egenskapsdialogrutan som används för att aktivera fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="b42af-110">The following image shows the **Remote Access** properties dialog used to enable remote access.</span></span>

![][ic719494]

<span data-ttu-id="b42af-111">Det finns två sätt att visa den **fjärråtkomst** egenskapsdialogrutan:</span><span class="sxs-lookup"><span data-stu-id="b42af-111">There are two ways to display the **Remote Access** properties dialog:</span></span>

* <span data-ttu-id="b42af-112">Klicka på den **Avancerat** länken i den **fjärråtkomst** avsnitt i den **publicera till Azure** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b42af-112">Click the **Advanced** link in the **Remote Access** section of the **Publish to Azure** dialog.</span></span>

* <span data-ttu-id="b42af-113">Öppna den **egenskaper** dialogrutan för din Azure-projekt.</span><span class="sxs-lookup"><span data-stu-id="b42af-113">Open the **Properties** dialog of your Azure project.</span></span>

<span data-ttu-id="b42af-114">När du skapar ett nytt projekt i Azure-distribution projektet inte fjärråtkomst som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="b42af-114">When you create a new Azure deployment project, the project will not have Remote Access enabled by default.</span></span> <span data-ttu-id="b42af-115">Du kan dock enkelt aktivera fjärråtkomst genom att ange användarnamn och lösenord i den **publicera till Azure** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b42af-115">However, you can easily enable remote access by specifying the user name and password in the **Publish to Azure** dialog.</span></span> <span data-ttu-id="b42af-116">Fjärråtkomst-lösenord har krypterats med X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="b42af-116">The Remote Access password is encrypted using X.509 certificates.</span></span> <span data-ttu-id="b42af-117">Ange ditt eget certifikat, om du inte använder kryptering är beroende av ett självsignerat certifikat som medföljer Azure-Plugin för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="b42af-117">If you do not use provide your own certificate, the encryption relies on a self-signed certificate shipped with the Azure Plugin for Eclipse.</span></span> <span data-ttu-id="b42af-118">Detta självsignerade certifikat finns i den **cert** mappen i projektet Azure lagras både som en offentlig certifikatfil (SampleRemoteAccessPublic.cer) och som en Personal Information Exchange (PFX) certifikat filen ( SampleRemoteAccessPrivate.pfx).</span><span class="sxs-lookup"><span data-stu-id="b42af-118">This self-signed certificate is in the **cert** folder of your Azure project, stored both as a public certificate file (SampleRemoteAccessPublic.cer) and as a Personal Information Exchange (PFX) certificate file (SampleRemoteAccessPrivate.pfx).</span></span> <span data-ttu-id="b42af-119">Denna innehåller den privata nyckeln för certifikatet och den har en standardlösenordet **Password1**.</span><span class="sxs-lookup"><span data-stu-id="b42af-119">The latter contains the private key for the certificate, and it has a default password, **Password1**.</span></span> <span data-ttu-id="b42af-120">Men eftersom lösenordet är allmänt kända, bör standardcertifikatet användas endast för andra syften inte för en Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="b42af-120">However, since this password is public knowledge, the default certificate should be used only for learning purposes, not for a production deployment.</span></span> <span data-ttu-id="b42af-121">Så än för andra syften, när du vill aktivera fjärrsessioner för dina distributioner klickar du på **Avancerat** länken i den **publicera till Azure** kan ange ett eget certifikat.</span><span class="sxs-lookup"><span data-stu-id="b42af-121">So other than for learning purposes, when you want to enabled remote sessions for your deployments, you should click the **Advanced** link in the **Publish to Azure** dialog to specify your own certificate.</span></span> <span data-ttu-id="b42af-122">Observera att du måste ladda upp PFX-versionen av certifikatet till din värdbaserade tjänst i Azure-hanteringsportalen, så att Azure kan dekryptera användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="b42af-122">Note that you'll need to upload the PFX version of the certificate to your hosted service within the Azure Management Portal, so that Azure can decrypt the user password.</span></span>

<span data-ttu-id="b42af-123">Resten av kursen visar hur du aktiverar fjärråtkomst för ett projekt för Azure-distribution som ursprungligen har skapats med fjärråtkomst har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="b42af-123">The remainder of the tutorial shows you how to enable remote access for an Azure deployment project that was initially created with remote access disabled.</span></span> <span data-ttu-id="b42af-124">För tillämpning av den här självstudiekursen skapar vi ett nytt självsignerat certifikat och dess .pfx-filen har ett valfritt lösenord i.</span><span class="sxs-lookup"><span data-stu-id="b42af-124">For purposes of this tutorial, we'll create a new self-signed certificate, and its .pfx file will have a password of your choice.</span></span> <span data-ttu-id="b42af-125">Du har också möjlighet att använda ett certifikat utfärdat av en certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="b42af-125">You also have the option of using a certificate issued by a certificate authority.</span></span>

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a><span data-ttu-id="b42af-126">Så här aktiverar du fjärråtkomst när du har distribuerat till Azure</span><span class="sxs-lookup"><span data-stu-id="b42af-126">How to enable Remote Access after you have deployed to Azure</span></span>
<span data-ttu-id="b42af-127">Använd följande steg för att aktivera fjärråtkomst när du har distribuerat till Azure:</span><span class="sxs-lookup"><span data-stu-id="b42af-127">To enable remote access after you have deployed to Azure, use the following steps:</span></span>

1. <span data-ttu-id="b42af-128">Logga in på Azure-hanteringsportalen med ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="b42af-128">Log into the Azure management portal using your Azure account</span></span>

2. <span data-ttu-id="b42af-129">I listan över **molntjänster**, Välj din distribuerade molntjänst</span><span class="sxs-lookup"><span data-stu-id="b42af-129">In your list of **Cloud Services**, select your deployed cloud service</span></span>

3. <span data-ttu-id="b42af-130">I cloud service webbsidan, klickar du på den **konfigurera** länk</span><span class="sxs-lookup"><span data-stu-id="b42af-130">In the cloud service web page, click the **Configure** link</span></span>

4. <span data-ttu-id="b42af-131">Klicka på längst ned på sidan för den **Remote** länk</span><span class="sxs-lookup"><span data-stu-id="b42af-131">On the bottom of the configuration page, click the **Remote** link</span></span>

5. <span data-ttu-id="b42af-132">När popup-dialogrutan visas:</span><span class="sxs-lookup"><span data-stu-id="b42af-132">When the pop-up dialog box appears:</span></span>
   
   * <span data-ttu-id="b42af-133">Ange den roll som du vill aktivera fjärråtkomst</span><span class="sxs-lookup"><span data-stu-id="b42af-133">Specify the Role you for which you want to enable remote access</span></span>

   * <span data-ttu-id="b42af-134">Markera den **aktivera Fjärrskrivbord** kryssruta</span><span class="sxs-lookup"><span data-stu-id="b42af-134">Click to select the **Enable Remote Desktop** checkbox</span></span>
   
   * <span data-ttu-id="b42af-135">Ange ett användarnamn och lösenord som du vill använda för fjärråtkomst</span><span class="sxs-lookup"><span data-stu-id="b42af-135">Specify a user name and password you want to use for remote access</span></span>
   
   * <span data-ttu-id="b42af-136">Välj certifikatet som ska användas</span><span class="sxs-lookup"><span data-stu-id="b42af-136">Select the certificate to use</span></span>

6. <span data-ttu-id="b42af-137">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="b42af-137">Click **OK**</span></span> 

<span data-ttu-id="b42af-138">Du ser ett meddelande om att din konfigurationsändring pågår, vilket kan ta några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="b42af-138">You will see a message stating that your configuration change is in progress, which may take a few minutes to complete.</span></span> <span data-ttu-id="b42af-139">När konfigurationsändringen har slutförts, följer du stegen i den **att logga in via fjärranslutning** senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b42af-139">After the configuration change has completed, follow the steps in the **To log in remotely** section later in this article.</span></span>

## <a name="how-to-enable-remote-access-in-your-package"></a><span data-ttu-id="b42af-140">Så här aktiverar du fjärråtkomst i paketet</span><span class="sxs-lookup"><span data-stu-id="b42af-140">How to enable Remote Access in your package</span></span>
1. <span data-ttu-id="b42af-141">Högerklicka på Azure-projekt i Eclipses Projektutforskaren ruta och klickar på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="b42af-141">Within Eclipse's Project Explorer pane, right-click your Azure project and click **Properties**.</span></span>

2. <span data-ttu-id="b42af-142">I den **egenskaper** dialogrutan Expandera **Azure** i den vänstra rutan och klicka på **fjärråtkomst**.</span><span class="sxs-lookup"><span data-stu-id="b42af-142">In the **Properties** dialog, expand **Azure** in the left-hand pane and click **Remote Access**.</span></span>

3. <span data-ttu-id="b42af-143">I den **fjärråtkomst** dialogrutan Kontrollera **aktivera alla roller att acceptera anslutningar till fjärrskrivbord med dessa inloggningsuppgifterna** är markerad.</span><span class="sxs-lookup"><span data-stu-id="b42af-143">In the **Remote Access** dialog, ensure **Enable all roles to accept Remote Desktop Connections with these login credentials** is checked.</span></span>

4. <span data-ttu-id="b42af-144">Ange ett användarnamn för anslutning till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="b42af-144">Specify a user name for the Remote Desktop connection.</span></span>

5. <span data-ttu-id="b42af-145">Ange och bekräfta lösenordet för användaren.</span><span class="sxs-lookup"><span data-stu-id="b42af-145">Specify and confirm the password for the user.</span></span> <span data-ttu-id="b42af-146">Användare och lösenord värdena i den här dialogrutan används när du gör en fjärrskrivbordsanslutning.</span><span class="sxs-lookup"><span data-stu-id="b42af-146">The user name and password values set in this dialog will be used when you make a Remote Desktop connection.</span></span> <span data-ttu-id="b42af-147">(Observera att detta är ett särskilt lösenord PFX-lösenordet).</span><span class="sxs-lookup"><span data-stu-id="b42af-147">(Note that this is a separate password from your PFX password.)</span></span>

6. <span data-ttu-id="b42af-148">Ange ett sista giltighetsdatum för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="b42af-148">Specify the expiration date for the user account.</span></span>

7. <span data-ttu-id="b42af-149">Klicka på **ny** att skapa ett nytt självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="b42af-149">Click **New** to create a new self-signed certificate.</span></span> <span data-ttu-id="b42af-150">(Du kan också välja ett certifikat från din arbetsyta eller filsystemet via den **arbetsytan** eller **FileSystem** knappar, respektive, men för den här kursen ska du skapa en ny certifikat.)</span><span class="sxs-lookup"><span data-stu-id="b42af-150">(Alternatively, you could select a certificate from your workspace or file system through the **Workspace** or **FileSystem** buttons, respectively, but for purposes of this tutorial we'll create a new certificate.)</span></span>

   * <span data-ttu-id="b42af-151">I den **nytt certifikat** dialogrutan, ange och bekräfta lösenordet som du använder för PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="b42af-151">In the **New Certificate** dialog, specify and confirm the password you'll use for your PFX file.</span></span>

   * <span data-ttu-id="b42af-152">Acceptera värdet som angetts för **namn (CN)**, eller Använd ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="b42af-152">Accept the value provided for **Name (CN)**, or use a custom name.</span></span>

   * <span data-ttu-id="b42af-153">Ange sökvägen och filnamnet som där det nya certifikatet i CER formuläret sparas.</span><span class="sxs-lookup"><span data-stu-id="b42af-153">Specify the path and file name where the new certificate, in .cer form, will be saved.</span></span> <span data-ttu-id="b42af-154">Du kan använda för det här steget och nästa steg i **cert** mappen för din Azure-projekt, men du bara välja en annan plats.</span><span class="sxs-lookup"><span data-stu-id="b42af-154">For this step and the next step, you could use the **cert** folder of your Azure project, but you're free to choose another location.</span></span> <span data-ttu-id="b42af-155">För tillämpning av den här kursen använder vi **c:\mycert\mycert.cer**.</span><span class="sxs-lookup"><span data-stu-id="b42af-155">For purposes of this tutorial, we'll use **c:\mycert\mycert.cer**.</span></span> <span data-ttu-id="b42af-156">(Skapa den **c:\mycert** mapp innan du fortsätter eller Använd en befintlig mapp om du vill.)</span><span class="sxs-lookup"><span data-stu-id="b42af-156">(Create the **c:\mycert** folder prior to proceeding, or use an existing folder if desired.)</span></span>

   * <span data-ttu-id="b42af-157">Ange sökvägen och filnamnet som där det nya certifikatet och dess privata nyckel i .pfx formuläret ska sparas.</span><span class="sxs-lookup"><span data-stu-id="b42af-157">Specify the path and file name where the new certificate and its private key, in .pfx form, will be saved.</span></span> <span data-ttu-id="b42af-158">För tillämpning av den här kursen använder vi **c:\mycert\mycert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="b42af-158">For purposes of this tutorial, we'll use **c:\mycert\mycert.pfx**.</span></span> <span data-ttu-id="b42af-159">Din **nytt certifikat** dialogrutan bör likna följande (uppdatera mappsökvägarna om du inte använde **c:\mycert**):</span><span class="sxs-lookup"><span data-stu-id="b42af-159">Your **New Certificate** dialog should look similar to the following (update the folder paths if you did not use **c:\mycert**):</span></span>
     
      ![][ic712275]

   * <span data-ttu-id="b42af-160">Klicka på **OK** att stänga den **nytt certifikat** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b42af-160">Click **OK** to close the **New Certificate** dialog.</span></span>

8. <span data-ttu-id="b42af-161">Din **fjärråtkomst** dialogrutan bör likna följande:</span><span class="sxs-lookup"><span data-stu-id="b42af-161">Your **Remote Access** dialog should look similar to the following:</span></span></p>
   
   ![][ic719495]

9. <span data-ttu-id="b42af-162">Klicka på **OK** att stänga den **fjärråtkomst** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b42af-162">Click **OK** to close the **Remote Access** dialog.</span></span>

<span data-ttu-id="b42af-163">Återskapa ditt program med build för distribution till molnet.</span><span class="sxs-lookup"><span data-stu-id="b42af-163">Rebuild your application, with the build set for deployment to cloud.</span></span>

## <a name="to-log-in-remotely"></a><span data-ttu-id="b42af-164">Logga in via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="b42af-164">To log in remotely</span></span>
<span data-ttu-id="b42af-165">När din rollinstans är klar kan logga du via fjärranslutning in till den virtuella datorn som är värd för ditt program.</span><span class="sxs-lookup"><span data-stu-id="b42af-165">Once your role instance is ready, you can remotely log in to the virtual machine that is hosting your application.</span></span>

* <span data-ttu-id="b42af-166">Om du använder Eclipse i Windows och du har valt den **starta Fjärrskrivbord på distribuera** alternativet under distributionen till Azure visas med en anslutning till fjärrskrivbord inloggningsskärm när distributionen startar.</span><span class="sxs-lookup"><span data-stu-id="b42af-166">If are using Eclipse on Windows and you selected the **Start remote desktop on deploy** option during your deployment to Azure, you will be presented with a Remote Desktop Connection logon screen when your deployment starts.</span></span> <span data-ttu-id="b42af-167">När du tillfrågas om användarnamn och lösenord, anger du de värden som du angett för den fjärranslutna användaren och kommer att kunna logga in.</span><span class="sxs-lookup"><span data-stu-id="b42af-167">When you are prompted for the user name and password, enter the values that you specified for the remote user and will be able to log in.</span></span>

* <span data-ttu-id="b42af-168">Ett annat sätt att logga in via fjärranslutning på <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure-hanteringsportalen</a>:</span><span class="sxs-lookup"><span data-stu-id="b42af-168">Another way to log in remotely is through the <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure Management Portal</a>:</span></span>
  
  * <span data-ttu-id="b42af-169">I den **molntjänster** vy av Azure-hanteringsportalen, klicka på Molntjänsten, **instanser**, klicka på en specifik instans och klicka sedan på den **Anslut** knappen.</span><span class="sxs-lookup"><span data-stu-id="b42af-169">Within the **Cloud Services** view of the Azure Management portal, click your cloud service, click **Instances**, click a specific instance, and then click the **Connect** button.</span></span> <span data-ttu-id="b42af-170">Den **Anslut** knappen visas som i kommandofältet följande:</span><span class="sxs-lookup"><span data-stu-id="b42af-170">The **Connect** button appears as the following in the command bar:</span></span>
    
      ![][ic659273]

  * <span data-ttu-id="b42af-171">När du klickar på den **Anslut** knappen uppmanas du för att öppna en RDP-fil.</span><span class="sxs-lookup"><span data-stu-id="b42af-171">After clicking the **Connect** button, you will be prompted to open an RDP file.</span></span> <span data-ttu-id="b42af-172">Öppna filen och följ anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="b42af-172">Open the file and follow the prompts.</span></span> <span data-ttu-id="b42af-173">(Du kan också spara filen på den lokala datorn och kör filen genom att dubbelklicka på den till fjärranslutna logga in till den virtuella datorn utan att först gå hanteringsportalen.)</span><span class="sxs-lookup"><span data-stu-id="b42af-173">(You could also save this file to your local computer, and then run the file by double-clicking it to remote log in to your virtual machine without needing to first go the management portal.)</span></span>

  * <span data-ttu-id="b42af-174">När du tillfrågas om användarnamn och lösenord, anger du de värden som du angett för den fjärranslutna användaren och kommer att kunna logga in.</span><span class="sxs-lookup"><span data-stu-id="b42af-174">When you are prompted for the user name and password, enter the values that you specified for the remote user and will be able to log in.</span></span>

> [!NOTE]
> <span data-ttu-id="b42af-175">Om du är på ett icke-Windows-operativsystem måste du använda en fjärrskrivbordsklient som är kompatibel med operativsystemet och följ stegen för att konfigurera klienten med inställningarna i RDP-filen som du har hämtat.</span><span class="sxs-lookup"><span data-stu-id="b42af-175">If you are on a non-Windows operating system, you need to use a Remote Desktop client that is compatible with your operating system and follow the steps to configure that client with the settings in the RDP file that you downloaded.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="b42af-176">Se även</span><span class="sxs-lookup"><span data-stu-id="b42af-176">See Also</span></span>
<span data-ttu-id="b42af-177">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="b42af-177">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="b42af-178">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="b42af-178">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="b42af-179">[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="b42af-179">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="b42af-180">Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="b42af-180">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
