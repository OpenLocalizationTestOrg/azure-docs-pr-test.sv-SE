---
title: "aaaNo koppling arbetsgruppen hittades för ett program med Application Proxy | Microsoft Docs"
description: "Åtgärda problem som kan uppstå när det finns ingen aktiv anslutning i en grupp för anslutningen för ditt program med hello Azure AD Application Proxy"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c4baf296b316db131929c9a7c618fb9960713e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a><span data-ttu-id="13888-103">Ingen koppling arbetsgruppen hittades för ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="13888-103">No working connector group found for an Application Proxy application</span></span>

<span data-ttu-id="13888-104">Den här artikeln hjälpen du tooresolve hello vanliga problem inför när det inte är en koppling upptäcktes för ett program med Application Proxy integrerat med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="13888-104">This article help you tooresolve hello common issues faced when there is not a connector detected for an Application Proxy application integrated with Azure Active Directory.</span></span>

## <a name="overview-of-steps"></a><span data-ttu-id="13888-105">Översikt över steg</span><span class="sxs-lookup"><span data-stu-id="13888-105">Overview of steps</span></span>
<span data-ttu-id="13888-106">Om det finns ingen aktiv anslutning i en grupp för kopplingen för programmet, finns det ett par sätt tooresolve hello problemet:</span><span class="sxs-lookup"><span data-stu-id="13888-106">If there is no working Connector in a Connector Group for your application, there are a few ways tooresolve hello problem:</span></span>

-   <span data-ttu-id="13888-107">Om du har inga kopplingar i hello grupp, kan du:</span><span class="sxs-lookup"><span data-stu-id="13888-107">If you have no connectors in hello group, you can:</span></span>

    -   <span data-ttu-id="13888-108">Hämta en ny koppling på hello rätt lokal server och tilldela den toothis grupp</span><span class="sxs-lookup"><span data-stu-id="13888-108">Download a new Connector on hello right on-prem server, and assign it toothis group</span></span>

    -   <span data-ttu-id="13888-109">Flytta en aktiv koppling till hello grupp</span><span class="sxs-lookup"><span data-stu-id="13888-109">Move an active Connector into hello group</span></span>

-   <span data-ttu-id="13888-110">Om du har inga aktiva kopplingar i hello grupp, kan du:</span><span class="sxs-lookup"><span data-stu-id="13888-110">If you have no active connectors in hello group, you can:</span></span>

    -   <span data-ttu-id="13888-111">Identifiera hello orsak din anslutning är inaktiv och lösa</span><span class="sxs-lookup"><span data-stu-id="13888-111">Identify hello reason your Connector is inactive and resolve</span></span>

    -   <span data-ttu-id="13888-112">Flytta en aktiv koppling till hello grupp</span><span class="sxs-lookup"><span data-stu-id="13888-112">Move an active Connector into hello group</span></span>

<span data-ttu-id="13888-113">tooknow som dessa är hello problemet öppna hello ”Application Proxy”-menyn i ditt program och titta på hello Connector grupp varningsmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="13888-113">tooknow which of these is hello issue, open hello “Application Proxy” menu in your Application, and look at hello Connector Group warning message.</span></span> <span data-ttu-id="13888-114">Det anger du antingen hello gruppen måste minst en koppling (du har ingen i hello grupp) eller att det finns inga aktiva anslutningar (även om du har förmodligen inaktiva kopplingar).</span><span class="sxs-lookup"><span data-stu-id="13888-114">It specify either that hello group needs at least one Connector (you have none in hello group) or that it has no active Connectors (though you likely have inactive Connectors).</span></span>

   ![Val av anslutningen i Azure Portal](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

<span data-ttu-id="13888-116">Mer information om de här alternativen finns i avsnittet hello motsvarande nedan.</span><span class="sxs-lookup"><span data-stu-id="13888-116">For details on each of these options, see hello corresponding section below.</span></span> <span data-ttu-id="13888-117">Var och en av dessa förutsätter att du startar från hanteringssidan om hello-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="13888-117">Each of these assumes that you are starting from hello Connector management page.</span></span> <span data-ttu-id="13888-118">Om du tittar på hello felmeddelandet ovan, fortsätter du toothis sidan genom att klicka på hello varningsmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="13888-118">If you are looking at hello error message above, you can go toothis page by clicking on hello warning message.</span></span> <span data-ttu-id="13888-119">Annars det hittar du genom att gå för**Azure Active Directory**, klicka på **företagsprogram**, sedan **Application Proxy.**</span><span class="sxs-lookup"><span data-stu-id="13888-119">Otherwise this can be found by going too**Azure Active Directory**, clicking on **Enterprise Applications**, then **Application Proxy.**</span></span>

   ![Kopplingen grupphantering i Azure Portal](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a><span data-ttu-id="13888-121">Hämta en ny koppling</span><span class="sxs-lookup"><span data-stu-id="13888-121">Download a new Connector</span></span>

<span data-ttu-id="13888-122">toodownload en ny koppling, Använd hello ”hämta anslutning” knappen hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="13888-122">toodownload a new Connector, use hello “Download Connector” button at hello top of hello page.</span></span>

<span data-ttu-id="13888-123">Obs hello Connector behov toobe installeras på en dator med fri toohello backend-programmet och vanligtvis är placerad hello samma server som hello program.</span><span class="sxs-lookup"><span data-stu-id="13888-123">note hello Connector needs toobe installed on a machine with direct line of sight toohello backend application, and is typically placed on hello same server as hello application.</span></span> <span data-ttu-id="13888-124">När du har hämtat, hello anslutningen ska visas i den här menyn.</span><span class="sxs-lookup"><span data-stu-id="13888-124">After downloading, hello Connector should appear in this menu.</span></span> <span data-ttu-id="13888-125">klickar hello koppling och använder hello ”Connector grupp” nedrullningsbara toomake att toohello rätt gruppen tillhör.</span><span class="sxs-lookup"><span data-stu-id="13888-125">click hello Connector, and use hello “Connector Group” drop-down toomake sure it belongs toohello right group.</span></span> <span data-ttu-id="13888-126">Spara hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="13888-126">Save hello change.</span></span>

   ![Hämta hello koppling från hello Azure-portalen](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a><span data-ttu-id="13888-128">Flytta en aktiv koppling</span><span class="sxs-lookup"><span data-stu-id="13888-128">Move an Active Connector</span></span>

<span data-ttu-id="13888-129">Om du har en aktiv koppling som ska tillhöra toohello grupp och har fri toohello målprogrammet backend kan du flytta hello Connector till hello som tilldelats gruppen.</span><span class="sxs-lookup"><span data-stu-id="13888-129">If you have an active Connector that should belong toohello group and has line of sight toohello target backend application, you can move hello Connector into hello assigned group.</span></span> <span data-ttu-id="13888-130">toodo Klicka på hello Connector.</span><span class="sxs-lookup"><span data-stu-id="13888-130">toodo so, click hello Connector.</span></span> <span data-ttu-id="13888-131">Använda hello nedrullningsbara tooselect hello rätt grupp i hello ”Connector” fältet och klickar du på Spara.</span><span class="sxs-lookup"><span data-stu-id="13888-131">In hello “Connector Group” field, use hello drop-down tooselect hello correct group, and click Save.</span></span>

## <a name="resolve-an-inactive-connector"></a><span data-ttu-id="13888-132">Lösa en inaktiv anslutning</span><span class="sxs-lookup"><span data-stu-id="13888-132">Resolve an inactive Connector</span></span>

<span data-ttu-id="13888-133">Om hello endast kopplingar i hello grupp är inaktiva de troligen på en dator som inte har alla nödvändiga portar för hello avblockerad.</span><span class="sxs-lookup"><span data-stu-id="13888-133">If hello only Connectors in hello group are inactive, they are likely on a machine that does not have all hello necessary ports unblocked.</span></span>

<span data-ttu-id="13888-134">Se hello portar felsöka dokument för information om undersöker problemet.</span><span class="sxs-lookup"><span data-stu-id="13888-134">see hello ports Troubleshoot document for details on investigating this problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13888-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="13888-135">Next steps</span></span>
[<span data-ttu-id="13888-136">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="13888-136">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)


