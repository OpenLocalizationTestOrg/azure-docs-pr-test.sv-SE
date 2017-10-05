---
title: "Installera Azure Toolkit för Eclipse | Microsoft Docs"
description: "Lär dig hur du installerar Azure Toolkit för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 35cddba38c364dfb2f6a8646b0014d48ca4cb795
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="8c1d7-103">Installera Azure Toolkit för Eclipse</span><span class="sxs-lookup"><span data-stu-id="8c1d7-103">Installing the Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="8c1d7-104">Azure-verktygen för Eclipse innehåller mallar och funktioner som gör att du kan enkelt skapa, utveckla, testa och distribuera Azure-program med hjälp av Eclipse-utvecklingsmiljön.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the Eclipse development environment.</span></span> <span data-ttu-id="8c1d7-105">Azure-verktygen för Eclipse är ett projekt med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-105">The Azure Toolkit for Eclipse is an Open Source project.</span></span> <span data-ttu-id="8c1d7-106">Källkoden är tillgänglig under MIT-licens från <https://github.com/microsoft/azure-tools-for-java>.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-106">The source code is available under the MIT License from <https://github.com/microsoft/azure-tools-for-java>.</span></span>

<span data-ttu-id="8c1d7-107">Följande steg visar hur du installerar Azure Toolkit för Eclipse.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-107">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="8c1d7-108">Så här installerar du Azure-verktygen för Eclipse</span><span class="sxs-lookup"><span data-stu-id="8c1d7-108">To install the Azure Toolkit for Eclipse</span></span>
1. <span data-ttu-id="8c1d7-109">Starta Eclipse.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-109">Start Eclipse.</span></span>
2. <span data-ttu-id="8c1d7-110">Klicka på den **hjälp** -menyn och klicka sedan på **installera ny programvara**som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-110">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
    ![Installera Azure Toolkit för Eclipse][01]
3. <span data-ttu-id="8c1d7-112">I den **tillgänglig programvara** dialogrutan, inom den **arbeta med** skriver `http://dl.microsoft.com/eclipse` följt av den **RETUR** nyckel.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-112">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse` followed by the **Enter** key.</span></span>
4. <span data-ttu-id="8c1d7-113">I den **namn** markerar **Azure Toolkit för Eclipse**, och avmarkerar **kontakta alla update platser under installationen att hitta nödvändig programvara**.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-113">In the **Name** pane, check **Azure Toolkit for Eclipse**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="8c1d7-114">Skärmen bör likna följande:</span><span class="sxs-lookup"><span data-stu-id="8c1d7-114">Your screen should appear similar to the following:</span></span>
   
    ![Installera Azure Toolkit för Eclipse][02]
5. <span data-ttu-id="8c1d7-116">Om du expanderar den **Azure Toolkit för Eclipse**, visas följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8c1d7-116">If you expand the **Azure Toolkit for Eclipse**, you will see the following items:</span></span>
   
   * <span data-ttu-id="8c1d7-117">**Application Insights plugin-program för Java**: den här komponenten kan du använda Azures telemetri loggning och analysis services för program och server-instanser.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-117">**Application Insights Plugin for Java**: This component allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span>
   * <span data-ttu-id="8c1d7-118">**Azure Access Control-tjänstfilter**: den här komponenten ger stöd för autentisering av användare med Azure ACS, att aktivera enkel inloggning scenarier och filernas identitet logik från programmet.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-118">**Azure Access Control Services Filter**: This component provides support for authenticating application users with Azure ACS, enabling single sign-on scenarios and externalizing identity logic from the application.</span></span>
   * <span data-ttu-id="8c1d7-119">**Azure vanliga Plugin**: den här komponenten tillhandahåller de vanliga funktioner som krävs av andra toolkit-komponenter.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-119">**Azure Common Plugin**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="8c1d7-120">**Azure Explorer för Eclipse**: den här komponenten tillhandahåller de vanliga funktioner som krävs av andra toolkit-komponenter.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-120">**Azure Explorer for Eclipse**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="8c1d7-121">**Azure-Plugin för Eclipse med Java**: den här komponenten ger stöd för utveckling av projekt som att skapa, testa och distribuera Java-program för Microsoft Azure-molnet i Eclipse och via kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-121">**Azure Plugin for Eclipse with Java**: This component provides support for developing projects that help build, test and deploy Java applications for the Microsoft Azure cloud in Eclipse and via command line.</span></span>
   * <span data-ttu-id="8c1d7-122">**Azure Web Apps plugin-program med Java**: den här komponenten ger stöd för att distribuera Java-webbprogram till Microsoft Azure Web App-behållare.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-122">**Azure Web Apps Plugin with Java**: This component provides support for deploying Java web applications to Microsoft Azure Web App containers.</span></span>
   * <span data-ttu-id="8c1d7-123">**Microsoft JDBC Driver 4.2 för SQL Server**: den här komponenten tillhandahåller JDBC API för SQL Server och Microsoft Azure SQL Database för Java Platform Enterprise Edition 8.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-123">**Microsoft JDBC Driver 4.2 for SQL Server**: This component provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span>
   * <span data-ttu-id="8c1d7-124">**Paketet för Apache Qpid klientbibliotek för JMS**: den här komponenten tillhandahåller JMS klientkomponenten från projektet Apache Qpid att programmet ska använda AMQP meddelanden i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-124">**Package for Apache Qpid Client Libraries for JMS**: This component provides the JMS client component from the Apache Qpid project to enable your application to use AMQP messaging in Microsoft Azure.</span></span>
   * <span data-ttu-id="8c1d7-125">**Paket för Microsoft Azure-biblioteken för Java**: den här komponenten ger API: er för att komma åt Microsoft Azure-tjänster, till exempel lagring, service bus, körtid och så vidare.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-125">**Package for Microsoft Azure Libraries for Java**: This component provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span>
6. <span data-ttu-id="8c1d7-126">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-126">Click **Next**.</span></span> <span data-ttu-id="8c1d7-127">(Om det uppstår ovanliga fördröjningar när du installerar verktyget kontrollerar du att **kontakta alla update platser under installationen att hitta nödvändig programvara** är avmarkerad.)</span><span class="sxs-lookup"><span data-stu-id="8c1d7-127">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>
7. <span data-ttu-id="8c1d7-128">I den **installera information** dialogrutan klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-128">In the **Install Details** dialog, click **Next**.</span></span>
   
    ![Granska information om installationen][03]
8. <span data-ttu-id="8c1d7-130">I den **granska licenser** dialogrutan Granska villkoren i licensavtalet.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-130">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="8c1d7-131">Om du accepterar villkoren i licensavtalet, klickar du på **jag accepterar villkoren i licensavtalet** och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-131">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="8c1d7-132">(Stegen förutsätter att du accepterar villkoren i licensavtalet.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-132">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="8c1d7-133">Om du inte accepterar villkoren i licensavtalet avsluta installationen.)</span><span class="sxs-lookup"><span data-stu-id="8c1d7-133">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
    ![Granska licenser][04]
   
    <span data-ttu-id="8c1d7-135">Eclipse ska hämta och installera de nödvändiga paketen.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-135">Eclipse will download and install the requisite packages.</span></span>
   
    ![Installationsförlopp][05]
9. <span data-ttu-id="8c1d7-137">Om du uppmanas starta om Eclipse för att slutföra installationen klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="8c1d7-137">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
    ![Starta om kommandotolken][06]

## <a name="see-also"></a><span data-ttu-id="8c1d7-139">Se även</span><span class="sxs-lookup"><span data-stu-id="8c1d7-139">See Also</span></span>
<span data-ttu-id="8c1d7-140">Mer information om Azure Toolkits för Java IDE:er finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="8c1d7-140">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="8c1d7-141">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="8c1d7-141">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="8c1d7-142">[Nyheter i Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="8c1d7-142">[What's New in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="8c1d7-143">*Installera Azure Toolkit för Eclipse (den här artikeln)*</span><span class="sxs-lookup"><span data-stu-id="8c1d7-143">*Installing the Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="8c1d7-144">[Inloggningsinstruktioner för Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="8c1d7-144">[Sign In Instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="8c1d7-145">[Skapa en Hello World-Webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="8c1d7-145">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="8c1d7-146">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="8c1d7-146">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="8c1d7-147">[Nyheter i Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="8c1d7-147">[What's New in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="8c1d7-148">[Installera Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="8c1d7-148">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="8c1d7-149">[Inloggningsinstruktioner för Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="8c1d7-149">[Sign In Instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="8c1d7-150">[Skapa en Hello World-Webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="8c1d7-150">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="8c1d7-151">Mer information om hur du använder Java i Azure finns i [Java-utvecklingscenter].</span><span class="sxs-lookup"><span data-stu-id="8c1d7-151">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<!-- URL List -->

<span data-ttu-id="8c1d7-152">[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="8c1d7-152">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="8c1d7-153">[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="8c1d7-153">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="8c1d7-154">[Skapa en Hello World-Webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="8c1d7-154">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="8c1d7-155">[Skapa en Hello World-Webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="8c1d7-155">[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
<span data-ttu-id="8c1d7-156">[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="8c1d7-156">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="8c1d7-157">[Inloggningsinstruktioner för Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="8c1d7-157">[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="8c1d7-158">[Inloggningsinstruktioner för Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="8c1d7-158">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="8c1d7-159">[Nyheter i Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="8c1d7-159">[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="8c1d7-160">[Nyheter i Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="8c1d7-160">[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="8c1d7-161">[Java-utvecklingscenter]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="8c1d7-161">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
