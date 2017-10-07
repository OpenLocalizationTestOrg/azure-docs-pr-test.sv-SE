---
title: "aaaAzure Active Directory Connect Health-åtgärder"
description: "Den här artikeln beskriver ytterligare åtgärder som kan utföras när du har distribuerat Azure AD Connect Health."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 1dddcee0bca3150ce08621c045a92a1b3ad9df30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-connect-health-operations"></a>Åtgärder i Azure Active Directory Connect Health
Det här avsnittet beskrivs hello olika åtgärder du kan utföra med hjälp av Azure Active Directory (AD Azure) Connect Health.

## <a name="enable-email-notifications"></a>Aktivera e-postaviseringar
Du kan konfigurera hello Azure AD Connect Health service toosend e-postmeddelanden när aviseringar anger identitetsinfrastrukturen inte är felfri. Detta inträffar när en avisering skapas och när den är löst.

![Skärmbild av Azure AD Connect Health e-postavisering inställningar](./media/active-directory-aadconnect-health/email_noti_discover.png)

> [!NOTE]
> E-postmeddelanden är aktiverade som standard.
>
>

### <a name="tooenable-azure-ad-connect-health-email-notifications"></a>tooenable Azure AD Connect Health e-postaviseringar
1. Öppna hello **aviseringar** bladet för hello-tjänsten som du vill tooreceive e-postmeddelande.
2. Hello Åtgärdsfältet klickar du på **Meddelandeinställningar**.
3. Hello e-postavisering växeln väljer **på**.
4. Markera kryssrutan för hello om du vill att alla globala administratörer tooreceive e-postmeddelanden.
5. Om du vill tooreceive e-postmeddelanden på e-postadresser, anger du dem i hello **ytterligare e-postmottagare** rutan. tooremove en e-postadress från den här listan, högerklickar du på hello-post och välj **ta bort**.
6. toofinalize hello ändringar klickar du på **spara**. Ändringarna börjar gälla förrän du sparar.

## <a name="delete-a-server-or-service-instance"></a>Ta bort en server eller service-instans

I vissa fall kanske du vill tooremove en server från som övervakas. Här är vad du behöver tooknow tooremove en server från hello Azure AD Connect Health-tjänsten.

När du tar bort en server vara medveten om följande hello:

* Den här åtgärden slutar att samla in ytterligare data från servern. Den här servern tas bort från hello övervakningstjänsten. När den här åtgärden är du inte kan tooview nya aviseringar, övervakning eller analytics användningsdata för den här servern.
* Hello Health-agenten avinstalleras inte från servern om den här åtgärden. Om du inte har avinstallerat hello Health-agenten innan du utför det här steget kan du se fel relaterade toohello Health-agenten på hello-servern.
* Den här åtgärden tar inte bort hello data som redan samlats in från den här servern. Data tas bort i enlighet med hello Azure policy för datalagring.
* När du utför den här åtgärden om du vill övervaka toostart hello samma server igen, måste du avinstallera och installera om hello Health-agenten på den här servern.

### <a name="toodelete-a-server-from-hello-azure-ad-connect-health-service"></a>toodelete en server från hello Azure AD Connect Health-tjänsten
Azure AD Connect Health för Active Directory Federation Services (AD FS) och Azure AD Connect (synkronisering):

1. Öppna hello **Server** bladet från hello **serverlista** bladet genom att välja hello server name toobe tas bort.
2. På hello **Server** bladet från hello Åtgärdsfältet klickar du på **ta bort**.
3. Bekräfta genom att skriva hello servernamnet i hello bekräftelse.
4. Klicka på **Ta bort**.

Azure AD Connect Health för Azure Active Directory Domain Services:

1. Öppna hello **domänkontrollanter** instrumentpanelen.
2. Välj hello domain controller toobe tas bort.
3. Hello Åtgärdsfältet klickar du på **ta bort markerade**.
4. Bekräfta hello åtgärd toodelete hello server.
5. Klicka på **Ta bort**.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Ta bort en instans av tjänsten från Azure AD Connect Health service
I vissa fall kanske du vill tooremove en instans av tjänsten. Här är vad du behöver tooknow tooremove en tjänst som instansen från hello Azure AD Connect Health-tjänsten.

När du tar bort en instans av tjänsten vara medveten om följande hello:

* Den här åtgärden tar bort hello aktuella tjänstinstansen från övervakningstjänsten hello.
* Den här åtgärden inte avinstallera eller ta bort hello Health-agenten från någon av hello-servrar som har övervakas som en del av den här tjänstinstansen. Om du inte har avinstallerat hello Health-agenten innan du utför det här steget kan du se fel relaterade toohello Health-agenten på hello-servrar.
* Alla data från den här tjänstinstansen tas bort i enlighet med hello Azure policy för datalagring.
* När du utför den här åtgärden om du vill toostart hello övervakningstjänsten, avinstallera och installera hello Health-agenten på alla hello-servrar. När du utför den här åtgärden om du vill övervaka hello samma server igen, avinstallera, installera och registrera toostart hello Health-agenten på servern.

#### <a name="toodelete-a-service-instance-from-hello-azure-ad-connect-health-service"></a>toodelete en tjänstinstans från hello Azure AD Connect Health-tjänsten
1. Öppna hello **Service** bladet från hello **Tjänstlista** bladet genom att välja hello tjänstidentifierare (servergruppens namn) som du vill tooremove.
2. På hello **Server** bladet från hello Åtgärdsfältet klickar du på **ta bort**.
3. Bekräfta genom att skriva hello tjänstnamnet i hello bekräftelserutan (till exempel: sts.contoso.com).
4. Klicka på **Ta bort**.
   <br><br>

[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Hantera åtkomst med rollbaserad åtkomstkontroll
[Rollbaserad åtkomstkontroll (RBAC)](../role-based-access-control-configure.md) för Azure AD Connect Health tillhandahåller åtkomst toousers och grupper än globala administratörer. RBAC tilldelar roller toohello avsedd användare och grupper och tillhandahåller en mekanism toolimit hello globala administratörer i din katalog.

### <a name="roles"></a>Roller
Azure AD Connect Health stöder hello följande inbyggda roller:

| Roll | Behörigheter |
| --- | --- |
| Ägare |Ägare kan *hantera åtkomst* (till exempel tilldela en roll tooa användare eller grupp), *se all information* (till exempel visa aviseringar) från hello-portalen och *ändra inställningar* () till exempel e-postmeddelanden) i Azure AD Connect Health. <br>Globala administratörer för Azure AD tilldelas rollen som standard, och detta kan inte ändras. |
| Deltagare |Deltagare kan *se all information* (till exempel visa aviseringar) från hello-portalen och *ändra inställningar* (till exempel e-postmeddelanden) i Azure AD Connect Health. |
| Läsare |Läsare kan *se all information* (till exempel visa aviseringar) från hello portalen i Azure AD Connect Health. |

Andra roller (till exempel Administratörer för användaren eller DevTest Labs användare) har ingen effekt tooaccess i Azure AD Connect Health, även om hello roller är tillgängliga i hello-portaler.

### <a name="access-scope"></a>Åtkomstscope
Azure AD Connect Health har stöd för hantering av åtkomst på två nivåer:

* **Alla instanser av tjänsten**: Detta är hello rekommenderas sökväg i de flesta fall. Den styr åtkomst för alla tjänstinstanser (till exempel en AD FS-servergrupp) över alla rolltyper av som övervakas av Azure AD Connect Health.
* **Tjänstinstansen**: I vissa fall kan du behöva toosegregate åtkomst baserat på rolltyper eller av en instans av tjänsten. I det här fallet kan du hantera åtkomst på hello servicenivå för instansen.  

Tillstånd beviljas om en användare har åtkomst antingen vid hello katalog eller service-instans nivå.

### <a name="allow-users-or-groups-access-tooazure-ad-connect-health"></a>Tillåt användare eller grupper åtkomst tooAzure AD Connect Health
hello följande steg visar hur tooallow åt.
#### <a name="step-1-select-hello-appropriate-access-scope"></a>Steg 1: Välj hello åtkomstbehörighet omfång
tooallow en användaråtkomst till hello *alla tjänstinstanser* nivå i Azure AD Connect Health, öppna hello huvudblad i Azure AD Connect Health.<br>

#### <a name="step-2-add-users-and-groups-and-assign-roles"></a>Steg 2: Lägg till användare och grupper och tilldela roller
1. Från hello **konfigurera** klickar du på **användare**.<br>
   ![Skärmbild av Azure AD Connect Health RBAC huvudblad, med användare som är markerat](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Välj **Lägg till**.
3. I hello **Välj en roll** rutan, Välj en roll (till exempel **ägare**).<br>
   ![Skärmbild av Azure AD Connect Health RBAC användare fönster](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Ange hello namnet eller identifieraren för hello riktade användare eller grupp. Du kan välja en eller flera användare eller grupper på hello samtidigt. Klicka på **Välj**.
   ![Skärmbild av Azure AD Connect Health RBAC användare fönster](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Välj **OK**.<br>
6. När hello rolltilldelning är klar visas hello användare och grupper i hello-listan.<br>
   ![Skärmbild av Azure AD Connect Health RBAC användare fönster, med nya användare markerat](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Nu hello listas användare och grupper har åtkomst till, enligt tootheir som tilldelats roller.

> [!NOTE]
> * Globala administratörer har fullständig åtkomst tooall hello operations alltid, men globala administratörskonton finns inte i hello som föregår listan.
> * hello bjuda in användare funktionen stöds inte i Azure AD Connect Health.
>
>

#### <a name="step-3-share-hello-blade-location-with-users-or-groups"></a>Steg 3: Dela hello bladet plats med användare eller grupper
1. När du tilldelar behörigheter för en användare har åtkomst till Azure AD Connect Health genom att gå [här](http://aka.ms/aadconnecthealth).
2. På bladet hello Fäst hello användaren hello bladet eller olika delar av det, toohello instrumentpanelen. Klicka bara på hello **PIN-kod toodashboard** ikon.<br>
   ![Skärmbild av Azure AD Connect Health RBAC Fäst bladet, med fästikonen markerat](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)

> [!NOTE]
> En användare med hello Reader roll som inte kan tooget Azure AD Connect Health-tillägget från hello Azure Marketplace. hello-användare kan inte utföra hello nödvändiga ”Skapa” åtgärden toodo så. hello-användare kan fortfarande få toohello bladet genom gå toohello föregående länk. Hello användaren kan fästa hello bladet toohello instrumentpanelen för senare användning.
>
>

### <a name="remove-users-or-groups"></a>Ta bort användare eller grupper
Du kan ta bort en användare eller grupp läggs tooAzure AD Connect Health RBAC. Bara högerklicka hello användaren eller gruppen och välj **ta bort**.<br>
![Skärmbild av Azure AD Connect Health RBAC användare fönster med ta bort markerat](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="next-steps"></a>Nästa steg
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installation av Azure AD Connect Health Agent](active-directory-aadconnect-health-agent-install.md)
* [Använda Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md)
* [Använda Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md)
* [Använda Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md)
* [Vanliga frågor och svar om Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versionshistorik för Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
