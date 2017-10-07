---
title: "aaaSign i instruktioner för hello Azure Toolkit för IntelliJ | Microsoft Docs"
description: "Lär dig hur toosign i tooMicrosoft Azure med hjälp av hello Azure Toolkit för IntelliJ."
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
ms.openlocfilehash: 2de781fc19267cce133b1e6456481497e165fce4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-instructions-for-hello-azure-toolkit-for-intellij"></a><span data-ttu-id="7091d-103">Logga in instruktioner för hello Azure Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="7091d-103">Sign-in instructions for hello Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="7091d-104">hello Azure Toolkit för IntelliJ erbjuder två metoder för att logga in tooyour Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="7091d-104">hello Azure Toolkit for IntelliJ provides two methods for signing in tooyour Azure account:</span></span>

  * <span data-ttu-id="7091d-105">**Interaktiva**: du anger dina autentiseringsuppgifter för Azure varje gång du loggar in i tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="7091d-105">**Interactive**: You enter your Azure credentials each time you sign in tooyour Azure account.</span></span>
  * <span data-ttu-id="7091d-106">**Automatisk**: du skapar en fil för autentiseringsuppgifter som du kan använda tooautomatically logga i tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="7091d-106">**Automated**: You create a credentials file that you can use tooautomatically sign in tooyour Azure account.</span></span>

<span data-ttu-id="7091d-107">hello följande avsnitt beskrivs hur toouse varje metod.</span><span class="sxs-lookup"><span data-stu-id="7091d-107">hello following sections describe how toouse each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-tooyour-azure-account-interactively"></a><span data-ttu-id="7091d-108">Logga in tooyour Azure-konto interaktivt</span><span class="sxs-lookup"><span data-stu-id="7091d-108">Sign in tooyour Azure account interactively</span></span>

<span data-ttu-id="7091d-109">toosign i tooAzure manuellt genom att ange dina autentiseringsuppgifter för Azure hello följande:</span><span class="sxs-lookup"><span data-stu-id="7091d-109">toosign in tooAzure by manually entering your Azure credentials, do hello following:</span></span>

1. <span data-ttu-id="7091d-110">Öppna projektet med IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="7091d-110">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="7091d-111">Klicka på **verktyg**, peka för**Azure**, och klicka sedan på **Azure logga In**.</span><span class="sxs-lookup"><span data-stu-id="7091d-111">Click **Tools**, point too**Azure**, and then click **Azure Sign In**.</span></span>

   ![Hej IntelliJ Azure logga In kommandot][I01]

3. <span data-ttu-id="7091d-113">I hello **Azure logga In** väljer **interaktiv**, och klicka sedan på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="7091d-113">In hello **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![hello Azure logga In fönster med interaktiv markerat][I02]

4. <span data-ttu-id="7091d-115">I hello **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="7091d-115">In hello **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![hello Azure inloggningsruta fönster][I03]

5. <span data-ttu-id="7091d-117">I hello **Välj prenumerationer** dialogrutan, Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7091d-117">In hello **Select Subscriptions** dialog box, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![dialogrutan Välj prenumerationer för hello][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="7091d-119">Logga ut från ditt Azure-konto när du har loggat in interaktivt</span><span class="sxs-lookup"><span data-stu-id="7091d-119">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="7091d-120">När du har konfigurerat ditt konto med hjälp av hello föregående steg, kommer du automatiskt loggas ut från varje gång du startar om IntelliJ IDEA ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="7091d-120">After you have configured your account by using hello preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="7091d-121">Men om du vill toosign utanför ditt Azure-konto *utan* omstart IntelliJ IDEA hello följande.</span><span class="sxs-lookup"><span data-stu-id="7091d-121">However, if you want toosign out of your Azure account *without* restarting IntelliJ IDEA, do hello following.</span></span>

1. <span data-ttu-id="7091d-122">I IntelliJ IDEA på hello **verktyg** -menyn, peka för**Azure**, och klicka sedan på **Azure logga ut**.</span><span class="sxs-lookup"><span data-stu-id="7091d-122">In IntelliJ IDEA, on hello **Tools** menu, point too**Azure**, and then click **Azure Sign Out**.</span></span>

   ![Hej IntelliJ Azure logga ut kommando][L01]

2. <span data-ttu-id="7091d-124">I hello **Azure logga ut** bekräftelsefönstret, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="7091d-124">In hello **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![hello Azure logga ut bekräftelsefönstret][L02]

## <a name="sign-in-tooyour-azure-account-automatically"></a><span data-ttu-id="7091d-126">Logga in tooyour Azure-konto automatiskt</span><span class="sxs-lookup"><span data-stu-id="7091d-126">Sign in tooyour Azure account automatically</span></span>

<span data-ttu-id="7091d-127">Det här avsnittet vägleder dig genom att skapa en fil med autentiseringsuppgifter som innehåller data för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="7091d-127">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="7091d-128">När du har slutfört den här processen, öppna projektet i Eclipse använder hello autentiseringsuppgifter filen tooautomatically logga i tooAzure varje gång du.</span><span class="sxs-lookup"><span data-stu-id="7091d-128">After you have completed this process, Eclipse uses hello credentials file tooautomatically sign you in tooAzure each time you open your project.</span></span>

1. <span data-ttu-id="7091d-129">Öppna projektet med IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="7091d-129">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="7091d-130">På hello **verktyg** -menyn, peka för**Azure**, och klicka sedan på **Azure logga In**.</span><span class="sxs-lookup"><span data-stu-id="7091d-130">On hello **Tools** menu, point too**Azure**, and then click **Azure Sign In**.</span></span>

   ![Hej IntelliJ Azure logga In kommandot][A01]

3. <span data-ttu-id="7091d-132">I hello **Azure logga In** väljer **automatisk**, och klicka sedan på **ny**.</span><span class="sxs-lookup"><span data-stu-id="7091d-132">In hello **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![hello Azure logga In fönster med automatisk markerat][A02]

4. <span data-ttu-id="7091d-134">I hello **Azure inloggningsruta** och ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="7091d-134">In hello **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![hello Azure inloggningsruta fönster][A03]

5. <span data-ttu-id="7091d-136">I hello **skapa autentiseringsfiler** fönster, Välj hello prenumerationer som du vill toouse en målkatalog och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="7091d-136">In hello **Create Authentication Files** window, select hello subscriptions that you want toouse, choose your destination directory, and then click **Start**.</span></span>

   ![hello skapa autentiseringsfiler fönster][A04]

6. <span data-ttu-id="7091d-138">I hello **tjänstens huvudnamn skapa Status** dialogrutan när filerna har skapats, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7091d-138">In hello **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![hello dialogrutan Skapa Status för tjänstens huvudnamn][A05]

7. <span data-ttu-id="7091d-140">I hello **Azure logga In** -fönstret klickar du på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="7091d-140">In hello **Azure Sign In** window, click **Sign in**.</span></span>

   ![Dialogruta i Azure][A06]

8. <span data-ttu-id="7091d-142">I hello **Välj prenumerationer** dialogrutan, Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7091d-142">In hello **Select Subscriptions** dialog box, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![dialogrutan Välj prenumerationer för hello][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="7091d-144">Logga ut från ditt Azure-konto när du har loggat in automatiskt</span><span class="sxs-lookup"><span data-stu-id="7091d-144">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="7091d-145">När du har konfigurerat ditt konto med hjälp av hello föregående steg, loggar hello Azure Toolkit du automatiskt in tooyour varje gång du startar om IntelliJ IDEA Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="7091d-145">After you have configured your account by using hello preceding steps, hello Azure Toolkit automatically signs you in tooyour Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="7091d-146">Dock toosign ut av ditt Azure-konto och förhindra hello Azure Toolkit från att logga in automatiskt, hello följande:</span><span class="sxs-lookup"><span data-stu-id="7091d-146">However, toosign out of your Azure account and prevent hello Azure Toolkit from signing you in automatically, do hello following:</span></span>

1. <span data-ttu-id="7091d-147">I IntelliJ IDEA på hello **verktyg** -menyn, peka för**Azure**, och klicka sedan på **Azure logga ut**.</span><span class="sxs-lookup"><span data-stu-id="7091d-147">In IntelliJ IDEA, on hello **Tools** menu, point too**Azure**, and then click **Azure Sign Out**.</span></span>

   ![Hej IntelliJ Azure logga ut kommando][L01]

2. <span data-ttu-id="7091d-149">I hello **Azure logga ut** bekräftelsefönstret, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="7091d-149">In hello **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![hello Azure logga ut bekräftelsefönstret][L03]

## <a name="sign-in-tooyour-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="7091d-151">Logga in tooyour Azure-konto automatiskt med hjälp av en befintlig fil för autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="7091d-151">Sign in tooyour Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="7091d-152">Om du logga ut från ditt Azure-konto när du använder IntelliJ IDEA, måste du använda en befintlig autentiseringsuppgifter filen tooautomatically logga in toohello konto igen.</span><span class="sxs-lookup"><span data-stu-id="7091d-152">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file tooautomatically sign back in toohello account.</span></span> <span data-ttu-id="7091d-153">tooconfigure hello Azure Toolkit för Eclipse toouse en befintlig fil i autentiseringsuppgifter hello följande:</span><span class="sxs-lookup"><span data-stu-id="7091d-153">tooconfigure hello Azure Toolkit for Eclipse toouse an existing credentials file, do hello following:</span></span>

1. <span data-ttu-id="7091d-154">Öppna projektet med IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="7091d-154">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="7091d-155">På hello **verktyg** -menyn, peka för**Azure**, och klicka sedan på **Azure logga In**.</span><span class="sxs-lookup"><span data-stu-id="7091d-155">On hello **Tools** menu, point too**Azure**, and then click **Azure Sign In**.</span></span>

   ![Hej IntelliJ Azure logga In kommandot][A01]

3. <span data-ttu-id="7091d-157">I hello **Azure logga In** väljer **automatisk**, och klicka sedan på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="7091d-157">In hello **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![hello Azure logga In fönster med automatisk markerat][A02]

4. <span data-ttu-id="7091d-159">I hello **Välj autentiseringsfilen** dialogrutan Välj en fil som skapats tidigare autentiseringsuppgifter och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="7091d-159">In hello **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![dialogrutan för hello Välj fil för autentisering][A08]

5. <span data-ttu-id="7091d-161">I hello **Azure logga In** -fönstret klickar du på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="7091d-161">In hello **Azure Sign In** window, click **Sign in**.</span></span>

   ![hello Azure logga In fönster med automatisk markerat][A06]

6. <span data-ttu-id="7091d-163">I hello **Välj prenumerationer** dialogrutan, Välj hello prenumerationer som du vill toouse och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7091d-163">In hello **Select Subscriptions** dialog box, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![dialogrutan Välj prenumerationer för hello][A07]

## <a name="next-steps"></a><span data-ttu-id="7091d-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7091d-165">Next steps</span></span>
<span data-ttu-id="7091d-166">Mer information om hello Azure-verktyg för Java IDEs finns hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="7091d-166">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="7091d-167">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7091d-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="7091d-168">[Vad är nytt i hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7091d-168">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="7091d-169">[Installera hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7091d-169">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="7091d-170">[Logga in anvisningar hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7091d-170">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="7091d-171">[Skapa en Hello World-webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="7091d-171">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="7091d-172">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="7091d-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="7091d-173">[Vad är nytt i hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="7091d-173">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="7091d-174">[Installera hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="7091d-174">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="7091d-175">*Logga in instruktioner för hello Azure Toolkit för IntelliJ* (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="7091d-175">*Sign-in instructions for hello Azure Toolkit for IntelliJ* (this article)</span></span>
  * <span data-ttu-id="7091d-176">[Skapa en Hello World-webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="7091d-176">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="7091d-177">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center] och hello [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="7091d-177">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga in anvisningar hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Sign-in instructions for hello Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
