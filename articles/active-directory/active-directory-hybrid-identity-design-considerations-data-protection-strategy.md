---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - definiera en strategi för data protection | Microsoft Docs"
description: "Du måste definiera hello strategin för skydd av data för hybrid identity lösning toomeet hello företagets krav som du har definierat."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e76fd1f4-340a-492a-84d9-e05f3b7cc396
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 8fd7ab364a09de3b60293a4a1cbb6e0fa4a3295d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Definiera en strategi för din hybrididentitetslösning dataskydd
I den här uppgiften definierar du hello data protection strategi för hybrid identity lösning toomeet hello företagets behov som du definierade i:

* [Fastställa kraven på dataskydd](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
* [Ange krav för innehållshantering](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
* [Fastställa krav på åtkomstkontroll](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
* [Fastställa kraven på incidentsvar](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Definiera alternativ för skydd av data
Som anges i [fastställa kraven för directory-synkronisering](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), Microsoft Azure AD kan synkronisera med din Active Directory Domain Services (AD DS) finns lokalt. Integreringen gör att organisationer tooleverage Azure AD tooverify användarens autentiseringsuppgifter när de försöker tooaccess företagets resurser. Detta kan göras för båda scenarierna: data i vila på lokalt och i hello molnet.  Åtkomst toodata i Azure AD kräver användarautentisering via en säkerhetstokentjänst (STS).

När autentiseras hello användarens huvudnamn (UPN) läses från hello autentiseringstoken och hello replikeras partition och behållare motsvarande bestäms toohello användarens domän. Information om hello användarens existens aktiverat läge och rollen används av hello auktorisering system toodetermine om hello begärda åtkomsten toohello mål klienten har behörighet för den här användaren i den här sessionen. Vissa auktoriserade åtgärder (skapa mer specifikt användare, återställning av lösenord) skapa en granskningslogg som kan användas av en klient administratören toomanage kompatibilitet arbete eller undersökningar.

Flytta data från ditt lokala datacenter i Azure Storage via en Internet-anslutning kan inte alltid möjligt på grund av toodata volym, bandbreddstillgänglighet eller andra överväganden. Hej [Azure Storage Import/Export Service](../storage/common/storage-import-export-service.md) innehåller en maskinvarubaserad alternativ för att placera/hämtning av stora mängder data i blob storage. Det gör att du toosend [BitLocker-krypterad](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) -hårddiskar direkt tooan Azure-datacenter där molnoperatörer kommer att överföra hello innehållet tooyour storage-konto eller kan de hämta ditt Azure data tooyour enheter tooreturn tooyou. Endast krypterade diskar accepteras för den här processen (med en BitLocker-nyckel som genererats av hello själva tjänsten under installationen av hello jobb). hello nätverksnyckeln BitLocker tooAzure separat, vilket ger out-of-band viktiga delar.

Eftersom data under överföringen kan ske i olika scenarier, är också relevant tooknow som använder Microsoft Azure [virtuella nätverk](https://azure.microsoft.com/documentation/services/virtual-network/) tooisolate klienternas trafik från varandra, med åtgärder som värd och Gäst-nivå brandväggar, IP-paketfilter, port blockerar och HTTPS-slutpunkter. De flesta av Azures intern kommunikation, inklusive infrastruktur-infrastruktur och infrastruktur-till-kund (lokalt), är också krypterade. Ett annat viktigt scenario är hello kommunikation inom Azure-Datacenter; Microsoft hanterar nätverk tooassure som ingen VM kan personifiera eller eavesdrop på hello IP-adressen för en annan. TLS/SSL används för att få åtkomst till Azure Storage eller Azure SQL-databaser, eller när tooCloud anslutningstjänsterna. I det här fallet ansvarar hello kundens administratör för att erhålla ett TLS/SSL-certifikat och distribuera den tootheir klient infrastruktur. Datatrafik flytta mellan virtuella datorer i samma distribution hello eller mellan klienter i en enda distribution via Microsoft Azure Virtual Network kan skyddas via krypterad kommunikation, till exempel HTTPS eller SSL/TLS-protokoll.

Beroende på hur du besvarade frågorna hello i [fastställa kraven på dataskydd](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md), bör du kunna toodetermine hur du vill tooprotect dina data och hello hybrididentitetslösning kan hjälpa dig på den. hello tabellen visar hello-alternativ som stöds av Azure och som är tillgängliga för varje scenario för skydd av data.

| Alternativ för skydd av data | I vila i hello moln | Vid rest lokala | Under överföring |
| --- | --- | --- | --- |
| BitLocker-diskkryptering |X |X | |
| SQL Server-databaser tooencrypt |X |X | |
| VM-till-VM-kryptering | | |X |
| SSL/TLS | | |X |
| VPN | | |X |

> [!NOTE]
> Läs [efterlevnad av funktionen](https://azure.microsoft.com/support/trust-center/services/) på [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) tooknow mer om hello-certifikat som överensstämmer med varje Azure-tjänsten.
> Jämförelse mellan dessa alternativ kan inte användas för den här uppgiften eftersom hello alternativ för skydd av data använder en multilayer metod. Se till att du använder sig av alla alternativ som är tillgängliga för varje tillstånd hello data kommer att.
>
>

## <a name="define-content-management-options"></a>Definiera alternativ för innehållshantering
Fördelen med att använda Azure AD toomanage en hybrididentitetsinfrastruktur är att hello processen är helt transparent från hello slutanvändarens perspektiv. hello användaren försöker tooaccess en delad resurs, hello resursen kräver autentisering, hello användaren har toosend en autentisering begäran tooAzure AD i ordning tooobtain hello token och komma åt hello resurs. Hela processen sker i bakgrunden utan interaktion från användaren. Det är också möjligt toogrant behörighet tooa [grupp](active-directory-manage-groups.md#getting-started-with-access-management) för användare i ordning tooallow dem tooperform vissa vanliga åtgärder.

Organisationer som är skadliga om datasekretess vanligtvis kräver dataklassificering för sina lösningar. Om den aktuella lokala infrastrukturen redan använder dataklassificering, är det möjligt tooleverage Azure AD som hello centrala lagringsplatsen för användarens identitet. Ett gemensamma verktyg för att det är används lokalt för dataklassificering kallas [Dataklassificering](https://msdn.microsoft.com/library/Hh204743.aspx) för Windows Server 2012 R2. Det här verktyget kan hjälpa tooidentify, klassificera och skydda data på filservrar i ditt privata moln. Det är också möjligt tooleverage hello [automatisk Filklassificering](https://technet.microsoft.com/library/hh831672.aspx) i Windows Server 2012 tooaccomplish detta.

Om din organisation inte har dataklassificering på plats, men måste tooprotect känsliga filer utan att lägga till nya servrar lokalt, kan de använda Microsoft [Azure Rights Management Service](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS använder kryptering, identitet och auktorisering principer toohelp som finns för säkra filer och e-post och den fungerar på flera enheter – telefoner, surfplattor och datorer. Eftersom Azure RMS är en molnbaserad tjänst bör det behövs ingen tooexplicitly konfigurera förtroenden med andra organisationer innan du kan dela skyddat innehåll med dem.. Samarbete mellan olika organisationer stöds automatiskt om de redan har en Office 365 eller Azure AD-katalog. Du kan också synkronisera precis hello katalogattribut som Azure RMS behöver toosupport en gemensam identitet för din lokala Active Directory-konton med hjälp av Azure Active Directory Sync Services (AAD Sync) eller Azure AD Connect.

En viktig del av innehållshantering är toounderstand som har tillgång till vilka resurser, en omfattande loggningsfunktioner är därför viktigt för hello lösning för Identitetshantering. Azure AD innehåller logg över 30 dagar, inklusive:

* Ändringar i medlemskap (ex: användaren lade tooGlobal administratörsroll)
* Autentiseringsuppgifter uppdateringar (ex: lösenordsändringar)
* Domänhantering (ex: Verifiera en anpassad domän, ta bort en domän)
* Lägga till eller ta bort program
* Användarhantering (ex: lägga till, ta bort, uppdaterar en användare)
* Lägga till eller ta bort licenser

> [!NOTE]
> Läs [Microsoft Azure-säkerhet och hantering av granskningslogg](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) tooknow mer om funktioner för loggning i Azure.
> Beroende på hur du besvarade frågorna hello i [fastställa kraven för innehållshantering](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md), bör du kunna toodetermine du hur hello innehåll toobe hanteras i din hybrididentitetslösning. Alla alternativ som visas i tabell 6 är kan integreras med Azure AD, är det viktigt toodefine som är mer lämpliga för dina affärsbehov.
>
>

| Alternativen för innehållshantering | Fördelar | Nackdelar |
| --- | --- | --- |
| Lokalt centraliserad (Active Directory Rights Management Server) |Fullständig kontroll över hello serverinfrastruktur ansvarar för att klassificera hello data <br> Inbyggda funktioner i Windows Server, behöver inte extra licens eller prenumeration <br> Kan integreras med Azure AD i ett hybridscenario <br> Stöder information rights management (IRM) funktioner i Microsoft Online services, till exempel Exchange Online och SharePoint Online, samt Office 365 <br> Har stöd för lokala Microsoft server-produkter, till exempel Exchange Server, SharePoint Server och filservrar som kör Windows Server och Filklassificeringsinfrastruktur (FCI). |Högre, Underhåll (hålla jämna steg med uppdateringar, konfiguration och eventuella uppgraderingar), eftersom IT äger hello Server <br> Kräv en serverinfrastruktur lokala<br> Doesn'tleverage funktioner i Azure internt |
| Centraliserad i hello molnet (Azure RMS) |Enklare toomanage jämfört med toohello lokal lösning <br> Kan integreras med AD DS i ett hybridscenario <br>  Helt integrerat med Azure AD <br> En server kräver inte lokala i ordning toodeploy hello service <br> Har stöd för lokala Microsoft server-produkter, till exempel Exchange Server, SharePoint, Server och filservrar som kör Windows Server och File Classification Infrastructure (FCI) <br> IT-avdelningen, kan ha fullständig kontroll över sina klientnyckel med BYOK-kapacitet. |Din organisation måste ha en molnprenumeration med stöd för RMS <br> Din organisation måste ha en Azure AD directory toosupport användarautentisering för RMS |
| Hybrid (Azure RMS är integrerad med, lokala Active Directory Rights Management Server) |Det här scenariot ackumuleras hello fördelar för både, centraliserad lokalt och i hello molnet. |Din organisation måste ha en molnprenumeration med stöd för RMS <br> Din organisation måste ha en Azure AD directory toosupport användarautentisering för RMS, <br> Kräver en anslutning mellan Azure-molntjänst och lokal infrastruktur |

## <a name="define-access-control-options"></a>Definiera alternativ för åtkomstkontroll
Genom att utnyttja hello autentisering, styra auktorisering och funktioner som är tillgängliga i Azure AD som du kommer att kunna tooenable ditt företag toouse centrala identitetsdatabas samtidigt som användarna och partners toouse enkel inloggning (SSO) enligt hello bilden nedan:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Centraliserad hantering och fullständigt integrering med andra kataloger

Azure Active Directory tillhandahåller enkel inloggning toothousands för SaaS-program och lokala webbprogram. Läs hello [kompatibilitetslista för Azure Active Directory federation: tredjeparts identitetsleverantörer som kan använda tooimplement enkel inloggning](https://msdn.microsoft.com/library/azure/jj679342.aspx) artikeln för mer information om hello SSO från tredje part som har testats av Microsoft. Den här funktionen kan organisationen tooimplement en mängd olika B2B-scenarier samtidigt som kontrollen över hello identitets- och åtkomsthantering. Under hello B2B designa processen är dock viktigt toounderstand hello autentiseringsmetod som ska användas av hello partner och verifiera om den här metoden stöds av Azure. Det här är för närvarande metoder som stöds av Azure AD:

* Security Assertion Markup Language (SAML)
* OAuth
* Kerberos
* Token
* Certifikat

> [!NOTE]
> läsa [Azure Active Directory-autentiseringsprotokoll](https://msdn.microsoft.com/library/azure/dn151124.aspx) tooknow mer information om varje protokoll och dess funktioner i Azure.
>
>

Använda hello Azure AD-stöd, mobil business-program kan använda hello samma enkelt Mobile Services autentisering upplevelse tooallow anställda toosign i deras mobila program med sina företagsuppgifter för Active Directory. Med den här funktionen stöds Azure AD som en identitetsleverantör i Mobile Services tillsammans med hello andra identitetsleverantörer vi stöder redan (som omfattar Microsoft Accounts, Facebook-ID, ID för Google och Twitter-ID). Om hello lokalt appar som använder hello användarens autentiseringsuppgifter på hello företagets AD DS, vara hello åtkomst från partner och användare som kommer från molnet hello transparent. Du kan hantera användares webbprogram för villkorlig åtkomstkontroll too(cloud-based), webb-API, Microsoft molntjänster, 3 part SaaS-program och interna () mobilklientprogram och har hello fördelarna med säkerhet, granskning, rapportering i en Placera. Det är dock rekommenderar toovalidate detta i en produktionsmiljö eller med ett begränsat antal användare.

> [!TIP]
> Det är viktigt toomention att Azure AD inte har en grupprincip som har AD DS. I princip för oordnade tooenforce för enheter behöver du en lösning för hantering av mobila enheter som [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).
>
>

När hello användaren autentiseras med hjälp av Azure AD, är det viktigt tooevaluate hello åtkomstnivå hello användaren har. hello åtkomstnivå hello användaren har över en resurs kan variera, Azure AD kan lägga till ett ytterligare säkerhetsskikt genom att kontrollera åtkomst toosome resurser men du måste också tänka på att hello resurs själva kan också ha sin egen lista över åtkomstkontroll separat, till exempel hello åtkomstkontroll för filer som finns på en filserver. hello figuren nedan sammanfattas hello nivåer för åtkomstkontroll som du kan ha i ett hybridscenario:

![](./media/hybrid-id-design-considerations/accesscontrol.png)

Varje interaktion i hello diagram på bild X representerar en access control-scenario som kan omfattas av Azure AD. Nedan finner du en beskrivning av varje scenario:

1. Villkorlig åtkomst tooapplications som finns lokalt: du kan använda registrerade enheter med åtkomstprinciper för program som är konfigurerade toouse AD FS med Windows Server 2012 R2. Mer information om hur du konfigurerar lokal villkorlig åtkomst finns i [Konfigurera lokal villkorlig åtkomst med hjälp av Azure Active Directory Device Registration](active-directory-conditional-access.md).

2. Access Control toohello Azure-portalen: Azure kan du även kontrollera åtkomst toohello portal genom att använda rollbaserad åtkomstkontroll (RBAC)). Den här metoden kan hello företagets toorestrict hello mängden åtgärder som en enskild person kan göra i hello Azure-portalen. Med RBAC toocontrol åtkomst toohello portal kan kan IT-administratörer delegera åtkomst med hjälp av följande metoder för hantering av åtkomst hello:

* Gruppbaserade rolltilldelning: du kan tilldela åtkomst tooAzure AD-grupper som kan synkroniseras från din lokala Active Directory. Detta gör att du tooleverage hello befintliga investeringar som organisationen har gjort i verktygsuppsättning och processer för att hantera grupper. Du kan också använda funktionen för hello delegerade gruppen hantering av Azure AD Premium.
* Utnyttjar inbyggda roller i Azure: du kan använda tre roller, ägare, deltagare och läsare, tooensure att användare och grupper har behörigheten toodo endast hello aktiviteter de behöver toodo sitt arbete.
* Detaljerade åtkomst tooresources: du kan tilldela roller toousers och grupper för en viss prenumeration, resursgrupp eller en enskild Azure resurs, till exempel en webbplats eller i databasen. På så sätt kan du se till att användarna har åtkomst tooall hello resurser som de behöver och ingen åtkomst tooresources att de inte behöver toomanage.

> [!NOTE]
> Om du skapar program och vill toocustomize hello åtkomstkontroll för dem, men det är också möjligt toouse Azure AD Application roller för auktorisering. Granska det [WebApp-RoleClaims-DotNet exempel](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) om hur toobuild app-toouse den här funktionen.
>
>

3. Villkorlig åtkomst för Office 365-program med Microsoft Intune: IT-administratörer kan etablera villkorlig åtkomst enheten principer toosecure företagsresurser, medan på hello samma tid så att informationsarbetare på kompatibla enheter tooaccess hello tjänster. Mer information finns i [Enhetsprinciper för villkorlig åtkomst för Office 365-tjänster](active-directory-conditional-access-device-policies.md).

4. Villkorlig åtkomst för Saas-appar: [funktionen](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) tooconfigure programspecifika multifaktorautentisering åtkomstregler och hello möjlighet tooblock åtkomst för användare kan du inte i ett betrott nätverk. Du kan använda hello multifaktorautentisering regler tooall användare som är tilldelade toohello program, eller enbart för användare i säkerhetsgrupper som angetts. Användare kan uteslutas från hello multifaktorautentisering krav om de använder hello program från en IP-adress som i innanför hello organisationens nätverk.

Jämförelse mellan dessa alternativ kan inte användas för den här uppgiften eftersom hello alternativ för åtkomstkontroll använder en multilayer metod. Se till att du använder sig av alla alternativ som är tillgängliga för varje scenario som kräver toocontrol åtkomst tooyour resurser.

## <a name="define-incident-response-options"></a>Definiera alternativ för incidenter
Azure AD kan hjälpa IT tooidentity potentiella säkerhetsrisker i hello miljö genom att övervaka användarens aktivitet IT kan dra nytta av Azure AD-åtkomst och rapporter kapaciteten toogain insyn i hello integritet och säkerhet för din organisations katalog. Med den här informationen kan avgöra IT-administratör bättre var möjligt säkerhetsrisker kan ligga så att de kan rätt toomitigate riskerna.  [Azure AD Premium-prenumeration](active-directory-get-started-premium.md) har en uppsättning säkerhetsrapporter som kan aktivera IT tooobtain informationen. [Azure AD-rapporter](active-directory-view-access-usage-reports.md) kategoriseras enligt nedan:

* **Avvikelseidentifiering rapporter**: innehåller händelser att påträffades toobe avvikande inloggning. Vårt mål är toomake du känner till denna aktivitet och aktivera toobe kan toomake fastställa om en händelse är misstänkt.
* **Integrerade program rapporten**: ger insikter om hur molnprogram används i din organisation. Azure Active Directory möjliggör integrering med tusentals molnprogram.
* **Felrapporter**: ange fel som kan uppstå vid etablering av konton tooexternal program.
* **Användarspecifika rapporter**: visa enheten/logga i aktivitetsdata för en viss användare.
* **Aktivitetsloggar**: innehåller en post på alla granskade händelser inom hello senaste 24 timmarna, senaste 7 dagarna eller senaste 30 dagarna, samt ändringar av aktiviteten och aktiviteten för återställning och registrering av lösenord.

> [!TIP]
> En annan rapport som kan också hello Incident svarsgrupp som arbetar med ett ärende är hello [användare med läckta autentiseringsuppgifter](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) rapporten.  Den här rapporten hämtar alla matchningar mellan dessa läckta autentiseringsuppgifter lista och din klient.
>
>

Andra viktiga inbyggda rapporter i Azure AD som kan användas vid en undersökning av incidenter och är:

* **Lösenordsåterställningsaktivitet**: ge Hej administratör insikter om hur aktivt återställning av lösenord som används i hello organisation.
* **Återställningsaktivitet för lösenord registrering**: ger insikter som användare har registrerat sina metoder för återställning av lösenord, och vilka metoder som de har valt.
* **Gruppera aktiviteten**: innehåller en historik över ändringarna toohello grupp (ex: användare läggs till eller tas bort) som har initierat hello åtkomstpanelen.

Dessutom toohello core reporting funktioner som är tillgängliga i Azure AD Premium kan utnyttjas under en Incident svar undersökningen IT kan dra nytta av kontrollrapport tooobtain information som:

* Ändringar i medlemskap (ex: användaren lade tooGlobal administratörsroll)
* Autentiseringsuppgifter uppdateringar (ex: lösenordsändringar)
* Domänhantering (ex: Verifiera en anpassad domän, ta bort en domän)
* Lägga till eller ta bort program
* Användarhantering (ex: lägga till, ta bort, uppdaterar en användare)
* Lägga till eller ta bort licenser

Jämförelse mellan dessa alternativ kan inte användas för den här uppgiften eftersom hello alternativ för incidenter använder en multilayer metod. Se till att du använder sig av alla alternativ som är tillgängliga för varje scenario som kräver toouse Azure AD-rapporteringsfunktion som en del av ditt företags incidenter.

## <a name="next-steps"></a>Nästa steg
[Fastställa hanteringsuppgifter för hybrid identity](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)
