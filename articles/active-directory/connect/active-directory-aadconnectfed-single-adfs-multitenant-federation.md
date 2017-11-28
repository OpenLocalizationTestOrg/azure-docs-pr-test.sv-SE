---
title: aaaFederating flera Azure AD med enda AD FS | Microsoft Docs
description: "I det här dokumentet får du lära dig hur toofederate flera Azure AD med en enda AD FS."
keywords: federate, ADFS, AD FS, multiple tenants, single AD FS, one ADFS, multi-tenant federation, multi-forest adfs, aad connect, federation, cross-tenant federation
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.openlocfilehash: 442192896b3b13f7bf9388396cd3769e194329d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#<a name="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs"></a><span data-ttu-id="f58f1-104">Federera flera instanser av Azure AD med en enda instans av AD FS</span><span class="sxs-lookup"><span data-stu-id="f58f1-104">Federate multiple instances of Azure AD with single instance of AD FS</span></span>

<span data-ttu-id="f58f1-105">En enda AD FS-servergrupp med hög tillgänglighet kan federera flera skogar om de har ett dubbelriktat förtroende.</span><span class="sxs-lookup"><span data-stu-id="f58f1-105">A single high available AD FS farm can federate multiple forests if they have 2-way trust between them.</span></span> <span data-ttu-id="f58f1-106">Dessa flera skogar kanske eller kanske inte stämmer överens toohello samma Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f58f1-106">These multiple forests may or may not correspond toohello same Azure Active Directory.</span></span> <span data-ttu-id="f58f1-107">Den här artikeln innehåller anvisningar för hur tooconfigure federation mellan en enskild AD FS-distribution och flera skogar som synkronisering toodifferent Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f58f1-107">This article provides instructions on how tooconfigure federation between a single AD FS deployment and more than one forests that sync toodifferent Azure AD.</span></span>

![Federation med flera innehavare med en enda AD FS](media/active-directory-aadconnectfed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> <span data-ttu-id="f58f1-109">Tillbakaskrivning av enheter och automatisk enhetskoppling stöds inte i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="f58f1-109">Device writeback and automatic device join are not supported in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="f58f1-110">Azure AD Connect kan inte vara används tooconfigure federation i det här scenariot som Azure AD Connect kan konfigurera federation för domäner i en enda Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f58f1-110">Azure AD Connect cannot be used tooconfigure federation in this scenario as Azure AD Connect can configure federation for domains in a single Azure AD.</span></span>

##<a name="steps-for-federating-ad-fs-with-multiple-azure-ad"></a><span data-ttu-id="f58f1-111">Steg för att federera AD FS med flera Azure AD-instanser</span><span class="sxs-lookup"><span data-stu-id="f58f1-111">Steps for federating AD FS with multiple Azure AD</span></span>

<span data-ttu-id="f58f1-112">Överväg att en domänen contoso.com i Azure Active Directory contoso.onmicrosoft.com redan är federerat med hello AD FS lokalt installerade på contoso.com lokala Active Directory-miljö.</span><span class="sxs-lookup"><span data-stu-id="f58f1-112">Consider a domain contoso.com in Azure Active Directory contoso.onmicrosoft.com is already federated with hello AD FS on-premises installed in contoso.com on-premises Active Directory environment.</span></span> <span data-ttu-id="f58f1-113">Fabrikam.com är en domän i Azure Active Directory fabrikam.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="f58f1-113">Fabrikam.com is a domain in fabrikam.onmicrosoft.com Azure Active Directory.</span></span>

##<a name="step-1-establish-a-two-way-trust"></a><span data-ttu-id="f58f1-114">Steg 1: Upprätta ett dubbelriktat förtroende</span><span class="sxs-lookup"><span data-stu-id="f58f1-114">Step 1: Establish a two-way trust</span></span>
 
<span data-ttu-id="f58f1-115">För AD FS i contoso.com toobe kan tooauthenticate användare i fabrikam.com krävs ett dubbelriktat förtroende mellan contoso.com och fabrikam.com. Följ hello riktlinje i det här [artikel](https://technet.microsoft.com/library/cc816590.aspx) toocreate hello dubbelriktat förtroende.</span><span class="sxs-lookup"><span data-stu-id="f58f1-115">For AD FS in contoso.com toobe able tooauthenticate users in fabrikam.com, a two-way trust is needed between contoso.com and fabrikam.com. Follow hello guideline in this [article](https://technet.microsoft.com/library/cc816590.aspx) toocreate hello two-way trust.</span></span>
 
##<a name="step-2-modify-contosocom-federation-settings"></a><span data-ttu-id="f58f1-116">Steg 2: Ändra federationsinställningarna för contoso.com</span><span class="sxs-lookup"><span data-stu-id="f58f1-116">Step 2: Modify contoso.com federation settings</span></span> 
 
<span data-ttu-id="f58f1-117">hello standard utfärdaren som angetts för en enda domän federerad tooAD FS är ”http://ADFSServiceFQDN/adfs/services/trust”, till exempel ”http://fs.contoso.com/adfs/services/trust”.</span><span class="sxs-lookup"><span data-stu-id="f58f1-117">hello default issuer set for a single domain federated tooAD FS is "http://ADFSServiceFQDN/adfs/services/trust", for example, “http://fs.contoso.com/adfs/services/trust”.</span></span> <span data-ttu-id="f58f1-118">Azure Active Directory kräver en unik utfärdare för varje federerad domän.</span><span class="sxs-lookup"><span data-stu-id="f58f1-118">Azure Active Directory requires unique issuer for each federated domain.</span></span> <span data-ttu-id="f58f1-119">Eftersom hello samma AD FS kommer toofederate två domäner, måste värdet för hello utfärdaren toobe ändras så att den är unik för varje domän som AD FS federates med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f58f1-119">Since hello same AD FS is going toofederate two domains, hello issuer value needs toobe modified so that it is unique for each domain AD FS federates with Azure Active Directory.</span></span> 
 
<span data-ttu-id="f58f1-120">Öppna Azure AD PowerShell på hello AD FS-servern och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f58f1-120">On hello AD FS server, open Azure AD PowerShell and perform hello following steps:</span></span>
 
<span data-ttu-id="f58f1-121">Ansluta toohello Azure Active Directory som innehåller hello domänen contoso.com Anslut MsolService hello federation uppdateringsinställningar för contoso.com uppdatering MsolFederatedDomain - DomainName contoso.com – SupportMultipleDomain</span><span class="sxs-lookup"><span data-stu-id="f58f1-121">Connect toohello Azure Active Directory that contains hello domain contoso.com Connect-MsolService Update hello federation settings for contoso.com Update-MsolFederatedDomain -DomainName contoso.com –SupportMultipleDomain</span></span>
 
<span data-ttu-id="f58f1-122">Utfärdare i hello domänfederationsinställningen kommer att ändras för ”http://contoso.com/adfs/services/trust” och en utgivningsprinciper anspråk regeln ska läggas till för hello Azure AD förlitande part tooissue hello rätt issuerId värde baserat på hello UPN-suffix.</span><span class="sxs-lookup"><span data-stu-id="f58f1-122">Issuer in hello domain federation setting will be changed too"http://contoso.com/adfs/services/trust" and an issuance claim rule will be added for hello Azure AD Relying Party Trust tooissue hello correct issuerId value based on hello UPN suffix.</span></span>
 
##<a name="step-3-federate-fabrikamcom-with-ad-fs"></a><span data-ttu-id="f58f1-123">Steg 3: Federera fabrikam.com med AD FS</span><span class="sxs-lookup"><span data-stu-id="f58f1-123">Step 3: Federate fabrikam.com with AD FS</span></span>
 
<span data-ttu-id="f58f1-124">I Azure AD powershell utföras sessionen hello följande: Anslut tooAzure Active Directory som innehåller hello domän fabrikam.com</span><span class="sxs-lookup"><span data-stu-id="f58f1-124">In Azure AD powershell session perform hello following steps: Connect tooAzure Active Directory that contains hello domain fabrikam.com</span></span>

    Connect-MsolService
<span data-ttu-id="f58f1-125">Konvertera hello fabrikam.com hanterade domänen toofederated:</span><span class="sxs-lookup"><span data-stu-id="f58f1-125">Convert hello fabrikam.com managed domain toofederated:</span></span>

    Convert-MsolDomainToFederated -DomainName anandmsft.com -Verbose -SupportMultipleDomain
 
<span data-ttu-id="f58f1-126">hello ovan åtgärden kommer federera hello domän fabrikam.com med hello samma AD FS.</span><span class="sxs-lookup"><span data-stu-id="f58f1-126">hello above operation will federate hello domain fabrikam.com with hello same AD FS.</span></span> <span data-ttu-id="f58f1-127">Du kan kontrollera hello Domäninställningar med hjälp av Get-MsolDomainFederationSettings för båda domänerna.</span><span class="sxs-lookup"><span data-stu-id="f58f1-127">You can verify hello domain settings by using Get-MsolDomainFederationSettings for both domains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f58f1-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f58f1-128">Next steps</span></span>
[<span data-ttu-id="f58f1-129">Ansluta Active Directory med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f58f1-129">Connect Active Directory with Azure Active Directory</span></span>](active-directory-aadconnect.md)
