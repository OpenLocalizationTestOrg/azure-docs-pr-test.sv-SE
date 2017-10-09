---
title: "aaaAccessing dina appar från valfri enhet | Microsoft Docs"
description: "Lär dig vilka klienter som stöds för Azure RemoteApp och hur tooaccess dina appar."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: fb7bd17d-7aa8-43fd-9278-f96e0e9308e4
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 15985b40d870e3155d4132063bf5b9677ff9afed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-your-apps-in-azure-remoteapp"></a><span data-ttu-id="bd748-103">Kom åt dina appar i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="bd748-103">Accessing your apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="bd748-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="bd748-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="bd748-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="bd748-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="bd748-106">En av hello beauties av Azure RemoteApp är att du kan komma åt appar från någon av dina enheter.</span><span class="sxs-lookup"><span data-stu-id="bd748-106">One of hello beauties of Azure RemoteApp is that you can access apps from any of your devices.</span></span> <span data-ttu-id="bd748-107">Ännu bättre kan börja arbeta på en enhet och sömlöst övergång tooa annan enhet och hämta höger där du slutade.</span><span class="sxs-lookup"><span data-stu-id="bd748-107">Even better, you can start working on one device and then seamlessly transition tooa second device and pick up right where you left off.</span></span> <span data-ttu-id="bd748-108">tooget igång du behöver toodownload hello rätt klient för din enhet och logga in toohello service.</span><span class="sxs-lookup"><span data-stu-id="bd748-108">tooget started you need toodownload hello appropriate client for your device and sign in toohello service.</span></span>

<span data-ttu-id="bd748-109">I det här avsnittet ska vi granska hello-klienter som för närvarande stöds och hur toodownload dem innan jag visar hur toosign i tooRemoteApp från varje hello-klienter.</span><span class="sxs-lookup"><span data-stu-id="bd748-109">In this topic, we'll review hello clients currently supported and how toodownload them before I show you how toosign in tooRemoteApp from each of hello clients.</span></span>

## <a name="supported-clients"></a><span data-ttu-id="bd748-110">Klienter som stöds</span><span class="sxs-lookup"><span data-stu-id="bd748-110">Supported clients</span></span>
<span data-ttu-id="bd748-111">Du kan komma åt RemoteApp med hello stegen nedan om enheten kör något av följande operativsystem:</span><span class="sxs-lookup"><span data-stu-id="bd748-111">You can access RemoteApp using hello steps below if your device is running one of these operating systems:</span></span>

* <span data-ttu-id="bd748-112">Windows 10</span><span class="sxs-lookup"><span data-stu-id="bd748-112">Windows 10</span></span> 
* <span data-ttu-id="bd748-113">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="bd748-113">Windows 8.1</span></span>
* <span data-ttu-id="bd748-114">Windows 8</span><span class="sxs-lookup"><span data-stu-id="bd748-114">Windows 8</span></span>
* <span data-ttu-id="bd748-115">Windows 7 Service Pack 1</span><span class="sxs-lookup"><span data-stu-id="bd748-115">Windows 7 Service Pack 1</span></span>
* <span data-ttu-id="bd748-116">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="bd748-116">Windows Phone 8.1</span></span>
* <span data-ttu-id="bd748-117">iOS</span><span class="sxs-lookup"><span data-stu-id="bd748-117">iOS</span></span>
* <span data-ttu-id="bd748-118">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="bd748-118">Mac OS X</span></span>
* <span data-ttu-id="bd748-119">Android</span><span class="sxs-lookup"><span data-stu-id="bd748-119">Android</span></span>

 <span data-ttu-id="bd748-120">Nyheter om tunna klienter? hello följande tunna Windows Embedded-klienter stöds:</span><span class="sxs-lookup"><span data-stu-id="bd748-120">What about thin clients? hello following Windows Embedded thin clients are supported:</span></span>

* <span data-ttu-id="bd748-121">Windows Embedded Standard 7</span><span class="sxs-lookup"><span data-stu-id="bd748-121">Windows Embedded Standard 7</span></span>
* <span data-ttu-id="bd748-122">Windows Embedded 8 Standard</span><span class="sxs-lookup"><span data-stu-id="bd748-122">Windows Embedded 8 Standard</span></span>
* <span data-ttu-id="bd748-123">Windows Embedded 8.1 Industry Pro</span><span class="sxs-lookup"><span data-stu-id="bd748-123">Windows Embedded 8.1 Industry Pro</span></span>
* <span data-ttu-id="bd748-124">Windows 10 IoT Enterprise</span><span class="sxs-lookup"><span data-stu-id="bd748-124">Windows 10 IoT Enterprise</span></span>

## <a name="downloading-hello-client"></a><span data-ttu-id="bd748-125">Hämtar hello-klient</span><span class="sxs-lookup"><span data-stu-id="bd748-125">Downloading hello client</span></span>
<span data-ttu-id="bd748-126">Oavsett vilken plattform som du använder, hello-klient som du behöver tooaccess RemoteApp finns på hello [fjärrskrivbord klienten ska ladda ned](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="bd748-126">No matter what platform you are using, hello client you need tooaccess RemoteApp can be found on hello [Remote Desktop client download](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) page.</span></span>

<span data-ttu-id="bd748-127">Klicka på hello olika länkar direkt startar antingen hämtar hello klienten eller skickar du hämtningssidan för toohello client hello app store för den plattformen.</span><span class="sxs-lookup"><span data-stu-id="bd748-127">Clicking hello different links will either directly start downloading hello client or will send you toohello client download page in hello app store for that platform.</span></span> <span data-ttu-id="bd748-128">Installera hello-klienten genom att följa hello anvisningar på hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="bd748-128">Install hello client by following hello instructions on hello screen.</span></span>

<span data-ttu-id="bd748-129">När du har installerat hello-klienten på enheten och starta den, hoppa toohello motsvarande avsnitt nedan toolearn hur toosign i tooRemoteApp från klienten.</span><span class="sxs-lookup"><span data-stu-id="bd748-129">Once you have installed hello client on your device and launched it, jump toohello corresponding section below toolearn how toosign in tooRemoteApp from that client.</span></span>

## <a name="android"></a><span data-ttu-id="bd748-130">Android</span><span class="sxs-lookup"><span data-stu-id="bd748-130">Android</span></span>
<span data-ttu-id="bd748-131">När du har installerat hello Microsoft Remote Desktop-appen från hello Google Play store, du kan hitta den i app-listan under **fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="bd748-131">Once you have installed hello Microsoft Remote Desktop app from hello Google Play store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="bd748-132">Starta hello app ger tooan tom anslutning Center om du redan använt hello app.</span><span class="sxs-lookup"><span data-stu-id="bd748-132">Launching hello app brings you tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="bd748-133">tooget igång med Azure RemoteApp, hello tryck på knappen Lägg till **”” + ””** och tryck på **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="bd748-133">tooget started with Azure RemoteApp, tap hello add button **""+""** and tap **Azure RemoteApp**.</span></span>    
   
     ![Tom Anslutningscentret](./media/remoteapp-clients/Android1.png)
2. <span data-ttu-id="bd748-135">Du måste toosign in med din e-postadress tooaccess hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="bd748-135">You need toosign in with your email address tooaccess hello service.</span></span> <span data-ttu-id="bd748-136">Tryck på **Kom igång**.</span><span class="sxs-lookup"><span data-stu-id="bd748-136">Tap **Get started**.</span></span>
   
    ![Logga in fråga](./media/remoteapp-clients/Android2.png)
3. <span data-ttu-id="bd748-138">Skriv på hello nästa sida i din **e-postadress** och tryck på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="bd748-138">On hello next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="bd748-139">Detta startar hello inloggningsprocessen med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bd748-139">This begins hello sign-in process using Azure Active Directory.</span></span>
   
    ![Första sidan i Azure Active Directory](./media/remoteapp-clients/Android3.png)
4. <span data-ttu-id="bd748-141">Följ instruktionerna för hello på hello-skärmen toosign in med ditt Microsoft-konto (tidigare kallade ”Live ID”) eller en organisations-ID.</span><span class="sxs-lookup"><span data-stu-id="bd748-141">Follow hello instructions on hello screen toosign in with your Microsoft account (previously called "LiveID") or organization ID.</span></span> <span data-ttu-id="bd748-142">När du är inloggad visas med en sida som visar en lista över alla hello inbjudningar som du har fått.</span><span class="sxs-lookup"><span data-stu-id="bd748-142">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="bd748-143">Om du kan välja hello inbjudningar som du litar på och tryck på **klar**.</span><span class="sxs-lookup"><span data-stu-id="bd748-143">If you are, select hello invitations you trust and tap **Done**.</span></span>    
   
    ![Inbjudningar sida](./media/remoteapp-clients/Android4.png)
5. <span data-ttu-id="bd748-145">När du godkänt ditt inbjudningar, hello lista över appar du har åtkomst toowill vara hämtade tooyour enhet och göras tillgänglig i hello anslutning Center.</span><span class="sxs-lookup"><span data-stu-id="bd748-145">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="bd748-146">Knacka på ett av hello appar toostart använder den.</span><span class="sxs-lookup"><span data-stu-id="bd748-146">Tap one of hello apps toostart using it.</span></span>
   
    ![Anslutningen Center med en feed](./media/remoteapp-clients/Android5.png)
6. <span data-ttu-id="bd748-148">Om du inte har en inbjudan ännu kan du prova ut hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="bd748-148">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="bd748-149">toodo så trycker du på **gå toofree utvärderingsversion** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="bd748-149">toodo so, tap **Go toofree trial** when prompted.</span></span>
   
    ![Demo feed fråga](./media/remoteapp-clients/Android6.png)
7. <span data-ttu-id="bd748-151">Detta ger dig åtkomst till tooa grundläggande uppsättning appar tooget du utgick från RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bd748-151">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Demo feed för Azure RemoteApp](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a><span data-ttu-id="bd748-153">iOS</span><span class="sxs-lookup"><span data-stu-id="bd748-153">iOS</span></span>
<span data-ttu-id="bd748-154">När du har installerat hello Microsoft Remote Desktop-appen från hello App store, du kan hitta den i app-listan under **fjärrskrivbordsklienten**.</span><span class="sxs-lookup"><span data-stu-id="bd748-154">Once you have installed hello Microsoft Remote Desktop app from hello App store, you can find it in your app list under **RD Client**.</span></span>

1. <span data-ttu-id="bd748-155">Starta hello app ger tooan tom anslutning Center om du redan använt hello app.</span><span class="sxs-lookup"><span data-stu-id="bd748-155">Launching hello app brings you tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="bd748-156">tooget igång med Azure RemoteApp, hello tryck på knappen Lägg till **”” + ””** och tryck på **lägga till Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="bd748-156">tooget started with Azure RemoteApp, tap hello add button **""+""** and tap **Add Azure RemoteApp**.</span></span>
   
    ![Tom Anslutningscentret](./media/remoteapp-clients/IOS1.png)
2. <span data-ttu-id="bd748-158">Du behöver toosign in med din e-postadress tooaccess hello tjänst, toostart för den här processen, ange din **e-postadress** och tryck på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="bd748-158">You need toosign in with your email address tooaccess hello service, toostart that process, type in your **email address** and tap **Continue**.</span></span>
   
    ![Logga in fråga](./media/remoteapp-clients/picture1.png)
3. <span data-ttu-id="bd748-160">Följ instruktionerna för hello på hello-skärmen toosign in med ditt Microsoft-konto (Live ID) eller organisation-ID.</span><span class="sxs-lookup"><span data-stu-id="bd748-160">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="bd748-161">När du är inloggad visas med en sida som visar en lista över alla hello inbjudningar som du har fått.</span><span class="sxs-lookup"><span data-stu-id="bd748-161">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="bd748-162">Om du kan välja hello inbjudningar som du litar på och tryck på **klar**.</span><span class="sxs-lookup"><span data-stu-id="bd748-162">If you are, select hello invitations you trust and tap **Done**.</span></span>
   
    ![Inbjudningar sida](./media/remoteapp-clients/IOS3.png)
4. <span data-ttu-id="bd748-164">När du godkänt ditt inbjudningar, hello lista över appar du har åtkomst toowill vara hämtade tooyour enhet och göras tillgänglig i hello anslutning Center.</span><span class="sxs-lookup"><span data-stu-id="bd748-164">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="bd748-165">Knacka på ett av hello appar toolaunch den och börja använda den.</span><span class="sxs-lookup"><span data-stu-id="bd748-165">Tap one of hello apps toolaunch it and start using it.</span></span>
   
    ![Anslutningen Center med en feed](./media/remoteapp-clients/IOS4.png)
5. <span data-ttu-id="bd748-167">Om du inte har en inbjudan ännu kan du prova ut hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="bd748-167">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="bd748-168">toodo så trycker du på **gå toofree utvärderingsversion** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="bd748-168">toodo so, tap **Go toofree trial** when prompted.</span></span>
   
    ![Demo feed fråga](./media/remoteapp-clients/IOS5.png)
6. <span data-ttu-id="bd748-170">Detta ger dig åtkomst till tooa grundläggande uppsättning appar tooget du utgick från RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bd748-170">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Demo feed för Azure RemoteApp](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a><span data-ttu-id="bd748-172">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="bd748-172">Mac OS X</span></span>
<span data-ttu-id="bd748-173">När du har installerat hello Microsoft Remote Desktop-appen från hello App store, du kan hitta den i app-listan under **Microsoft Remote Desktop**.</span><span class="sxs-lookup"><span data-stu-id="bd748-173">Once you have installed hello Microsoft Remote Desktop app from hello App store, you can find it in your app list under **Microsoft Remote Desktop**.</span></span>

1. <span data-ttu-id="bd748-174">Starta hello app ger tooan tom anslutning Center om du redan använt hello app.</span><span class="sxs-lookup"><span data-stu-id="bd748-174">Launching hello app brings you tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="bd748-175">tooget igång med Azure RemoteApp, klicka på hello **Azure RemoteApp** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd748-175">tooget started with Azure RemoteApp, click hello **Azure RemoteApp** button.</span></span>
   
    ![Tom Anslutningscentret](./media/remoteapp-clients/Mac1.png)
2. <span data-ttu-id="bd748-177">Du behöver toosign in med din e-postadress tooaccess hello tjänst, toostart som bearbetar, trycker du på **Kom igång**.</span><span class="sxs-lookup"><span data-stu-id="bd748-177">You need toosign in with your email address tooaccess hello service, toostart that process, tap **Get Started**.</span></span>
   
    ![Logga in fråga](./media/remoteapp-clients/Mac2.png)
3. <span data-ttu-id="bd748-179">Skriv på hello nästa sida i din **e-postadress** och tryck på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="bd748-179">On hello next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="bd748-180">Detta startar hello logga in med hjälp av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bd748-180">This begins hello sign in process using Azure Active Directory.</span></span>
   
    ![Första sidan i Azure Active Directory](./media/remoteapp-clients/picture2.png)
4. <span data-ttu-id="bd748-182">Följ instruktionerna för hello på hello-skärmen toosign in med ditt Microsoft-konto (Live ID) eller organisation-ID.</span><span class="sxs-lookup"><span data-stu-id="bd748-182">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="bd748-183">När du är inloggad visas med en sida som visar en lista över alla hello inbjudningar som du har fått.</span><span class="sxs-lookup"><span data-stu-id="bd748-183">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="bd748-184">Om du kan markera hello inbjudningar som du litar på och stänga dialogrutan hello.</span><span class="sxs-lookup"><span data-stu-id="bd748-184">If you are, select hello invitations you trust and close hello dialog.</span></span>
   
    ![Inbjudningar sida](./media/remoteapp-clients/Mac4.png)
5. <span data-ttu-id="bd748-186">När du godkänt ditt inbjudningar, hello lista över appar du har åtkomst toowill vara hämtade tooyour enhet och göras tillgänglig i hello anslutning Center.</span><span class="sxs-lookup"><span data-stu-id="bd748-186">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="bd748-187">Dubbelklicka på en av hello appar toolaunch den och börja använda den.</span><span class="sxs-lookup"><span data-stu-id="bd748-187">Double-click one of hello apps toolaunch it and start using it.</span></span>
   
    ![Anslutningen Center med en feed](./media/remoteapp-clients/Mac5.png)
6. <span data-ttu-id="bd748-189">Om du inte har en inbjudan ännu kan du prova ut hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="bd748-189">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="bd748-190">toodo Klicka på **gå toofree utvärderingsversion** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="bd748-190">toodo so, click **Go toofree trial** when prompted.</span></span>
   
    ![Demo feed fråga](./media/remoteapp-clients/Mac6.png)
7. <span data-ttu-id="bd748-192">Detta ger dig åtkomst till tooa grundläggande uppsättning appar tooget du utgick från RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bd748-192">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Demo feed för Azure RemoteApp](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a><span data-ttu-id="bd748-194">Windows (alla versioner utom Windows Phone)</span><span class="sxs-lookup"><span data-stu-id="bd748-194">Windows (All supported versions except Windows Phone)</span></span>
<span data-ttu-id="bd748-195">hello klienten startar automatiskt när den är klar installerar, men när du behöver tooaccess igen den finns i app-listan under hello namn **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="bd748-195">hello client launches automatically after it finishes installing, however when you need tooaccess it again later it can be found in your app list under hello name **Azure RemoteApp**.</span></span>

1. <span data-ttu-id="bd748-196">Starta hello klienten hello första sidan som du ser enare välkomnar tooAzure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bd748-196">Ater launching hello client, hello first page you see welcomes you tooAzure RemoteApp.</span></span> <span data-ttu-id="bd748-197">tooproceed, klicka på **Kom igång**.</span><span class="sxs-lookup"><span data-stu-id="bd748-197">tooproceed, click on **Get Started**.</span></span>
   
    ![Konfigurationssidan för hello Azure RemoteApp-klienten](./media/remoteapp-clients/Windows1.png)
2. <span data-ttu-id="bd748-199">hello nästa sida startar hello logga i processen för Azure RemoteApp med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bd748-199">hello next page starts hello sign in process for Azure RemoteApp using Azure Active Directory.</span></span> <span data-ttu-id="bd748-200">Den här processen borde se bekant ut om du har använt Microsoft-tjänster i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="bd748-200">This process should look familiar if you have used Microsoft services in hello past.</span></span> <span data-ttu-id="bd748-201">Starta genom att skriva din **e-postadress** och på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="bd748-201">Start by typing your **email address** and click **Continue**.</span></span>
   
    ![Första Azure Active Directory-fråga](./media/remoteapp-clients/Windows2.png)
3. <span data-ttu-id="bd748-203">Följ instruktionerna för hello på hello-skärmen toosign in med ditt Microsoft-konto (Live ID) eller organisation-ID.</span><span class="sxs-lookup"><span data-stu-id="bd748-203">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="bd748-204">När du är inloggad visas med en sida som visar en lista över alla hello inbjudningar som du har fått.</span><span class="sxs-lookup"><span data-stu-id="bd748-204">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="bd748-205">Om du kan välja hello inbjudningar som du litar på och klicka på **klar**.</span><span class="sxs-lookup"><span data-stu-id="bd748-205">If you are, select hello invitations you trust and click **Done**.</span></span>
   
    ![Inbjudningar sidan hello Azure RemoteApp-klienten](./media/remoteapp-clients/Windows3.png)
4. <span data-ttu-id="bd748-207">När du godkänt ditt inbjudningar, hello lista över appar du har åtkomst toowill vara hämtade tooyour enhet och göras tillgänglig i hello anslutning Center.</span><span class="sxs-lookup"><span data-stu-id="bd748-207">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="bd748-208">Dubbelklicka på en av hello appar toolaunch den och börja använda den.</span><span class="sxs-lookup"><span data-stu-id="bd748-208">Double-click one of hello apps toolaunch it and start using it.</span></span>
   
    ![Anslutningen mittpunkt hello Azure RemoteApp-klienten](./media/remoteapp-clients/Windows4.png)
5. <span data-ttu-id="bd748-210">Oroa dig inte om ingen har skickat en inbjudan ännu, har vi dig!</span><span class="sxs-lookup"><span data-stu-id="bd748-210">If no one has sent you an invitation yet, don't worry we've got you covered!</span></span> <span data-ttu-id="bd748-211">Du har fortfarande åtkomst tooa demo samling så att du kan testa ut hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="bd748-211">You'll still have access tooa demo collection so you can test out hello service.</span></span>
   
    ![Demo feed för Azure RemoteApp](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a><span data-ttu-id="bd748-213">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="bd748-213">Windows Phone 8.1</span></span>
<span data-ttu-id="bd748-214">När du har installerat hello Microsoft Remote Desktop-appen från hello Windows Phone 8.1 store, du kan hitta den i app-listan under **fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="bd748-214">Once you have installed hello Microsoft Remote Desktop app from hello Windows Phone 8.1 store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="bd748-215">Starta hello app ger dig direkt tooan tom anslutning Center om du redan använt hello app.</span><span class="sxs-lookup"><span data-stu-id="bd748-215">Launching hello app brings you directly tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="bd748-216">tooget igång med Azure RemoteApp, hello tryck på knappen Lägg till **”” + ””** längst hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="bd748-216">tooget started with Azure RemoteApp, tap hello add button **""+""** at hello bottom of hello screen.</span></span>
   
    ![Tom Anslutningscentret](./media/remoteapp-clients/WinPhone1.png)
2. <span data-ttu-id="bd748-218">Tryck sedan på **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="bd748-218">Next, tap on **Azure RemoteApp**.</span></span>
   
    ![Lägg till objekt på sidan](./media/remoteapp-clients/WinPhone2.png)
3. <span data-ttu-id="bd748-220">Du behöver toosign in med din e-postadress tooaccess hello tjänst, toostart som bearbetar, trycker du på **ansluta**.</span><span class="sxs-lookup"><span data-stu-id="bd748-220">You need toosign in with your email address tooaccess hello service, toostart that process, tap **connect**.</span></span>
   
    ![Logga in fråga](./media/remoteapp-clients/WinPhone3.png)
4. <span data-ttu-id="bd748-222">Skriv på hello nästa sida i din **e-postadress** och tryck på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="bd748-222">On hello next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="bd748-223">Detta startar hello logga in med hjälp av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bd748-223">This begins hello sign in process using Azure Active Directory.</span></span>
   
    ![Första sidan i Azure Active Directory](./media/remoteapp-clients/WinPhone4.png)
5. <span data-ttu-id="bd748-225">Följ instruktionerna för hello på hello-skärmen toosign in med ditt Microsoft-konto (Live ID) eller organisation-ID.</span><span class="sxs-lookup"><span data-stu-id="bd748-225">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="bd748-226">När du är inloggad visas med en sida som visar en lista över alla hello inbjudningar som du har fått.</span><span class="sxs-lookup"><span data-stu-id="bd748-226">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="bd748-227">Om du kan välja hello inbjudningar som du litar på och tryck på **spara**.</span><span class="sxs-lookup"><span data-stu-id="bd748-227">If you are, select hello invitations you trust and tap **save**.</span></span>
   
    ![Inbjudningar sida](./media/remoteapp-clients/WinPhone5.png)
6. <span data-ttu-id="bd748-229">När du godkänt ditt inbjudningar, hello lista över appar du har åtkomst toowill vara hämtade tooyour enhet och göras tillgänglig i hello anslutning Center.</span><span class="sxs-lookup"><span data-stu-id="bd748-229">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="bd748-230">Knacka på ett av hello appar toolaunch den och börja använda den.</span><span class="sxs-lookup"><span data-stu-id="bd748-230">Tap one of hello apps toolaunch it and start using it.</span></span>
   
    ![Anslutningen Center med en feed](./media/remoteapp-clients/WinPhone6.png)
7. <span data-ttu-id="bd748-232">Om du inte har en inbjudan ännu kan du prova ut hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="bd748-232">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="bd748-233">toodo så trycker du på **Ja** när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="bd748-233">toodo so, tap **yes** when prompted.</span></span>
   
    ![Demo feed fråga](./media/remoteapp-clients/WinPhone7.png)
8. <span data-ttu-id="bd748-235">Detta ger dig åtkomst till tooa grundläggande uppsättning appar tooget du utgick från RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bd748-235">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Demo feed för Azure RemoteApp](./media/remoteapp-clients/WinPhone8.png)

