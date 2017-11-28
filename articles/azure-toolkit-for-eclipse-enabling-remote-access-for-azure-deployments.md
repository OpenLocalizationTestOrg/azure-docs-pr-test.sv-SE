---
title: "aaaEnabling fjärråtkomst för Azure-distributioner i Eclipse"
description: "Lär dig hur tooenable fjärråtkomst för Azure-distributioner med hello Azure Toolkit för Eclipse."
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
ms.openlocfilehash: 00c2bf22c1f3ec792098f154f771c87506e87881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a><span data-ttu-id="55f6b-103">Aktivera fjärråtkomst för Azure-distributioner i Eclipse</span><span class="sxs-lookup"><span data-stu-id="55f6b-103">Enabling Remote Access for Azure Deployments in Eclipse</span></span>
<span data-ttu-id="55f6b-104">toohelp felsöka dina distributioner, du kan aktivera och använda Remote Access tooconnect toohello virtuell dator som värd för din distribution.</span><span class="sxs-lookup"><span data-stu-id="55f6b-104">toohelp troubleshoot your deployments, you may enable and use Remote Access tooconnect toohello virtual machine hosting your deployment.</span></span> <span data-ttu-id="55f6b-105">hello funktioner för fjärråtkomst är beroende av hello Remote Desktop Protocol (RDP).</span><span class="sxs-lookup"><span data-stu-id="55f6b-105">hello Remote Access functionality relies on hello Remote Desktop Protocol (RDP).</span></span> <span data-ttu-id="55f6b-106">Du kan konfigurera fjärråtkomst för din distribution när du har publicerat den tooAzure eller om du använder Eclipse med ett Windows-operativsystem, kan du konfigurera fjärråtkomst innan du publicerar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="55f6b-106">You can configure Remote Access for your deployment after you have published it tooAzure, or if you are using Eclipse with a Windows operating system, you can configure Remote Access before you publish tooAzure.</span></span> <span data-ttu-id="55f6b-107">Observera att du behöver en fjärrskrivbordsklient som är kompatibel med operativsystemet i ordning tooconnect tooyour distributionens virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="55f6b-107">Note that you will need a remote desktop client that is compatible with your operating system in order tooconnect tooyour deployment's virtual machine in Azure.</span></span>

## <a name="how-tooenable-remote-access-before-you-deploy-tooazure"></a><span data-ttu-id="55f6b-108">Hur tooenable fjärråtkomst innan du distribuerar tooAzure</span><span class="sxs-lookup"><span data-stu-id="55f6b-108">How tooenable Remote Access before you deploy tooAzure</span></span>
> [!NOTE]
> <span data-ttu-id="55f6b-109">tooenable fjärråtkomst innan du distribuerar ditt program tooAzure måste toobe kör Eclipse i Windows.</span><span class="sxs-lookup"><span data-stu-id="55f6b-109">tooenable Remote Access before you deploy your application tooAzure, you need toobe running Eclipse on Windows.</span></span>
> 
> 

<span data-ttu-id="55f6b-110">hello följande bild visar hello **fjärråtkomst** egenskapsdialogrutan används tooenable fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="55f6b-110">hello following image shows hello **Remote Access** properties dialog used tooenable remote access.</span></span>

![][ic719494]

<span data-ttu-id="55f6b-111">Det finns två sätt toodisplay hello **fjärråtkomst** egenskapsdialogrutan:</span><span class="sxs-lookup"><span data-stu-id="55f6b-111">There are two ways toodisplay hello **Remote Access** properties dialog:</span></span>

* <span data-ttu-id="55f6b-112">Klicka på hello **Avancerat** länken i hello **fjärråtkomst** avsnitt i hello **publicera tooAzure** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="55f6b-112">Click hello **Advanced** link in hello **Remote Access** section of hello **Publish tooAzure** dialog.</span></span>

* <span data-ttu-id="55f6b-113">Öppna hello **egenskaper** dialogrutan för din Azure-projekt.</span><span class="sxs-lookup"><span data-stu-id="55f6b-113">Open hello **Properties** dialog of your Azure project.</span></span>

<span data-ttu-id="55f6b-114">När du skapar ett nytt projekt i Azure-distribution hello projektet inte fjärråtkomst som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="55f6b-114">When you create a new Azure deployment project, hello project will not have Remote Access enabled by default.</span></span> <span data-ttu-id="55f6b-115">Du kan dock enkelt aktivera fjärråtkomst genom att ange hello användarnamn och lösenord i hello **publicera tooAzure** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="55f6b-115">However, you can easily enable remote access by specifying hello user name and password in hello **Publish tooAzure** dialog.</span></span> <span data-ttu-id="55f6b-116">hello fjärråtkomst lösenord har krypterats med X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="55f6b-116">hello Remote Access password is encrypted using X.509 certificates.</span></span> <span data-ttu-id="55f6b-117">Ange ditt eget certifikat, om du inte använder hello kryptering är beroende av ett självsignerat certifikat som medföljer hello Azure plugin-program för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="55f6b-117">If you do not use provide your own certificate, hello encryption relies on a self-signed certificate shipped with hello Azure Plugin for Eclipse.</span></span> <span data-ttu-id="55f6b-118">Detta självsignerade certifikat finns i hello **cert** mappen i projektet Azure lagras både som en offentlig certifikatfil (SampleRemoteAccessPublic.cer) och som en Personal Information Exchange (PFX) certifikat filen ( SampleRemoteAccessPrivate.pfx).</span><span class="sxs-lookup"><span data-stu-id="55f6b-118">This self-signed certificate is in hello **cert** folder of your Azure project, stored both as a public certificate file (SampleRemoteAccessPublic.cer) and as a Personal Information Exchange (PFX) certificate file (SampleRemoteAccessPrivate.pfx).</span></span> <span data-ttu-id="55f6b-119">hello senare innehåller hello privata nyckeln för hello certifikatet och den har en standardlösenordet **Password1**.</span><span class="sxs-lookup"><span data-stu-id="55f6b-119">hello latter contains hello private key for hello certificate, and it has a default password, **Password1**.</span></span> <span data-ttu-id="55f6b-120">Men eftersom detta lösenord är allmänt kända, bör hello standardcertifikatet användas endast för andra syften inte för en Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="55f6b-120">However, since this password is public knowledge, hello default certificate should be used only for learning purposes, not for a production deployment.</span></span> <span data-ttu-id="55f6b-121">Så än för andra syften, när du vill tooenabled fjärrsessioner för dina distributioner klickar du hello **Avancerat** länken i hello **publicera tooAzure** dialogrutan toospecify egna certifikat.</span><span class="sxs-lookup"><span data-stu-id="55f6b-121">So other than for learning purposes, when you want tooenabled remote sessions for your deployments, you should click hello **Advanced** link in hello **Publish tooAzure** dialog toospecify your own certificate.</span></span> <span data-ttu-id="55f6b-122">Observera att du behöver tooupload hello PFX-versionen av hello certifikat tooyour värdtjänsten inom hello Azure-hanteringsportalen, så att Azure kan dekryptera hello användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="55f6b-122">Note that you'll need tooupload hello PFX version of hello certificate tooyour hosted service within hello Azure Management Portal, so that Azure can decrypt hello user password.</span></span>

<span data-ttu-id="55f6b-123">hello resten av hello kursen visar hur tooenable fjärråtkomst för ett projekt för Azure-distribution som ursprungligen har skapats med fjärråtkomst har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="55f6b-123">hello remainder of hello tutorial shows you how tooenable remote access for an Azure deployment project that was initially created with remote access disabled.</span></span> <span data-ttu-id="55f6b-124">För tillämpning av den här självstudiekursen skapar vi ett nytt självsignerat certifikat och dess .pfx-filen har ett valfritt lösenord i.</span><span class="sxs-lookup"><span data-stu-id="55f6b-124">For purposes of this tutorial, we'll create a new self-signed certificate, and its .pfx file will have a password of your choice.</span></span> <span data-ttu-id="55f6b-125">Du kan också ha hello möjlighet att använda ett certifikat utfärdat av en certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="55f6b-125">You also have hello option of using a certificate issued by a certificate authority.</span></span>

## <a name="how-tooenable-remote-access-after-you-have-deployed-tooazure"></a><span data-ttu-id="55f6b-126">Hur tooenable fjärråtkomst när du har distribuerat tooAzure</span><span class="sxs-lookup"><span data-stu-id="55f6b-126">How tooenable Remote Access after you have deployed tooAzure</span></span>
<span data-ttu-id="55f6b-127">tooenable fjärråtkomst när du har distribuerat tooAzure, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="55f6b-127">tooenable remote access after you have deployed tooAzure, use hello following steps:</span></span>

1. <span data-ttu-id="55f6b-128">Logga in på hello Azure-hanteringsportalen med ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="55f6b-128">Log into hello Azure management portal using your Azure account</span></span>

2. <span data-ttu-id="55f6b-129">I listan över **molntjänster**, Välj din distribuerade molntjänst</span><span class="sxs-lookup"><span data-stu-id="55f6b-129">In your list of **Cloud Services**, select your deployed cloud service</span></span>

3. <span data-ttu-id="55f6b-130">Klicka på webbsidan hello cloud service, hello **konfigurera** länk</span><span class="sxs-lookup"><span data-stu-id="55f6b-130">In hello cloud service web page, click hello **Configure** link</span></span>

4. <span data-ttu-id="55f6b-131">Klicka på hello längst ned på sidan för konfiguration av hello hello **Remote** länk</span><span class="sxs-lookup"><span data-stu-id="55f6b-131">On hello bottom of hello configuration page, click hello **Remote** link</span></span>

5. <span data-ttu-id="55f6b-132">När hello popup-dialogrutan visas:</span><span class="sxs-lookup"><span data-stu-id="55f6b-132">When hello pop-up dialog box appears:</span></span>
   
   * <span data-ttu-id="55f6b-133">Ange hello roll du som du vill tooenable fjärråtkomst</span><span class="sxs-lookup"><span data-stu-id="55f6b-133">Specify hello Role you for which you want tooenable remote access</span></span>

   * <span data-ttu-id="55f6b-134">Klicka på tooselect hello **aktivera Fjärrskrivbord** kryssruta</span><span class="sxs-lookup"><span data-stu-id="55f6b-134">Click tooselect hello **Enable Remote Desktop** checkbox</span></span>
   
   * <span data-ttu-id="55f6b-135">Ange ett användarnamn och lösenord som du vill använda toouse för fjärråtkomst</span><span class="sxs-lookup"><span data-stu-id="55f6b-135">Specify a user name and password you want toouse for remote access</span></span>
   
   * <span data-ttu-id="55f6b-136">Välj hello certifikat toouse</span><span class="sxs-lookup"><span data-stu-id="55f6b-136">Select hello certificate toouse</span></span>

6. <span data-ttu-id="55f6b-137">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="55f6b-137">Click **OK**</span></span> 

<span data-ttu-id="55f6b-138">Du ser ett meddelande om att din konfigurationsändring pågår, vilket kan ta några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="55f6b-138">You will see a message stating that your configuration change is in progress, which may take a few minutes toocomplete.</span></span> <span data-ttu-id="55f6b-139">När hello konfigurationsändringen har slutförts, så hello i hello **toolog i via fjärranslutning** senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="55f6b-139">After hello configuration change has completed, follow hello steps in hello **toolog in remotely** section later in this article.</span></span>

## <a name="how-tooenable-remote-access-in-your-package"></a><span data-ttu-id="55f6b-140">Hur tooenable fjärråtkomst i paketet</span><span class="sxs-lookup"><span data-stu-id="55f6b-140">How tooenable Remote Access in your package</span></span>
1. <span data-ttu-id="55f6b-141">Högerklicka på Azure-projekt i Eclipses Projektutforskaren ruta och klickar på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="55f6b-141">Within Eclipse's Project Explorer pane, right-click your Azure project and click **Properties**.</span></span>

2. <span data-ttu-id="55f6b-142">I hello **egenskaper** dialogrutan Expandera **Azure** i hello vänstra fönstret och klicka på **fjärråtkomst**.</span><span class="sxs-lookup"><span data-stu-id="55f6b-142">In hello **Properties** dialog, expand **Azure** in hello left-hand pane and click **Remote Access**.</span></span>

3. <span data-ttu-id="55f6b-143">I hello **fjärråtkomst** dialogrutan Kontrollera **aktivera alla roller tooaccept anslutningar till fjärrskrivbord med dessa inloggningsuppgifterna** är markerad.</span><span class="sxs-lookup"><span data-stu-id="55f6b-143">In hello **Remote Access** dialog, ensure **Enable all roles tooaccept Remote Desktop Connections with these login credentials** is checked.</span></span>

4. <span data-ttu-id="55f6b-144">Ange ett användarnamn för hello fjärrskrivbordsanslutning.</span><span class="sxs-lookup"><span data-stu-id="55f6b-144">Specify a user name for hello Remote Desktop connection.</span></span>

5. <span data-ttu-id="55f6b-145">Ange och bekräfta hello hello användares lösenord.</span><span class="sxs-lookup"><span data-stu-id="55f6b-145">Specify and confirm hello password for hello user.</span></span> <span data-ttu-id="55f6b-146">hello användarens namn och lösenord värdena i den här dialogrutan används när du gör en fjärrskrivbordsanslutning.</span><span class="sxs-lookup"><span data-stu-id="55f6b-146">hello user name and password values set in this dialog will be used when you make a Remote Desktop connection.</span></span> <span data-ttu-id="55f6b-147">(Observera att detta är ett särskilt lösenord PFX-lösenordet).</span><span class="sxs-lookup"><span data-stu-id="55f6b-147">(Note that this is a separate password from your PFX password.)</span></span>

6. <span data-ttu-id="55f6b-148">Ange hello utgångsdatum för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="55f6b-148">Specify hello expiration date for hello user account.</span></span>

7. <span data-ttu-id="55f6b-149">Klicka på **ny** toocreate ett nytt självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="55f6b-149">Click **New** toocreate a new self-signed certificate.</span></span> <span data-ttu-id="55f6b-150">(Du kan också välja ett certifikat från din arbetsyta eller filsystemet via hello **arbetsytan** eller **FileSystem** knappar, respektive, men för den här kursen ska du skapa en ny certifikat.)</span><span class="sxs-lookup"><span data-stu-id="55f6b-150">(Alternatively, you could select a certificate from your workspace or file system through hello **Workspace** or **FileSystem** buttons, respectively, but for purposes of this tutorial we'll create a new certificate.)</span></span>

   * <span data-ttu-id="55f6b-151">I hello **nytt certifikat** dialogrutan, ange och bekräfta hello-lösenord som du använder för PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="55f6b-151">In hello **New Certificate** dialog, specify and confirm hello password you'll use for your PFX file.</span></span>

   * <span data-ttu-id="55f6b-152">Acceptera hello-värdet som angetts för **namn (CN)**, eller Använd ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="55f6b-152">Accept hello value provided for **Name (CN)**, or use a custom name.</span></span>

   * <span data-ttu-id="55f6b-153">Ange hello sökväg och filnamn där hello nytt certifikat i CER formuläret sparas.</span><span class="sxs-lookup"><span data-stu-id="55f6b-153">Specify hello path and file name where hello new certificate, in .cer form, will be saved.</span></span> <span data-ttu-id="55f6b-154">För detta steg och hello nästa steg, kan du använda hello **cert** mappen för din Azure-projekt, men du är ledigt toochoose en annan plats.</span><span class="sxs-lookup"><span data-stu-id="55f6b-154">For this step and hello next step, you could use hello **cert** folder of your Azure project, but you're free toochoose another location.</span></span> <span data-ttu-id="55f6b-155">För tillämpning av den här kursen använder vi **c:\mycert\mycert.cer**.</span><span class="sxs-lookup"><span data-stu-id="55f6b-155">For purposes of this tutorial, we'll use **c:\mycert\mycert.cer**.</span></span> <span data-ttu-id="55f6b-156">(Skapa hello **c:\mycert** mappen tidigare tooproceeding eller Använd en befintlig mapp om du vill.)</span><span class="sxs-lookup"><span data-stu-id="55f6b-156">(Create hello **c:\mycert** folder prior tooproceeding, or use an existing folder if desired.)</span></span>

   * <span data-ttu-id="55f6b-157">Ange hello sökväg och filnamn där hello nytt certifikat och dess privata nyckel i .pfx formuläret ska sparas.</span><span class="sxs-lookup"><span data-stu-id="55f6b-157">Specify hello path and file name where hello new certificate and its private key, in .pfx form, will be saved.</span></span> <span data-ttu-id="55f6b-158">För tillämpning av den här kursen använder vi **c:\mycert\mycert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="55f6b-158">For purposes of this tutorial, we'll use **c:\mycert\mycert.pfx**.</span></span> <span data-ttu-id="55f6b-159">Din **nytt certifikat** dialogrutan bör se ut ungefär toohello följande (uppdatera hello mappsökvägar om du inte använde **c:\mycert**):</span><span class="sxs-lookup"><span data-stu-id="55f6b-159">Your **New Certificate** dialog should look similar toohello following (update hello folder paths if you did not use **c:\mycert**):</span></span>
     
      ![][ic712275]

   * <span data-ttu-id="55f6b-160">Klicka på **OK** tooclose hello **nytt certifikat** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="55f6b-160">Click **OK** tooclose hello **New Certificate** dialog.</span></span>

8. <span data-ttu-id="55f6b-161">Din **fjärråtkomst** dialogrutan bör se ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="55f6b-161">Your **Remote Access** dialog should look similar toohello following:</span></span></p>
   
   ![][ic719495]

9. <span data-ttu-id="55f6b-162">Klicka på **OK** tooclose hello **fjärråtkomst** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="55f6b-162">Click **OK** tooclose hello **Remote Access** dialog.</span></span>

<span data-ttu-id="55f6b-163">Återskapar ditt program, med hello bygga för distribution toocloud.</span><span class="sxs-lookup"><span data-stu-id="55f6b-163">Rebuild your application, with hello build set for deployment toocloud.</span></span>

## <a name="toolog-in-remotely"></a><span data-ttu-id="55f6b-164">toolog i via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="55f6b-164">toolog in remotely</span></span>
<span data-ttu-id="55f6b-165">När din rollinstans är klar kan du logga i toohello virtuell dator som är värd för ditt program.</span><span class="sxs-lookup"><span data-stu-id="55f6b-165">Once your role instance is ready, you can remotely log in toohello virtual machine that is hosting your application.</span></span>

* <span data-ttu-id="55f6b-166">Om du använder Eclipse i Windows och valda hello **starta Fjärrskrivbord på distribuera** alternativet under din distribution tooAzure visas med en anslutning till fjärrskrivbord inloggningsskärm när distributionen startar.</span><span class="sxs-lookup"><span data-stu-id="55f6b-166">If are using Eclipse on Windows and you selected hello **Start remote desktop on deploy** option during your deployment tooAzure, you will be presented with a Remote Desktop Connection logon screen when your deployment starts.</span></span> <span data-ttu-id="55f6b-167">När du tillfrågas om hello användarnamn och lösenord Ange hello-värden som du angett för hello fjärranvändare och kommer att kunna toolog i.</span><span class="sxs-lookup"><span data-stu-id="55f6b-167">When you are prompted for hello user name and password, enter hello values that you specified for hello remote user and will be able toolog in.</span></span>

* <span data-ttu-id="55f6b-168">Ett annat sätt toolog i är via fjärranslutning via hello <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure-hanteringsportalen</a>:</span><span class="sxs-lookup"><span data-stu-id="55f6b-168">Another way toolog in remotely is through hello <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure Management Portal</a>:</span></span>
  
  * <span data-ttu-id="55f6b-169">Inom hello **molntjänster** vy över hello Azure Management portal på Molntjänsten, klicka på **instanser**, klicka på en specifik instans och klicka på hello **Anslut**knappen.</span><span class="sxs-lookup"><span data-stu-id="55f6b-169">Within hello **Cloud Services** view of hello Azure Management portal, click your cloud service, click **Instances**, click a specific instance, and then click hello **Connect** button.</span></span> <span data-ttu-id="55f6b-170">Hej **Anslut** knappen visas som hello följande i kommandofältet hello:</span><span class="sxs-lookup"><span data-stu-id="55f6b-170">hello **Connect** button appears as hello following in hello command bar:</span></span>
    
      ![][ic659273]

  * <span data-ttu-id="55f6b-171">När du klickar på hello **Anslut** knappen, kommer du att ange tooopen en RDP-fil.</span><span class="sxs-lookup"><span data-stu-id="55f6b-171">After clicking hello **Connect** button, you will be prompted tooopen an RDP file.</span></span> <span data-ttu-id="55f6b-172">Öppna hello-filen och följ anvisningarna för hello.</span><span class="sxs-lookup"><span data-stu-id="55f6b-172">Open hello file and follow hello prompts.</span></span> <span data-ttu-id="55f6b-173">(Du kan också spara filen tooyour lokala datorn och kör sedan hello-filen genom att dubbelklicka på den tooremote logg i tooyour virtuella datorn utan att behöva toofirst gå hello-hanteringsportalen.)</span><span class="sxs-lookup"><span data-stu-id="55f6b-173">(You could also save this file tooyour local computer, and then run hello file by double-clicking it tooremote log in tooyour virtual machine without needing toofirst go hello management portal.)</span></span>

  * <span data-ttu-id="55f6b-174">När du tillfrågas om hello användarnamn och lösenord Ange hello-värden som du angett för hello fjärranvändare och kommer att kunna toolog i.</span><span class="sxs-lookup"><span data-stu-id="55f6b-174">When you are prompted for hello user name and password, enter hello values that you specified for hello remote user and will be able toolog in.</span></span>

> [!NOTE]
> <span data-ttu-id="55f6b-175">Om du är på ett icke-Windows-operativsystem måste toouse en fjärrskrivbordsklient som är kompatibel med operativsystemet och följ hello steg tooconfigure klienten med hello inställningarna i hello RDP-filen som du har hämtat.</span><span class="sxs-lookup"><span data-stu-id="55f6b-175">If you are on a non-Windows operating system, you need toouse a Remote Desktop client that is compatible with your operating system and follow hello steps tooconfigure that client with hello settings in hello RDP file that you downloaded.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="55f6b-176">Se även</span><span class="sxs-lookup"><span data-stu-id="55f6b-176">See Also</span></span>
<span data-ttu-id="55f6b-177">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="55f6b-177">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="55f6b-178">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="55f6b-178">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="55f6b-179">[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="55f6b-179">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="55f6b-180">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="55f6b-180">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
