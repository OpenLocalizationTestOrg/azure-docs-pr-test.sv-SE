---
title: "aaaSet upp PowerShell toocreate en virtuell dator för hello Marketplace | Microsoft Docs"
description: "Instruktioner för att installera Azure PowerShell och använda det som en valfri process flow toocreate VM-avbildningar toodeploy till, och sälja på, hello Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
ms.openlocfilehash: cd2ebad7472248b8f921706e1a8c82d41f33b9cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-powershell-toocreate-an-offer-for-hello-azure-marketplace"></a><span data-ttu-id="d921a-103">Konfigurera Azure PowerShell toocreate ett erbjudande om hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="d921a-103">Set up Azure PowerShell toocreate an offer for hello Azure Marketplace</span></span>
<span data-ttu-id="d921a-104">Detaljerad information om hur tooset upp PowerShell i Azure, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d921a-104">For detailed information on how tooset up PowerShell in Azure, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="d921a-105">En enkel metod är toouse hello certifikat-metoden, som hämtar och importerar ett certifikat som krävs för autentisering.</span><span class="sxs-lookup"><span data-stu-id="d921a-105">A simple approach is toouse hello certificate method, which downloads and imports a certificate needed for authentication.</span></span> <span data-ttu-id="d921a-106">tooobtain hello behövs certifikat använder hello **Get-AzurePublishSettingsFile** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d921a-106">tooobtain hello needed certificate, use hello **Get-AzurePublishSettingsFile** cmdlet.</span></span> <span data-ttu-id="d921a-107">Spara hello-filen när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="d921a-107">Save hello file when you're prompted.</span></span> <span data-ttu-id="d921a-108">tooimport hello certifikatet till en PowerShell-session, Använd hello **importera AzurePublishSettingsFile** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d921a-108">tooimport hello certificate into a PowerShell session, use hello **Import-AzurePublishSettingsFile** cmdlet.</span></span>

<span data-ttu-id="d921a-109">tooconfigure och lagra hello vanliga Microsoft Azure-prenumerationsinställningar för hello PowerShell-sessionen använder hello **Set AzureSubscription** och **Välj AzureSubscription** cmdlets:</span><span class="sxs-lookup"><span data-stu-id="d921a-109">tooconfigure and store hello common Microsoft Azure subscription settings for hello PowerShell session, use hello **Set-AzureSubscription** and **Select-AzureSubscription** cmdlets:</span></span>

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

<span data-ttu-id="d921a-110">hello första kommandot associerar standardkontot för lagring med hello prenumeration (behövs för vissa etablering VM-åtgärder).</span><span class="sxs-lookup"><span data-stu-id="d921a-110">hello first command associates a default storage account with hello subscription (needed for some VM provisioning operations).</span></span>  <span data-ttu-id="d921a-111">hello gör andra hello prenumeration hello aktuella en (som identifieras av andra cmdlet: ar).</span><span class="sxs-lookup"><span data-stu-id="d921a-111">hello second makes hello subscription hello current one (recognized by other cmdlets).</span></span>

## <a name="see-also"></a><span data-ttu-id="d921a-112">Se även</span><span class="sxs-lookup"><span data-stu-id="d921a-112">See also</span></span>
* [<span data-ttu-id="d921a-113">Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="d921a-113">Getting started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)
* [<span data-ttu-id="d921a-114">Skapa en avbildning av virtuell dator för hello Marketplace</span><span class="sxs-lookup"><span data-stu-id="d921a-114">Creating a virtual machine image for hello Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)

