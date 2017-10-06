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
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän
Den här artikeln visar hur du kan aktivera säker Lightweight Directory Access Protocol (LDAPS) för din Azure AD Domain Services-hanterad domän. Säker LDAP kallas även ”Lightweight Directory Access Protocol (LDAP) över Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.

## <a name="before-you-begin"></a>Innan du börjar
tooperform hello uppgifterna som listas i den här artikeln, behöver du:

1. En giltig **Azure-prenumeration**.
2. En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.
3. **Azure AD Domain Services** måste vara aktiverat för hello Azure AD-katalogen. Om du inte gjort det, följ alla hello uppgifter som beskrivs i hello [Kom igång-guiden](active-directory-ds-getting-started.md).
4. En **certifikat toobe används tooenable säkert LDAP**.

   * **Rekommenderade** -erhåller ett certifikat från en betrodd offentlig certifikatutfärdare. Det här konfigurationsalternativet är säkrare.
   * Alternativt kan du kan också välja för[skapa ett självsignerat certifikat](#task-1---obtain-a-certificate-for-secure-ldap) enligt nedan.

<br>

### <a name="requirements-for-hello-secure-ldap-certificate"></a>Krav för hello säker LDAP-certifikat
Hämta ett giltigt certifikat per hello följande riktlinjer innan du aktiverar säker LDAP. Det uppstår fel om du försöker tooenable säkert LDAP för din hanterade domän med ett ogiltigt felaktigt certifikat.

1. **Betrodda utfärdare** -hello certifikatet måste utfärdas av en certifikatutfärdare som betrodd av datorer som ansluter toohello hanterade domänen med säker LDAP. Den här utfärdare kan vara en offentlig certifikatutfärdare som betrodd av dessa datorer.
2. **Livslängd** -hello certifikatet måste vara giltiga för minst hello nästa 3-6 månader. Säkert LDAP åtkomst tooyour hanterad domän avbryts när hello certifikatet upphör att gälla.
3. **Ämnesnamn** -hello Subjektnamnet på certifikatet hello måste vara ett jokertecken för din hanterade domän. Till exempel om din domän är med namnet ”contoso100.com” hello certifikatets ämnesnamn måste vara ' *. contoso100.com ”. Ange hello DNS-namn (Alternativt ämnesnamn) toothis jokertecken namn.
4. **Nyckelanvändning** - hello certifikat konfigureras för hello följande använder - digitala signaturer och nyckelchiffrering.
5. **Certifikat syfte** -hello certifikatet måste vara giltigt för serverautentisering för SSL.

> [!NOTE]
> **Företagscertifikatutfärdare:** Azure AD Domain Services stöder inte användning av säker LDAP-certifikat som utfärdats av din organisations utfärdare av företagscertifikat. Den här begränsningen beror hello-tjänsten inte har förtroende för ditt företags-CA som en rotcertifikatutfärdare. 
>
>

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Uppgift 1 – skaffa ett certifikat för säker LDAP
hello första uppgiften innebär att erhålla ett certifikat som används för säker LDAP åtkomst toohello hanterade domän. Du kan välja mellan två alternativ:

* Skaffa ett certifikat från en certifikatutfärdare. hello-utfärdare kan vara en offentlig certifikatutfärdare.
* Skapa ett självsignerat certifikat.

### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Alternativet (rekommenderas) – skaffa ett säkert LDAP-certifikat från en certifikatutfärdare
Om din organisation hämtar certifikat från en offentlig certifikatutfärdare, måste tooobtain hello säker LDAP-certifikat från den offentlig certifikatutfärdaren.

När du begär ett certifikat, se till att du uppfyller alla hello kraven som beskrivs i [kraven för hello säker LDAP certifikatet](#requirements-for-the-secure-ldap-certificate).

> [!NOTE]
> Klientdatorer som behöver tooconnect toohello hanterade domänen med säker LDAP måste lita hello hello säker LDAP-certifikatets utfärdare.
>
>

### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Alternativ B – skapa ett självsignerat certifikat för säker LDAP
Om du inte räknar toouse ett certifikat från en offentlig certifikatutfärdare, kan du välja toocreate ett självsignerat certifikat för säker LDAP.

**Skapa ett självsignerat certifikat med hjälp av PowerShell**

På Windows-dator öppnar du ett nytt PowerShell-fönster som **administratör** och typen hello följande kommandon, toocreate ett nytt självsignerat certifikat.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

I föregående exempel hello, ersätter '*. contoso100.com ”med hello DNS-domännamnet för din hanterade domän. Till exempel om du har skapat en hanterad domän som kallas ”contoso100.onmicrosoft.com” kan ersätta '*. contoso100.com ”i hello ovan skriptet med ' *. contoso100.onmicrosoft.com').

![Välja Azure AD-katalog](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

hello nyskapade självsignerade certifikatet placeras i hello lokala datorns certifikatarkiv.


## <a name="next-step"></a>Nästa steg
[Uppgift 2 – exportera hello säker LDAP certifikat tooa. PFX-fil](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md)
