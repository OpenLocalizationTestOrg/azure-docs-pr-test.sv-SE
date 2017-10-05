---
title: "Logga In anvisningar för Azure Toolkit Eclipse | Microsoft Docs"
description: "Lär dig mer om att logga in på Microsoft Azure med hjälp av Azure-verktygen för Eclipse."
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
ms.openlocfilehash: 02dd9935086c4c40d9ed54cc9ff2412ca96889f5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="3119c-103">Azure inloggning anvisningar för Azure Toolkit Eclipse</span><span class="sxs-lookup"><span data-stu-id="3119c-103">Azure Sign In Instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="3119c-104">Azure-verktygen för Eclipse erbjuder två metoder för att logga in på ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="3119c-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  * <span data-ttu-id="3119c-105">**Interaktiva** – när du använder den här metoden ska du ange dina autentiseringsuppgifter för Azure varje gång du loggar in i ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="3119c-105">**Interactive** - when you are using this method, you will enter your Azure credentials each time you sign into your Azure account.</span></span>
  * <span data-ttu-id="3119c-106">**Automatisk** – när du använder den här metoden skapar du en fil med autentiseringsuppgifter som innehåller service principal data, efter vilken du kan använda filen autentiseringsuppgifter för att automatiskt logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="3119c-106">**Automated** - when you are using this method, you will create a credentials file which contains your service principal data, after which you can use the credentials file to automatically sign into your Azure account.</span></span>

<span data-ttu-id="3119c-107">Stegen i följande avsnitt beskriver hur du använder varje metod.</span><span class="sxs-lookup"><span data-stu-id="3119c-107">The steps in the following sections will describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a><span data-ttu-id="3119c-108">Logga in på ditt Azure-konto interaktivt</span><span class="sxs-lookup"><span data-stu-id="3119c-108">Signing into your Azure account interactively</span></span>

<span data-ttu-id="3119c-109">Följande steg illustrerar hur du logga in på Azure manuellt genom att ange dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="3119c-109">The following steps will illustrate how to sign into Azure by manually entering your Azure credentials.</span></span>

1. <span data-ttu-id="3119c-110">Öppna projektet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="3119c-110">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="3119c-111">Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="3119c-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse-menyn för Azure inloggning][I01]

1. <span data-ttu-id="3119c-113">När den **Azure logga In** dialogrutan Välj **interaktiv**, och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="3119c-113">When the **Azure Sign In** dialog box appears, select **Interactive**, and then click **Sign In**.</span></span>

   ![Logga In dialogrutan][I02]

1. <span data-ttu-id="3119c-115">När den **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="3119c-115">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Dialogruta i Azure][I03]

1. <span data-ttu-id="3119c-117">När den **Välj prenumerationer** visas dialogrutan Välj den prenumeration som du vill använda och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3119c-117">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Dialogrutan Välj prenumerationer][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a><span data-ttu-id="3119c-119">Logga ut från ditt Azure-konto när du loggar in interaktivt</span><span class="sxs-lookup"><span data-stu-id="3119c-119">Signing out of your Azure account when you signed in interactively</span></span>

<span data-ttu-id="3119c-120">När du har konfigurerat stegen i föregående avsnitt, kommer du automatiskt loggas ut från ditt Azure-konto varje gång du startar om Eclipse.</span><span class="sxs-lookup"><span data-stu-id="3119c-120">After you have configured the steps in the previous section, you will automatically signed out of your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="3119c-121">Om du vill logga ut från ditt Azure-konto utan att starta om Eclipse du dock använda följande steg.</span><span class="sxs-lookup"><span data-stu-id="3119c-121">However, if you want to sign out of your Azure account without restarting Eclipse, use the following steps.</span></span>

1. <span data-ttu-id="3119c-122">I Eclipse klickar du på **verktyg**, klicka på **Azure**, och klicka sedan på **logga ut**.</span><span class="sxs-lookup"><span data-stu-id="3119c-122">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Eclipse-menyn för Azure logga ut][L01]

1. <span data-ttu-id="3119c-124">När den **Azure logga ut** dialogrutan visas klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="3119c-124">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Logga ut dialogrutan][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a><span data-ttu-id="3119c-126">Logga in på ditt Azure-konto automatiskt och skapa en fil med autentiseringsuppgifter som ska användas i framtiden</span><span class="sxs-lookup"><span data-stu-id="3119c-126">Signing into your Azure account automatically and creating a credentials file to use in the future</span></span>

<span data-ttu-id="3119c-127">Följande steg får du hjälp med att skapa en fil med autentiseringsuppgifter som innehåller dina viktigaste data service.</span><span class="sxs-lookup"><span data-stu-id="3119c-127">The following steps will walk you through creating a credentials file which contains your service principal data.</span></span> <span data-ttu-id="3119c-128">När du har slutfört de här stegen, används automatiskt filen autentiseringsuppgifter till automatiskt logga in på Azure varje gång du öppnar projektet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="3119c-128">Once you have completed these steps, Eclipse will automatically use the credentials file to automatically sign you into Azure each time you open your project.</span></span>

1. <span data-ttu-id="3119c-129">Öppna projektet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="3119c-129">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="3119c-130">Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="3119c-130">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse-menyn för Azure inloggning][A01]

1. <span data-ttu-id="3119c-132">När den **Azure logga In** dialogrutan Välj **automatisk**, och klicka sedan på **ny**.</span><span class="sxs-lookup"><span data-stu-id="3119c-132">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **New**.</span></span>

   ![Logga In dialogrutan][A02]

1. <span data-ttu-id="3119c-134">När den **Azure Log In** dialogrutan Ange dina autentiseringsuppgifter för Azure och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="3119c-134">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Dialogruta i Azure][A03]

1. <span data-ttu-id="3119c-136">När den **skapa autentiseringsfiler** visas dialogrutan Välj den prenumeration som du vill använda en målkatalog och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="3119c-136">When the **Create authentication files** dialog box appears, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![Dialogruta i Azure][A04]

1. <span data-ttu-id="3119c-138">Den **tjänstens huvudnamn Creatation Status** dialogrutan visas, och när filerna har skapats, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3119c-138">The **Service Principal Creatation Status** dialog box will be displayed, and after your files have been created successfully, click **OK**.</span></span>

   ![Dialogrutan tjänstens huvudnamn Creatation Status][A05]

1. <span data-ttu-id="3119c-140">När den **Azure logga In** dialogrutan visas klickar du på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="3119c-140">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Dialogruta i Azure][A06]

1. <span data-ttu-id="3119c-142">När den **Välj prenumerationer** visas dialogrutan Välj den prenumeration som du vill använda och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3119c-142">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a><span data-ttu-id="3119c-144">Logga ut från ditt Azure-konto när du loggar in automatiskt</span><span class="sxs-lookup"><span data-stu-id="3119c-144">Signing out of your Azure account when you signed in automatically</span></span>

<span data-ttu-id="3119c-145">När du har konfigurerat stegen i föregående avsnitt, loggas Azure Toolkit du automatiskt till ditt Azure-konto varje gång du startar om Eclipse.</span><span class="sxs-lookup"><span data-stu-id="3119c-145">After you have configured the steps in the previous section, the Azure Toolkit will automatically sign you into your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="3119c-146">Men om du vill logga ut från ditt Azure-konto och förhindra att Azure-Toolkit logga in automatiskt, Använd följande steg.</span><span class="sxs-lookup"><span data-stu-id="3119c-146">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, use the following steps.</span></span>

1. <span data-ttu-id="3119c-147">I Eclipse klickar du på **verktyg**, klicka på **Azure**, och klicka sedan på **logga ut**.</span><span class="sxs-lookup"><span data-stu-id="3119c-147">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Eclipse-menyn för Azure logga ut][L01]

1. <span data-ttu-id="3119c-149">När den **Azure logga ut** dialogrutan visas klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="3119c-149">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Logga ut dialogrutan][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a><span data-ttu-id="3119c-151">När du loggar in automatiskt med hjälp av en fil med autentiseringsuppgifter som du redan har skapat ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="3119c-151">Signing into your Azure account automatically using a credentials file which you have already created</span></span>

<span data-ttu-id="3119c-152">Om du logga ut från Azure när du använder Eclipse, måste du konfigurera om Azure-verktygen för Eclipse att använda en fil med autentiseringsuppgifter som har skapats innan du automatiskt kan logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="3119c-152">If you sign out of Azure when you are using Eclipse, you will need to reconfigure the Azure Toolkit for Eclipse to use a credentials file which have created before you can automatically sign into your Azure acccount.</span></span> <span data-ttu-id="3119c-153">Följande steg vägleder dig genom konfigurationen av Azure-verktyget om du vill använda en befintlig fil med autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3119c-153">The following steps will walk you through configuring the Azure Toolkit to use an existing credentials file.</span></span>

1. <span data-ttu-id="3119c-154">Öppna projektet i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="3119c-154">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="3119c-155">Klicka på **verktyg**, klicka på **Azure**, och klicka sedan på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="3119c-155">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Eclipse-menyn för Azure inloggning][A01]

1. <span data-ttu-id="3119c-157">När den **Azure logga In** dialogrutan Välj **automatisk**, och klicka sedan på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="3119c-157">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **Browse**.</span></span>

   ![Logga In dialogrutan][A02]

1. <span data-ttu-id="3119c-159">När den **Välj autentiserad fil** visas dialogrutan Välj en fil med autentiseringsuppgifter som du skapade tidigare och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="3119c-159">When the **Select Authenticated File** dialog box appears, select a credentials file which you created earlier, and then click **Select**.</span></span>

   ![Logga In dialogrutan][A08]

1. <span data-ttu-id="3119c-161">När den **Azure logga In** dialogrutan visas klickar du på **logga In**.</span><span class="sxs-lookup"><span data-stu-id="3119c-161">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Dialogruta i Azure][A06]

1. <span data-ttu-id="3119c-163">När den **Välj prenumerationer** visas dialogrutan Välj den prenumeration som du vill använda och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3119c-163">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Dialogrutan Välj prenumerationer][A07]

## <a name="see-also"></a><span data-ttu-id="3119c-165">Se även</span><span class="sxs-lookup"><span data-stu-id="3119c-165">See Also</span></span>
<span data-ttu-id="3119c-166">Mer information om Azure Toolkits för Java IDE:er finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="3119c-166">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="3119c-167">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="3119c-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="3119c-168">[Nyheter i Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="3119c-168">[What's New in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="3119c-169">[Installera Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="3119c-169">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="3119c-170">*Logga In anvisningar för Azure Toolkit Eclipse (den här artikeln)*</span><span class="sxs-lookup"><span data-stu-id="3119c-170">*Sign In Instructions for the Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="3119c-171">[Skapa en Hello World-Webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="3119c-171">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="3119c-172">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="3119c-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="3119c-173">[Nyheter i Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="3119c-173">[What's New in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="3119c-174">[Installera Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="3119c-174">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="3119c-175">[Inloggningsinstruktioner för Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="3119c-175">[Sign In Instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="3119c-176">[Skapa en Hello World-Webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="3119c-176">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="3119c-177">Mer information om hur du använder Azure med Java finns på [Azure Java Developer Center] och i [Java Tools för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="3119c-177">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="3119c-178">[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="3119c-178">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="3119c-179">[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="3119c-179">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="3119c-180">[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="3119c-180">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="3119c-181">[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="3119c-181">[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="3119c-182">[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="3119c-182">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="3119c-183">[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="3119c-183">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
<span data-ttu-id="3119c-184">[Inloggningsinstruktioner för Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="3119c-184">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="3119c-185">[Nyheter i Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="3119c-185">[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="3119c-186">[Nyheter i Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="3119c-186">[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="3119c-187">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="3119c-187">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="3119c-188">[Java Tools för Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="3119c-188">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

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
