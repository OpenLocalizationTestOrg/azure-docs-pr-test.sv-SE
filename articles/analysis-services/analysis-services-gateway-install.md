---
title: aaaInstall lokala datagateway | Microsoft Docs
description: "Lär dig hur tooinstall och konfigurera en lokal datagateway."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/22/2017
ms.author: owend
ms.openlocfilehash: e2878bf765c82910d452ae2cdd9264a343ec1990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a><span data-ttu-id="15920-103">Installera och konfigurera en gateway för lokala data</span><span class="sxs-lookup"><span data-stu-id="15920-103">Install and configure an on-premises data gateway</span></span>
<span data-ttu-id="15920-104">En lokal datagateway krävs när en eller flera Azure Analysis Services-servrar i hello samma region ansluta tooon lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="15920-104">An on-premises data gateway is required when one or more Azure Analysis Services servers in hello same region connect tooon-premises data sources.</span></span> <span data-ttu-id="15920-105">toolearn mer om hello gateway finns [lokala datagateway](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="15920-105">toolearn more about hello gateway, see [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15920-106">Krav</span><span class="sxs-lookup"><span data-stu-id="15920-106">Prerequisites</span></span>
<span data-ttu-id="15920-107">**Minimikrav:**</span><span class="sxs-lookup"><span data-stu-id="15920-107">**Minimum Requirements:**</span></span>

* <span data-ttu-id="15920-108">4.5 för .NET framework</span><span class="sxs-lookup"><span data-stu-id="15920-108">.NET 4.5 Framework</span></span>
* <span data-ttu-id="15920-109">64-bitars version av Windows 7 / Windows Server 2008 R2 (eller senare)</span><span class="sxs-lookup"><span data-stu-id="15920-109">64-bit version of Windows 7 / Windows Server 2008 R2 (or later)</span></span>

<span data-ttu-id="15920-110">**Rekommenderat:**</span><span class="sxs-lookup"><span data-stu-id="15920-110">**Recommended:**</span></span>

* <span data-ttu-id="15920-111">8 kärnor CPU</span><span class="sxs-lookup"><span data-stu-id="15920-111">8 Core CPU</span></span>
* <span data-ttu-id="15920-112">8 GB minne</span><span class="sxs-lookup"><span data-stu-id="15920-112">8 GB Memory</span></span>
* <span data-ttu-id="15920-113">64-bitarsversionen av Windows 2012 R2 (eller senare)</span><span class="sxs-lookup"><span data-stu-id="15920-113">64-bit version of Windows 2012 R2 (or later)</span></span>

<span data-ttu-id="15920-114">**Viktigt att tänka på:**</span><span class="sxs-lookup"><span data-stu-id="15920-114">**Important considerations:**</span></span>

* <span data-ttu-id="15920-115">Under installationen när du registrerar din gateway med Azure, har hello standardregion för din prenumeration valts.</span><span class="sxs-lookup"><span data-stu-id="15920-115">During setup, when registering your gateway with Azure, hello default region for your subscription is selected.</span></span> <span data-ttu-id="15920-116">Du kan välja en annan region.</span><span class="sxs-lookup"><span data-stu-id="15920-116">You can choose a different region.</span></span> <span data-ttu-id="15920-117">Om du har servrar i flera regioner, måste du installera en gateway för varje region.</span><span class="sxs-lookup"><span data-stu-id="15920-117">If you have servers in more than one region, you must install a gateway for each region.</span></span> 
* <span data-ttu-id="15920-118">hello gateway kan inte installeras på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="15920-118">hello gateway cannot be installed on a domain controller.</span></span>
* <span data-ttu-id="15920-119">Endast en gateway kan installeras på en dator.</span><span class="sxs-lookup"><span data-stu-id="15920-119">Only one gateway can be installed on a single computer.</span></span>
* <span data-ttu-id="15920-120">Installera hello gateway på en dator som finns kvar på och går inte toosleep.</span><span class="sxs-lookup"><span data-stu-id="15920-120">Install hello gateway on a computer that remains on and does not go toosleep.</span></span>
* <span data-ttu-id="15920-121">Installera inte hello gateway i ett datornätverk trådlöst anslutna tooyour.</span><span class="sxs-lookup"><span data-stu-id="15920-121">Do not install hello gateway on a computer wirelessly connected tooyour network.</span></span> <span data-ttu-id="15920-122">Prestanda kan minskas.</span><span class="sxs-lookup"><span data-stu-id="15920-122">Performance can be diminished.</span></span>


## <span data-ttu-id="15920-123"><a name="download"></a>Ladda ned</span><span class="sxs-lookup"><span data-stu-id="15920-123"><a name="download"></a>Download</span></span>
 [<span data-ttu-id="15920-124">Hämta hello gateway</span><span class="sxs-lookup"><span data-stu-id="15920-124">Download hello gateway</span></span>](https://aka.ms/azureasgateway)

## <span data-ttu-id="15920-125"><a name="install"></a>Installera</span><span class="sxs-lookup"><span data-stu-id="15920-125"><a name="install"></a>Install</span></span>

1. <span data-ttu-id="15920-126">Kör installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="15920-126">Run setup.</span></span>

2. <span data-ttu-id="15920-127">Välj en plats, acceptera hello och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="15920-127">Select a location, accept hello terms, and then click **Install**.</span></span>

   ![Installera platsen och licensvillkoren](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. <span data-ttu-id="15920-129">Välj **lokala datagateway (rekommenderas)**.</span><span class="sxs-lookup"><span data-stu-id="15920-129">Select **On-premises data gateway (recommended)**.</span></span> <span data-ttu-id="15920-130">Azure Analysis Services stöder inte personliga läge.</span><span class="sxs-lookup"><span data-stu-id="15920-130">Azure Analysis Services does not support personal mode.</span></span>

   ![Välj typ av gateway](media/analysis-services-gateway-install/aas-gateway-installer-shared.png)

4. <span data-ttu-id="15920-132">Ange ett konto toosign i tooAzure.</span><span class="sxs-lookup"><span data-stu-id="15920-132">Enter an account toosign in tooAzure.</span></span> <span data-ttu-id="15920-133">hello-kontot måste vara i din klient Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="15920-133">hello account must be in your tenant's Azure Active Directory.</span></span> <span data-ttu-id="15920-134">Det här kontot används för hello gateway-administratören.</span><span class="sxs-lookup"><span data-stu-id="15920-134">This account is used for hello gateway administrator.</span></span> 

   ![Ange ett konto toosign i tooAzure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > <span data-ttu-id="15920-136">Om du loggar in med ett domänkonto, kommer att mappas tooyour organisationens konto i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15920-136">If you sign in with a domain account, it will be mapped tooyour organizational account in Azure AD.</span></span> <span data-ttu-id="15920-137">Ditt organisationskonto används som hello hello gateway-administratören.</span><span class="sxs-lookup"><span data-stu-id="15920-137">Your organizational account will be used as hello hello gateway administrator.</span></span>

## <span data-ttu-id="15920-138"><a name="register"></a>Registrera dig</span><span class="sxs-lookup"><span data-stu-id="15920-138"><a name="register"></a>Register</span></span>
<span data-ttu-id="15920-139">Du måste registrera hello lokal instans som du installerat med hello Gateway Cloud Service i ordning toocreate en gateway-resurs i Azure.</span><span class="sxs-lookup"><span data-stu-id="15920-139">In order toocreate a gateway resource in Azure, you must register hello local instance you installed with hello Gateway Cloud Service.</span></span> 

1.  <span data-ttu-id="15920-140">Välj **registrera en ny gateway på den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="15920-140">Select **Register a new gateway on this computer**.</span></span>

    ![Registrera dig](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. <span data-ttu-id="15920-142">Skriv ett namn och återställa nyckeln för din gateway.</span><span class="sxs-lookup"><span data-stu-id="15920-142">Type a name and recovery key for your gateway.</span></span> <span data-ttu-id="15920-143">Som standard använder hello gateway standardregion för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="15920-143">By default, hello gateway uses your subscription's default region.</span></span> <span data-ttu-id="15920-144">Om du behöver tooselect en annan region, Välj **ändra Region**.</span><span class="sxs-lookup"><span data-stu-id="15920-144">If you need tooselect a different region, select **Change Region**.</span></span>

   ![Registrera dig](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <span data-ttu-id="15920-146"><a name="create-resource"></a>Skapa en Azure gateway-resurs</span><span class="sxs-lookup"><span data-stu-id="15920-146"><a name="create-resource"></a>Create an Azure gateway resource</span></span>
<span data-ttu-id="15920-147">När du har installerat och registrerat din gateway, måste toocreate en gateway-resurs i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="15920-147">After you've installed and registered your gateway, you need toocreate a gateway resource in your Azure subscription.</span></span> <span data-ttu-id="15920-148">Logga in tooAzure med hello samma konto som du använde när du registrerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="15920-148">Sign in tooAzure with hello same account you used when registering hello gateway.</span></span>

1. <span data-ttu-id="15920-149">I Azure-portalen klickar du på **skapa en ny tjänst** > **Enterprise Integration** > **lokala datagateway**  >   **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="15920-149">In Azure portal, click **Create a new service** > **Enterprise Integration** > **On-premises data gateway** > **Create**.</span></span>

   ![Skapa en gateway-resurs](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. <span data-ttu-id="15920-151">I **skapa anslutningen gateway**, ange dessa inställningar:</span><span class="sxs-lookup"><span data-stu-id="15920-151">In **Create connection gateway**, enter these settings:</span></span>

    * <span data-ttu-id="15920-152">**Namnet**: Ange ett namn för din gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="15920-152">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="15920-153">**Prenumerationen**: Välj hello Azure-prenumeration tooassociate med gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="15920-153">**Subscription**: Select hello Azure subscription tooassociate with your gateway resource.</span></span> 
    <span data-ttu-id="15920-154">Den här prenumerationen ska vara hello samma prenumeration som servrarna finns i.</span><span class="sxs-lookup"><span data-stu-id="15920-154">This subscription should be hello same subscription your servers are in.</span></span>
   
      <span data-ttu-id="15920-155">Hej standardabonnemang baseras på hello Azure-konto som du använde toosign i.</span><span class="sxs-lookup"><span data-stu-id="15920-155">hello default subscription is based on hello Azure account that you used toosign in.</span></span>

    * <span data-ttu-id="15920-156">**Resursgrupp**: Skapa en resursgrupp eller välj en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="15920-156">**Resource group**: Create a resource group or select an existing resource group.</span></span>

    * <span data-ttu-id="15920-157">**Plats**: Välj hello region som du har registrerat din gateway i.</span><span class="sxs-lookup"><span data-stu-id="15920-157">**Location**: Select hello region you registered your gateway in.</span></span>

    * <span data-ttu-id="15920-158">**Installationen namnet**: om gatewayinstallationen av inte är markerat, Välj hello gatewayen har registrerats.</span><span class="sxs-lookup"><span data-stu-id="15920-158">**Installation Name**: If your gateway installation isn't already selected, select hello gateway registered.</span></span> 

    <span data-ttu-id="15920-159">När du är klar klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="15920-159">When you're done, click **Create**.</span></span>

## <span data-ttu-id="15920-160"><a name="connect-servers"></a>Anslut servrar toohello gatewayresursen</span><span class="sxs-lookup"><span data-stu-id="15920-160"><a name="connect-servers"></a>Connect servers toohello gateway resource</span></span>

1. <span data-ttu-id="15920-161">I din Azure Analysis Services översikt över server, klickar du på **lokala Data Gateway**.</span><span class="sxs-lookup"><span data-stu-id="15920-161">In your Azure Analysis Services server overview, click **On-Premises Data Gateway**.</span></span>

   ![Anslut servern toogateway](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. <span data-ttu-id="15920-163">I **Välj en lokala Data Gateway tooconnect**, väljer du gateway-resurs och klicka sedan på **Anslut markerad gateway**.</span><span class="sxs-lookup"><span data-stu-id="15920-163">In **Pick an On-Premises Data Gateway tooconnect**, select your gateway resource, and then click **Connect selected gateway**.</span></span>

   ![Anslut servern toogateway resursen](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > <span data-ttu-id="15920-165">Om din gateway inte visas i listan hello din server är förmodligen inte hello samma region som hello region som du angav när du registrerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="15920-165">If your gateway does not appear in hello list, your server is likely not in hello same region as hello region you specified when registering hello gateway.</span></span> 

<span data-ttu-id="15920-166">Det var allt.</span><span class="sxs-lookup"><span data-stu-id="15920-166">That's it.</span></span> <span data-ttu-id="15920-167">Om du behöver tooopen portar eller göra felsökning vara säker på att toocheck ut [lokala datagateway](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="15920-167">If you need tooopen ports or do any troubleshooting, be sure toocheck out [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="15920-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15920-168">Next steps</span></span>
* [<span data-ttu-id="15920-169">Hantera Analysis Services</span><span class="sxs-lookup"><span data-stu-id="15920-169">Manage Analysis Services</span></span>](analysis-services-manage.md)   
* [<span data-ttu-id="15920-170">Hämta data från Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="15920-170">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)
