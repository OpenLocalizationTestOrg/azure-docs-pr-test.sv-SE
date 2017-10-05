---
title: "Konfigurera säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs"
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
ms.openlocfilehash: 93afa49166c5b31d23237c308b9d34f6d6f3507d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="81317-103">Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="81317-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="81317-104">Den här artikeln visar hur du kan aktivera säker Lightweight Directory Access Protocol (LDAPS) för din Azure AD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="81317-104">This article shows how you can enable Secure Lightweight Directory Access Protocol (LDAPS) for your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="81317-105">Säker LDAP kallas även ”Lightweight Directory Access Protocol (LDAP) över Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.</span><span class="sxs-lookup"><span data-stu-id="81317-105">Secure LDAP is also known as 'Lightweight Directory Access Protocol (LDAP) over Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="81317-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="81317-106">Before you begin</span></span>
<span data-ttu-id="81317-107">Om du vill utföra åtgärderna i den här artikeln behöver du:</span><span class="sxs-lookup"><span data-stu-id="81317-107">To perform the tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="81317-108">En giltig **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="81317-108">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="81317-109">En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.</span><span class="sxs-lookup"><span data-stu-id="81317-109">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="81317-110">**Azure AD Domain Services** måste vara aktiverat för Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="81317-110">**Azure AD Domain Services** must be enabled for the Azure AD directory.</span></span> <span data-ttu-id="81317-111">Om du inte gjort det, följer du de uppgifter som beskrivs i den [Kom igång-guiden](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="81317-111">If you haven't done so, follow all the tasks outlined in the [Getting Started guide](active-directory-ds-getting-started.md).</span></span>
4. <span data-ttu-id="81317-112">En **certifikat som används för att aktivera säker LDAP**.</span><span class="sxs-lookup"><span data-stu-id="81317-112">A **certificate to be used to enable secure LDAP**.</span></span>

   * <span data-ttu-id="81317-113">**Rekommenderade** -erhåller ett certifikat från en betrodd offentlig certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="81317-113">**Recommended** - Obtain a certificate from a trusted public certification authority.</span></span> <span data-ttu-id="81317-114">Det här konfigurationsalternativet är säkrare.</span><span class="sxs-lookup"><span data-stu-id="81317-114">This configuration option is more secure.</span></span>
   * <span data-ttu-id="81317-115">Alternativt kan du kan också välja att [skapa ett självsignerat certifikat](#task-1---obtain-a-certificate-for-secure-ldap) enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="81317-115">Alternately, you may also choose to [create a self-signed certificate](#task-1---obtain-a-certificate-for-secure-ldap) as shown later in this article.</span></span>

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a><span data-ttu-id="81317-116">Krav för säker LDAP-certifikat</span><span class="sxs-lookup"><span data-stu-id="81317-116">Requirements for the secure LDAP certificate</span></span>
<span data-ttu-id="81317-117">Hämta ett giltigt certifikat enligt följande riktlinjer innan du aktiverar säker LDAP.</span><span class="sxs-lookup"><span data-stu-id="81317-117">Acquire a valid certificate per the following guidelines, before you enable secure LDAP.</span></span> <span data-ttu-id="81317-118">Det uppstår fel vid försök att aktivera säker LDAP för din hanterade domän med ett ogiltigt felaktigt certifikat.</span><span class="sxs-lookup"><span data-stu-id="81317-118">You encounter failures if you try to enable secure LDAP for your managed domain with an invalid/incorrect certificate.</span></span>

1. <span data-ttu-id="81317-119">**Betrodda utfärdare** -certifikatet måste utfärdas av en certifikatutfärdare som betrodd av datorer som ansluter till den hanterade domänen med säker LDAP.</span><span class="sxs-lookup"><span data-stu-id="81317-119">**Trusted issuer** - The certificate must be issued by an authority trusted by computers connecting to the managed domain using secure LDAP.</span></span> <span data-ttu-id="81317-120">Den här utfärdare kan vara en offentlig certifikatutfärdare som betrodd av dessa datorer.</span><span class="sxs-lookup"><span data-stu-id="81317-120">This authority may be a public certification authority trusted by these computers.</span></span>
2. <span data-ttu-id="81317-121">**Livslängd** -certifikatet måste vara giltigt för minst de kommande 3-6 månaderna.</span><span class="sxs-lookup"><span data-stu-id="81317-121">**Lifetime** - The certificate must be valid for at least the next 3-6 months.</span></span> <span data-ttu-id="81317-122">Säker LDAP-åtkomst till din hanterade domän avbryts när certifikatet upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="81317-122">Secure LDAP access to your managed domain is disrupted when the certificate expires.</span></span>
3. <span data-ttu-id="81317-123">**Ämnesnamn** -Subjektnamnet på certifikatet måste vara ett jokertecken för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="81317-123">**Subject name** - The subject name on the certificate must be a wildcard for your managed domain.</span></span> <span data-ttu-id="81317-124">Till exempel om din domän har namnet ”contoso100.com”, certifikatets ämnesnamn måste vara ' *. contoso100.com ”.</span><span class="sxs-lookup"><span data-stu-id="81317-124">For instance, if your domain is named 'contoso100.com', the certificate's subject name must be '*.contoso100.com'.</span></span> <span data-ttu-id="81317-125">Ange DNS-namn (Alternativt ämnesnamn) till den här jokertecken-namn.</span><span class="sxs-lookup"><span data-stu-id="81317-125">Set the DNS name (subject alternate name) to this wildcard name.</span></span>
4. <span data-ttu-id="81317-126">**Nyckelanvändning** -certifikatet måste vara konfigurerad för följande använder - digitala signaturer och nyckelchiffrering.</span><span class="sxs-lookup"><span data-stu-id="81317-126">**Key usage** - The certificate must be configured for the following uses - Digital signatures and key encipherment.</span></span>
5. <span data-ttu-id="81317-127">**Certifikat syfte** -certifikatet måste vara giltigt för serverautentisering för SSL.</span><span class="sxs-lookup"><span data-stu-id="81317-127">**Certificate purpose** - The certificate must be valid for SSL server authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="81317-128">**Företagscertifikatutfärdare:** Azure AD Domain Services stöder inte användning av säker LDAP-certifikat som utfärdats av din organisations utfärdare av företagscertifikat.</span><span class="sxs-lookup"><span data-stu-id="81317-128">**Enterprise Certification Authorities:** Azure AD Domain Services does not support using secure LDAP certificates issued by your organization's enterprise certification authority.</span></span> <span data-ttu-id="81317-129">Den här begränsningen beror på att tjänsten inte har förtroende för ditt företags-CA som en rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="81317-129">This restriction is because the service does not trust your enterprise CA as a root certification authority.</span></span> 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a><span data-ttu-id="81317-130">Uppgift 1 – skaffa ett certifikat för säker LDAP</span><span class="sxs-lookup"><span data-stu-id="81317-130">Task 1 - obtain a certificate for secure LDAP</span></span>
<span data-ttu-id="81317-131">Den första aktiviteten innebär att erhålla ett certifikat som används för säker LDAP-åtkomst till den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="81317-131">The first task involves obtaining a certificate used for secure LDAP access to the managed domain.</span></span> <span data-ttu-id="81317-132">Du kan välja mellan två alternativ:</span><span class="sxs-lookup"><span data-stu-id="81317-132">You have two options:</span></span>

* <span data-ttu-id="81317-133">Skaffa ett certifikat från en certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="81317-133">Obtain a certificate from a certification authority.</span></span> <span data-ttu-id="81317-134">Behörighet kan vara en offentlig certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="81317-134">The authority may be a public certification authority.</span></span>
* <span data-ttu-id="81317-135">Skapa ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="81317-135">Create a self-signed certificate.</span></span>

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a><span data-ttu-id="81317-136">Alternativet (rekommenderas) – skaffa ett säkert LDAP-certifikat från en certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="81317-136">Option A (Recommended) - Obtain a secure LDAP certificate from a certification authority</span></span>
<span data-ttu-id="81317-137">Om din organisation hämtar certifikat från en offentlig certifikatutfärdare, måste du hämta säker LDAP-certifikatet från den offentlig certifikatutfärdaren.</span><span class="sxs-lookup"><span data-stu-id="81317-137">If your organization obtains its certificates from a public certification authority, you need to obtain the secure LDAP certificate from that public certification authority.</span></span>

<span data-ttu-id="81317-138">När du begär ett certifikat, se till att du uppfyller alla krav som beskrivs i [krav på säker LDAP-certifikatets](#requirements-for-the-secure-ldap-certificate).</span><span class="sxs-lookup"><span data-stu-id="81317-138">When requesting a certificate, ensure that you satisfy all the requirements outlined in [Requirements for the secure LDAP certificate](#requirements-for-the-secure-ldap-certificate).</span></span>

> [!NOTE]
> <span data-ttu-id="81317-139">Klientdatorer som behöver ansluta till den hanterade domänen med säker LDAP måste ha förtroende för säker LDAP-certifikatets utfärdare.</span><span class="sxs-lookup"><span data-stu-id="81317-139">Client computers that need to connect to the managed domain using secure LDAP must trust the issuer of the secure LDAP certificate.</span></span>
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a><span data-ttu-id="81317-140">Alternativ B – skapa ett självsignerat certifikat för säker LDAP</span><span class="sxs-lookup"><span data-stu-id="81317-140">Option B - Create a self-signed certificate for secure LDAP</span></span>
<span data-ttu-id="81317-141">Om du inte räknar med att använda ett certifikat från en offentlig certifikatutfärdare, kan du välja att skapa ett självsignerat certifikat för säker LDAP.</span><span class="sxs-lookup"><span data-stu-id="81317-141">If you do not expect to use a certificate from a public certification authority, you may choose to create a self-signed certificate for secure LDAP.</span></span>

<span data-ttu-id="81317-142">**Skapa ett självsignerat certifikat med hjälp av PowerShell**</span><span class="sxs-lookup"><span data-stu-id="81317-142">**Create a self-signed certificate using PowerShell**</span></span>

<span data-ttu-id="81317-143">På Windows-dator öppnar du ett nytt PowerShell-fönster som **administratör** och Skriv följande kommandon, för att skapa ett nytt självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="81317-143">On your Windows computer, open a new PowerShell window as **Administrator** and type the following commands, to create a new self-signed certificate.</span></span>

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

<span data-ttu-id="81317-144">I föregående exempel, ersätter '*. contoso100.com ”med DNS-domännamnet för din hanterade domän. Till exempel om du har skapat en hanterad domän som kallas ”contoso100.onmicrosoft.com” kan ersätta '*. contoso100.com ”i skriptet ovan med ' *. contoso100.onmicrosoft.com').</span><span class="sxs-lookup"><span data-stu-id="81317-144">In the preceding sample, replace '*.contoso100.com' with the DNS domain name of your managed domain. For example, if you created a managed domain called 'contoso100.onmicrosoft.com', replace '*.contoso100.com' in the above script with '*.contoso100.onmicrosoft.com').</span></span>

![Välja Azure AD-katalog](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

<span data-ttu-id="81317-146">Det nyskapade självsignerade certifikatet placeras i den lokala datorns certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="81317-146">The newly created self-signed certificate is placed in the local machine's certificate store.</span></span>


## <a name="next-step"></a><span data-ttu-id="81317-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81317-147">Next step</span></span>
[<span data-ttu-id="81317-148">Uppgift 2 – exportera säker LDAP-certifikatet till en. PFX-fil</span><span class="sxs-lookup"><span data-stu-id="81317-148">Task 2 - export the secure LDAP certificate to a .PFX file</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
