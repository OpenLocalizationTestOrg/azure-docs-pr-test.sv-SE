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
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a>Aktivera Azure Active Directory Domain Services med hello klassiska Azure-portalen

## <a name="task-3-enable-azure-active-directory-domain-services"></a>Uppgift 3: aktivera Azure Active Directory Domain Services
I det här steget aktivera Azure Active Directory Domain Services (Azure AD DS) för din katalog genom att göra hello följande steg:

1. Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Välj hello hello vänster **Active Directory** knappen.
3. Välj hello Azure Active Directory (Azure AD)-klienten (katalogen) som du vill tooenable Azure AD DS.

    ![Välja Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. På hello **preview directory** klickar du på hello **konfigurera** fliken.

    ![Fliken Konfigurera för katalogen](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Under **domäntjänster**, ändra hello **aktivera domain services för den här katalogen** alternativet för**Ja**.  
    Ytterligare konfigurationsalternativ för Azure Active Directory Domain Services visas på hello sida.

    ![Aktivera Domain Services](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > När du aktiverar Azure Active Directory Domain Services för din klient genererar Azure AD och lagrar hello Kerberos- och NTLM-hashvärdena som krävs för att autentisera användare.
   >
   >
6. Ange hello **DNS-domännamnet för domäntjänster**.

   * hello standarddomännamnet för hello katalogen (med en **. onmicrosoft.com** suffix) väljs som standard.

   * hello lista innehåller alla domäner som har konfigurerats för Azure AD-katalogen, inklusive både verifierade och Ej verifierad domäner som du konfigurerar på hello **domäner** fliken.

   * Du kan också ange ett anpassat domännamn. I det här exemplet hello domännamn är *contoso100.com*.

     > [!WARNING]
     > hello-prefixet för ett angivet domännamn (till exempel *contoso100* i hello *contoso100.com* domännamn) får innehålla högst 15 tecken. Du kan inte skapa en Azure Active Directory Domain Services-domän med ett prefix som innehåller fler än 15 tecken.
     >
     >
7. Se till att hello DNS-domännamn som du har valt för hello hanterade domänen inte redan finns i hello virtuella nätverk. I synnerhet Kontrollera toosee om:

   * Du redan har en domän med hello samma DNS-domännamn på hello virtuellt nätverk.

   * hello virtuella nätverk som du har valt har en VPN-anslutning med ditt lokala nätverk och du har en domän med hello samma DNS-namnet på ditt lokala nätverk.

   * Du har en befintlig molntjänst med det namnet på hello virtuellt nätverk.
8. Välj ett virtuellt nätverk som du vill använda Azure Active Directory Domain Services toobe tillgängliga. Välj hello virtuella nätverk och dedikerad undernät som du skapade i hello **Anslut domäntjänster toothis virtuellt nätverk** listrutan. Tänk också hello följande:

   * Se till att hello virtuella nätverk som du har angett hör tooan Azure-region som stöds av Azure Active Directory Domain Services. tooascertain hello Azure-regioner där Azure Active Directory Domain Services är tillgängligt, se [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/).

   * Virtuella nätverk som tillhör tooa region där Azure Active Directory Domain Services inte stöds visas inte i hello nedrullningsbara listan.

   * Använd en dedikerad undernät inom hello virtuellt nätverk för Azure Active Directory Domain Services. Gör *inte* Välj hello gateway-undernätet. Se [Nätverksöverväganden](active-directory-ds-networking.md).

   * På motsvarande sätt visas inte virtuella nätverk som har skapats med hjälp av Azure Resource Manager i hello nedrullningsbara listan. Resource Manager-baserade virtuella nätverk stöds för närvarande inte av Azure Active Directory Domain Services.
9. tooenable Azure Active Directory Domain Services, hello i åtgärdsfönstret längst ned hello hello-sidan, klickar du på **spara**.
    * Medan Azure Active Directory Domain Services aktiveras för din katalog, hello sidan visar statusen *väntande*.

        ![Fönstret Enable Domain Services (Aktivera domäntjänster)](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > Azure Active Directory Domain Services ger hög tillgänglighet för din hanterade domän. När du har aktiverat Azure Active Directory Domain Services är hello IP-adresser på vilken domän tjänster som är tillgängliga på hello virtuella nätverk visas en i taget. hello andra IP-adressen visas strax efter hello först som snart hello tjänsten aktiverar hög tillgänglighet för din domän. När hög tillgänglighet har konfigurerats och aktiverats för din domän, bör du se två IP-adresser i hello **domäntjänster** avsnitt i hello **konfigurera** fliken.
        >
        >
    * Efter cirka 20 too30 minuter hello första IP-adressen på vilken domän tjänster som är tillgängliga på det virtuella nätverket i hello **IP-adress** på hello **konfigurera** sidan.

        ![Fönstret Domain Services (Domäntjänster) med den första etablerade IP-adressen](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * När hög tillgänglighet är tillgängligt för din domän visas två IP-adresser på hello-sidan. Den hanterade domänen är tillgänglig i ditt valda virtuella nätverk på dessa två IP-adresser.

10. Observera hello två IP-adresser så att du kan uppdatera hello DNS-inställningarna för det virtuella nätverket. På så sätt kan virtuella datorer på hello virtuellt nätverk tooconnect toohello domän för åtgärder som till exempel domänanslutning.

    ![Fönstret Domain Services (Domäntjänster) med de två etablerade IP-adresserna](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Synkronisering tooyour hanterad domän tar ett tag beroende på hello storlek för din Azure AD-klient (till exempel hello antal användare eller grupper). Den här synkroniseringsprocessen sker i bakgrunden hello. För stora klienter med tiotusentals objekt kan det ta en dag eller två för alla användare, gruppmedlemskap och autentiseringsuppgifter toobe synkroniseras.
>
>

## <a name="next-step"></a>Nästa steg
[Uppgift 4: uppdatera hello DNS-inställningarna för hello Azure-nätverk](active-directory-ds-getting-started-update-dns.md)
