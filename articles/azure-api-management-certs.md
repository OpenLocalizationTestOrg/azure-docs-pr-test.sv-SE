---
title: aaaUpload ett certifikat om Azure API | Microsoft Docs
description: "Lär dig hur tooupload athe Management API-certifikatet för hello klassiska Azure-portalen."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: adegeo
ms.openlocfilehash: 8294d7131cfb01dba664bd4fd04b6fc22c1e93ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a><span data-ttu-id="f052c-103">Ladda upp ett Hanteringscertifikat för Azure API för hantering</span><span class="sxs-lookup"><span data-stu-id="f052c-103">Upload an Azure Management API Management Certificate</span></span>
<span data-ttu-id="f052c-104">Certifikat kan tooauthenticate med hello klassiska distributionsmodellen som tillhandahålls av Azure.</span><span class="sxs-lookup"><span data-stu-id="f052c-104">Management certificates allow you tooauthenticate with hello classic deployment model provided by Azure.</span></span> <span data-ttu-id="f052c-105">Många program och verktyg (till exempel Visual Studio eller hello Azure SDK) använda dessa certifikat tooautomate konfiguration och distribution av olika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f052c-105">Many programs and tools (such as Visual Studio or hello Azure SDK) use these certificates tooautomate configuration and deployment of various Azure services.</span></span> 

> [!WARNING]
> <span data-ttu-id="f052c-106">Var försiktig!</span><span class="sxs-lookup"><span data-stu-id="f052c-106">Be careful!</span></span> <span data-ttu-id="f052c-107">Dessa typer av certifikat att alla som autentiserar med dem toomanage hello prenumeration de hör till.</span><span class="sxs-lookup"><span data-stu-id="f052c-107">These types of certificates allow anyone who authenticates with them toomanage hello subscription they are associated with.</span></span>
>
>

<span data-ttu-id="f052c-108">Om du vill ha mer information om Azure certifikat (inklusive att skapa ett självsignerat certifikat), se [certifikat översikt för Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span><span class="sxs-lookup"><span data-stu-id="f052c-108">If you'd like more information about Azure certificates (including creating a self-signed certificate), see [Certificates overview for Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).</span></span>

<span data-ttu-id="f052c-109">Du kan också använda [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) tooauthenticate-klientkod för automatisering.</span><span class="sxs-lookup"><span data-stu-id="f052c-109">You can also use [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) tooauthenticate client-code for automation purposes.</span></span>

## <a name="upload-a-management-certificate"></a><span data-ttu-id="f052c-110">Ladda upp ett hanteringscertifikat</span><span class="sxs-lookup"><span data-stu-id="f052c-110">Upload a management certificate</span></span>
<span data-ttu-id="f052c-111">När du har ett certifikat skapas, (.cer-fil med endast hello offentlig nyckel) kan du överföra den till hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f052c-111">Once you have a management certificate created, (.cer file with only hello public key) you can upload it into hello portal.</span></span> <span data-ttu-id="f052c-112">När hello certifikat är tillgänglig i hello portal, kan alla med ett matchande certifikat (privat nyckel) ansluta till hello Management API och komma åt hello resurser för hello associerade prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f052c-112">When hello certificate is available in hello portal, anyone with a matching certificate (private key) can connect through hello Management API and access hello resources for hello associated subscription.</span></span>

1. <span data-ttu-id="f052c-113">Logga in toohello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f052c-113">Log in toohello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="f052c-114">Klicka på **fler tjänster** hello längst ned i Azure-tjänsten listan Välj **prenumerationer** i hello _allmänna_ tjänstgruppen.</span><span class="sxs-lookup"><span data-stu-id="f052c-114">Click **More services** at hello bottom Azure service list, then select **Subscriptions** in hello _General_ service group.</span></span>

    ![Prenumerationen-menyn](./media/azure-api-management-certs/subscriptions_menu.png)

3. <span data-ttu-id="f052c-116">Kontrollera att tooselect hello korrekt prenumeration som du vill tooassociate med hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="f052c-116">Make sure tooselect hello correct subscription that you want tooassociate with hello certificate.</span></span>     
4. <span data-ttu-id="f052c-117">När du har valt hello korrekt prenumeration, tryck på **hanteringscertifikat** i hello _inställningar_ grupp.</span><span class="sxs-lookup"><span data-stu-id="f052c-117">After you have selected hello correct subscription, press **Management certificates** in hello _Settings_ group.</span></span>

    ![Inställningar](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. <span data-ttu-id="f052c-119">Tryck på hello **överför** knappen.</span><span class="sxs-lookup"><span data-stu-id="f052c-119">Press hello **Upload** button.</span></span>

    ![Ladda upp på sidan för certifikat](./media/azure-api-management-certs/certificates_page.png)
6. <span data-ttu-id="f052c-121">Fyll i hello dialogrutan information och tryck på **överför**.</span><span class="sxs-lookup"><span data-stu-id="f052c-121">Fill out hello dialog information and press **Upload**.</span></span>

    ![Inställningar](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a><span data-ttu-id="f052c-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f052c-123">Next steps</span></span>
<span data-ttu-id="f052c-124">Nu när du har ett certifikat som är associerade med en prenumeration kan du (när du har installerat hello matchande certifikat lokalt) via programmering ansluta toohello [klassiska distributionsmodellen REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) och automatisera Hej olika Azure-resurser som också är kopplade till den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="f052c-124">Now that you have a management certificate associated with a subscription, you can (after you have installed hello matching certificate locally) programmatically connect toohello [classic deployment model REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) and automate hello various Azure resources that are also associated with that subscription.</span></span>
