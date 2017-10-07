---
title: "aaaAzure AD + Active Directory-krav för Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur tooset in Active Directory toowork med Azure RemoteApp."
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
ms.openlocfilehash: c1c4a7ad6fb96ec4d479fdc231f03d81b58b2b71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a><span data-ttu-id="47d3f-103">Azure AD + Active Directory-krav för Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="47d3f-103">Azure AD + Active Directory requirements for Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="47d3f-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="47d3f-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="47d3f-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="47d3f-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="47d3f-106">För hybrid Azure RemoteApp-samlingen eller för en molnsamling som du vill toofederate med AD Connect måste toodo hello följande.</span><span class="sxs-lookup"><span data-stu-id="47d3f-106">For your Azure RemoteApp hybrid collection or for a cloud collection that you want toofederate using AD Connect, you need toodo hello following.</span></span>

### <a name="connect-azure-ad-and-active-directory"></a><span data-ttu-id="47d3f-107">Azure AD Connect och Active Directory</span><span class="sxs-lookup"><span data-stu-id="47d3f-107">Connect Azure AD and Active Directory</span></span>
<span data-ttu-id="47d3f-108">Om du vill tooconnect Azure AD-klienten och din lokala Active Directory-miljöer, använder du AD Connect.</span><span class="sxs-lookup"><span data-stu-id="47d3f-108">If you want tooconnect your Azure AD tenant and your on-premises Active Directory environments, use AD Connect.</span></span> <span data-ttu-id="47d3f-109">Det tar du bara [4 klick](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tooconnect hello två kataloger.</span><span class="sxs-lookup"><span data-stu-id="47d3f-109">It will take you only [4 clicks](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tooconnect hello two directories.</span></span>

<span data-ttu-id="47d3f-110">Obs! – katalogsynkronisering krävs för hybridsamlingar.</span><span class="sxs-lookup"><span data-stu-id="47d3f-110">Note - Directory synchronization is required for hybrid collections.</span></span>

### <a name="make-sure-your-domaincom-match"></a><span data-ttu-id="47d3f-111">Kontrollera att din ”@domain.com” matchar</span><span class="sxs-lookup"><span data-stu-id="47d3f-111">Make sure your "@domain.com" match</span></span>
<span data-ttu-id="47d3f-112">Innan du börjar måste du kontrollera om den hello UPN för din lokala skog matchar hello suffix för Azure AD-domän.</span><span class="sxs-lookup"><span data-stu-id="47d3f-112">Before you get started, make sure that hello UPN for your on-premises forest matches hello suffix of your Azure AD domain.</span></span> 

<span data-ttu-id="47d3f-113">När du har skapat hello UPN domänsuffix i Azure AD, alla användare som loggar in på Azure RemoteApp ska logga in som ”användare @<hello suffix you set up>”.</span><span class="sxs-lookup"><span data-stu-id="47d3f-113">After you set up hello UPN domain suffix in Azure AD, all users logging into Azure RemoteApp will log in as "user@<hello suffix you set up>."</span></span> <span data-ttu-id="47d3f-114">Se till att användarna kan också logga in med hello samma user@suffix i hello lokal domän.</span><span class="sxs-lookup"><span data-stu-id="47d3f-114">Make sure that users can also log in with hello same user@suffix into hello on-premises domain.</span></span> <span data-ttu-id="47d3f-115">I vissa fall kan du konfigurera ett domännamn i Azure AD när du anger ett annat domänsuffix för hello användaren på-plats.</span><span class="sxs-lookup"><span data-stu-id="47d3f-115">In certain cases you can set up one domain name in Azure AD while specifying a different domain suffix for hello user on-prem.</span></span> <span data-ttu-id="47d3f-116">I det här fallet kan användarna inte kan tooconnect tooany domänanslutna datorer eller resurser via Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="47d3f-116">In this case, your users won't be able tooconnect tooany domain-joined computers or resources through Azure RemoteApp.</span></span>

<span data-ttu-id="47d3f-117">Om du ställer in din domän UPN-suffix i AAD som contoso.com, men vissa användare på lokala/AD finns konfigurerade toolog med till exempel @contoso.uk, och sedan dessa användare inte kommer att kunna toocorrectly logga in på hello ARA samling.</span><span class="sxs-lookup"><span data-stu-id="47d3f-117">For example, if you set up your UPN domain suffix in AAD as contoso.com, but some users on premises/AD are configured toolog in with @contoso.uk, then those users will not be able toocorrectly log into hello ARA collection.</span></span> <span data-ttu-id="47d3f-118">Användare måste vara i UPN i AAD och AD hello samma för hello inloggningen toobe möjliga ”</span><span class="sxs-lookup"><span data-stu-id="47d3f-118">Users UPN in AAD and AD must be hello same for hello login toobe possible”</span></span>

### <a name="create-objects-for-azure-remoteapp"></a><span data-ttu-id="47d3f-119">Skapa objekt för Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="47d3f-119">Create objects for Azure RemoteApp</span></span>
<span data-ttu-id="47d3f-120">Du måste också toocreate hello följande lokala Active Directory-objekt:</span><span class="sxs-lookup"><span data-stu-id="47d3f-120">You also need toocreate hello following on-premises Active Directory objects:</span></span>

* <span data-ttu-id="47d3f-121">En service-kontot tooprovide åtkomst toodomain resurser för RemoteApp-program genom att anslutas RDSH slutpunkterna toohello lokal domän.</span><span class="sxs-lookup"><span data-stu-id="47d3f-121">A service account tooprovide access toodomain resources for RemoteApp programs by joining RDSH end points toohello on-premises domain.</span></span>
* <span data-ttu-id="47d3f-122">En organisationsenhet (OU) toocontain RemoteApp-datorobjekt.</span><span class="sxs-lookup"><span data-stu-id="47d3f-122">An Organizational Unit (OU) toocontain RemoteApp machine objects.</span></span> <span data-ttu-id="47d3f-123">Hello OU används rekommenderade (men krävs inte) tooisolate hello konton och principer som du vill använda med RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="47d3f-123">Use of hello OU is recommended (but not required) tooisolate hello accounts and policies you will use with RemoteApp.</span></span>

<span data-ttu-id="47d3f-124">Du måste båda av dessa objekt när du skapar en RemoteApp-samlingen så att du toodo här först.</span><span class="sxs-lookup"><span data-stu-id="47d3f-124">You need both of these objects when you create your RemoteApp collection, so be sure toodo these steps first.</span></span>

