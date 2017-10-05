---
title: "Logga in instruktioner för Azure-Toolkit för IntelliJ | Microsoft Docs"
description: "Lär dig mer om att logga in på Microsoft Azure med hjälp av Azure-verktyget för IntelliJ."
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
ms.openlocfilehash: 4e2ed072bdaea0a71fef042c0c72b7656a42bbe8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a><span data-ttu-id="af33f-103">Logga in instruktioner för Azure-Toolkit för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="af33f-103">Sign-in instructions for the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="af33f-104">Azure-verktygen för IntelliJ erbjuder två metoder för att logga in på ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="af33f-104">The Azure Toolkit for IntelliJ provides two methods for signing in to your Azure account:</span></span>

  * <span data-ttu-id="af33f-105">**Interaktiva**: du anger dina autentiseringsuppgifter för Azure varje gång du loggar in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="af33f-105">**Interactive**: You enter your Azure credentials each time you sign in to your Azure account.</span></span>
  * <span data-ttu-id="af33f-106">**Automatisk**: du skapar en fil med autentiseringsuppgifter som du kan använda för att automatiskt logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="af33f-106">**Automated**: You create a credentials file that you can use to automatically sign in to your Azure account.</span></span>

<span data-ttu-id="af33f-107">I följande avsnitt beskrivs hur du använder varje metod.</span><span class="sxs-lookup"><span data-stu-id="af33f-107">The following sections describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-interactively"></a><span data-ttu-id="af33f-108">Logga in interaktivt på ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="af33f-108">Sign in to your Azure account interactively</span></span>

<span data-ttu-id="af33f-109">Om du vill logga in på Azure manuellt genom att ange dina autentiseringsuppgifter för Azure, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="af33f-109">To sign in to Azure by manually entering your Azure credentials, do the following:</span></span>

1. <span data-ttu-id="af33f-110">Öppna projektet med IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="af33f-110">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="af33f-111">Klicka på **verktyg**, peka på **Azure**, och klicka sedan på **Azure logga In**.</span><span class="sxs-lookup"><span data-stu-id="af33f-111">Click **Tools**, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Kommandot IntelliJ Azure logga In][I01]

3. <span data-ttu-id="af33f-113">I den **Azure logga In** väljer **interaktiv**, och klicka sedan på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="af33f-113">In the **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![Fönstret Azure logga In med interaktiv markerat][I02]

4. <span data-ttu-id="af33f-115">I den **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="af33f-115">In the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Dialogrutan för Azure-inloggning][I03]

5. <span data-ttu-id="af33f-117">I den **Välj prenumerationer** dialogrutan, Välj prenumerationer som du vill använda och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="af33f-117">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Dialogrutan Välj prenumerationer][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="af33f-119">Logga ut från ditt Azure-konto när du har loggat in interaktivt</span><span class="sxs-lookup"><span data-stu-id="af33f-119">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="af33f-120">När du har konfigurerat ditt konto med hjälp av föregående steg, kommer du automatiskt loggas ut från varje gång du startar om IntelliJ IDEA ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="af33f-120">After you have configured your account by using the preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="af33f-121">Men om du vill logga ut från ditt Azure-konto *utan* omstart IntelliJ IDEA gör du följande.</span><span class="sxs-lookup"><span data-stu-id="af33f-121">However, if you want to sign out of your Azure account *without* restarting IntelliJ IDEA, do the following.</span></span>

1. <span data-ttu-id="af33f-122">I IntelliJ IDEA på den **verktyg** menyn, peka på **Azure**, och klicka sedan på **Azure logga ut**.</span><span class="sxs-lookup"><span data-stu-id="af33f-122">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![Kommandot IntelliJ Azure logga ut][L01]

2. <span data-ttu-id="af33f-124">I den **Azure logga ut** bekräftelsefönstret, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="af33f-124">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Bekräftelsefönstret Azure logga ut][L02]

## <a name="sign-in-to-your-azure-account-automatically"></a><span data-ttu-id="af33f-126">Logga in på kontot automatiskt</span><span class="sxs-lookup"><span data-stu-id="af33f-126">Sign in to your Azure account automatically</span></span>

<span data-ttu-id="af33f-127">Det här avsnittet vägleder dig genom att skapa en fil med autentiseringsuppgifter som innehåller data för tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="af33f-127">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="af33f-128">När du har slutfört den här processen använder filen autentiseringsuppgifter automatiskt logga in dig till Azure varje gång du öppnar projektet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="af33f-128">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure each time you open your project.</span></span>

1. <span data-ttu-id="af33f-129">Öppna projektet med IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="af33f-129">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="af33f-130">På den **verktyg** menyn, peka på **Azure**, och klicka sedan på **Azure logga In**.</span><span class="sxs-lookup"><span data-stu-id="af33f-130">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Kommandot IntelliJ Azure logga In][A01]

3. <span data-ttu-id="af33f-132">I den **Azure logga In** väljer **automatisk**, och klicka sedan på **ny**.</span><span class="sxs-lookup"><span data-stu-id="af33f-132">In the **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![Fönstret Azure logga In med automatisk markerat][A02]

4. <span data-ttu-id="af33f-134">I den **Azure inloggningsruta** och ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="af33f-134">In the **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Dialogrutan för Azure-inloggning][A03]

5. <span data-ttu-id="af33f-136">I den **skapa autentiseringsfiler** fönster, Välj den prenumeration som du vill använda en målkatalog och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="af33f-136">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![Fönstret Skapa filer för autentisering][A04]

6. <span data-ttu-id="af33f-138">I den **tjänstens huvudnamn skapa Status** dialogrutan när filerna har skapats, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="af33f-138">In the **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![Dialogrutan Skapa Status för tjänstens huvudnamn][A05]

7. <span data-ttu-id="af33f-140">I den **Azure logga In** -fönstret klickar du på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="af33f-140">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![Dialogruta i Azure][A06]

8. <span data-ttu-id="af33f-142">I den **Välj prenumerationer** dialogrutan, Välj prenumerationer som du vill använda och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="af33f-142">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="af33f-144">Logga ut från ditt Azure-konto när du har loggat in automatiskt</span><span class="sxs-lookup"><span data-stu-id="af33f-144">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="af33f-145">När du har konfigurerat ditt konto med hjälp av föregående steg, loggar Azure Toolkit automatiskt du in på ditt Azure-konto varje gång du startar om IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="af33f-145">After you have configured your account by using the preceding steps, the Azure Toolkit automatically signs you in to your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="af33f-146">Men om du vill logga ut från ditt Azure-konto och förhindrar att logga in automatiskt i Azure Toolkit gör du följande:</span><span class="sxs-lookup"><span data-stu-id="af33f-146">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, do the following:</span></span>

1. <span data-ttu-id="af33f-147">I IntelliJ IDEA på den **verktyg** menyn, peka på **Azure**, och klicka sedan på **Azure logga ut**.</span><span class="sxs-lookup"><span data-stu-id="af33f-147">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![Kommandot IntelliJ Azure logga ut][L01]

2. <span data-ttu-id="af33f-149">I den **Azure logga ut** bekräftelsefönstret, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="af33f-149">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Bekräftelsefönstret Azure logga ut][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="af33f-151">Logga in på kontot automatiskt med hjälp av en befintlig fil för autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="af33f-151">Sign in to your Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="af33f-152">Om du logga ut från ditt Azure-konto när du använder IntelliJ IDEA, måste du använda en befintlig fil autentiseringsuppgifter automatiskt logga in igen på kontot.</span><span class="sxs-lookup"><span data-stu-id="af33f-152">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file to automatically sign back in to the account.</span></span> <span data-ttu-id="af33f-153">Konfigurera Azure-verktygen för Eclipse gör följande att använda en befintlig fil med autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="af33f-153">To configure the Azure Toolkit for Eclipse to use an existing credentials file, do the following:</span></span>

1. <span data-ttu-id="af33f-154">Öppna projektet med IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="af33f-154">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="af33f-155">På den **verktyg** menyn, peka på **Azure**, och klicka sedan på **Azure logga In**.</span><span class="sxs-lookup"><span data-stu-id="af33f-155">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Kommandot IntelliJ Azure logga In][A01]

3. <span data-ttu-id="af33f-157">I den **Azure logga In** väljer **automatisk**, och klicka sedan på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="af33f-157">In the **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![Fönstret Azure logga In med automatisk markerat][A02]

4. <span data-ttu-id="af33f-159">I den **Välj autentiseringsfilen** dialogrutan Välj en fil som skapats tidigare autentiseringsuppgifter och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="af33f-159">In the **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![Dialogrutan Välj fil för autentisering][A08]

5. <span data-ttu-id="af33f-161">I den **Azure logga In** -fönstret klickar du på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="af33f-161">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![Fönstret Azure logga In med automatisk markerat][A06]

6. <span data-ttu-id="af33f-163">I den **Välj prenumerationer** dialogrutan, Välj prenumerationer som du vill använda och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="af33f-163">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="next-steps"></a><span data-ttu-id="af33f-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af33f-165">Next steps</span></span>
<span data-ttu-id="af33f-166">Mer information om Azure Toolkits för Java IDE:er finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="af33f-166">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="af33f-167">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="af33f-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="af33f-168">[Vad är nytt i Azure-verktygen för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="af33f-168">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="af33f-169">[Installera Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="af33f-169">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="af33f-170">[Logga in-instruktioner för Azure-verktygen för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="af33f-170">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="af33f-171">[Skapa en Hello World-webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="af33f-171">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="af33f-172">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="af33f-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="af33f-173">[Vad är nytt i Azure-verktygen för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="af33f-173">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="af33f-174">[Installera Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="af33f-174">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="af33f-175">*Logga in instruktioner för Azure-Toolkit för IntelliJ* (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="af33f-175">*Sign-in instructions for the Azure Toolkit for IntelliJ* (this article)</span></span>
  * <span data-ttu-id="af33f-176">[Skapa en Hello World-webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="af33f-176">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="af33f-177">Mer information om hur du använder Azure med Java finns på [Azure Java Developer Center] och i [Java Tools för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="af33f-177">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="af33f-178">[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="af33f-178">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="af33f-179">[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="af33f-179">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="af33f-180">[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="af33f-180">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="af33f-181">[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="af33f-181">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="af33f-182">[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="af33f-182">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="af33f-183">[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="af33f-183">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="af33f-184">[Logga in-instruktioner för Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="af33f-184">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
<span data-ttu-id="af33f-185">[Vad är nytt i Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="af33f-185">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="af33f-186">[Vad är nytt i Azure-verktygen för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="af33f-186">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="af33f-187">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="af33f-187">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="af33f-188">[Java Tools för Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="af33f-188">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

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
