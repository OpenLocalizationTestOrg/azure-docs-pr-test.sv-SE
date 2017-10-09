---
title: "aaaGet igång med Azure Notification Hubs genom att använda Baidu | Microsoft Docs"
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toopush meddelanden tooAndroid enheter med hjälp av Baidu."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 23bde1ea-f978-43b2-9eeb-bfd7b9edc4c1
ms.service: notification-hubs
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: mobile-baidu
ms.workload: mobile
ms.date: 08/19/2016
ms.author: yuaxu
ms.openlocfilehash: 2767fdd3bb04674e7a531634237cc05cd8c21cb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-using-baidu"></a><span data-ttu-id="c3b27-103">Kom igång med Notification Hub genom att använda Baidu</span><span class="sxs-lookup"><span data-stu-id="c3b27-103">Get started with Notification Hubs using Baidu</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="c3b27-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c3b27-104">Overview</span></span>
<span data-ttu-id="c3b27-105">Baidu cloud push är en kinesisk molntjänst som du kan använda toosend push-meddelanden toomobile enheter.</span><span class="sxs-lookup"><span data-stu-id="c3b27-105">Baidu cloud push is a Chinese cloud service that you can use toosend push notifications toomobile devices.</span></span> <span data-ttu-id="c3b27-106">Den här tjänsten är användbar i Kina, där leverera push-meddelanden tooAndroid är komplex på grund av hello förekomsten av olika appbutiker och push-tjänsterna, dessutom toohello tillgängligheten för Android-enheter som inte är vanligtvis anslutna tooGCM (Google Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="c3b27-106">This service is useful in China, where delivering push notifications tooAndroid is complex because of hello presence of different app stores and push services, in addition toohello availability of Android devices that are not typically connected tooGCM (Google Cloud Messaging).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c3b27-107">Krav</span><span class="sxs-lookup"><span data-stu-id="c3b27-107">Prerequisites</span></span>
<span data-ttu-id="c3b27-108">För den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="c3b27-108">This tutorial requires:</span></span>

* <span data-ttu-id="c3b27-109">Android SDK (vi antar att du använder Eclipse), som du kan hämta från hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">webbplatsen för Android</a></span><span class="sxs-lookup"><span data-stu-id="c3b27-109">Android SDK (we assume that you use Eclipse), which you can download from hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android site</a></span></span>
* <span data-ttu-id="c3b27-110">[Mobile Services Android SDK]</span><span class="sxs-lookup"><span data-stu-id="c3b27-110">[Mobile Services Android SDK]</span></span>
* <span data-ttu-id="c3b27-111">[Baidu Push Android SDK]</span><span class="sxs-lookup"><span data-stu-id="c3b27-111">[Baidu Push Android SDK]</span></span>

> [!NOTE]
> <span data-ttu-id="c3b27-112">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c3b27-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="c3b27-113">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="c3b27-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c3b27-114">Mer information om den kostnadsfria utvärderingsversionen av Azure finns [här](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="c3b27-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).</span></span>
> 
> 

## <a name="create-a-baidu-account"></a><span data-ttu-id="c3b27-115">Skapa ett Baidu-konto</span><span class="sxs-lookup"><span data-stu-id="c3b27-115">Create a Baidu account</span></span>
<span data-ttu-id="c3b27-116">toouse Baidu måste du ha ett Baidu-konto.</span><span class="sxs-lookup"><span data-stu-id="c3b27-116">toouse Baidu, you must have a Baidu account.</span></span> <span data-ttu-id="c3b27-117">Om du redan har en logga in toohello [Baidu-portalen] och hoppa över toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="c3b27-117">If you already have one, log in toohello [Baidu portal] and skip toohello next step.</span></span> <span data-ttu-id="c3b27-118">Se annars hello följa anvisningar om hur toocreate ett Baidu-konto.</span><span class="sxs-lookup"><span data-stu-id="c3b27-118">Otherwise, see hello following instructions on how toocreate a Baidu account.</span></span>  

1. <span data-ttu-id="c3b27-119">Gå toohello [Baidu-portalen] och klicka på hello**登录**(**inloggning**) länk.</span><span class="sxs-lookup"><span data-stu-id="c3b27-119">Go toohello [Baidu portal] and click hello **登录** (**Login**) link.</span></span> <span data-ttu-id="c3b27-120">Klicka på**立即注册**toostart hello konto registreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="c3b27-120">Click **立即注册** toostart hello account registration process.</span></span>
   
   ![][1]
2. <span data-ttu-id="c3b27-121">Ange information om hello krävs – telefon/e-postadress, lösenord och Verifieringskod, och klicka på **registreringen**.</span><span class="sxs-lookup"><span data-stu-id="c3b27-121">Enter hello required details—phone/email address, password, and verification code—and click **Signup**.</span></span>
   
   ![][2]
3. <span data-ttu-id="c3b27-122">Du kommer att skickas en e-toohello e-postadress som du angav med en länk tooactivate Baidu-konto.</span><span class="sxs-lookup"><span data-stu-id="c3b27-122">You will be sent an email toohello email address that you entered with a link tooactivate your Baidu account.</span></span>
   
   ![][3]
4. <span data-ttu-id="c3b27-123">Logga in tooyour e-postkonto, öppna aktiveringsmejlet hello och klicka på hello aktivering länken tooactivate Baidu-konto.</span><span class="sxs-lookup"><span data-stu-id="c3b27-123">Log in tooyour email account, open hello Baidu activation mail, and click hello activation link tooactivate your Baidu account.</span></span>
   
   ![][4]

<span data-ttu-id="c3b27-124">När du har ett aktiverat Baidu-konto kan logga in toohello [Baidu-portalen].</span><span class="sxs-lookup"><span data-stu-id="c3b27-124">Once you have an activated Baidu account, log in toohello [Baidu portal].</span></span>

## <a name="register-as-a-baidu-developer"></a><span data-ttu-id="c3b27-125">Registrera dig som Baidu-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="c3b27-125">Register as a Baidu developer</span></span>
1. <span data-ttu-id="c3b27-126">När du har loggat in toohello [Baidu-portalen], klickar du på**更多 >>** (**mer**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-126">Once you have logged in toohello [Baidu portal], click **更多>>** (**more**).</span></span>
   
      ![][5]
2. <span data-ttu-id="c3b27-127">Rulla nedåt i hello**站长与开发者服务 (webbadministratör och utvecklare tjänster)** avsnittet och klicka på**百度开放云平台**(**Baidu öppna molnplattform**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-127">Scroll down in hello **站长与开发者服务 (Webmaster and Developer Services)** section and click **百度开放云平台** (**Baidu open cloud platform**).</span></span>
   
      ![][6]
3. <span data-ttu-id="c3b27-128">Klicka på nästa sida hello**开发者服务**(**Developer Services**) på hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="c3b27-128">On hello next page, click **开发者服务** (**Developer Services**) on hello top-right corner.</span></span>
   
      ![][7]
4. <span data-ttu-id="c3b27-129">Klicka på nästa sida hello**注册开发者**(**registrerade utvecklare**) hello-menyn på hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="c3b27-129">On hello next page, click **注册开发者** (**Registered Developers**) from hello menu on hello top-right corner.</span></span>
   
      ![][8]
5. <span data-ttu-id="c3b27-130">Ange ditt namn, en beskrivning och ett mobiltelefonnummer för att få ett textmeddelande för verifiering. Klicka sedan på **送验证码** (**Skicka verifieringskod**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-130">Enter your name, a description, and a mobile phone number for receiving a verification text message, and then click **送验证码** (**Send Verification Code**).</span></span> <span data-ttu-id="c3b27-131">Du måste tooenclose hello landskoden inom parentes för internationella telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="c3b27-131">For international phone numbers, you need tooenclose hello country code in parentheses.</span></span> <span data-ttu-id="c3b27-132">För ett mobiltelefonnummer i USA är det exempelvis **(1) 1234567890**.</span><span class="sxs-lookup"><span data-stu-id="c3b27-132">For example, for a United States number, it is **(1)1234567890**.</span></span>
   
      ![][9]
6. <span data-ttu-id="c3b27-133">Du bör få ett textmeddelande med ett verifieringsnummer, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="c3b27-133">You should then receive a text message with a verification number, as shown in hello following example:</span></span>
   
      ![][10]
7. <span data-ttu-id="c3b27-134">Ange hello verifieringsnummer från hello-meddelande i**验证码**(**bekräftelsekoden**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-134">Enter hello verification number from hello message in **验证码** (**Confirmation code**).</span></span>
8. <span data-ttu-id="c3b27-135">Slutför hello developer registrering genom när hello Baidu och klicka på**提交**(**skicka**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-135">Finally, complete hello developer registration by accepting hello Baidu agreement and clicking **提交** (**Submit**).</span></span> <span data-ttu-id="c3b27-136">Följande sida på slutförande av registrering hello visas:</span><span class="sxs-lookup"><span data-stu-id="c3b27-136">You will see hello following page on successful completion of registration:</span></span>
   
      ![][11]

## <a name="create-a-baidu-cloud-push-project"></a><span data-ttu-id="c3b27-137">Skapa ett Baidu Cloud Push-projekt</span><span class="sxs-lookup"><span data-stu-id="c3b27-137">Create a Baidu cloud push project</span></span>
<span data-ttu-id="c3b27-138">När du skapar ett Baidu Cloud Push-projekt, kommer du att få ett app-ID, en API-nyckel och en hemlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="c3b27-138">When you create a Baidu cloud push project, you receive your app ID, API key, and secret key.</span></span>

1. <span data-ttu-id="c3b27-139">När du har loggat in toohello [Baidu-portalen], klickar du på**更多 >>** (**mer**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-139">Once you have logged in toohello [Baidu portal], click **更多>>** (**more**).</span></span>
   
      ![][5]
2. <span data-ttu-id="c3b27-140">Rulla nedåt i hello**站长与开发者服务**(**webbadministratör och utvecklare tjänster**) avsnittet och klicka på**百度开放云平台**(**Baidu öppna molnplattform**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-140">Scroll down in hello **站长与开发者服务** (**Webmaster and Developer Services**) section and click **百度开放云平台** (**Baidu open cloud platform**).</span></span>
   
      ![][6]
3. <span data-ttu-id="c3b27-141">Klicka på nästa sida hello**开发者服务**(**Developer Services**) på hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="c3b27-141">On hello next page, click **开发者服务** (**Developer Services**) on hello top-right corner.</span></span>
   
      ![][7]
4. <span data-ttu-id="c3b27-142">Klicka på nästa sida hello**云推送**(**Cloud Push**) från hello**云服务**(**molntjänster**) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c3b27-142">On hello next page, click **云推送** (**Cloud Push**) from hello **云服务** (**Cloud Services**) section.</span></span>
   
      ![][12]
5. <span data-ttu-id="c3b27-143">När du är registrerad utvecklare kan du se**管理控制台**(**hanteringskonsolen**) på hello översta menyn.</span><span class="sxs-lookup"><span data-stu-id="c3b27-143">Once you are a registered developer, you see **管理控制台** (**Management Console**) at hello top menu.</span></span> <span data-ttu-id="c3b27-144">Klicka på **开发者服务管理** (**Tjänstehantering för utvecklare**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-144">Click **开发者服务管理** (**Developers Service Management**).</span></span>
   
      ![][13]
6. <span data-ttu-id="c3b27-145">Klicka på nästa sida hello**创建工程**(**skapa projekt**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-145">On hello next page, click **创建工程** (**Create Project**).</span></span>
   
      ![][14]
7. <span data-ttu-id="c3b27-146">Ange ett programnamn och klicka på **创建** (**Skapa**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-146">Enter an application name and click **创建** (**Create**).</span></span>
   
      ![][15]
8. <span data-ttu-id="c3b27-147">När ett Baidu Cloud Push-projekt har skapats på rätt sätt, visas en sida med **AppID**, **API-nyckeln** och en **hemlig nyckel**.</span><span class="sxs-lookup"><span data-stu-id="c3b27-147">Upon successful creation of a Baidu cloud push project, you see a page with **AppID**, **API Key**, and **Secret Key**.</span></span> <span data-ttu-id="c3b27-148">Anteckna hello API-nyckel och hemlig nyckel som ska användas senare.</span><span class="sxs-lookup"><span data-stu-id="c3b27-148">Make a note of hello API key and secret key, which we will use later.</span></span>
   
      ![][16]
9. <span data-ttu-id="c3b27-149">Konfigurera hello projekt för push-meddelanden genom att klicka på**云推送**(**Cloud Push**) hello vänster.</span><span class="sxs-lookup"><span data-stu-id="c3b27-149">Configure hello project for push notifications by clicking **云推送** (**Cloud Push**) on hello left pane.</span></span>
   
      ![][31]
10. <span data-ttu-id="c3b27-150">Klicka på nästa sida hello hello**推送设置**(**Push inställningar**) knappen.</span><span class="sxs-lookup"><span data-stu-id="c3b27-150">On hello next page, click hello **推送设置** (**Push settings**) button.</span></span>
    
    ![][32]  
11. <span data-ttu-id="c3b27-151">Lägg till hello paketnamn som du ska använda i din Android-projekt i hello på konfigurationssidan för hello**应用包名**(**programpaket**) fält och klicka på**保存设置**() **Spara**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-151">On hello configuration page, add hello package name that you will be using in your Android project in hello **应用包名** (**Application package**) field, and then click **保存设置** (**Save**).</span></span>  
    
    ![][33]

<span data-ttu-id="c3b27-152">Du ser hello**保存成功!** (**Ändringarna har sparats!**) visas.</span><span class="sxs-lookup"><span data-stu-id="c3b27-152">You see hello **保存成功！** (**Successfully saved!**) message.</span></span>

## <a name="configure-your-notification-hub"></a><span data-ttu-id="c3b27-153">Konfigurera meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="c3b27-153">Configure your notification hub</span></span>
1. <span data-ttu-id="c3b27-154">Logga in toohello [klassiska Azure-portalen], och klicka sedan på **+ ny** längst hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="c3b27-154">Sign in toohello [Azure Classic Portal], and then click **+NEW** at hello bottom of hello screen.</span></span>
2. <span data-ttu-id="c3b27-155">Klicka på **Apptjänster**, sedan på**Service Bus** och därefter på **Meddelandehubb**. Slutligen klickar du på **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="c3b27-155">Click **App Services**, click **Service Bus**, click **Notification Hub**, and then click **Quick Create**.</span></span>
3. <span data-ttu-id="c3b27-156">Ange ett namn för din **Meddelandehubben**väljer hello **Region** och hello **Namespace** där den här meddelandehubben ska skapas och klicka sedan på  **Skapa en ny Meddelandehubb**.</span><span class="sxs-lookup"><span data-stu-id="c3b27-156">Provide a name for your **Notification Hub**, select hello **Region** and hello **Namespace** where this notification hub will be created, and then click **Create a New Notification Hub**.</span></span>  
   
      ![][17]
4. <span data-ttu-id="c3b27-157">Klicka på hello namnområde där du skapade din meddelandehubb och klicka sedan på **Meddelandehubbar** hello överst.</span><span class="sxs-lookup"><span data-stu-id="c3b27-157">Click hello namespace in which you created your notification hub, and then click **Notification Hubs** at hello top.</span></span>
   
      ![][18]
5. <span data-ttu-id="c3b27-158">Välj hello meddelandehubb som du skapat och klicka sedan på **konfigurera** hello översta menyn.</span><span class="sxs-lookup"><span data-stu-id="c3b27-158">Select hello notification hub that you created, and then click **Configure** from hello top menu.</span></span>
   
      ![][19]
6. <span data-ttu-id="c3b27-159">Bläddra nedåt toohello **meddelandeinställningar för baidu** och ange hello API-nyckel och hemliga nyckel som du fick från konsolen för hello Baidu tidigare för Baidu cloud push-projekt.</span><span class="sxs-lookup"><span data-stu-id="c3b27-159">Scroll down toohello **baidu notification settings** section and enter hello API key and secret key that you obtained from hello Baidu console previously for your Baidu cloud push project.</span></span> <span data-ttu-id="c3b27-160">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c3b27-160">Click **Save**.</span></span>
   
      ![][20]
7. <span data-ttu-id="c3b27-161">Klicka på hello **instrumentpanelen** fliken hello överst för meddelandehubben hello och klicka sedan på **visa anslutningssträng**.</span><span class="sxs-lookup"><span data-stu-id="c3b27-161">Click hello **Dashboard** tab at hello top for hello notification hub, and then click **View Connection String**.</span></span>
   
      ![][21]
8. <span data-ttu-id="c3b27-162">Anteckna hello **DefaultListenSharedAccessSignature** och **DefaultFullSharedAccessSignature** från hello **anslutningsinformation för åtkomst** fönster.</span><span class="sxs-lookup"><span data-stu-id="c3b27-162">Make a note of hello **DefaultListenSharedAccessSignature** and **DefaultFullSharedAccessSignature** from hello **Access connection information** window.</span></span>
   
    ![][22]

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="c3b27-163">Ansluta din app toohello notification hub</span><span class="sxs-lookup"><span data-stu-id="c3b27-163">Connect your app toohello notification hub</span></span>
1. <span data-ttu-id="c3b27-164">Skapa ett nytt Android-projekt i Eclipse ADT (**Arkiv** > **Ny** > **Android-approjekt**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-164">In Eclipse ADT, create a new Android project (**File** > **New** > **Android Application Project**).</span></span>
   
    ![][23]
2. <span data-ttu-id="c3b27-165">Ange en **programnamn** och se till att hello **minsta nödvändiga SDK** -versionen är inställd för**API 16: Android 4.1**.</span><span class="sxs-lookup"><span data-stu-id="c3b27-165">Enter an **Application Name** and ensure that hello **Minimum Required SDK** version is set too**API 16: Android 4.1**.</span></span>
   
    ![][24]
3. <span data-ttu-id="c3b27-166">Klicka på **nästa** och fortsätta följa hello guiden tills hello **Skapa aktivitet** visas.</span><span class="sxs-lookup"><span data-stu-id="c3b27-166">Click **Next** and continue following hello wizard until hello **Create Activity** window appears.</span></span> <span data-ttu-id="c3b27-167">Se till att **tom aktivitet** är markerad och välj slutligen **Slutför** toocreate ett nytt Android-program.</span><span class="sxs-lookup"><span data-stu-id="c3b27-167">Make sure that **Blank Activity** is selected, and finally select **Finish** toocreate a new Android Application.</span></span>
   
    ![][25]
4. <span data-ttu-id="c3b27-168">Kontrollera att hello **mål för projektgenerering** är korrekt.</span><span class="sxs-lookup"><span data-stu-id="c3b27-168">Make sure that hello **Project Build Target** is set correctly.</span></span>
   
    ![][26]
5. <span data-ttu-id="c3b27-169">Hämta filen för hello notification-hubs-0.4.jar från hello **filer** för hello [Notification-Hubs-Android-SDK på Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4).</span><span class="sxs-lookup"><span data-stu-id="c3b27-169">Download hello notification-hubs-0.4.jar file from hello **Files** tab of hello [Notification-Hubs-Android-SDK on Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4).</span></span> <span data-ttu-id="c3b27-170">Lägg till hello filen toohello **libs** i ditt Eclipse-projekt och uppdatera hello *libs* mapp.</span><span class="sxs-lookup"><span data-stu-id="c3b27-170">Add hello file toohello **libs** folder of your Eclipse project, and refresh hello *libs* folder.</span></span>
6. <span data-ttu-id="c3b27-171">Hämta och packa upp hello [Baidu Push Android SDK]öppnar hello **libs** mapp och sedan kopiera hello **pushservice x.y.z** jar-filen och hello **armeabi**  &  **mips** mappar i hello **libs** mappen för din Android-App.</span><span class="sxs-lookup"><span data-stu-id="c3b27-171">Download and unzip hello [Baidu Push Android SDK], open hello **libs** folder, and then copy hello **pushservice-x.y.z** jar file and hello **armeabi** & **mips** folders in hello **libs** folder of your Android application.</span></span>
7. <span data-ttu-id="c3b27-172">Öppna hello **AndroidManifest.xml** för ditt Android projekt och Lägg till hello behörigheter som krävs av hello Baidu-SDK.</span><span class="sxs-lookup"><span data-stu-id="c3b27-172">Open hello **AndroidManifest.xml** file of your Android project and add hello permissions that are required by hello Baidu SDK.</span></span>
   
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
8. <span data-ttu-id="c3b27-173">Lägg till hello **android: name** egenskapen tooyour **programmet** element i **AndroidManifest.xml**och ersätter *yourprojectname* (för exempelvis **com.example.BaiduTest**).</span><span class="sxs-lookup"><span data-stu-id="c3b27-173">Add hello **android:name** property tooyour **application** element in **AndroidManifest.xml**, replacing *yourprojectname* (for example, **com.example.BaiduTest**).</span></span> <span data-ttu-id="c3b27-174">Kontrollera att projektnamnet motsvarar hello något som du konfigurerade i hello Baidu-konsolen.</span><span class="sxs-lookup"><span data-stu-id="c3b27-174">Make sure that this project name matches hello one that you configured in hello Baidu console.</span></span>
   
        <application android:name="yourprojectname.DemoApplication"
9. <span data-ttu-id="c3b27-175">Lägg till följande konfiguration i hello programmet elementet efter hello hello **. MainActivity** genom ersätter *yourprojectname* (till exempel **com.example.BaiduTest**):</span><span class="sxs-lookup"><span data-stu-id="c3b27-175">Add hello following configuration within hello application element after hello **.MainActivity** activity element, replacing *yourprojectname* (for example, **com.example.BaiduTest**):</span></span>
   
        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>
   
        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>
10. <span data-ttu-id="c3b27-176">Lägg till en ny klass med namnet **ConfigurationSettings.java** toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="c3b27-176">Add a new class called **ConfigurationSettings.java** toohello project.</span></span>
    
     ![][28]
    
     ![][29]
11. <span data-ttu-id="c3b27-177">Lägg till följande kod tooit hello:</span><span class="sxs-lookup"><span data-stu-id="c3b27-177">Add hello following code tooit:</span></span>
    
        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }
    
    <span data-ttu-id="c3b27-178">Värdet för hello **API_KEY** med det du hämtade från Baidu cloud-projekt för hello tidigare **NotificationHubName** med ditt meddelandehubbsnamn från hello klassiska Azure-portalen och  **NotificationHubConnectionString** med DefaultListenSharedAccessSignature från hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c3b27-178">Set hello value of **API_KEY** with what you retrieved from hello Baidu cloud project earlier, **NotificationHubName** with your notification hub name from hello Azure Classic Portal and **NotificationHubConnectionString** with DefaultListenSharedAccessSignature from hello Azure Classic Portal.</span></span>
12. <span data-ttu-id="c3b27-179">Lägg till en ny klass med namnet **DemoApplication.java**, och Lägg till följande kod tooit hello:</span><span class="sxs-lookup"><span data-stu-id="c3b27-179">Add a new class called **DemoApplication.java**, and add hello following code tooit:</span></span>
    
        import com.baidu.frontia.FrontiaApplication;
    
        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }
13. <span data-ttu-id="c3b27-180">Lägg till en annan ny klass med namnet **MyPushMessageReceiver.java**, och Lägg till följande kod tooit hello.</span><span class="sxs-lookup"><span data-stu-id="c3b27-180">Add another new class called **MyPushMessageReceiver.java**, and add hello following code tooit.</span></span> <span data-ttu-id="c3b27-181">Det är hello-klassen som hanterar hello push-meddelanden som tas emot från hello Baidu push-servern.</span><span class="sxs-lookup"><span data-stu-id="c3b27-181">It is hello class that handles hello push notifications that are received from hello Baidu push server.</span></span>
    
        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;
    
        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG tooLog */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();
    
            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;
    
                try {
                    if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }
    
                registerWithNotificationHubs();
            }
    
            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                 + ConfigurationSettings.NotificationHubName + "'"
                                 + " with UserId - '"
                                 + mUserId + "' and Channel Id - '"
                                 + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }
    
            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }
    
            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }
14. <span data-ttu-id="c3b27-182">Öppna **MainActivity.java**, och Lägg till följande toohello hello **onCreate** metod:</span><span class="sxs-lookup"><span data-stu-id="c3b27-182">Open **MainActivity.java**, and add hello following toohello **onCreate** method:</span></span>
    
            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);
15. <span data-ttu-id="c3b27-183">Öppna följande importuttryck överst hello hello:</span><span class="sxs-lookup"><span data-stu-id="c3b27-183">Open hello following import statements at hello top:</span></span>
    
            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

## <a name="send-notifications-tooyour-app"></a><span data-ttu-id="c3b27-184">Skicka meddelanden tooyour appen</span><span class="sxs-lookup"><span data-stu-id="c3b27-184">Send notifications tooyour app</span></span>
<span data-ttu-id="c3b27-185">Du kan snabbt testa ta emot meddelanden i appen genom att skicka meddelanden i hello [Azure-portalen](https://portal.azure.com/) med hello **skicka** knappen på hello meddelandehubb som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="c3b27-185">You can quickly test receiving notifications in your app by sending notifications in hello [Azure portal](https://portal.azure.com/) using hello **Send** button on hello notification hub, as shown in hello following screen:</span></span>

![](./media/notification-hubs-baidu-get-started/notification-hub-test-send-baidu.png)

<span data-ttu-id="c3b27-186">Push-meddelanden skickas vanligtvis i en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek.</span><span class="sxs-lookup"><span data-stu-id="c3b27-186">Push notifications are normally sent in a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="c3b27-187">Om ett bibliotek inte är tillgänglig för din serverdel kan du använda hello REST API direkt toosend meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c3b27-187">If a library is not available for your back-end, you can use hello REST API directly toosend notification messages .</span></span>

<span data-ttu-id="c3b27-188">I den här självstudiekursen kommer vi enkelhet och hur du testar klientappen genom att skicka meddelanden med hello .NET SDK för meddelandehubbar i ett konsolprogram i stället för en serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="c3b27-188">In this tutorial, we keep it simple and just demonstrate testing your client app by sending notifications using hello .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="c3b27-189">Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) självstudiekurs som hello nästa steg för att skicka meddelanden från en ASP.NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="c3b27-189">We recommend hello [Use Notification Hubs toopush notifications toousers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) tutorial as hello next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="c3b27-190">Hello följande metoder kan dock användas för att skicka meddelanden:</span><span class="sxs-lookup"><span data-stu-id="c3b27-190">However, hello following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="c3b27-191">**REST-gränssnittet**: du kan använda meddelanden på alla backend-plattformar med hello [REST-gränssnittet](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3b27-191">**REST Interface**:  You can support notification on any backend platform using  hello [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="c3b27-192">**Microsoft Azure Notification Hubs .NET SDK**: hello Nuget Package Manager för Visual Studio, köra [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="c3b27-192">**Microsoft Azure Notification Hubs .NET SDK**: In hello Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="c3b27-193">**Node.js**: [hur toouse Notification Hubs från Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="c3b27-193">**Node.js**: [How toouse Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>
* <span data-ttu-id="c3b27-194">**Mobile Apps**: ett exempel på hur toosend meddelanden från en serverdel för Azure Apptjänst Mobilappar som är integrerad med Notification Hubs finns [Lägg till push-meddelanden tooyour mobilappen](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="c3b27-194">**Mobile Apps**: For an example of how toosend notifications from an Azure App Service Mobile Apps backend that's integrated with Notification Hubs, see [Add push notifications tooyour mobile app](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span></span>
* <span data-ttu-id="c3b27-195">**Java / PHP**: ett exempel på hur hello toosend meddelanden med hjälp av REST API: er, se ”hur toouse Notification Hubs från Java/PHP” ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="c3b27-195">**Java / PHP**: For an example of how toosend notifications by using hello REST APIs, see "How toouse Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

## <a name="optional-send-notifications-from-a-net-console-app"></a><span data-ttu-id="c3b27-196">(Valfritt) Skicka meddelanden från en .NET-konsolapp</span><span class="sxs-lookup"><span data-stu-id="c3b27-196">(Optional) Send notifications from a .NET console app.</span></span>
<span data-ttu-id="c3b27-197">I det här avsnittet kommer du att lära dig hur du skickar meddelanden med hjälp av en .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="c3b27-197">In this section, we show sending a notification using a .NET console app.</span></span>

1. <span data-ttu-id="c3b27-198">Skapa en ny Visual C#-konsolapp:</span><span class="sxs-lookup"><span data-stu-id="c3b27-198">Create a new Visual C# console application:</span></span>
   
    ![][30]
2. <span data-ttu-id="c3b27-199">Hello fönstret Package Manager-konsolen, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:</span><span class="sxs-lookup"><span data-stu-id="c3b27-199">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="c3b27-200">Den här instruktionen lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.</span><span class="sxs-lookup"><span data-stu-id="c3b27-200">This instruction adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
3. <span data-ttu-id="c3b27-201">Öppna hello filen **Program.cs** och Lägg till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="c3b27-201">Open hello file **Program.cs** and add hello following using statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
4. <span data-ttu-id="c3b27-202">I din `Program` klassen, Lägg till följande metod hello och ersätta *DefaultFullSharedAccessSignatureSASConnectionString* och *NotificationHubName* med hello-värden som du har.</span><span class="sxs-lookup"><span data-stu-id="c3b27-202">In your `Program` class, add hello following method and replace *DefaultFullSharedAccessSignatureSASConnectionString* and *NotificationHubName* with hello values that you have.</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }
5. <span data-ttu-id="c3b27-203">Lägg till följande rader i hello din **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="c3b27-203">Add hello following lines in your **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();

## <a name="test-your-app"></a><span data-ttu-id="c3b27-204">Testa din app</span><span class="sxs-lookup"><span data-stu-id="c3b27-204">Test your app</span></span>
<span data-ttu-id="c3b27-205">den här appen med en riktig mobil bara ansluta tootest hello phone tooyour datorn med hjälp av en USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="c3b27-205">tootest this app with an actual phone, just connect hello phone tooyour computer by using a USB cable.</span></span> <span data-ttu-id="c3b27-206">Den här åtgärden läser din app till hello kopplade phone.</span><span class="sxs-lookup"><span data-stu-id="c3b27-206">This action loads your app onto hello attached phone.</span></span>

<span data-ttu-id="c3b27-207">tootest appen med emulatorn hello hello Eclipse översta verktygsfältet klickar du på **kör**, och välj sedan din app: startar hello emulator, belastning och kör hello app.</span><span class="sxs-lookup"><span data-stu-id="c3b27-207">tootest this app with hello emulator, on hello Eclipse top toolbar, click **Run**, and then select your app: it starts hello emulator, loads, and runs hello app.</span></span>

<span data-ttu-id="c3b27-208">hello appen hämtar hello ”userId” och ”channelId” från hello Baidu Push notification service och registrerar hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="c3b27-208">hello app retrieves hello 'userId' and 'channelId' from hello Baidu Push notification service and registers with hello notification hub.</span></span>

<span data-ttu-id="c3b27-209">Du kan använda hello felsökningsfliken av hello Azure klassiska Portal toosend ett testmeddelande.</span><span class="sxs-lookup"><span data-stu-id="c3b27-209">toosend a test notification, you can use hello debug tab of hello Azure Classic Portal.</span></span> <span data-ttu-id="c3b27-210">Om du har byggt hello .NET-konsolprogram för Visual Studio bara trycka hello F5 i Visual Studio toorun hello-program.</span><span class="sxs-lookup"><span data-stu-id="c3b27-210">If you built hello .NET console application for Visual Studio, just press hello F5 key in Visual Studio toorun hello application.</span></span> <span data-ttu-id="c3b27-211">hello skickar ett meddelande som visas i hello övre meddelandefältet i enheten eller emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c3b27-211">hello application sends a notification that appears in hello top notification area of your device or emulator.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[klassiska Azure-portalen]: https://manage.windowsazure.com/
[Baidu-portalen]: http://www.baidu.com/
