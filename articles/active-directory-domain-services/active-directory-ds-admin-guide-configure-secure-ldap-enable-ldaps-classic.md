---
title: "aaaConfigure säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs"
description: "Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9d563c45-9578-410d-96c8-355af64feae8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: a0d6e2faf474b1f0cbe157fb4ae2754b1d521ef9
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


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-classic-azure-portal"></a>Uppgift 3 – aktivera säker LDAP för hello hanterade domänen med hello klassiska Azure-portalen
tooenable säkert LDAP, utför följande konfigurationssteg hello:

1. Navigera toohello  **[klassiska Azure-portalen](https://manage.windowsazure.com)**.
2. Välj hello **Active Directory** i hello vänstra fönstret.
3. Välj hello Azure AD-katalog (även hänvisade tooas ”klient”), som du har aktiverat Azure AD Domain Services.

    ![Välja Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klicka på hello **konfigurera** fliken.

    ![Fliken Konfigurera för katalogen](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Bläddra nedåt toohello avsnittet **domäntjänster**. Du bör se en alternativet **säker LDAP (LDAPS)** som visas i följande skärmbild hello:

    ![Konfigurationsavsnittet Domäntjänster](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. Klicka på hello **konfigurera certifikat...**  knappen toobring in hello **konfigurera certifikat för säker LDAP** dialogrutan.

    ![Konfigurera certifikat för säker LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. Klicka på hello mappen ikonen följande **PFX-filen med certifikat** toospecify hello PFX-fil som innehåller hello-certifikat som du vill toouse för säker LDAP åtkomst toohello hanterade domän. Ange också hello lösenordet du angav när du exporterar hello certifikatets toohello PFX-fil. Klicka på hello klar hello ned-knappen.

    ![Ange säker LDAP PFX-filen och lösenord](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. Hej **domäntjänster** avsnitt i hello **konfigurera** ska hämta nedtonad och är i hello **väntande...**  tillstånd om en stund. Under denna tid hello LDAPS certifikat har verifierats för Precision och säker LDAP har konfigurerats för din hanterade domän.

    ![Säkert LDAP - väntetillstånd](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > Det tar cirka 10 too15 minuter tooenable säkert LDAP för din hanterade domän. Om hello säker LDAP-certifikat inte matchar hello krävs kriterier, säker LDAP har inte aktiverats för din katalog och du ser ett fel. Till exempel hello domännamn är felaktig, hello certifikatet redan har upphört att gälla eller upphör snart att gälla.
   >
   >

9. När säker LDAP har aktiverats för din hanterade domän, hello **väntande...**  meddelandet ska försvinner. Du bör se hello tumavtrycket för hello certifikat visas.

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-hello-internet"></a>Uppgift 4 – aktivera säker LDAP åtkomst över hello internet
**Frivillig uppgift** - om du inte planerar tooaccess hello hanterade domänen med LDAPS över Hej internet, hoppa över det här konfiguration.

Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).

1. Du bör se ett alternativ för**hello aktivera säker LDAP åtkomst via INTERNET** i hello **domäntjänster** avsnitt i hello **konfigurera** sidan. Det här alternativet anges för**nr** som standard eftersom internet access toohello hanterade domänen via säker LDAP är inaktiverad som standard.

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. Växla **hello aktivera säker LDAP åtkomst via INTERNET** för**Ja**. Klicka på hello **spara** hello nedre panelen-knappen.
    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)
3. Hej **domäntjänster** avsnitt i hello **konfigurera** ska hämta nedtonad och är i hello **väntande...**  tillstånd om en stund. Efter en stund är åtkomst tooyour hanterade Internetdomän över säker LDAP aktiverat.

    ![Säkert LDAP - väntetillstånd](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > Det tar cirka 10 minuter tooenable Internetåtkomst via säker LDAP för din hanterade domän.
   >
   >
4. När säker LDAP åtkomst tooyour hanterade domänen via hello internet har aktiverats, hello **väntande...**  meddelandet ska försvinner. Du bör se hello externa IP-adress som kan vara används tooaccess din katalog över LDAPS hello fältet **extern IP-adress för LDAPS åtkomst**.

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a>Uppgift 5 – konfigurera DNS-tooaccess hello hanterade domänen från hello internet
**Frivillig uppgift** - om du inte planerar tooaccess hello hanterade domänen med LDAPS över Hej internet, hoppa över det här konfiguration.

Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 4](#task-4---enable-secure-ldap-access-over-the-internet).

När du har aktiverat säker LDAP-åtkomst via hello internet för din hanterade domän måste tooupdate DNS så att klientdatorerna kan hitta den här hanterade domänen. Hello slutet av uppgift 4, visas en extern IP-adress på hello **konfigurera** fliken i **extern IP-adress för LDAPS åtkomst**.

Konfigurera externa DNS-providern så att hello DNS-namnet på hello hanterade domän (till exempel ldaps.contoso100.com) pekar toothis extern IP-adress. I vårt exempel behöver vi toocreate hello följande DNS-post:

    ldaps.contoso100.com  -> 52.165.38.113

Det – nu är du redo tooconnect toohello hanterade domänen med hjälp av säker LDAP över hello internet.

> [!WARNING]
> Kom ihåg att klientdatorer måste lita hello utfärdaren av hello LDAPS certifikat toobe kan tooconnect har toohello hanterade domänen med LDAPS. Om du använder en företagscertifikatutfärdare eller en betrodd offentlig certifikatutfärdare, behöver du inte toodo allt eftersom dessa certifikatutfärdare litar på klientdatorer. Om du använder ett självsignerat certifikat måste tooinstall hello offentliga tillhör hello självsignerat certifikat till hello betrodda certifikatarkiv på hello-klientdator.
>
>


## <a name="lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a>Låsning LDAPS komma åt tooyour hanterade domänen via hello internet
> [!NOTE]
> **Frivillig uppgift** - om du inte har aktiverat LDAPS åtkomst toohello hanterad domän över Hej internet, hoppa över det här konfiguration.
>
>

Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 4](#task-4---enable-secure-ldap-access-over-the-internet).

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
