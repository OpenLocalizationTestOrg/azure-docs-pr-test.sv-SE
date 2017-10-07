---
title: "aaaLicense Azure Active Directory-användare i hello klassiska Azure-portalen | Microsoft Docs"
description: "Beskrivning av Microsoft Azure Active Directory-licensiering, hur det fungerar, hur tooget startas och bästa praxis, inklusive Office 365, Microsoft Intune och Azure Active Directory Premium och Basic"
services: active-directory
keywords: Azure AD-licensiering
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d769e8c6-7581-43f5-a3b4-de4b1dca2344
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;H1Hack27Feb2017
ms.openlocfilehash: 4d5f244cbee2ae37a30976f70b5d4f21c3516d90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-microsoft-azure-active-directory-licensing-in-hello-azure-classic-portal"></a>Vad är Microsoft Azure Active Directory-licensiering i hello klassiska Azure-portalen?

> [!div class="op_single_selector"]
> * [Hämta Azure portal instruktioner](active-directory-licensing-get-started-azure-portal.md)
> * [Azure klassiska portal info](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (AD Azure) är Microsofts identitet som en tjänst (IDaaS) och plattform. Azure AD finns i ett antal funktions- och versioner från Azure AD-lediga som är tillgängliga med alla Microsoft-tjänster, till exempel Office 365, Dynamics, Microsoft Intune och Azure (Azure AD inte genererar eventuella kostnader för användning i detta läge), tooAzure AD betald versioner, till exempel Enterprise Mobility Suite (EMS), Azure AD Premium och Basic samt Azure Multi-Factor Authentication (MFA). Precis som många Microsoft online services som mest Azure AD betald versioner levereras via per användare rättigheter som i Office 365, Microsoft Intune och Azure AD. I dessa fall hello service köp representeras med en eller flera prenumerationer och varje prenumeration som omfattar ett antal licenser före köp i din klient. Per användare rättigheter uppnås genom licenstilldelning, skapa en länk mellan hello användar- och hello produkt, aktivera hello tjänstkomponenter för hello användare och förbrukar en hello förskottsbetald licenser.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. Hur tooassign administratörsroller i hello Azure AD admin center finns [licens dig och dina användare i Azure AD](active-directory-licensing-get-started-azure-portal.md).

[Försök Azure AD premium nu.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [!NOTE]
> Azure AD-administrationsportalen är en del av hello klassiska Azure-portalen. När du använder Azure AD inte kräver några Azure inköp, åtkomst till den här portalen kräver en aktiv Azure-prenumeration eller en [Azure-provprenumeration](https://azure.microsoft.com/pricing/free-trial/).
>
>

En översikt av funktionerna i Azure AD-tjänsten finns [vad är Azure AD](active-directory-whatis.md).
[Lär dig mer om Azure AD-servicenivåer](https://azure.microsoft.com/support/legal/sla/)

> [!NOTE]
> Azure betala per användning prenumerationer skiljer sig: visas också i katalogen kan dessa prenumerationer skapa ett Azure-resurser och mappa dem tooyour betalningsmetod. I det här fallet finns inga licensantal som är associerade med hello prenumeration. Användarorganisation med hello prenumeration hello användarnas åtkomst toomanaging prenumerationsresurser uppnås genom att ge dem behörighet toooperate på Azure-resurser mappas toohello prenumeration.
>
>

## <a name="how-does-azure-ad-licensing-work"></a>Hur fungerar licensiering i Azure AD?
Licens-baserade (rätt-baserat) Azure AD-tjänster fungerar genom att aktivera en prenumeration i Azure AD directory/service-klienten. När hello prenumeration är aktiv hello funktioner hanteras av administratörer för directory-tjänsten och används av licensierade användare.

När du köper eller aktiverar Enterprise Mobility Suite, Azure AD Premium eller Azure AD Basic, katalogen uppdateras med hello-prenumeration, inklusive giltighetsperioden och förskottsbetald licenser. Din prenumerationsinformation inklusive status, nästa livscykel händelse och hello antalet tilldelade eller tillgängliga licenser är tillgängliga via hello klassiska Azure-portalen hello licenser fliken för hello specifik katalog. Detta är också toobest plats toomanage licenstilldelningar.

Varje prenumeration består av en eller flera serviceplaner, varje mappning hello ingår funktionsnivån för hello tjänsttypen; till exempel Azure AD Azure MFA, Microsoft Intune, Exchange Online eller SharePoint Online. Azure AD-licenshantering kräver inte tjänstnivåhantering för planen. Detta skiljer sig från Office 365 som förlitar sig på den här avancerad konfiguration läge toomanage tooincluded åtkomsttjänster. Azure AD beroende i tjänstekonfigurationen tooenable funktioner och hantera enskilda behörigheter.

Information om Azure AD-prenumerationer hanteras i allmänhet via hello klassiska Azure-portalen hello licenser fliken för hello specifik katalog. Azure AD-prenumerationer, med undantag för hello av Azure AD Premium, visas inte i hello Office-portalen.

> [!IMPORTANT]
> Azure AD Premium och Basic, samt Enterprise Mobility Suite-prenumerationer är begränsat tootheir etablerats directory /-klient. Prenumerationer kan inte delas mellan kataloger eller använda tooentitle användare i andra kataloger. Flytta en prenumeration mellan kataloger är möjligt, men kräver att skicka ett supportärende eller avbryta och köpa nytt hello gäller direkta inköp.
>
> När du köper Azure AD eller Enterprise Mobility Suite via volymlicensiering aktiveringen av prenumerationen sker automatiskt när hello avtalet innehåller andra Microsoft Online services, t.ex. Office 365.
>
>

Betald Azure AD-funktioner hello directory span hello bredd. Exempel:

* Gruppbaserad tilldelning tooapplications som är aktiverad under hello specifikt program som du hanterar.
* Avancerade och hanteringsfunktioner för självbetjäning är tillgängliga i hello specifik grupp eller under hello directory-konfigurationen.
* Premium säkerhetsrapporter finns på fliken för hello-rapportering
* Molnet programidentifiering visas i hello Azure-portalen under identitet.

### <a name="assigning-licenses"></a>Tilldela licenser
Även om hur du skaffar en prenumeration är alla måste tooconfigure betald funktioner, kräver med hjälp av din Azure AD betald funktioner distribuerar licenser toohello rätt personer. I allmänhet måste alla användare som ska ha åtkomst tooor som hanteras via en Azure AD betald funktionen tilldelas en licens. En licenstilldelning finns en mappning mellan en användare och en köpta tjänst, till exempel Azure AD Premium eller Basic eller Enterprise Mobility Suite.

Det är enkelt att hantera vilka användare i din katalog ska ha en licens. Det kan åstadkommas genom att tilldela tooa grupp toocreate tilldelningsreglerna via hello Azure AD-administrationsportalen eller genom att tilldela licenser direkt toohello rätt personer via en portal, PowerShell eller API: er. När du tilldelar licenser tooa grupp tilldelats alla medlemmar i gruppen en licens. Om användare läggs till eller tas bort från hello gruppen blir de tilldelade eller borttagna hello lämplig licens. Grupptilldelning kan utnyttja alla tillgängliga tooyou för grupp-hantering och är konsekvent med gruppbaserad tilldelning tooapplications. Med den här metoden kan du ställa in regler så att alla användare i din katalog tilldelas automatiskt, se till att alla med hello lämpliga befattning har en licens eller även delegera hello beslut tooother chefer i hello organisation.

Med gruppbaserade licenstilldelning ärver alla användare som saknar en användningsplats hello katalogplatsen under tilldelning. Den här platsen kan ändras av Hej administratör när som helst. I fall där hello Automatisk tilldelning misslyckades på grund av tooerror hello användarinformation under att licenstypen motsvarar det aktuella tillståndet.

## <a name="getting-started-with-azure-ad-licensing"></a>Kom igång med licensieringen av Azure AD
Komma igång med Azure AD är enkelt; Du kan alltid skapa katalogen som en del av registreringen tooa kostnadsfri utvärderingsversion av Azure. [Mer information om att logga som en organisation](sign-up-organization.md). hello följande kan hjälpa dig att kontrollera att din katalog bäst stämmer överens med andra Microsoft-tjänster som du kan använda eller planerar tooconsume och dina mål för att få hello-tjänsten.

Här följer några metodtips:

* Om du redan använder någon av Microsofts organisationens tjänster kan har du redan en Azure AD-katalog. I det här fallet bör du fortsätta toouse hello samma katalog för andra tjänster, så att core Identitetshantering, inklusive etablering och hybrid SSO kan användas för hello-tjänster. Användarna får en enda inloggningen och drar nytta av bättre funktioner för hello-tjänster. Därför, om du väljer toobuy en Azure AD betald service för din arbetsstyrka, rekommenderar vi att du använder hello samma directory toodo detta.
* Om du planerar toouse Azure AD för en annan uppsättning användare (partners, kunder och så vidare), eller om du vill tooevaluate Azure AD-tjänster och vill toodo som avskilt om produktionstjänsten, eller om du behöver toosetup begränsat läge för tjänsterna rekommenderar vi att du först skapa en ny katalog via hello Azure Azure klassiska portal. [Lär dig mer om hur du skapar en ny Azure AD-katalog i hello klassiska Azure-portalen](active-directory-licensing-directory-independence.md). hello ny katalog kommer att skapas med ditt konto som en extern användare med globala administratörsbehörigheter. När du loggar in toohello Azure klassiska portal med det här kontot du ska kunna toosee katalogen och åtkomst till alla administrationsuppgifter för katalogen. Vi rekommenderar att du skapar ett lokalt konto med rätt behörighet toomanage andra Microsoft-tjänster (de som inte kan nås via hello klassiska Azure-portalen). [Mer information om hur du skapar användarkonton i Azure AD](active-directory-create-users.md).

> [!NOTE]
> Azure AD stöder ”externa-användare”, som är användarkonton i en instans av Azure AD som har skapats med ett Microsoft-konto (MSA) eller en identitet för Azure AD från en annan katalog. Medan vi är upptagna stöds utökar den här funktionen i alla organisationens Microsoft-tjänster just nu dessa konton inte i några av hello services erfarenheter. till exempel stöder hello Office 365-administrationsportalen för närvarande inte dessa användare. Därför externa användare med Microsoft-konton inte kan tooaccess hello Office 365-administrationsportalen, medan externa användare från andra Azure AD-kataloger kommer att ignoreras. Katalogen där hello användaren ursprungligen skapades, vara tillgängligt via dessa upplevelser i hello senare fallet endast hello användarens lokala kontot, hello Azure AD eller Office 365.
>
>

Som det anges, har Azure AD olika betald versioner. Dessa versioner har några mindre skillnader i deras köp tillgänglighet:

| Produkt | EA/VL | Öppet | CSP | MPN användarrättigheter | Inköp | Utvärdering |
| --- | --- | --- | --- | --- | --- | --- |
| Enterprise Mobility Suite |X |X |X |X | |X |
| Azure AD Premium |X |X |X | |X |X |
| Azure AD Basic |X |X |X |X |<br /> |<br /> |

### <a name="select-one-or-more-license-trials"></a>Välj en eller flera licens försök
 I samtliga fall måste aktivera du en Azure AD Premium eller Enterprise Mobility Suite-provprenumeration genom att välja specifika hello utvärderingsversion som du vill använda hello licenser på fliken i din katalog. Antingen utvärderingsversionen innehåller en 30-dagars prenumeration med 100 licenser.

![Azure Active Directory-testversion planer](./media/active-directory-licensing-what-is/trial_plans.png)

![Enterprise Mobility Suite utvärderingslicens planer](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Aktiva utvärderingslicens planer](./media/active-directory-licensing-what-is/active_license_trials.png)

### <a name="assign-licenses"></a>Tilldela licenser
När hello prenumeration är aktiv, bör du tilldela en licens tooyourself och uppdatera hello webbläsare tooensure du ser alla funktioner. hello nästa steg är tooassign licenser toohello användare som behöver tooaccess eller inkluderas i betald Azure AD-funktioner. Som nämnts ovan i ”tilldela licenser”, Hej bästa sätt toodo detta är tooidentify hello gruppen som representerar hello önskad målgrupp och tilldela den toohello licens; på så sätt kan tilldelas användare som har lagts till eller tagits bort från hello över dess livscykel tooor tas bort från hello-licens.

tooassign en licensgrupp tooa eller enskilda användare, Välj hello licensplan du skulle t.ex. tooassign och klicka på **tilldela** i hello kommandofält.

![Aktiva utvärderingslicens planer](./media/active-directory-licensing-what-is/assign_licenses.png)

När hello tilldelning dialogen för hello valda planen kan du välja användare och lägga till dem toohello **tilldela** hello högra kolumnen. Du kan bläddra igenom hello användarlistan eller Sök efter specifika personer med hello söker glas på hello uppifrån höger om hello användaren rutnätet. tooassign grupper och välj ”grupper” Hej **visa** menyn och klicka sedan på hello Kontrollera hello rätt toorefresh hello tilldelningar som visas.

![Tilldela licenser toogroups](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Nu kan du söka eller bläddra igenom grupper och lägga till dem toohello **tilldela** kolumn i hello samma sätt. Du kan använda dessa tooassign en kombination av användare och grupper i en enda åtgärd. toocomplete Hej tilldelningen, klicka hello kontroll i hello nedre högra hörnet av hello-sidan.

![Licens tilldelning pågående meddelande](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

När en grupp har tilldelats, ärver dess medlemmar hello licenser inom 30 minuter, men vanligen inom 1 till 2 minuter.

Tilldelningsfel kan uppstå under Azure AD-licenstilldelning, men är relativt ovanliga. Potentiella Tilldelningsfel är begränsade till:

* Tilldelningen konflikt - när en användare har tidigare tilldelats en licens som inte är kompatibel med aktuellt hello-licens. I det här fallet kräver tilldela hello ny licens att ta bort hello föregående.
* Överskred tillgängliga licenser - hello antalet användare i tilldelade grupper överskrider tillgängliga licenser hello användarnas tilldelningsstatus kommer att användas i en tooassign misslyckades på grund av toomissing licenser.

### <a name="view-assigned-licenses"></a>Visa tilldelade licenser
En sammanfattning av tilldelade licenser, inklusive tillgängliga tilldelade och nästa prenumeration livscykel händelse visas på hello **licenser** fliken.

![Visa hello antalet tilldelade licenser](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

En detaljerad lista över tilldelade användare och grupper, inklusive tilldelningsstatus och sökväg (direkt eller ärvd från en eller flera grupper) är tillgänglig när du navigerar till en licensplan.

![Information om visning av licenser som tilldelats för en licensplan](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Det är lika enkelt som att tilldela dem att ta bort licenser. Om hello användare tilldelas direkt eller för en grupp, du kan ta bort hello-licens genom att välja hello licenstypen, väljer du **ta bort**, lägga till hello användare eller grupp toohello ta bort listan och bekräfta hello-åtgärd. Du kan öppna en licenstyp, Välj hello specifika användare eller grupp, och tryck på **ta bort** i hello kommandofält. tooend en användares arv av en licens från en grupp bara ta bort hello användaren från hello grupp.

### <a name="extending-trials"></a>Utöka försök
Utvärderingsversion tillägg för kunder som är tillgängliga som självbetjäning hello Office 365-portalen. En kund-administratör kan navigera toohello [Office-portalen](https://portal.office.com/#Billing) (åtkomst beror på behörigheter för hello Office-portalen) och välj din Azure AD Premium-utvärderingsversion. Klicka på hello **förlänga utvärderingsperioden** länka och följer instruktionerna för hello. Du behöver tooenter ett kreditkort, men den kommer inte att debiteras.

![Utöka en licens utvärderingsversion i hello Office-portalen](./media/active-directory-licensing-what-is/extend_license_trial.png)

Kunder kan också begära en förlängd genom att skicka en supportförfrågan. En kund-administratör kan navigera toohello Office 365-portalen [supportsidan](http://aka.ms/extendAADtrial) (åtkomst beror på behörigheter för hello Office supportsidan). På den här sidan väljer du ”prenumerationer och försök” under funktioner och ”utvärderingsversion frågor” under symtom. Slutligen, ange information om hello omständigheter

![Utöka en licens utvärderingsversion med ett supportärende](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Nästa steg
Nu kan du vara redo tooconfigure och använda vissa Azure AD Premium-funktioner.

* [Lösenordsåterställning via självbetjäning](active-directory-manage-passwords.md)
* [Grupphantering via självbetjäning](active-directory-accessmanagement-self-service-group-management.md)
* [Azure AD Connect hed](active-directory-aadconnect-health.md)
* [Gruppen tilldelning tooapplications](active-directory-manage-groups.md)
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* [Inköp av Azure AD Premium-licenser](http://aka.ms/buyaadp)
