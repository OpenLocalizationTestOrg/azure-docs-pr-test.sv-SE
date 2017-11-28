---
title: "aaaConfigure säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs"
description: "Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 99e44d917b115d7f7a67ff0a5e22c5c9165287e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="35db6-103">Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="35db6-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="35db6-104">Den här artikeln visar hur du kan aktivera säker Lightweight Directory Access Protocol (LDAPS) för din Azure AD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="35db6-104">This article shows how you can enable Secure Lightweight Directory Access Protocol (LDAPS) for your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="35db6-105">Säker LDAP kallas även ”Lightweight Directory Access Protocol (LDAP) över Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.</span><span class="sxs-lookup"><span data-stu-id="35db6-105">Secure LDAP is also known as 'Lightweight Directory Access Protocol (LDAP) over Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="35db6-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="35db6-106">Before you begin</span></span>
<span data-ttu-id="35db6-107">tooperform hello uppgifterna som listas i den här artikeln, behöver du:</span><span class="sxs-lookup"><span data-stu-id="35db6-107">tooperform hello tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="35db6-108">En giltig **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="35db6-108">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="35db6-109">En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.</span><span class="sxs-lookup"><span data-stu-id="35db6-109">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="35db6-110">**Azure AD Domain Services** måste vara aktiverat för hello Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="35db6-110">**Azure AD Domain Services** must be enabled for hello Azure AD directory.</span></span> <span data-ttu-id="35db6-111">Om du inte gjort det, följ alla hello uppgifter som beskrivs i hello [Kom igång-guiden](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="35db6-111">If you haven't done so, follow all hello tasks outlined in hello [Getting Started guide](active-directory-ds-getting-started.md).</span></span>
4. <span data-ttu-id="35db6-112">En **certifikat toobe används tooenable säkert LDAP**.</span><span class="sxs-lookup"><span data-stu-id="35db6-112">A **certificate toobe used tooenable secure LDAP**.</span></span>

   * <span data-ttu-id="35db6-113">**Rekommenderade** -erhåller ett certifikat från en betrodd offentlig certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="35db6-113">**Recommended** - Obtain a certificate from a trusted public certification authority.</span></span> <span data-ttu-id="35db6-114">Det här konfigurationsalternativet är säkrare.</span><span class="sxs-lookup"><span data-stu-id="35db6-114">This configuration option is more secure.</span></span>
   * <span data-ttu-id="35db6-115">Alternativt kan du kan också välja för[skapa ett självsignerat certifikat](#task-1---obtain-a-certificate-for-secure-ldap) enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="35db6-115">Alternately, you may also choose too[create a self-signed certificate](#task-1---obtain-a-certificate-for-secure-ldap) as shown later in this article.</span></span>

<br>

### <a name="requirements-for-hello-secure-ldap-certificate"></a><span data-ttu-id="35db6-116">Krav för hello säker LDAP-certifikat</span><span class="sxs-lookup"><span data-stu-id="35db6-116">Requirements for hello secure LDAP certificate</span></span>
<span data-ttu-id="35db6-117">Hämta ett giltigt certifikat per hello följande riktlinjer innan du aktiverar säker LDAP.</span><span class="sxs-lookup"><span data-stu-id="35db6-117">Acquire a valid certificate per hello following guidelines, before you enable secure LDAP.</span></span> <span data-ttu-id="35db6-118">Det uppstår fel om du försöker tooenable säkert LDAP för din hanterade domän med ett ogiltigt felaktigt certifikat.</span><span class="sxs-lookup"><span data-stu-id="35db6-118">You encounter failures if you try tooenable secure LDAP for your managed domain with an invalid/incorrect certificate.</span></span>

1. <span data-ttu-id="35db6-119">**Betrodda utfärdare** -hello certifikatet måste utfärdas av en certifikatutfärdare som betrodd av datorer som ansluter toohello hanterade domänen med säker LDAP.</span><span class="sxs-lookup"><span data-stu-id="35db6-119">**Trusted issuer** - hello certificate must be issued by an authority trusted by computers connecting toohello managed domain using secure LDAP.</span></span> <span data-ttu-id="35db6-120">Den här utfärdare kan vara en offentlig certifikatutfärdare som betrodd av dessa datorer.</span><span class="sxs-lookup"><span data-stu-id="35db6-120">This authority may be a public certification authority trusted by these computers.</span></span>
2. <span data-ttu-id="35db6-121">**Livslängd** -hello certifikatet måste vara giltiga för minst hello nästa 3-6 månader.</span><span class="sxs-lookup"><span data-stu-id="35db6-121">**Lifetime** - hello certificate must be valid for at least hello next 3-6 months.</span></span> <span data-ttu-id="35db6-122">Säkert LDAP åtkomst tooyour hanterad domän avbryts när hello certifikatet upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="35db6-122">Secure LDAP access tooyour managed domain is disrupted when hello certificate expires.</span></span>
3. <span data-ttu-id="35db6-123">**Ämnesnamn** -hello Subjektnamnet på certifikatet hello måste vara ett jokertecken för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="35db6-123">**Subject name** - hello subject name on hello certificate must be a wildcard for your managed domain.</span></span> <span data-ttu-id="35db6-124">Till exempel om din domän är med namnet ”contoso100.com” hello certifikatets ämnesnamn måste vara ' *. contoso100.com ”.</span><span class="sxs-lookup"><span data-stu-id="35db6-124">For instance, if your domain is named 'contoso100.com', hello certificate's subject name must be '*.contoso100.com'.</span></span> <span data-ttu-id="35db6-125">Ange hello DNS-namn (Alternativt ämnesnamn) toothis jokertecken namn.</span><span class="sxs-lookup"><span data-stu-id="35db6-125">Set hello DNS name (subject alternate name) toothis wildcard name.</span></span>
4. <span data-ttu-id="35db6-126">**Nyckelanvändning** - hello certifikat konfigureras för hello följande använder - digitala signaturer och nyckelchiffrering.</span><span class="sxs-lookup"><span data-stu-id="35db6-126">**Key usage** - hello certificate must be configured for hello following uses - Digital signatures and key encipherment.</span></span>
5. <span data-ttu-id="35db6-127">**Certifikat syfte** -hello certifikatet måste vara giltigt för serverautentisering för SSL.</span><span class="sxs-lookup"><span data-stu-id="35db6-127">**Certificate purpose** - hello certificate must be valid for SSL server authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="35db6-128">**Företagscertifikatutfärdare:** Azure AD Domain Services stöder inte användning av säker LDAP-certifikat som utfärdats av din organisations utfärdare av företagscertifikat.</span><span class="sxs-lookup"><span data-stu-id="35db6-128">**Enterprise Certification Authorities:** Azure AD Domain Services does not support using secure LDAP certificates issued by your organization's enterprise certification authority.</span></span> <span data-ttu-id="35db6-129">Den här begränsningen beror hello-tjänsten inte har förtroende för ditt företags-CA som en rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="35db6-129">This restriction is because hello service does not trust your enterprise CA as a root certification authority.</span></span> 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a><span data-ttu-id="35db6-130">Uppgift 1 – skaffa ett certifikat för säker LDAP</span><span class="sxs-lookup"><span data-stu-id="35db6-130">Task 1 - obtain a certificate for secure LDAP</span></span>
<span data-ttu-id="35db6-131">hello första uppgiften innebär att erhålla ett certifikat som används för säker LDAP åtkomst toohello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="35db6-131">hello first task involves obtaining a certificate used for secure LDAP access toohello managed domain.</span></span> <span data-ttu-id="35db6-132">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="35db6-132">You have two options:</span></span>

* <span data-ttu-id="35db6-133">Skaffa ett certifikat från en certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="35db6-133">Obtain a certificate from a certification authority.</span></span> <span data-ttu-id="35db6-134">hello-utfärdare kan vara en offentlig certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="35db6-134">hello authority may be a public certification authority.</span></span>
* <span data-ttu-id="35db6-135">Skapa ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="35db6-135">Create a self-signed certificate.</span></span>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a><span data-ttu-id="35db6-136">Alternativet (rekommenderas) – skaffa ett säkert LDAP-certifikat från en certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="35db6-136">Option A (Recommended) - Obtain a secure LDAP certificate from a certification authority</span></span>
<span data-ttu-id="35db6-137">Om din organisation hämtar certifikat från en offentlig certifikatutfärdare, måste tooobtain hello säker LDAP-certifikat från den offentlig certifikatutfärdaren.</span><span class="sxs-lookup"><span data-stu-id="35db6-137">If your organization obtains its certificates from a public certification authority, you need tooobtain hello secure LDAP certificate from that public certification authority.</span></span>

<span data-ttu-id="35db6-138">När du begär ett certifikat, se till att du uppfyller alla hello kraven som beskrivs i [kraven för hello säker LDAP certifikatet](#requirements-for-the-secure-ldap-certificate).</span><span class="sxs-lookup"><span data-stu-id="35db6-138">When requesting a certificate, ensure that you satisfy all hello requirements outlined in [Requirements for hello secure LDAP certificate](#requirements-for-the-secure-ldap-certificate).</span></span>

> [!NOTE]
> <span data-ttu-id="35db6-139">Klientdatorer som behöver tooconnect toohello hanterade domänen med säker LDAP måste lita hello hello säker LDAP-certifikatets utfärdare.</span><span class="sxs-lookup"><span data-stu-id="35db6-139">Client computers that need tooconnect toohello managed domain using secure LDAP must trust hello issuer of hello secure LDAP certificate.</span></span>
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a><span data-ttu-id="35db6-140">Alternativ B – skapa ett självsignerat certifikat för säker LDAP</span><span class="sxs-lookup"><span data-stu-id="35db6-140">Option B - Create a self-signed certificate for secure LDAP</span></span>
<span data-ttu-id="35db6-141">Om du inte räknar toouse ett certifikat från en offentlig certifikatutfärdare, kan du välja toocreate ett självsignerat certifikat för säker LDAP.</span><span class="sxs-lookup"><span data-stu-id="35db6-141">If you do not expect toouse a certificate from a public certification authority, you may choose toocreate a self-signed certificate for secure LDAP.</span></span>

<span data-ttu-id="35db6-142">**Skapa ett självsignerat certifikat med hjälp av PowerShell**</span><span class="sxs-lookup"><span data-stu-id="35db6-142">**Create a self-signed certificate using PowerShell**</span></span>

<span data-ttu-id="35db6-143">På Windows-dator öppnar du ett nytt PowerShell-fönster som **administratör** och typen hello följande kommandon, toocreate ett nytt självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="35db6-143">On your Windows computer, open a new PowerShell window as **Administrator** and type hello following commands, toocreate a new self-signed certificate.</span></span>

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

<span data-ttu-id="35db6-144">I föregående exempel hello, ersätter '*. contoso100.com ”med hello DNS-domännamnet för din hanterade domän. Till exempel om du har skapat en hanterad domän som kallas ”contoso100.onmicrosoft.com” kan ersätta '*. contoso100.com ”i hello ovan skriptet med ' *. contoso100.onmicrosoft.com').</span><span class="sxs-lookup"><span data-stu-id="35db6-144">In hello preceding sample, replace '*.contoso100.com' with hello DNS domain name of your managed domain. For example, if you created a managed domain called 'contoso100.onmicrosoft.com', replace '*.contoso100.com' in hello above script with '*.contoso100.onmicrosoft.com').</span></span>

![Välja Azure AD-katalog](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

<span data-ttu-id="35db6-146">hello nyskapade självsignerade certifikatet placeras i hello lokala datorns certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="35db6-146">hello newly created self-signed certificate is placed in hello local machine's certificate store.</span></span>


## <a name="next-step"></a><span data-ttu-id="35db6-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35db6-147">Next step</span></span>
[<span data-ttu-id="35db6-148">Uppgift 2 – exportera hello säker LDAP certifikat tooa. PFX-fil</span><span class="sxs-lookup"><span data-stu-id="35db6-148">Task 2 - export hello secure LDAP certificate tooa .PFX file</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
