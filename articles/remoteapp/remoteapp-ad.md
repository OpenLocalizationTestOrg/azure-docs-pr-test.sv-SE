---
title: "Azure AD + Active Directory-krav för Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om att konfigurera Active Directory för att fungera med Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 66366b25-6012-45fa-a4f6-da0ddfe0b486
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 78008a032faa93795cc02b720d68a0c6f5f16e9a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a><span data-ttu-id="2ba01-103">Azure AD + Active Directory-krav för Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="2ba01-103">Azure AD + Active Directory requirements for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="2ba01-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="2ba01-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="2ba01-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="2ba01-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="2ba01-106">Du måste göra följande för hybrid Azure RemoteApp-samlingen eller för en molnsamling som du vill federera med AD Connect.</span><span class="sxs-lookup"><span data-stu-id="2ba01-106">For your Azure RemoteApp hybrid collection or for a cloud collection that you want to federate using AD Connect, you need to do the following.</span></span>

### <a name="connect-azure-ad-and-active-directory"></a><span data-ttu-id="2ba01-107">Azure AD Connect och Active Directory</span><span class="sxs-lookup"><span data-stu-id="2ba01-107">Connect Azure AD and Active Directory</span></span>
<span data-ttu-id="2ba01-108">Om du vill ansluta din Azure AD-klient och din lokala Active Directory-miljöer, använder du AD Connect.</span><span class="sxs-lookup"><span data-stu-id="2ba01-108">If you want to connect your Azure AD tenant and your on-premises Active Directory environments, use AD Connect.</span></span> <span data-ttu-id="2ba01-109">Det tar du bara [4 klick](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) att ansluta två kataloger.</span><span class="sxs-lookup"><span data-stu-id="2ba01-109">It will take you only [4 clicks](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) to connect the two directories.</span></span>

<span data-ttu-id="2ba01-110">Obs! – katalogsynkronisering krävs för hybridsamlingar.</span><span class="sxs-lookup"><span data-stu-id="2ba01-110">Note - Directory synchronization is required for hybrid collections.</span></span>

### <a name="make-sure-your-domaincom-match"></a><span data-ttu-id="2ba01-111">Kontrollera att din ”@domain.com” matchar</span><span class="sxs-lookup"><span data-stu-id="2ba01-111">Make sure your "@domain.com" match</span></span>
<span data-ttu-id="2ba01-112">Innan du börjar måste du kontrollera att UPN för den lokala skogen matchar suffixet för Azure AD-domän.</span><span class="sxs-lookup"><span data-stu-id="2ba01-112">Before you get started, make sure that the UPN for your on-premises forest matches the suffix of your Azure AD domain.</span></span> 

<span data-ttu-id="2ba01-113">När du har skapat domänsuffixet UPN i Azure AD, alla användare som loggar in på Azure RemoteApp ska logga in som ”användare @<the suffix you set up>”.</span><span class="sxs-lookup"><span data-stu-id="2ba01-113">After you set up the UPN domain suffix in Azure AD, all users logging into Azure RemoteApp will log in as "user@<the suffix you set up>."</span></span> <span data-ttu-id="2ba01-114">Kontrollera att användare kan också logga in med samma user@suffix i den lokala domänen.</span><span class="sxs-lookup"><span data-stu-id="2ba01-114">Make sure that users can also log in with the same user@suffix into the on-premises domain.</span></span> <span data-ttu-id="2ba01-115">I vissa fall kan du konfigurera ett domännamn i Azure AD när du anger ett annat domänsuffix för en användare på-plats.</span><span class="sxs-lookup"><span data-stu-id="2ba01-115">In certain cases you can set up one domain name in Azure AD while specifying a different domain suffix for the user on-prem.</span></span> <span data-ttu-id="2ba01-116">I detta fall kan användarna inte att ansluta till alla domänanslutna datorer eller resurser via Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="2ba01-116">In this case, your users won't be able to connect to any domain-joined computers or resources through Azure RemoteApp.</span></span>

<span data-ttu-id="2ba01-117">Om du ställer in din domän UPN-suffix i AAD som contoso.com, men vissa användare på lokala/AD har konfigurerats för att logga in med till exempel @contoso.uk, och sedan dessa användare inte korrekt logga in i samlingen ARA.</span><span class="sxs-lookup"><span data-stu-id="2ba01-117">For example, if you set up your UPN domain suffix in AAD as contoso.com, but some users on premises/AD are configured to log in with @contoso.uk, then those users will not be able to correctly log into the ARA collection.</span></span> <span data-ttu-id="2ba01-118">Användare UPN i AAD och AD måste vara samma för inloggningen till vara möjligt ”</span><span class="sxs-lookup"><span data-stu-id="2ba01-118">Users UPN in AAD and AD must be the same for the login to be possible”</span></span>

### <a name="create-objects-for-azure-remoteapp"></a><span data-ttu-id="2ba01-119">Skapa objekt för Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="2ba01-119">Create objects for Azure RemoteApp</span></span>
<span data-ttu-id="2ba01-120">Du måste också skapa följande lokal Active Directory-objekt:</span><span class="sxs-lookup"><span data-stu-id="2ba01-120">You also need to create the following on-premises Active Directory objects:</span></span>

* <span data-ttu-id="2ba01-121">Ett tjänstkonto för att ge åtkomst till resurser i domänen för RemoteApp-program genom att anslutas RDSH-slutpunkter till den lokala domänen.</span><span class="sxs-lookup"><span data-stu-id="2ba01-121">A service account to provide access to domain resources for RemoteApp programs by joining RDSH end points to the on-premises domain.</span></span>
* <span data-ttu-id="2ba01-122">En organisationsenhet (OU) som innehåller RemoteApp-datorobjekt.</span><span class="sxs-lookup"><span data-stu-id="2ba01-122">An Organizational Unit (OU) to contain RemoteApp machine objects.</span></span> <span data-ttu-id="2ba01-123">Användning av Organisationsenheten rekommenderas (men krävs inte) att isolera konton och principer som du vill använda med RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="2ba01-123">Use of the OU is recommended (but not required) to isolate the accounts and policies you will use with RemoteApp.</span></span>

<span data-ttu-id="2ba01-124">Du måste båda av dessa objekt när du skapar en RemoteApp-samlingen måste du utföra de här stegen först.</span><span class="sxs-lookup"><span data-stu-id="2ba01-124">You need both of these objects when you create your RemoteApp collection, so be sure to do these steps first.</span></span>

