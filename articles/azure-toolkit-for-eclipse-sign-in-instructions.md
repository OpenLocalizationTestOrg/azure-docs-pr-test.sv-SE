---
title: "aaaSign i instruktioner för hello Azure Toolkit för Eclipse | Microsoft Docs"
description: "Lär dig hur toosign i Microsoft Azure med hjälp av hello Azure Toolkit för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 95be64750ca0147f76dae8f364fad80cb9ccc969
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sign-in-instructions-for-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="ab383-103">Azure logga i instruktioner för hello Azure Toolkit för Eclipse</span><span class="sxs-lookup"><span data-stu-id="ab383-103">Azure Sign In Instructions for hello Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="ab383-104">hello Azure Toolkit för Eclipse erbjuder två metoder för att logga in på ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="ab383-104">hello Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  * <span data-ttu-id="ab383-105">**Interaktiva** – när du använder den här metoden ska du ange dina autentiseringsuppgifter för Azure varje gång du loggar in i ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ab383-105">**Interactive** - when you are using this method, you will enter your Azure credentials each time you sign into your Azure account.</span></span>
  * <span data-ttu-id="ab383-106">**Automatisk** – när du använder den här metoden skapar du en fil med autentiseringsuppgifter som innehåller service principal data, efter vilken du kan använda hello autentiseringsuppgifter filen tooautomatically loggar till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ab383-106">**Automated** - when you are using this method, you will create a credentials file which contains your service principal data, after which you can use hello credentials file tooautomatically sign into your Azure account.</span></span>

<span data-ttu-id="ab383-107">hello stegen i hello följande avsnitt beskriver hur toouse varje metod.</span><span class="sxs-lookup"><span data-stu-id="ab383-107">hello steps in hello following sections will describe how toouse each method.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a><span data-ttu-id="ab383-108">Logga in på ditt Azure-konto interaktivt</span><span class="sxs-lookup"><span data-stu-id="ab383-108">Signing into your Azure account interactively</span></span>

<span data-ttu-id="ab383-109">hello följande illustrerar hur toosign till Azure genom att manuellt ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="ab383-109">hello following steps will illustrate how toosign into Azure by manually entering your Azure credentials.</span></span>

1. <span data-ttu-id="ab383-110">Öppna projektet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ab383-110">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="ab383-111">Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ab383-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse-menyn för Azure inloggning][I01]

1. <span data-ttu-id="ab383-113">När hello **Azure logga In** dialogrutan Välj **interaktiv**, och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ab383-113">When hello **Azure Sign In** dialog box appears, select **Interactive**, and then click **Sign In**.</span></span>

   ![Logga In dialogrutan][I02]

1. <span data-ttu-id="ab383-115">När hello **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ab383-115">When hello **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Dialogruta i Azure][I03]

1. <span data-ttu-id="ab383-117">När hello **Välj prenumerationer** dialogrutan Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab383-117">When hello **Select Subscriptions** dialog box appears, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Dialogrutan Välj prenumerationer][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a><span data-ttu-id="ab383-119">Logga ut från ditt Azure-konto när du loggar in interaktivt</span><span class="sxs-lookup"><span data-stu-id="ab383-119">Signing out of your Azure account when you signed in interactively</span></span>

<span data-ttu-id="ab383-120">När du har konfigurerat hello stegen i föregående hello-avsnittet, kommer du automatiskt loggas ut från ditt Azure-konto varje gång du startar om Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ab383-120">After you have configured hello steps in hello previous section, you will automatically signed out of your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="ab383-121">Om du vill toosign utanför ditt Azure-konto utan att starta om Eclipse kan du dock använda hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="ab383-121">However, if you want toosign out of your Azure account without restarting Eclipse, use hello following steps.</span></span>

1. <span data-ttu-id="ab383-122">I Eclipse klickar du på **verktyg**, klicka på **Azure**, och klicka sedan på **logga ut**.</span><span class="sxs-lookup"><span data-stu-id="ab383-122">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Eclipse-menyn för Azure logga ut][L01]

1. <span data-ttu-id="ab383-124">När hello **Azure logga ut** dialogrutan visas klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="ab383-124">When hello **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Logga ut dialogrutan][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-toouse-in-hello-future"></a><span data-ttu-id="ab383-126">Logga in på ditt Azure-konto automatiskt och skapa ett autentiseringsuppgifter filen toouse i hello framtida</span><span class="sxs-lookup"><span data-stu-id="ab383-126">Signing into your Azure account automatically and creating a credentials file toouse in hello future</span></span>

<span data-ttu-id="ab383-127">hello får följande steg du hjälp med att skapa en fil med autentiseringsuppgifter som innehåller dina viktigaste data service.</span><span class="sxs-lookup"><span data-stu-id="ab383-127">hello following steps will walk you through creating a credentials file which contains your service principal data.</span></span> <span data-ttu-id="ab383-128">När du har slutfört de här stegen, Eclipse kommer automatiskt öppnar Använd hello autentiseringsuppgifter filen tooautomatically inloggning till Azure varje gång du projektet.</span><span class="sxs-lookup"><span data-stu-id="ab383-128">Once you have completed these steps, Eclipse will automatically use hello credentials file tooautomatically sign you into Azure each time you open your project.</span></span>

1. <span data-ttu-id="ab383-129">Öppna projektet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ab383-129">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="ab383-130">Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ab383-130">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse-menyn för Azure inloggning][A01]

1. <span data-ttu-id="ab383-132">När hello **Azure logga In** dialogrutan Välj **automatisk**, och klicka sedan på **ny**.</span><span class="sxs-lookup"><span data-stu-id="ab383-132">When hello **Azure Sign In** dialog box appears, select **Automated**, and then click **New**.</span></span>

   ![Logga In dialogrutan][A02]

1. <span data-ttu-id="ab383-134">När hello **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ab383-134">When hello **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Dialogruta i Azure][A03]

1. <span data-ttu-id="ab383-136">När hello **skapa autentiseringsfiler** dialogrutan Välj hello prenumerationer som du vill toouse en målkatalog och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="ab383-136">When hello **Create authentication files** dialog box appears, select hello subscriptions that you want toouse, choose your destination directory, and then click **Start**.</span></span>

   ![Dialogruta i Azure][A04]

1. <span data-ttu-id="ab383-138">Hej **tjänstens huvudnamn Creatation Status** dialogrutan visas, och när filerna har skapats, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab383-138">hello **Service Principal Creatation Status** dialog box will be displayed, and after your files have been created successfully, click **OK**.</span></span>

   ![Dialogrutan tjänstens huvudnamn Creatation Status][A05]

1. <span data-ttu-id="ab383-140">När hello **Azure logga In** dialogrutan visas klickar du på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ab383-140">When hello **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Dialogruta i Azure][A06]

1. <span data-ttu-id="ab383-142">När hello **Välj prenumerationer** dialogrutan Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab383-142">When hello **Select Subscriptions** dialog box appears, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a><span data-ttu-id="ab383-144">Logga ut från ditt Azure-konto när du loggar in automatiskt</span><span class="sxs-lookup"><span data-stu-id="ab383-144">Signing out of your Azure account when you signed in automatically</span></span>

<span data-ttu-id="ab383-145">När du har konfigurerat hello stegen i föregående avsnitt i hello, loggas hello Azure Toolkit du automatiskt till ditt Azure-konto varje gång du startar om Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ab383-145">After you have configured hello steps in hello previous section, hello Azure Toolkit will automatically sign you into your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="ab383-146">Toosign utanför ditt Azure-konto och förhindra att hello Azure Toolkit loggar du in automatiskt, Använd hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="ab383-146">However, toosign out of your Azure account and prevent hello Azure Toolkit from signing you in automatically, use hello following steps.</span></span>

1. <span data-ttu-id="ab383-147">I Eclipse klickar du på **verktyg**, klicka på **Azure**, och klicka sedan på **logga ut**.</span><span class="sxs-lookup"><span data-stu-id="ab383-147">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Eclipse-menyn för Azure logga ut][L01]

1. <span data-ttu-id="ab383-149">När hello **Azure logga ut** dialogrutan visas klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="ab383-149">When hello **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Logga ut dialogrutan][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a><span data-ttu-id="ab383-151">När du loggar in automatiskt med hjälp av en fil med autentiseringsuppgifter som du redan har skapat ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="ab383-151">Signing into your Azure account automatically using a credentials file which you have already created</span></span>

<span data-ttu-id="ab383-152">Om du loggar ut ur Azure när du använder Eclipse behöver tooreconfigure hello Azure Toolkit för Eclipse toouse en fil med autentiseringsuppgifter som har skapats innan du automatiskt kan logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ab383-152">If you sign out of Azure when you are using Eclipse, you will need tooreconfigure hello Azure Toolkit for Eclipse toouse a credentials file which have created before you can automatically sign into your Azure acccount.</span></span> <span data-ttu-id="ab383-153">hello får följande steg du konfigurerar hello Azure Toolkit toouse en befintlig fil för autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ab383-153">hello following steps will walk you through configuring hello Azure Toolkit toouse an existing credentials file.</span></span>

1. <span data-ttu-id="ab383-154">Öppna projektet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="ab383-154">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="ab383-155">Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ab383-155">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse-menyn för Azure inloggning][A01]

1. <span data-ttu-id="ab383-157">När hello **Azure logga In** dialogrutan Välj **automatisk**, och klicka sedan på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="ab383-157">When hello **Azure Sign In** dialog box appears, select **Automated**, and then click **Browse**.</span></span>

   ![Logga In dialogrutan][A02]

1. <span data-ttu-id="ab383-159">När hello **Välj autentiserad fil** visas dialogrutan Välj en fil med autentiseringsuppgifter som du skapade tidigare och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="ab383-159">When hello **Select Authenticated File** dialog box appears, select a credentials file which you created earlier, and then click **Select**.</span></span>

   ![Logga In dialogrutan][A08]

1. <span data-ttu-id="ab383-161">När hello **Azure logga In** dialogrutan visas klickar du på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="ab383-161">When hello **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Dialogruta i Azure][A06]

1. <span data-ttu-id="ab383-163">När hello **Välj prenumerationer** dialogrutan Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab383-163">When hello **Select Subscriptions** dialog box appears, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="see-also"></a><span data-ttu-id="ab383-165">Se även</span><span class="sxs-lookup"><span data-stu-id="ab383-165">See Also</span></span>
<span data-ttu-id="ab383-166">Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="ab383-166">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="ab383-167">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="ab383-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="ab383-168">[Vad är nytt i hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="ab383-168">[What's New in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="ab383-169">[Installera hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="ab383-169">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="ab383-170">*Logga i instruktioner för hello Azure Toolkit för Eclipse (den här artikeln)*</span><span class="sxs-lookup"><span data-stu-id="ab383-170">*Sign In Instructions for hello Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="ab383-171">[Skapa en Hello World-Webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="ab383-171">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="ab383-172">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ab383-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="ab383-173">[Vad är nytt i hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ab383-173">[What's New in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="ab383-174">[Installera hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ab383-174">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="ab383-175">[Logga i instruktioner för hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ab383-175">[Sign In Instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="ab383-176">[Skapa en Hello World-Webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ab383-176">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="ab383-177">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="ab383-177">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Sign In Instructions for hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga i instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java-utvecklingscenter]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
