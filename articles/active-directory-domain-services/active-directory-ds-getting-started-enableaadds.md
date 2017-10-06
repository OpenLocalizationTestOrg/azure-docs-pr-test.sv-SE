---
title: 'Azure Active Directory Domain Services: Aktivera Azure Active Directory Domain Services | Microsoft Docs'
description: Aktivera Azure Active Directory Domain Services med hello klassiska Azure-portalen
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 6263eb1849808a7c85e572e1046bc9039362dd9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a><span data-ttu-id="ad003-103">Aktivera Azure Active Directory Domain Services med hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ad003-103">Enable Azure Active Directory Domain Services using hello Azure classic portal</span></span>

## <a name="task-3-enable-azure-active-directory-domain-services"></a><span data-ttu-id="ad003-104">Uppgift 3: aktivera Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="ad003-104">Task 3: enable Azure Active Directory Domain Services</span></span>
<span data-ttu-id="ad003-105">I det här steget aktivera Azure Active Directory Domain Services (Azure AD DS) för din katalog genom att göra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ad003-105">In this task, you enable Azure Active Directory Domain Services (Azure AD DS) for your directory by doing hello following steps:</span></span>

1. <span data-ttu-id="ad003-106">Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ad003-106">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="ad003-107">Välj hello hello vänster **Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="ad003-107">In hello left pane, select hello **Active Directory** button.</span></span>
3. <span data-ttu-id="ad003-108">Välj hello Azure Active Directory (Azure AD)-klienten (katalogen) som du vill tooenable Azure AD DS.</span><span class="sxs-lookup"><span data-stu-id="ad003-108">Select hello Azure Active Directory (Azure AD) tenant (directory) for which you want tooenable Azure AD DS.</span></span>

    ![Välja Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="ad003-110">På hello **preview directory** klickar du på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="ad003-110">On hello **preview directory** page, click hello **Configure** tab.</span></span>

    ![Fliken Konfigurera för katalogen](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="ad003-112">Under **domäntjänster**, ändra hello **aktivera domain services för den här katalogen** alternativet för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="ad003-112">Under **domain services**, change hello **Enable domain services for this directory** option too**Yes**.</span></span>  
    <span data-ttu-id="ad003-113">Ytterligare konfigurationsalternativ för Azure Active Directory Domain Services visas på hello sida.</span><span class="sxs-lookup"><span data-stu-id="ad003-113">Additional Azure Active Directory Domain Services configuration options appear on hello page.</span></span>

    ![Aktivera Domain Services](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > <span data-ttu-id="ad003-115">När du aktiverar Azure Active Directory Domain Services för din klient genererar Azure AD och lagrar hello Kerberos- och NTLM-hashvärdena som krävs för att autentisera användare.</span><span class="sxs-lookup"><span data-stu-id="ad003-115">When you enable Azure Active Directory Domain Services for your tenant, Azure AD generates and stores hello Kerberos and NTLM credential hashes that are required for authenticating users.</span></span>
   >
   >
6. <span data-ttu-id="ad003-116">Ange hello **DNS-domännamnet för domäntjänster**.</span><span class="sxs-lookup"><span data-stu-id="ad003-116">Specify hello **DNS domain name of domain services**.</span></span>

   * <span data-ttu-id="ad003-117">hello standarddomännamnet för hello katalogen (med en **. onmicrosoft.com** suffix) väljs som standard.</span><span class="sxs-lookup"><span data-stu-id="ad003-117">hello default domain name of hello directory (with a **.onmicrosoft.com** suffix) is selected by default.</span></span>

   * <span data-ttu-id="ad003-118">hello lista innehåller alla domäner som har konfigurerats för Azure AD-katalogen, inklusive både verifierade och Ej verifierad domäner som du konfigurerar på hello **domäner** fliken.</span><span class="sxs-lookup"><span data-stu-id="ad003-118">hello list contains all domains that have been configured for your Azure AD directory, including both verified and unverified domains that you configure on hello **Domains** tab.</span></span>

   * <span data-ttu-id="ad003-119">Du kan också ange ett anpassat domännamn.</span><span class="sxs-lookup"><span data-stu-id="ad003-119">You can also enter a custom domain name.</span></span> <span data-ttu-id="ad003-120">I det här exemplet hello domännamn är *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="ad003-120">In this example, hello custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="ad003-121">hello-prefixet för ett angivet domännamn (till exempel *contoso100* i hello *contoso100.com* domännamn) får innehålla högst 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="ad003-121">hello prefix of your specified domain name (for example, *contoso100* in hello *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="ad003-122">Du kan inte skapa en Azure Active Directory Domain Services-domän med ett prefix som innehåller fler än 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="ad003-122">You cannot create an Azure Active Directory Domain Services domain with a prefix containing more than 15 characters.</span></span>
     >
     >
7. <span data-ttu-id="ad003-123">Se till att hello DNS-domännamn som du har valt för hello hanterade domänen inte redan finns i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="ad003-123">Ensure that hello DNS domain name you have chosen for hello managed domain does not already exist in hello virtual network.</span></span> <span data-ttu-id="ad003-124">I synnerhet Kontrollera toosee om:</span><span class="sxs-lookup"><span data-stu-id="ad003-124">Specifically, check toosee whether:</span></span>

   * <span data-ttu-id="ad003-125">Du redan har en domän med hello samma DNS-domännamn på hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ad003-125">You already have a domain with hello same DNS domain name on hello virtual network.</span></span>

   * <span data-ttu-id="ad003-126">hello virtuella nätverk som du har valt har en VPN-anslutning med ditt lokala nätverk och du har en domän med hello samma DNS-namnet på ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="ad003-126">hello virtual network you've selected has a VPN connection with your on-premises network, and you have a domain with hello same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="ad003-127">Du har en befintlig molntjänst med det namnet på hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ad003-127">You have an existing cloud service with that name on hello virtual network.</span></span>
8. <span data-ttu-id="ad003-128">Välj ett virtuellt nätverk som du vill använda Azure Active Directory Domain Services toobe tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="ad003-128">Select a virtual network on which you want Azure Active Directory Domain Services toobe available.</span></span> <span data-ttu-id="ad003-129">Välj hello virtuella nätverk och dedikerad undernät som du skapade i hello **Anslut domäntjänster toothis virtuellt nätverk** listrutan.</span><span class="sxs-lookup"><span data-stu-id="ad003-129">Select hello virtual network and dedicated subnet you created in hello **Connect domain services toothis virtual network** drop-down list.</span></span> <span data-ttu-id="ad003-130">Tänk också hello följande:</span><span class="sxs-lookup"><span data-stu-id="ad003-130">Also consider hello following:</span></span>

   * <span data-ttu-id="ad003-131">Se till att hello virtuella nätverk som du har angett hör tooan Azure-region som stöds av Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="ad003-131">Ensure that hello virtual network that you have specified belongs tooan Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="ad003-132">tooascertain hello Azure-regioner där Azure Active Directory Domain Services är tillgängligt, se [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="ad003-132">tooascertain hello Azure regions where Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>

   * <span data-ttu-id="ad003-133">Virtuella nätverk som tillhör tooa region där Azure Active Directory Domain Services inte stöds visas inte i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="ad003-133">Virtual networks that belong tooa region where Azure Active Directory Domain Services is not supported do not show up in hello drop-down list.</span></span>

   * <span data-ttu-id="ad003-134">Använd en dedikerad undernät inom hello virtuellt nätverk för Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="ad003-134">Use a dedicated subnet within hello virtual network for Azure Active Directory Domain Services.</span></span> <span data-ttu-id="ad003-135">Gör *inte* Välj hello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="ad003-135">Do *not* select hello gateway subnet.</span></span> <span data-ttu-id="ad003-136">Se [Nätverksöverväganden](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="ad003-136">See [networking considerations](active-directory-ds-networking.md).</span></span>

   * <span data-ttu-id="ad003-137">På motsvarande sätt visas inte virtuella nätverk som har skapats med hjälp av Azure Resource Manager i hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="ad003-137">Similarly, virtual networks that were created by using Azure Resource Manager do not appear in hello drop-down list.</span></span> <span data-ttu-id="ad003-138">Resource Manager-baserade virtuella nätverk stöds för närvarande inte av Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="ad003-138">Resource Manager-based virtual networks are not currently supported by Azure Active Directory Domain Services.</span></span>
9. <span data-ttu-id="ad003-139">tooenable Azure Active Directory Domain Services, hello i åtgärdsfönstret längst ned hello hello-sidan, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ad003-139">tooenable Azure Active Directory Domain Services, in hello task pane at hello bottom of hello page, click **Save**.</span></span>
    * <span data-ttu-id="ad003-140">Medan Azure Active Directory Domain Services aktiveras för din katalog, hello sidan visar statusen *väntande*.</span><span class="sxs-lookup"><span data-stu-id="ad003-140">While Azure Active Directory Domain Services is being enabled for your directory, hello page displays a status of *Pending*.</span></span>

        ![Fönstret Enable Domain Services (Aktivera domäntjänster)](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > <span data-ttu-id="ad003-142">Azure Active Directory Domain Services ger hög tillgänglighet för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="ad003-142">Azure Active Directory Domain Services provides high availability for your managed domain.</span></span> <span data-ttu-id="ad003-143">När du har aktiverat Azure Active Directory Domain Services är hello IP-adresser på vilken domän tjänster som är tillgängliga på hello virtuella nätverk visas en i taget.</span><span class="sxs-lookup"><span data-stu-id="ad003-143">After you enable Azure Active Directory Domain Services, hello IP addresses at which domain services are available on hello virtual network are displayed one at a time.</span></span> <span data-ttu-id="ad003-144">hello andra IP-adressen visas strax efter hello först som snart hello tjänsten aktiverar hög tillgänglighet för din domän.</span><span class="sxs-lookup"><span data-stu-id="ad003-144">hello second IP address is displayed shortly after hello first, as soon hello service enables high availability for your domain.</span></span> <span data-ttu-id="ad003-145">När hög tillgänglighet har konfigurerats och aktiverats för din domän, bör du se två IP-adresser i hello **domäntjänster** avsnitt i hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="ad003-145">When high availability is configured and active for your domain, you should see two IP addresses in hello **domain services** section of hello **Configure** tab.</span></span>
        >
        >
    * <span data-ttu-id="ad003-146">Efter cirka 20 too30 minuter hello första IP-adressen på vilken domän tjänster som är tillgängliga på det virtuella nätverket i hello **IP-adress** på hello **konfigurera** sidan.</span><span class="sxs-lookup"><span data-stu-id="ad003-146">After about 20 too30 minutes, hello first IP address at which domain services are available on your virtual network in hello **IP address** field on hello **Configure** page.</span></span>

        ![Fönstret Domain Services (Domäntjänster) med den första etablerade IP-adressen](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * <span data-ttu-id="ad003-148">När hög tillgänglighet är tillgängligt för din domän visas två IP-adresser på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="ad003-148">When high availability is operational for your domain, two IP addresses are displayed on hello page.</span></span> <span data-ttu-id="ad003-149">Den hanterade domänen är tillgänglig i ditt valda virtuella nätverk på dessa två IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="ad003-149">Your managed domain is available on your selected virtual network at these two IP addresses.</span></span>

10. <span data-ttu-id="ad003-150">Observera hello två IP-adresser så att du kan uppdatera hello DNS-inställningarna för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="ad003-150">Note hello two IP addresses so that you can update hello DNS settings for your virtual network.</span></span> <span data-ttu-id="ad003-151">På så sätt kan virtuella datorer på hello virtuellt nätverk tooconnect toohello domän för åtgärder som till exempel domänanslutning.</span><span class="sxs-lookup"><span data-stu-id="ad003-151">Doing so enables virtual machines on hello virtual network tooconnect toohello domain for operations such as domain join.</span></span>

    ![Fönstret Domain Services (Domäntjänster) med de två etablerade IP-adresserna](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> <span data-ttu-id="ad003-153">Synkronisering tooyour hanterad domän tar ett tag beroende på hello storlek för din Azure AD-klient (till exempel hello antal användare eller grupper).</span><span class="sxs-lookup"><span data-stu-id="ad003-153">Depending on hello size of your Azure AD tenant (for example, hello number of users or groups), synchronization tooyour managed domain takes a while.</span></span> <span data-ttu-id="ad003-154">Den här synkroniseringsprocessen sker i bakgrunden hello.</span><span class="sxs-lookup"><span data-stu-id="ad003-154">This synchronization process happens in hello background.</span></span> <span data-ttu-id="ad003-155">För stora klienter med tiotusentals objekt kan det ta en dag eller två för alla användare, gruppmedlemskap och autentiseringsuppgifter toobe synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="ad003-155">For large tenants with tens of thousands of objects, it might take a day or two for all users, group memberships, and credentials toobe synchronized.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="ad003-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ad003-156">Next step</span></span>
[<span data-ttu-id="ad003-157">Uppgift 4: uppdatera hello DNS-inställningarna för hello Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="ad003-157">Task 4: update hello DNS settings for hello Azure virtual network</span></span>](active-directory-ds-getting-started-update-dns.md)
