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
ms.openlocfilehash: 484ff1a197a651bccb2b416448056acf69b0d8c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-dns-settings-for-hello-azure-virtual-network"></a><span data-ttu-id="c2eb7-103">Uppdatera DNS-inställningarna för hello Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="c2eb7-103">Update DNS settings for hello Azure virtual network</span></span>
## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a><span data-ttu-id="c2eb7-104">Uppgift 4: Uppdatera DNS-inställningarna för hello Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="c2eb7-104">Task 4: Update DNS settings for hello Azure virtual network</span></span>
<span data-ttu-id="c2eb7-105">Du har aktiverat Azure Active Directory Domain Services för din katalog i hello som föregår konfigurationsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-105">In hello preceding configuration tasks, you have successfully enabled Azure Active Directory Domain Services for your directory.</span></span> <span data-ttu-id="c2eb7-106">hello nästa uppgift är tooensure att datorer i hello virtuella nätverk kan ansluta och använda dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-106">hello next task is tooensure that computers within hello virtual network can connect and consume these services.</span></span> <span data-ttu-id="c2eb7-107">I den här artikeln uppdatera hello DNS-serverinställningarna för virtuellt nätverk toopoint toohello två IP-adresserna där Azure Active Directory Domain Services är tillgängligt på hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-107">In this article, you update hello DNS server settings for your virtual network toopoint toohello two IP addresses where Azure Active Directory Domain Services is available on hello virtual network.</span></span>

> [!NOTE]
> <span data-ttu-id="c2eb7-108">När du har aktiverat Azure Active Directory Domain Services för hello directory Observera hello IP-adresser för Azure Active Directory Domain Services som visas på hello **konfigurera** för din katalog.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-108">After you've enabled Azure Active Directory Domain Services for hello directory, note hello IP addresses for Azure Active Directory Domain Services that are displayed on hello **Configure** tab of your directory.</span></span>
>
>

<span data-ttu-id="c2eb7-109">tooupdate hello DNS-Serverinställningen för hello virtuella nätverk som du har aktiverat Azure Active Directory Domain Services, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c2eb7-109">tooupdate hello DNS server setting for hello virtual network in which you have enabled Azure Active Directory Domain Services, complete hello following steps:</span></span>

1. <span data-ttu-id="c2eb7-110">Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="c2eb7-110">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="c2eb7-111">I hello vänster och välj **nätverk**.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-111">In hello left pane, select **Networks**.</span></span>  
    <span data-ttu-id="c2eb7-112">Hej **nätverk** öppnas.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-112">hello **Networks** window opens.</span></span>

    ![Fönstret virtuellt nätverk](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. <span data-ttu-id="c2eb7-114">På hello **virtuella nätverk** fliken, väljer hello virtuella nätverk som du har aktiverat Azure Active Directory Domain Services tooview dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-114">On hello **Virtual Networks** tab, select hello virtual network in which you enabled Azure Active Directory Domain Services tooview its properties.</span></span>
4. <span data-ttu-id="c2eb7-115">Klicka på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-115">Click hello **Configure** tab.</span></span>

    ![Fönstret virtuellt nätverk](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. <span data-ttu-id="c2eb7-117">I hello **DNS-servrar** ange både hello IP-adresserna som visades i hello **Domain Services** avsnittet hello **konfigurera** för din katalog.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-117">In hello **DNS servers** section, enter both of hello IP addresses that were displayed in hello **Domain Services** section on hello **Configure** tab of your directory.</span></span>
6. <span data-ttu-id="c2eb7-118">toosave hello DNS-serverinställningarna för det här virtuella nätverket hello i åtgärdsfönstret längst ned hello hello-fönstret klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-118">toosave hello DNS server settings for this virtual network, in hello task pane at hello bottom of hello window, click **Save**.</span></span>

   ![Uppdatera hello DNS-serverinställningarna för hello virtuellt nätverk](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  <span data-ttu-id="c2eb7-120">Virtuella datorer i nätverket hello bara hämta hello nya DNS-inställningarna efter en omstart.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-120">Virtual machines in hello network only get hello new DNS settings after a restart.</span></span> <span data-ttu-id="c2eb7-121">Om du behöver dem tooget hello uppdatera DNS-inställningar nu direkt kan utlösa en omstart genom hello portal, PowerShell eller hello CLI.</span><span class="sxs-lookup"><span data-stu-id="c2eb7-121">If you need them tooget hello updated DNS settings right away, trigger a restart either by hello portal, PowerShell, or hello CLI.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="c2eb7-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c2eb7-122">Next steps</span></span>
<span data-ttu-id="c2eb7-123">Uppgift 5: [aktivera lösenord synkronisering tooAzure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="c2eb7-123">Task 5: [Enable password synchronization tooAzure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)</span></span>
