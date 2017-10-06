---
title: "Azure Active Directory Domain Services: Uppdatera DNS-inställningarna för hello virtuella Azure-nätverket | Microsoft Docs"
description: "Komma igång med Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: maheshu
ms.openlocfilehash: e6eaff555cb9b7bb89ab7581d8de0b8cfc844529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-preview"></a><span data-ttu-id="637ba-103">Aktivera Azure Active Directory Domain Services (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="637ba-103">Enable Azure Active Directory Domain Services (Preview)</span></span>

## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a><span data-ttu-id="637ba-104">Uppgift 4: uppdatera DNS-inställningarna för hello Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="637ba-104">Task 4: update DNS settings for hello Azure virtual network</span></span>
<span data-ttu-id="637ba-105">Du har aktiverat Azure Active Directory Domain Services för din katalog i hello som föregår konfigurationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="637ba-105">In hello preceding configuration tasks, you have successfully enabled Azure Active Directory Domain Services for your directory.</span></span> <span data-ttu-id="637ba-106">hello nästa uppgift är tooensure att datorer i hello virtuella nätverk kan ansluta och använda dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="637ba-106">hello next task is tooensure that computers within hello virtual network can connect and consume these services.</span></span> <span data-ttu-id="637ba-107">I den här artikeln uppdatera hello DNS-serverinställningarna för virtuellt nätverk toopoint toohello två IP-adresserna där Azure Active Directory Domain Services är tillgängligt på hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="637ba-107">In this article, you update hello DNS server settings for your virtual network toopoint toohello two IP addresses where Azure Active Directory Domain Services is available on hello virtual network.</span></span>

<span data-ttu-id="637ba-108">tooupdate hello DNS-Serverinställningen för hello virtuella nätverk som du har aktiverat Azure Active Directory Domain Services, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="637ba-108">tooupdate hello DNS server setting for hello virtual network in which you have enabled Azure Active Directory Domain Services, complete hello following steps:</span></span>

1. <span data-ttu-id="637ba-109">Hej **översikt** fliken visas en uppsättning **krävs konfigurationssteg** toobe utföras när din hanterade domän är helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="637ba-109">hello **Overview** tab lists a set of **Required configuration steps** toobe performed after your managed domain is fully provisioned.</span></span> <span data-ttu-id="637ba-110">hello första konfigurationssteget är **uppdatera DNS-serverinställningarna för det virtuella nätverket**.</span><span class="sxs-lookup"><span data-stu-id="637ba-110">hello first configuration step is **Update DNS server settings for your virtual network**.</span></span>

    ![Domain Services - översiktsflik vid full etablering](./media/getting-started/domain-services-provisioned-overview.png)

2. <span data-ttu-id="637ba-112">När din domän är helt etablerad, visas två IP-adresser i den här panelen.</span><span class="sxs-lookup"><span data-stu-id="637ba-112">When your domain is fully provisioned, two IP addresses are displayed in this tile.</span></span> <span data-ttu-id="637ba-113">IP-adresserna representerar en domänkontrollant för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="637ba-113">Each of these IP addresses represents a domain controller for your managed domain.</span></span>

3. <span data-ttu-id="637ba-114">toocopy hello första IP-Adressen tooclipboard och klickar på hello Kopiera knappen Nästa tooit.</span><span class="sxs-lookup"><span data-stu-id="637ba-114">toocopy hello first IP address tooclipboard, click hello copy button next tooit.</span></span> <span data-ttu-id="637ba-115">Klicka på hello **konfigurera DNS-servrar** knappen.</span><span class="sxs-lookup"><span data-stu-id="637ba-115">Then click hello **Configure DNS servers** button.</span></span>

4. <span data-ttu-id="637ba-116">Klistra in hello första IP-adressen i hello **lägga till DNS-server** TextBox-kontroll i hello **DNS-servrar** bladet.</span><span class="sxs-lookup"><span data-stu-id="637ba-116">Paste hello first IP address into hello **Add DNS server** textbox in hello **DNS servers** blade.</span></span> <span data-ttu-id="637ba-117">Rullar vågrätt toohello lämnas toocopy hello andra IP-adressen och klistra in den i hello **lägga till DNS-server** textruta.</span><span class="sxs-lookup"><span data-stu-id="637ba-117">Scroll horizontally toohello left toocopy hello second IP address and paste it into hello **Add DNS server** textbox.</span></span>

    ![Domain Services – uppdatera DNS](./media/getting-started/domain-services-update-dns.png)

5. <span data-ttu-id="637ba-119">Klicka på **spara** när du är klar tooupdate hello DNS-servrar för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="637ba-119">Click **Save** when you are done tooupdate hello DNS servers for hello virtual network.</span></span>

> [!NOTE]
> <span data-ttu-id="637ba-120">Virtuella datorer i nätverket hello bara hämta hello nya DNS-inställningarna efter en omstart.</span><span class="sxs-lookup"><span data-stu-id="637ba-120">Virtual machines in hello network only get hello new DNS settings after a restart.</span></span> <span data-ttu-id="637ba-121">Om du behöver dem tooget hello uppdatera DNS-inställningar nu direkt kan utlösa en omstart genom hello portal, PowerShell eller hello CLI.</span><span class="sxs-lookup"><span data-stu-id="637ba-121">If you need them tooget hello updated DNS settings right away, trigger a restart either by hello portal, PowerShell, or hello CLI.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="637ba-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="637ba-122">Next step</span></span>
[<span data-ttu-id="637ba-123">Uppgift 5: aktivera lösenord synkronisering tooAzure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="637ba-123">Task 5: enable password synchronization tooAzure Active Directory Domain Services</span></span>](active-directory-ds-getting-started-password-sync.md)
