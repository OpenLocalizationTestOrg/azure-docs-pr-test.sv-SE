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
ms.openlocfilehash: 8781285cd02d690788056b985b017efd7e4d6b3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän

## <a name="before-you-begin"></a>Innan du börjar
Se till att du har slutfört [uppgift 2 - export hello säker LDAP certifikat tooa. PFX-filen](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).

Välj om toouse hello Förhandsgranska Azure-portaler eller hello Azure klassiska portal toocomplete den här uppgiften.
> [!div class="op_single_selector"]
> * **Azure-portalen (förhandsgranskning)**: [aktivera säkert LDAP med hello Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
> * **Klassiska Azure-portalen**: [aktivera säkert LDAP med hello klassiska Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)
>
>


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-azure-portal-preview"></a>Uppgift 3 – aktivera säker LDAP för hello hanterade domänen med hello Azure-portalen (förhandsgranskning)
tooenable säkert LDAP, utför följande konfigurationssteg hello:

1. Navigera toohello  **[Azure-portalen](https://portal.azure.com)**.

2. Sök efter domain services i hello **söka resurser** sökrutan. Välj **Azure AD Domain Services** från hello sökresultatet. Hej **Azure AD Domain Services** bladet visar din hanterade domän.

    ![Hitta hanterad domän som har etablerats](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Klicka på hello hanterade domän (till exempel ”contoso100.com”) toosee hello namn mer information om hello domän.

    ![DS - Etableringsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. Klicka på **säker LDAP** hello navigeringsfönstret.

    ![DS - säker LDAP-bladet](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. Säkert LDAP åtkomst tooyour hanterad domän är inaktiverad som standard. Växla **säker LDAP** för**aktivera**.

    ![Aktivera säker LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. Som standard hanteras säker LDAP åtkomst tooyour domänen via hello internet är inaktiverad. Växla **Tillåt säker LDAP åtkomst via hello internet** för**aktivera**om det behövs. 

6. Klicka på hello mappen ikonen följande **. PFX-filen med säker LDAP-certifikat**. Ange hello sökvägen toohello PFX-filen med hello certifikat för säker LDAP åtkomst toohello hanterade domän.

7. Ange hello **lösenord toodecrypt. PFX-filen**. Ange hello samma lösenord som du använde när du exporterar hello certifikatets toohello PFX-fil.

8. När du är klar klickar du på hello **spara** knappen.

9. Du ser ett meddelande som informerar säker LDAP konfigureras för hello-hanterad domän. Du kan inte ändra andra inställningar för hello domän tills åtgärden är slutförd.

    ![Konfigurera säker LDAP för hello-hanterad domän](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> Det tar cirka 10 too15 minuter tooenable säkert LDAP för din hanterade domän. Om hello säker LDAP-certifikat inte matchar hello krävs kriterier, säker LDAP har inte aktiverats för din katalog och du ser ett fel. Till exempel hello domännamn är felaktig, hello certifikatet redan har upphört att gälla eller upphör snart att gälla. I det här fallet, försök igen med ett giltigt certifikat.
>
>

<br>

## <a name="task-4---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a>Uppgift 4 – konfigurera DNS-tooaccess hello hanterade domänen från hello internet
> [!NOTE]
> **Frivillig uppgift** - om du inte planerar tooaccess hello hanterade domänen med LDAPS över Hej internet, hoppa över det här konfiguration.
>
>

Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

När du har aktiverat säker LDAP-åtkomst via hello internet för din hanterade domän måste tooupdate DNS så att klientdatorerna kan hitta den här hanterade domänen. Hello slutet av uppgift 3, visas en extern IP-adress på hello **egenskaper** bladet i **extern IP-adress för LDAPS åtkomst**.

Konfigurera externa DNS-providern så att hello DNS-namnet på hello hanterade domän (till exempel ldaps.contoso100.com) pekar toothis extern IP-adress. I vårt exempel behöver vi toocreate hello följande DNS-post:

    ldaps.contoso100.com  -> 52.165.38.113

Det – nu är du redo tooconnect toohello hanterade domänen med hjälp av säker LDAP över hello internet.

> [!WARNING]
> Kom ihåg att klientdatorer måste lita hello utfärdaren av hello LDAPS certifikat toobe kan tooconnect har toohello hanterade domänen med LDAPS. Om du använder en betrodd offentlig certifikatutfärdare, behöver du inte toodo allt eftersom dessa certifikatutfärdare litar på klientdatorer. Om du använder ett självsignerat certifikat, installera hello offentliga del av hello självsignerat certifikat till hello betrodda certifikatarkiv på hello-klientdator.
>
>


## <a name="task-5---lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a>Uppgift 5 - låsning LDAPS komma åt tooyour hanterade domänen via hello internet
> [!NOTE]
> **Frivillig uppgift** - om du inte har aktiverat LDAPS åtkomst toohello hanterad domän över Hej internet, hoppa över det här konfiguration.
>
>

Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).

Visa din hanterade domän för LDAPS åtkomst över hello representerar internet en säkerhetsrisk. hello hanterad domän kan nås från hello internet på hello-port som används för säker LDAP (det vill säga port 636). Därför kan du välja toorestrict åtkomst toohello hanterade domänen toospecific kända IP-adresser. För förbättrad säkerhet skapar en nätverkssäkerhetsgrupp (NSG) och associera den med hello undernät där du har aktiverat Azure AD Domain Services.

hello i tabellen nedan visas ett exempel på en NSG som du kan konfigurera toolock ned säker LDAP åtkomst via hello internet. hello NSG innehåller en uppsättning regler som tillåter inkommande LDAPS åtkomst via TCP-port 636 endast från en angiven mängd av IP-adresser. hello 'DenyAll' Standardregeln gäller tooall andra inkommande trafik från hello internet. hello NSG regeln tooallow LDAPS åtkomst via hello internet från den angivna IP-adresser har högre prioritet än hello DenyAll NSG regeln.

![Exempel NSG toosecure LDAPS åtkomst via hello internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Mer information** - [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).

<br>

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
* [Administrera Grupprincip i en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-group-policy.md)
* [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md)
* [Skapa en säkerhetsgrupp för nätverk](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
