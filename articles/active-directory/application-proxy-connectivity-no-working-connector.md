---
title: "Ingen koppling arbetsgruppen hittades för ett program med Application Proxy | Microsoft Docs"
description: "Åtgärda problem som kan uppstå när det finns ingen aktiv anslutning i en grupp för anslutningen för ditt program med Azure AD Application Proxy"
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
ms.openlocfilehash: 4945958deedc8a1d9989ff901192c03a5363b4dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a><span data-ttu-id="d57d8-103">Ingen koppling arbetsgruppen hittades för ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="d57d8-103">No working connector group found for an Application Proxy application</span></span>

<span data-ttu-id="d57d8-104">Den här artikeln hjälp dig att lösa vanliga problem inför när det inte är en koppling upptäcktes för ett program med Application Proxy integrerat med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d57d8-104">This article help you to resolve the common issues faced when there is not a connector detected for an Application Proxy application integrated with Azure Active Directory.</span></span>

## <a name="overview-of-steps"></a><span data-ttu-id="d57d8-105">Översikt över steg</span><span class="sxs-lookup"><span data-stu-id="d57d8-105">Overview of steps</span></span>
<span data-ttu-id="d57d8-106">Om det finns ingen aktiv anslutning i en grupp för kopplingen för programmet, finns det några sätt att lösa problemet:</span><span class="sxs-lookup"><span data-stu-id="d57d8-106">If there is no working Connector in a Connector Group for your application, there are a few ways to resolve the problem:</span></span>

-   <span data-ttu-id="d57d8-107">Om du har inga kopplingar i gruppen kan du:</span><span class="sxs-lookup"><span data-stu-id="d57d8-107">If you have no connectors in the group, you can:</span></span>

    -   <span data-ttu-id="d57d8-108">Hämta en ny koppling på rätt lokal server och tilldela den här gruppen</span><span class="sxs-lookup"><span data-stu-id="d57d8-108">Download a new Connector on the right on-prem server, and assign it to this group</span></span>

    -   <span data-ttu-id="d57d8-109">Flytta en aktiv koppling till gruppen</span><span class="sxs-lookup"><span data-stu-id="d57d8-109">Move an active Connector into the group</span></span>

-   <span data-ttu-id="d57d8-110">Om du har inga aktiva kopplingar i gruppen kan du:</span><span class="sxs-lookup"><span data-stu-id="d57d8-110">If you have no active connectors in the group, you can:</span></span>

    -   <span data-ttu-id="d57d8-111">Identifiera orsaken din anslutning är inaktiv och lösa</span><span class="sxs-lookup"><span data-stu-id="d57d8-111">Identify the reason your Connector is inactive and resolve</span></span>

    -   <span data-ttu-id="d57d8-112">Flytta en aktiv koppling till gruppen</span><span class="sxs-lookup"><span data-stu-id="d57d8-112">Move an active Connector into the group</span></span>

<span data-ttu-id="d57d8-113">Om du vill veta vilka av dessa är problemet, öppna menyn ”Application Proxy” i ditt program och titta på varningsmeddelandet Connector grupp.</span><span class="sxs-lookup"><span data-stu-id="d57d8-113">To know which of these is the issue, open the “Application Proxy” menu in your Application, and look at the Connector Group warning message.</span></span> <span data-ttu-id="d57d8-114">Det anger att gruppen måste minst en koppling (du har ingen i gruppen) eller att det finns inga aktiva anslutningar (även om du har förmodligen inaktiva kopplingar).</span><span class="sxs-lookup"><span data-stu-id="d57d8-114">It specify either that the group needs at least one Connector (you have none in the group) or that it has no active Connectors (though you likely have inactive Connectors).</span></span>

   ![Val av anslutningen i Azure Portal](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

<span data-ttu-id="d57d8-116">Mer information om de här alternativen finns i avsnittet motsvarande nedan.</span><span class="sxs-lookup"><span data-stu-id="d57d8-116">For details on each of these options, see the corresponding section below.</span></span> <span data-ttu-id="d57d8-117">Var och en av dessa förutsätter att du startar från sidan för hantering av anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d57d8-117">Each of these assumes that you are starting from the Connector management page.</span></span> <span data-ttu-id="d57d8-118">Om du tittar på felmeddelandet går du till den här sidan genom att klicka på varningsmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="d57d8-118">If you are looking at the error message above, you can go to this page by clicking on the warning message.</span></span> <span data-ttu-id="d57d8-119">Annars det hittar du genom att gå till **Azure Active Directory**, klicka på **företagsprogram**, sedan **Application Proxy.**</span><span class="sxs-lookup"><span data-stu-id="d57d8-119">Otherwise this can be found by going to **Azure Active Directory**, clicking on **Enterprise Applications**, then **Application Proxy.**</span></span>

   ![Kopplingen grupphantering i Azure Portal](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a><span data-ttu-id="d57d8-121">Hämta en ny koppling</span><span class="sxs-lookup"><span data-stu-id="d57d8-121">Download a new Connector</span></span>

<span data-ttu-id="d57d8-122">Använd knappen ”Hämta anslutning” längst upp på sidan om du vill hämta en ny koppling.</span><span class="sxs-lookup"><span data-stu-id="d57d8-122">To download a new Connector, use the “Download Connector” button at the top of the page.</span></span>

<span data-ttu-id="d57d8-123">Observera anslutningen måste vara installerad på en dator med fri till backend-programmet och placeras vanligtvis på samma server som programmet.</span><span class="sxs-lookup"><span data-stu-id="d57d8-123">note the Connector needs to be installed on a machine with direct line of sight to the backend application, and is typically placed on the same server as the application.</span></span> <span data-ttu-id="d57d8-124">När du har hämtat, ska kopplingen visas i den här menyn.</span><span class="sxs-lookup"><span data-stu-id="d57d8-124">After downloading, the Connector should appear in this menu.</span></span> <span data-ttu-id="d57d8-125">Klicka på anslutningen och använda listrutan ”Connector grupp” för att se till att det tillhör gruppen rätt.</span><span class="sxs-lookup"><span data-stu-id="d57d8-125">click the Connector, and use the “Connector Group” drop-down to make sure it belongs to the right group.</span></span> <span data-ttu-id="d57d8-126">Spara ändringen.</span><span class="sxs-lookup"><span data-stu-id="d57d8-126">Save the change.</span></span>

   ![Ladda ned anslutningen från Azure-portalen](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a><span data-ttu-id="d57d8-128">Flytta en aktiv koppling</span><span class="sxs-lookup"><span data-stu-id="d57d8-128">Move an Active Connector</span></span>

<span data-ttu-id="d57d8-129">Om du har en aktiv koppling som ska tillhöra gruppen och har fri backend målprogrammet kan du flytta kopplingen till grupp.</span><span class="sxs-lookup"><span data-stu-id="d57d8-129">If you have an active Connector that should belong to the group and has line of sight to the target backend application, you can move the Connector into the assigned group.</span></span> <span data-ttu-id="d57d8-130">Gör du genom att klicka på kopplingen.</span><span class="sxs-lookup"><span data-stu-id="d57d8-130">To do so, click the Connector.</span></span> <span data-ttu-id="d57d8-131">I fältet ”Connector grupp” använder du i listrutan och välj rätt grupp klickar du på Spara.</span><span class="sxs-lookup"><span data-stu-id="d57d8-131">In the “Connector Group” field, use the drop-down to select the correct group, and click Save.</span></span>

## <a name="resolve-an-inactive-connector"></a><span data-ttu-id="d57d8-132">Lösa en inaktiv anslutning</span><span class="sxs-lookup"><span data-stu-id="d57d8-132">Resolve an inactive Connector</span></span>

<span data-ttu-id="d57d8-133">Om endast kopplingar i gruppen är inaktiva sannolikt de på en dator som inte har de nödvändiga portarna som avblockerad.</span><span class="sxs-lookup"><span data-stu-id="d57d8-133">If the only Connectors in the group are inactive, they are likely on a machine that does not have all the necessary ports unblocked.</span></span>

<span data-ttu-id="d57d8-134">Se portar felsöka dokument för mer information på undersöker problemet.</span><span class="sxs-lookup"><span data-stu-id="d57d8-134">see the ports Troubleshoot document for details on investigating this problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d57d8-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d57d8-135">Next steps</span></span>
[<span data-ttu-id="d57d8-136">Förstå Azure AD Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="d57d8-136">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)


