---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - fastställa införandestrategin för hybrid identity livscykel | Microsoft Docs"
description: "Bidrar till att definiera hello hybrid identity hanteringsuppgifter enligt toohello alternativen för varje fas i livscykeln."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 420b6046-bd9b-4fce-83b0-72625878ae71
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 86ec0a9896f069bc93e49e06006954848f8e4d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Fastställa införandestrategin för hybrid identity livscykel
I det här steget ska du definiera hello identitet strategi för hybrid identity lösning toomeet hello företagets behov som du definierade i [fastställa hanteringsuppgifter för hybrid identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).

toodefine hello hybrid identity hanteringsuppgifter enligt toohello slutpunkt till slutpunkt identitet livscykel presenteras tidigare i det här steget, har du tooconsider hello-alternativ som är tillgängliga för varje fas i livscykeln.

## <a name="access-management-and-provisioning"></a>Hantering och etablering
Med en bra konto åtkomstlösning, kan din organisation spåra exakt vem som har åtkomst till toowhat information över hello organisationen.

Åtkomstkontroll är en viktig funktion av en central, enda punkt etablering system. Förutom att skydda känslig information åtkomstkontroller för att visa befintliga konton som har icke-godkända tillstånd eller är inte längre behövs. toocontrol föråldrade konton, hello etablering system länkar tillsammans kontoinformation med auktoritär information om hello användarna som äger hello konton. Auktoritativa användaren identitetsinformation underhålls vanligtvis i hello databaser och kataloger på personalavdelningen.

Konton i avancerade IT-företag med hundratals olika parametrar som definierar hello myndigheter och dessa uppgifter kan styras av systemet etablering. Nya användare kan identifieras med hello data som du anger från hello auktoritativ datakälla. funktionen för hello till begäran om godkännande initierar hello processer som godkänna (eller avvisa) resurs etablering för dem.

| Livscykeln för hantering av fas | Lokalt | Molnet | Hybrid |
| --- | --- | --- | --- |
| Kontohantering och etablering |Med hjälp av serverrollen för hello Active Directory® Domain Services (AD DS) kan du skapa en skalbar, säker och hanterbar infrastruktur för användare och resurshantering och erbjuda stöd för katalogaktiverade applikationer som Microsoft® Exchange Server. <br><br> [Du kan etablera grupper i AD DS via en Identity manager](https://technet.microsoft.com/library/ff686261.aspx) <br>[Du kan etablera användare i AD DS](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Administratörer kan använda access control toomanage användare komma åt tooshared resurser av säkerhetsskäl. I Active Directory administreras åtkomstkontroll på hello objektnivå genom att ange olika nivåer av åtkomst eller behörigheter tooobjects, till exempel fullständig behörighet, Skriv-, läs- eller ingen åtkomst. Åtkomstkontroll i Active Directory definierar hur olika användare kan använda Active Directory-objekt. Behörigheter för objekt i Active Directory är som standard toohello mest säkra inställningen. |Du har toocreate ett konto för varje användare som kommer åt en Microsoft-molntjänst. Du kan också ändra användarkonton eller ta bort dem när de inte längre behövs. Användare har inte administratörsbehörighet som standard, men du kan tilldela dem (valfritt). Mer information finns i [hantera användare i Azure AD](active-directory-create-users.md). <br><br> En av hello huvudfunktioner är hello möjlighet toomanage åtkomst tooresources i Azure Active Directory. Dessa resurser kan vara en del av hello directory som hello fallet med behörigheter toomanage objekt genom roller i hello katalog eller resurser som är externa toohello katalog, t.ex SaaS-program, Azure-tjänster, och SharePoint-webbplatser eller lokala resurser. <br><br> Vid hello är center av Azure Active Directorys åtkomst hanteringslösning hello säkerhetsgrupp. Hej resursägare (eller Hej administratör för hello katalog) tilldela en grupp tooprovide en vissa åtkomst rätt toohello resurser de äger. hello medlemmar i hello gruppen kommer att tillhandahållas hello åtkomst och hello resursägare kan delegera hello rätt toomanage hello medlemslista för en grupp toosomeone annan – till exempel en avdelningschef eller administratör supportavdelning<br> <br> hello hantera grupper i Azure AD-avsnittet finns mer information om hur du hanterar åtkomst via grupper. |Utöka Active Directory identiteter i hello moln via synkronisering och federation |

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll
Rollbaserad åtkomstkontroll (RBAC) använder roller och provisioning principer tooevaluate, testa och tillämpa dina affärsprocesser och regler för att bevilja åtkomst toousers. Viktiga administratörer skapa principer för etablering och tilldela användare tooroles och som definierar en uppsättning rättigheter tooresources för dessa roller. RBAC utökar hello identity management-lösningen toouse programvarubaserad processer och minska manuella användaråtgärder i hello etableringsprocessen.
Azure AD-RBAC kan hello företagets toorestrict hello mängden åtgärder som en enskild person kan göra när han har åtkomst tooAzure hanteringsportalen. Med RBAC toocontrol åtkomst toohello portal kan IT-administratörer ca delegera åtkomst med hjälp av följande åtkomsthantering hello närmar sig:

* **Gruppbaserade rolltilldelning**: du kan tilldela åtkomst tooAzure AD-grupper som kan synkroniseras från din lokala Active Directory. Detta gör att du tooleverage hello befintliga investeringar som organisationen har gjort i verktygsuppsättning och processer för att hantera grupper. Du kan också använda funktionen för hello delegerade gruppen hantering av Azure AD Premium.
* **Utnyttjar inbyggda roller i Azure**: du kan använda tre roller, ägare, deltagare och läsare, tooensure att användare och grupper har behörigheten toodo endast hello aktiviteter de behöver toodo sitt arbete.
* **Detaljerade åtkomst tooresources**: du kan tilldela roller toousers och grupper för en viss prenumeration, resursgrupp eller en enskild Azure resurs, till exempel en webbplats eller i databasen. På så sätt kan du se till att användarna har åtkomst tooall hello resurser som de behöver och ingen åtkomst tooresources att de inte behöver toomanage.

## <a name="provisioning-and-other-customization-options"></a>Etablering och andra alternativ för anpassning
Ditt team kan använda business planer och krav toodecide hur mycket toocustomize hello identitetslösning. Stora företag kan till exempel kräva en stegvis distributionsplan för arbetsflöden och anpassade kort som baseras på en tidslinje för att etablera inkrementellt program som används ofta i geografiska områden. En annan anpassning planen kan innehålla två eller flera program toobe etableras i hela företaget, efter lyckad testning. Interaktion från användaren program kan anpassas och procedurer för att etablera resurser kan vara ändrade tooaccommodate Automatisk etablering.

Du kan ta bort etableringen tooremove en tjänst eller en komponent. Till exempel innebär avetablering ett konto att hello kontot har tagits bort från en resurs.

hello hybrid modell för etablering av resurser kombinerar begäran och rollbaserad metoder som stöds både av Azure AD. Ett företag kanske vill tooautomate åtkomst med rollbaserad tilldelning för en delmängd av anställda eller hanterade system. Ett företag kan också hantera alla andra förfrågningar eller undantag via en frågebaserad modell. Vissa företag kan börja med manuell tilldelning och utvecklas mot en hybrid-modell med en avsikt är att ett fullständigt rollbaserad distributionen vid ett senare tillfälle.

Andra företag kanske opraktiska för business orsaker tooachieve fullständig rollbaserad etablering och målet en hybrid-metod som önskat mål. Fortfarande andra företag kan vara uppfyllda endast frågebaserad etablering, och inte vill tooinvest extra arbete toodefine och hantera rollbaserad, automatiserad etablering principer.

## <a name="license-management"></a>Licenshantering
Gruppbaserade licenshantering i Azure AD kan administratörer tilldela användare tooa säkerhetsgrupp och Azure AD tilldelar automatiskt licenser tooall hello medlemmar i gruppen för hello. Om en användare tas bort från gruppen hello eller senare lägga till tilldelats en licens automatiskt eller tas bort efter behov.

Du kan använda grupper som du synkroniserar från lokala AD eller hantera i Azure AD. Koppla ihop detta med Azure AD premium Self-Service Group Management delegera du enkelt licens tilldelning toohello lämplig beslutsfattare. Du kan vara säker på att problem som licens konflikter och saknas lokaliseringsuppgifter sorteras automatiskt.

## <a name="self-regulating-user-administration"></a>Automatisk reglerande Användaradministration
När din organisation startar tooprovision resurser över alla interna organisationer implementera hello automatisk reglerande användaren administration kapaciteten. Du kan spara hello fördelar och fördelarna med etablering användare över organisationens gränser. I den här miljön återspeglas automatiskt en ändring i användarens status i åtkomsträttigheter över organisationens gränser och geografiska områden. Du kan minska kostnaderna för etablering och förenkla hello åtkomst och godkännande processer. hello implementering inser hello fullt ut för att implementera rollbaserad åtkomstkontroll för slutpunkt till slutpunkt access management i din organisation. Du kan minska administrativa kostnader genom automatisk förfaranden som reglerar användaretablering. Du kan förbättra säkerheten genom att automatisera tvingande säkerhetsprinciper, och förenkla och centralisera Livscykelhantering för användaren och resursetablering för stora användare.

> [!NOTE]
> Mer information finns i Konfigurera Azure AD för självbetjäningsportalen programhantering åtkomst
> 
> 

Licens-baserade (rätt-baserat) Azure AD-tjänster fungerar genom att aktivera en prenumeration i Azure AD directory/service-klienten. När hello prenumeration är aktiv hello funktioner hanteras av administratörer för directory-tjänsten och används av licensierade användare. Mer information finns i hur fungerar Azure AD-licensiering arbete?
Integrering med andra 3 part-leverantörer

Azure Active Directory tillhandahåller enkel inloggning på och förbättrad programmet åtkomst säkerhet toothousands av SaaS-program och lokala webbprogram. En detaljerad lista över Azure Active Directory-programgalleriet för SaaS-program som stöds finns i kompatibilitetslistan för Azure Active Directory federation: tredjeparts identitetsleverantörer som kan använda tooimplement enkel inloggning

## <a name="define-synchronization-management"></a>Definiera hantering av datasynkronisering
Om du integrerar dina lokala kataloger med Azure AD kan du hjälpa dina användare att bli mer produktiva genom att tillhandahålla en gemensam identitet för åtkomst både till molnet och lokala resurser. Med den här integreringen kan användare och företag dra nytta av hello följande:

* Organisationer kan ge användarna en gemensam hybrididentitet över lokala eller molnbaserade tjänster utnyttja Windows Server Active Directory och sedan ansluta tooAzure Active Directory.
* Administratörer kan ge villkorad tillgång baserat på programresursen, enheten och användarens identitet, nätverksplats och multifaktorautentisering.
* Användare kan använda sina gemensam identitet via konton i Azure AD tooOffice 365, Intune, SaaS-appar och program från tredje part.
* Utvecklare kan skapa program som utnyttjar hello vanliga identitetsmodellen integrering av program i Active Directory lokalt eller Azure för molnbaserade program

hello innehåller följande bild ett exempel på en övergripande bild av processen för synkronisering av identitet.

![](./media/hybrid-id-design-considerations/identitysync.png)

Processen för synkronisering av identitet

Granska hello följande tabell toocompare hello synkroniseringsalternativ:

| Alternativ för synkronisering av Management | Fördelar | Nackdelar |
| --- | --- | --- |
| Sync-baserade (via DirSync eller AADConnect) |Användare och grupper som synkroniseras från molnet och lokalt <br>  **Princip för värdegruppen**: Kontoprinciper kan anges via Active Directory, som ger Hej administratör hello möjlighet toomanage lösenordsprinciper, arbetsstation, begränsningar, lås ut kontroller, och mer, utan att behöva ytterligare tooperform uppgifter i hello molnet.  <br>  **Åtkomstkontroll**: kan begränsa åtkomst toohello molntjänst så att hello tjänster kan nås via hello företagsmiljö via online servrar eller båda. <br>  Minskad supportsamtal: om användarna har färre lösenord tooremember, de är mindre troligt tooforget dem. <br>  Säkerhet: Användaridentiteter och information skyddas eftersom alla hello servrar och tjänster som används i enkel inloggning, hanterat och kontrollerade lokalt. <br>  Stöd för stark autentisering: du kan använda stark autentisering (även kallat tvåfaktorsautentisering) med hello-Molntjänsten. Om du använder stark autentisering, måste du använda enkel inloggning. | |
| Federationsbaserat (via AD FS) |Aktiverad som en säkerhetstokentjänst (STS). När du konfigurerar en STS tooprovide enkel inloggning åtkomst med Microsoft-Molntjänsten, skapar du ett federerat förtroende mellan din lokala STS och hello federerade domänen som du har angett i Azure AD-klienten. <br> Innebär att slutanvändarna toouse hello samma uppsättning autentiseringsuppgifter tooobtain åtkomst toomultiple resurser <br>användare har inte toomaintain flera uppsättningar med autentiseringsuppgifter. Ännu, hello användare har tooprovide sina autentiseringsuppgifter tooeach en hello deltagande resurser., B2B och B2C-scenarier som stöds. |Kräver särskild personal för distribution och underhåll av dedikerade lokal AD FS-servrarna. Det finns begränsningar hello användningen av stark autentisering om du planerar din STS toouse AD FS. Mer information finns i [konfigurera avancerade alternativ för AD FS 2.0](http://go.microsoft.com/fwlink/?linkid=235649). |

> [!NOTE]
> Mer information finns i [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
> 
> 

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

