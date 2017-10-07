---
title: aaaSecuring PaaS-distributioner | Microsoft Docs
description: " Förstå hello fördelarna med PaaS jämfört med andra molntjänster tjänstmodeller och Läs rekommenderade metoder för att säkra din Azure PaaS-distribution. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: techlake
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: terrylan
ms.openlocfilehash: b94fe03b6f3a43d82e1e6e834f10a423e4c1db95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-deployments"></a>Att säkra PaaS-distributioner

Den här artikeln innehåller information som hjälper dig att:

- Förstå hello fördelarna med värd för program i hello moln
- Utvärdera hello fördelarna med plattform som en tjänst (PaaS) jämfört med andra molntjänster tjänstmodeller
- Växla säkerhet från en strategi för nätverk till Central tooan identitet till Central perimeter säkerhet
- Implementera Allmänt PaaS säkerhetsrekommendationer om bästa praxis

## <a name="cloud-security-advantages"></a>Fördelarna med molnet
Det finns säkerhet fördelar toobeing i hello molnet. I en lokal miljö organisationer troligen har unmet ansvar och begränsade resurser tillgängliga tooinvest i säkerhet, vilket skapar en miljö där angripare är kan tooexploit säkerhetsproblem i alla skikt.

![Fördelarna med molnet era][1]

Organisationer är kan tooimprove sina hotidentifiering och svarstider med hjälp av en provider molnbaserade säkerhetsfunktioner och moln för tillgångsinformation.  Genom att ändra ansvarsområden toohello molntjänstleverantör kan organisationer få mer säkerhet täckning, vilket gör dem tooreallocate säkerhetsresurser och budget tooother business prioriteter.

## <a name="division-of-responsibility"></a>Uppdelning av ansvar
Det är viktigt toounderstand hello divisionen av ansvar mellan dig och Microsoft. Lokal du äger hello hela stack men när du flyttar toohello moln några ansvarsområden överföra tooMicrosoft. hello följande ansvar matris visar hello hello stack-områden i en SaaS-PaaS och IaaS-distribution som du är ansvarig för och Microsoft ansvarar för.

![Ansvar zoner][2]

För alla moln distributionstyper äger du dina data och identiteter. Du ansvarar för att skydda hello säkerheten för dina data och identiteter, lokala resurser och hello moln-komponenter som du kan styra (som varierar beroende på typ av tjänst).

Ansvarsområden som behålls alltid av du oavsett hello typ av distribution är:

- Data
- Slutpunkter
- Konto
- Åtkomsthantering

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Fördelarna med en PaaS molnet tjänstmodell
Hej med samma ansvar matris ska vi titta på hello fördelarna med en Azure PaaS-distribution jämfört med lokalt.

![Fördelarna med PaaS][3]

Starta längst hello hello-stacken, hello fysisk infrastruktur, minskar Microsoft vanliga risker och ansvarsområden. Eftersom hello Microsoft cloud övervakas kontinuerligt av Microsoft, är det svårt tooattack. Den passar inte för en angripare toopursue hello Microsoft cloud som mål. Om hello angripare har många pengar och resurser, hello angripare är sannolikt toomove på tooanother målet.  

Hello mitten av hello-stacken finns det ingen skillnad mellan en PaaS-distribution och lokalt. När hello programnivå och hello-konto och komma åt hanteringslager har liknande risker. I hello nästa steg i den här artikeln kan vägleder vi dig toobest metoder för att eliminera eller minska riskerna.

Överst hello i hello-stacken, datastyrning och rights management ta på en risk att kan begränsas med nyckelhantering. (Key management ingår i metodtips.) Hantering av nycklar är en ytterligare ansvar, har en PaaS-distribution som du inte längre att toomanage så att du kan flytta resurser tookey management områden.

hello Azure-plattformen ger dig också starkt DDoS-skydd genom att använda olika nätverksbaserade tekniker. Men har alla typer av nätverksbaserade DDoS skyddsmetod sina begränsningar på grundval av per länk och per datacenter. toohelp undvika hello påverkan av den stora DDoS-attacker, du kan dra nytta av Azures kärnor av molnkapacitet för att aktivera du tooquickly och automatiskt skala ut toodefend mot DDoS-attacker. Vi ska gå in mer i detalj på hur du kan göra detta i hello rekommenderade metoder för artiklar.

## <a name="modernizing-hello-defenders-mindset"></a>Modernizing hello defender tänkesätt
PaaS medför distributioner en SKIFT i din övergripande metoden toosecurity. Du växlar från behöver toocontrol allt själv toosharing ansvar med Microsoft.

En annan betydande skillnaden mellan PaaS och traditionella lokala distributioner är en ny vy över vad definierar hello säkerhetsinformation perimeternätverk. Tidigare hello primär lokal säkerhet perimeter var ditt nätverk och mest lokalt säkerhet Designer Använd hello nätverk som dess primära säkerhet pivot. För PaaS-distributioner Service du bättre genom att beakta identitet toobe hello säkerhetsinformation perimeternätverk.

## <a name="identity-as-hello-primary-security-perimeter"></a>Identitet som hello säkerhetsinformation perimeternätverk
En av fem grundläggande egenskaper hello av molntjänster är bred nätverksåtkomst, vilket gör nätverket till Central tänker mindre relevant. hello målet för mycket av molntjänster är tooallow användare tooaccess resurser oavsett plats. För de flesta användare kommer deras plats toobe någonstans på hello Internet.

hello följande bild visar hur hello säkerhet perimeternätverk har utvecklats från ett nätverk perimeter tooan identitet perimeternätverk. Säkerheten blir mer eller mindre om försvara nätverket om samtidigt skyddar dina data, samt hantera hello säkerheten för dina appar och användare. hello viktigaste skillnaden är att du vill att toopush säkerhet närmare toowhat's viktiga tooyour företagets.

![Identitet som den nya säkerhet perimeternätverk][4]

Azure PaaS-tjänster (till exempel webbroller och Azure SQL) anges inledningsvis lite eller ingen traditionellt nätverk perimeter försvar. Det gick att tolka att hello elementet syftet var toobe exponeras toohello Internet (webbroll) och att autentisering ger hello nya perimeternätverk (till exempel BLOB eller Azure SQL).

Moderna säkerhetspraxis förutsätter att hello angriparen har utsatts för intrång hello nätverket perimeternätverk. Därför har moderna defense praxis flyttat tooidentity. Organisationer måste upprätta en identitetsbaserade säkerhet perimeternätverket med stark autentisering och auktorisering hygien (bästa metoder).

## <a name="recommendations-for-managing-hello-identity-perimeter"></a>Rekommendationer för hantering av hello identitet perimeternätverk

Principer och mönster för hello nätverket perimeternätverk har funnits i åren. Däremot har hello branschen relativt mindre erfarenhet av att använda identitet som hello säkerhetsinformation perimeternätverk. Med SA som, vi har ackumulerats tillräckligt med upplevelse tooprovide några allmänna rekommendationer beprövade hello fältet och tillämpa tooalmost alla PaaS-tjänster.

hello följande sammanfattas en allmän bästa praxis metoden toomanaging perimeternätverk din identitet.

- **Förlora dina nycklar eller autentiseringsuppgifter** att säkra nycklar och autentiseringsuppgifter är mycket viktigt toosecure PaaS-distributioner. Att förlora nycklar och autentiseringsuppgifter är ett vanligt problem. En bra lösning är toouse en central lösning där nycklar och hemligheter kan lagras i maskinvarusäkerhetsmoduler (HSM). Azure tillhandahåller en HSM i hello moln med [Azure Key Vault](../key-vault/key-vault-whatis.md).
- **Placera inte autentiseringsuppgifter och andra hemligheter i källkoden eller GitHub** hello endast sak värre än att förlora dina nycklar och autentiseringsuppgifter har en obehörig part få åtkomst till toothem. Angripare är kan tootake nytta av bot tekniker toofind nycklar och hemligheter som lagras i koden databaser, till exempel GitHub. Placera inte nyckeln och hemligheter i dessa offentliga källkodslager.
- **Skydda din VM hanteringsgränssnitt på hybrid PaaS- och IaaS** IaaS och PaaS-tjänster körs på virtuella datorer (VM). Beroende på hello typ av tjänst, finns flera hanteringsgränssnitt som gör att du tooremote hantera dessa virtuella datorer direkt. Protokoll för fjärrhantering som [protokollet SSH (Secure Shell)](https://en.wikipedia.org/wiki/Secure_Shell), [Remote Desktop Protocol (RDP)](https://support.microsoft.com/kb/186607), och [fjärr-PowerShell](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) kan användas. I allmänhet rekommenderar vi att du inte aktivera direkt fjärråtkomst tooVMs från hello Internet. Om de är tillgängliga, bör du använda alternativa metoder, till exempel med ett virtuellt privat nätverk till Azure-nätverk. Om alternativa metoder är inte tillgängliga, och sedan kontrollera att du använder komplexa lösenfraser och i förekommande fall, tvåfaktorsautentisering (exempelvis [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)).
- **Använd stark autentisering och auktorisering plattformar**

  - Använd federerade identiteter i Azure AD i stället för anpassad användare lagrar. När du använder federerade identiteter kan du dra nytta av en synsätt, plattform och du delegerar hello hantering av auktoriserade identiteter tooyour partners. En metod för federerad identitet är särskilt viktigt i situationer när anställda avslutas och att information måste toobe speglas genom flera identitet och auktorisering system.
  - Använd mekanismer för autentisering och auktorisering av plattform som anges i stället för anpassad kod. hello orsak är att utveckla anpassade Autentiseringskod kan vara tillförlitligt. De flesta av utvecklarna inte säkerhetsexperter och osannolikt toobe medveten om hello detaljerad och nyansrik och hello senaste utvecklingen inom autentisering och auktorisering. Extern kod (till exempel från Microsoft) är ofta mycket säkerhet ses över.
  - Använda multifaktorautentisering. Multifaktorautentisering är hello aktuell standard för autentisering och auktorisering, eftersom den förhindrar hello säkerhet svagheter med användarnamn och lösenord typer av autentisering. Åtkomst tooboth hello Azure (portal/fjärråtkomst PowerShell) hanteringsgränssnitt och toocustomer mot tjänster bör utformas och konfigurerade toouse [Azure Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md).
  - Använd standard autentiseringsprotokoll, till exempel OAuth2 och Kerberos. Dessa protokoll har omfattande peer granskat och troligtvis implementeras som en del av din plattformsbibliotek för autentisering och auktorisering.

## <a name="next-steps"></a>Nästa steg
I den här artikeln fokuserar vi på fördelarna med en Azure PaaS-distribution. Lär dig sedan rekommenderade metoder för att säkra din PaaS webb- och mobile-lösningar. Vi börjar med Azure App Service, Azure SQL Database och Azure SQL Data Warehouse. Artiklar om rekommenderade metoder för andra Azure-tjänster blir tillgängliga ges länkar i hello följande lista:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Azure SQL Database och Azure SQL Data Warehouse](security-paas-applications-using-sql.md)
- Azure Storage
- Azure REDIS-Cache
- Azure Service Bus
- Web Application brandväggar

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
