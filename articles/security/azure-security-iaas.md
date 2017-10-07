---
title: "aaaSecurity bästa praxis för IaaS arbetsbelastningar i Azure | Microsoft Docs"
description: " hello migrering av arbetsbelastningar tooAzure IaaS ger möjligheter tooreevaluate vårt Designer "
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: barclayn
ms.openlocfilehash: 9cee1ca6effe9561e51dc8b945e7388ffea169b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Rekommenderade säkerhetsmetoder för IaaS-arbetsbelastningar i Azure

Eftersom du startade tänka på att flytta arbetsbelastningar tooAzure infrastruktur som en tjänst (IaaS), insåg du förmodligen att vissa förutsättningar är bekant. Du kanske redan har erfarenhet att skydda virtuella miljöer. När du flyttar tooAzure IaaS kan du tillämpa dina kunskaper i att skydda virtuella miljöer och använda en ny uppsättning alternativ toohelp säker dina tillgångar.

Låt oss börja med säger att vi inte kan förvänta toobring lokala resurser som en tooAzure. hello nya utmaningar och hello nya alternativ gör att en affärsmöjlighet tooreevaluate befintlig deigns, verktyg och processer.

Ditt ansvar för säkerhet baseras på hello typ av tjänst i molnet. hello sammanfattas diagrammet nedan hello balans mellan ansvar för både Microsoft och du:


![Ansvarsområden](./media/azure-security-iaas/sec-cloudstack-new.png)


Diskuterar vi några hello-alternativ som är tillgängliga i Azure som kan hjälpa dig att uppfylla organisationens säkerhetskrav. Tänk på att säkerhetskrav kan variera för olika typer av arbetsbelastningar. Inte ett av tipsen skydda ensamt dina system. Precis som något annat i säkerhet har toochoose hello lämpliga alternativ och se hur hello lösningar kan kompletterar varandra genom att fylla luckor.

## <a name="use-privileged-access-workstations"></a>Använd arbetsstationer med privilegierad åtkomst

Organisationer sig ofta faller toocyberattacks eftersom administratörer utför åtgärder när du använder konton med utökade behörigheter. Vanligtvis det här är inte medvetet göra men eftersom befintliga konfiguration och processer som tillåter den. De flesta av dessa användare förstå hello risken för dessa åtgärder från en konceptuell synvinkel men fortfarande välja toodo dem.

Gör saker som e-post- och bläddra hello Internet verkar vara ofarliga försöker tillräckligt. Men de kan det hända att utökade konton toocompromise av skadliga aktörer som kan använda webbläsarens aktiviteter, särskilt utformad e-post eller andra tekniker toogain åtkomst tooyour enterprise. Vi rekommenderar starkt hello användning av säker hantering av arbetsstationer för att utföra alla aktiviteter i Azure administration, som ett sätt att minska exponeringen tooaccidental kompromettering.

Privilegierad åtkomst arbetsstationer (PAWs) innehåller ett dedikerat operativsystem känsliga--en som är skyddat från Internet-attacker och hotvektorer. Avgränsa dessa känsliga uppgifter och konton från hello daglig användning arbetsstationer och enheter ger starkt skydd från nätfiskeattacker, program och OS säkerhetsrisker, olika personifiering attacker och autentiseringsuppgifter stöld av attacker som tangenttryckning loggning, Pass-the-Hash och Pass-the-Ticket.

Hej PAW metoden är en utökning av hello väletablerade och rekommenderade praxis toouse ett enskilt tilldelade administratörskonto som skiljer sig från ett standardanvändarkonto. En PAW innehåller en pålitlig arbetsstation för de känsliga kontona.

Mer information och implementering vägledning finns [arbetsstationer med privilegierad åtkomst](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations).

## <a name="use-multi-factor-authentication"></a>Använda Multifaktorautentisering

I tidigare hello var ditt nätverk perimeter används toocontrol åtkomst toocorporate data. I molnet första, mobila först världen, identitet är hello kontrollplan: använda toocontrol tooIaaS åtkomsttjänster från valfri enhet. Du också använda den tooget synlighet och insyn i var och hur data som används. Skydda hello digitala identitet Azure användare är hello hörnsten för att skydda dina prenumerationer från identitetsstöld och andra cybercrimes.

En av hello fungerar bäst stegen för att toosecure ett konto är tooenable tvåfaktorsautentisering. Tvåfaktorsautentisering är ett sätt att autentisera med hjälp av något i tillägg tooa lösenord. Det minimeras hello risken för åtkomst av någon som hanterar tooget lösenordet.

[Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) hjälper dig att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning. Den ger stark autentisering med ett antal alternativ för enkel verifiering--telefonsamtal, textmeddelande eller mobilapp. Användare välja hello-metod som de själva föredrar.

hello enklaste sättet toouse Multi-Factor Authentication är hello Microsoft Authenticator-mobilappen som kan användas på mobila enheter som kör Windows, iOS och Android. Med hello senaste versionen av Windows 10 och hello integrering av lokala Active Directory med Azure Active Directory (Azure AD), [Windows Hello för företag](../active-directory/active-directory-azureadjoin-passport-deployment.md) kan användas för sömlös tooAzure för enkel inloggning resurser. I det här fallet används hello Windows 10-enhet som hello tvåfaktorsautentisering för autentisering.

För konton som hanterar din Azure-prenumeration och för konton som kan logga in toovirtual datorer ger med hjälp av Multi-Factor Authentication dig mycket bättre säkerhet än att använda endast ett lösenord. Andra former av tvåfaktorsautentisering fungerar lika bra, men distribuerar dem kan vara komplicerade om de inte redan är i produktion.

hello följande skärmbild visar några av hello tillgängliga alternativ för Multifaktorautentisering i Azure:

![Multi-Factor Authentication-alternativ](./media/azure-security-iaas/mfa-options.png)

## <a name="limit-and-constrain-administrative-access"></a>Begränsa och begränsa administrativ åtkomst

Det är mycket viktigt att skydda hello-konton som kan hantera Azure-prenumerationen. hello röjande av någon av dessa konton negeras hello värdet för alla hello andra åtgärder som att du kan ta tooensure hello sekretess och integritet för dina data. Som nyligen illustreras med hello [Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) läckage av sekretessbelagda uppgifter interna attacker innebära en stor hot toohello övergripande säkerheten för en organisation.

Utvärdera personer för administratörsbehörighet genom följande kriterier liknande toothese:

- Utför de uppgifter som kräver administratörsbehörighet?
- Hur ofta utförs hello uppgifter?
- Finns det en särskild anledning varför hello aktiviteter inte kan utföras av en annan administratör åt?

Dokumentera alla andra kända alternativa metoder toogranting hello behörighet och varför var inte är godkända.

hello användning av just-in-time-administration förhindrar hello onödiga förekomsten av konton med förhöjd behörighet under perioder när de rättigheterna som inte behövs. Konton ha utökade rättigheter för en begränsad tid så att administratörer kan utföra sina uppgifter. Dessa rättigheter tas sedan bort hello slutet av ett SKIFT eller när en aktivitet har slutförts.

Du kan använda [Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md) toomanage, övervaka och kontrollera åtkomst till i din organisation. Det hjälper dig att vara medveten om hello vidta åtgärder som enskilda användare i din organisation. Medför det också just-in-time-administration tooAzure AD genom att introducera hello begreppet berättigade administratörer. Dessa är personer som har konton med hello potentiella toobe beviljat administratörsrättigheter. Dessa typer av användare kan gå igenom ett aktiveringen och beviljas administratörsrättigheter för en begränsad tid.


## <a name="use-devtest-labs"></a>Använd DevTest Labs

Med Azure för labs och utvecklingsmiljöer kan organisationer toogain flexibilitet i testning och utveckling av tar bort hello fördröjningar som introducerar maskinvara inköp. Tyvärr bristande tidigare erfarenhet av Azure eller en önskan toohelp påskynda antagandet leda Hej administratör toobe alltför Tillåtande med tilldelning av användarrättigheter. Den här risken kan oavsiktligt exponera hello organisation toointernal attacker. Vissa användare kan beviljas mycket mer åtkomst än de ska ha.

Hej [Azure DevTest Labs](../devtest-lab/devtest-lab-overview.md) tjänsten använder [rollbaserad åtkomstkontroll i](../active-directory/role-based-access-control-what-is.md) (RBAC). Med RBAC kan särskilja du uppgifter i din grupp i roller som ger endast hello åtkomstnivå som krävs för användare toodo sitt arbete. RBAC innehåller fördefinierade roller (ägare, lab-användare och deltagare). Du kan även använda dessa roller tooassign rättigheter tooexternal tillverkare och förenklar samarbete.

Det är möjligt toocreate extra eftersom DevTest Labs använder RBAC, [anpassade roller](../devtest-lab/devtest-lab-grant-user-permissions-to-specific-lab-policies.md). DevTest Labs förenklar inte bara hello hanteringen av behörigheter, det förenklar hello processen för att få miljöer som har etablerats. Du kan också hantera andra vanliga utmaningar team som jobbar på utvecklings- och testmiljöer. Det krävs vissa förberedelser, men hello lång sikt, det kan göra det enklare för din grupp.

Azure DevTest Labs funktionerna inkluderar:

- Administrativ kontroll över hello alternativ tillgängliga toousers. Hej administratör hantera centralt t.ex. tillåtna storlekar på VM, maximalt antal virtuella datorer och virtuella datorer startas och stängs av.
- Automatisering labb miljö skapas.
- Kostnadsuppföljning.
- Förenklad distribution av virtuella datorer för tillfälliga samarbete.
- Självbetjäning som gör att användare tooprovision sina övningar med hjälp av mallar.
- Hantera och begränsa förbrukning.

![Med hjälp av DevTest Labs toocreate ett labb](./media/azure-security-iaas/devtestlabs.png)

Utan extra kostnad är associerad med hello användning av DevTest Labs. hello skapa labs, principer, mallar och artefakter är kostnadsfri. Du betalar endast hello Azure-resurser används i din labs, till exempel virtuella datorer, lagringskonton och virtuella nätverk.



## <a name="control-and-limit-endpoint-access"></a>Kontroll- och limit endpoint-åtkomst

Värd för labs eller produktionssystem i Azure innebär att dina system måste toobe tillgänglig från hello Internet. Som standard en ny Windows virtuell dator har hello RDP-porten som är tillgänglig från hello Internet och en virtuell Linux-dator har hello SSH-port öppna. Vidtar åtgärder toolimit exponeras slutpunkter är nödvändiga toominimize hello risken för obehörig åtkomst.

Med hjälp av tekniker i Azure kan du begränsa hello åtkomst toothose administrativa slutpunkter. Du kan använda i Azure, [nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md) (NSG: er). När du använder Azure Resource Manager för distribution, begränsa NSG: er hello åtkomst från alla nätverk toojust hello management slutpunkter (RDP eller SSH). När du funderar NSG: er tror router ACL: er. Du kan använda dem tootightly kontroll hello nätverkskommunikationen mellan olika segment av dina Azure-nätverk. Detta är liknande toocreating nätverk i perimeternätverk eller andra isolerade nätverk. De inspektera inte hello trafik, men de att med nätverkssegmentering.


I Azure, kan du konfigurera en [plats-till-plats VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) från ditt lokala nätverk. En plats-till-plats-VPN utökar dina lokala nätverk toohello moln. Detta ger dig en annan affärsmöjlighet toouse NSG: er, eftersom du kan också ändra hello NSG: er toonot tillåter åtkomst varifrån som helst andra hello lokala nätverket. Sedan kan du kräva att administreras av första anslutande toohello Azure-nätverk via VPN.

hello kan plats-till-plats VPN vara mest bra i fall där du är värd för produktionssystem som är nära integrerad med din lokala resurser i Azure.

Du kan också använda hello [punkt-till-plats](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) alternativet i situationer där du vill att toomanage system som inte behöver komma åt tooon lokala resurser. Dessa system kan isoleras i sina egna virtuella Azure-nätverket. Administratörer kan VPN i hello Azure värdbaserade miljön från sina administrativa arbetsstation.

>[!NOTE]
>Du kan använda antingen VPN-alternativet tooreconfigure hello ACL: er på hello NSG: er toonot Tillåt åtkomst toomanagement slutpunkter från hello Internet.

Ett annat alternativ vara värt att ta hänsyn till är en [fjärrskrivbordsgateway](../multi-factor-authentication/multi-factor-authentication-get-started-server-rdg.md) distribution. Du kan använda den här distributionen toosecurely ansluta tooRemote skrivbord servrar via HTTPS, medan tillämpa mer detaljerad kontrollerar toothose anslutningar.

Funktioner som du skulle ha åtkomst till tooinclude:

- Administratören alternativ toolimit anslutningar toorequests från specifika system.
- Autentisering med smartkort eller Azure Multi-Factor Authentication.
- Kontroll över vilka system någon ansluta toovia hello gateway.
- Kontroll över enheten och disk-omdirigering.

## <a name="use-a-key-management-solution"></a>Använda en lösning för hantering av nycklar

Säker nyckelhantering är mycket viktigt tooprotecting data i hello molnet. Med [Azure Key Vault](../key-vault/key-vault-whatis.md), på ett säkert sätt kan du lagra krypteringsnycklar och hemligheter små som lösenord i maskinvarusäkerhetsmoduler (HSM). För ytterligare säkerhet kan du importera eller generera nycklar i HSM-moduler.

Microsoft processer dina nycklar i FIPS 140-2 Level 2 verifierade HSM (maskinvara och inbyggd programvara). Övervaka och granska nyckelanvändning med Azure loggning: skicka loggar till Azure eller Security Information and Event Management SIEM ()-system för ytterligare analys och hotidentifiering identifiering.

Vem som helst med en Azure-prenumeration kan skapa och använda nyckelvalv. Även om Key Vault hjälper utvecklare och säkerhetsadministratörer, kan de implementeras och hanteras av en administratör som ansvarar för att hantera Azure-tjänster i en organisation.


## <a name="encrypt-virtual-disks-and-disk-storage"></a>Kryptera virtuella diskar och disklagring

[Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) adresser hello hot om datastöld eller exponering från obehörig åtkomst uppnås genom att flytta en disk. hello disken kan vara anslutna tooanother system som ett sätt att kringgå andra säkerhetskontroller. Disken använder kryptering [BitLocker](https://technet.microsoft.com/library/hh831713) i Windows och DM-Crypt i Linux tooencrypt operativsystem och dataenheter. Azure Disk Encryption kan integreras med Key Vault toocontrol och hantera hello krypteringsnycklar. Den är tillgänglig för standard virtuella datorer och virtuella datorer med premium-lagring.

Mer information finns i [Azure Disk Encryption i Windows och Linux IaaS-VM](azure-security-disk-encryption.md).

[Azure Storage Service-kryptering](../storage/common/storage-service-encryption.md) hjälper dig att skydda dina data i vila. Det är aktiverat på hello lagringsnivå för kontot. Den krypterar data som skrivs i våra datacenter och dekrypteras automatiskt när du öppnar den. Den stöder hello följande scenarier:

- Kryptering av blockblobbar, tilläggsblobbar och sidblobbar
- Kryptering av arkiverade virtuella hårddiskar och mallar online tooAzure från lokala
- Kryptering av underliggande Operativsystemet och datadiskarna för IaaS-VM som du skapat med hjälp av de virtuella hårddiskarna

Innan du fortsätter med Azure Storage kryptering ska du vara medveten om två begränsningar:

- Det är inte tillgänglig på klassiska lagringskonton.
- Endast de data som skrivs när kryptering är aktiverat krypteras.

## <a name="use-a-centralized-security-management-system"></a>Använd ett hanteringssystem för centraliserad säkerhet

Servrarna måste toobe övervakas för korrigering, konfiguration, händelser och aktiviteter som anses vara säkerhetsfrågor. tooaddress de gäller, kan du använda [Security Center](https://azure.microsoft.com/services/security-center/) och [Operations Management Suite säkerhet och efterlevnad](https://azure.microsoft.com/services/security-center/). Båda dessa alternativ utöver hello konfigurationen i hello-operativsystemet. De ger också övervakning av hello konfigurationen av hello underliggande infrastruktur som nätverkskonfigurationen och Använd virtuell installation.

## <a name="manage-operating-systems"></a>Hantera operativsystem

I en IaaS-distribution är fortfarande ansvarar för hello hello system som du distribuerar, precis som alla andra servrar eller arbetsstationer i din miljö. Korrigering, härdning, relaterade tilldelning av användarrättigheter och andra aktiviteter toohello underhålla systemet fortfarande är ditt ansvar. För system som är nära integrerad med din lokala resurser måste du kanske vill toouse hello samma verktyg och procedurer som du använder lokalt för sådant som antivirus program mot skadlig kod, uppdatering och säkerhetskopiering.

### <a name="harden-systems"></a>Skydda system
Alla virtuella datorer i Azure IaaS bör vara härdat så att de exponerar endast slutpunkter som krävs för hello-program som är installerade. För Windows-datorer, följ hello rekommendationer som Microsoft publicerar som baslinjer för hello [Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) lösning.

Security Compliance Manager är ett kostnadsfritt verktyg. Du kan använda den tooquickly konfigurera och hantera stationära datorer, traditionella datacenter och privata och offentliga moln med hjälp av Grupprincip och System Center Configuration Manager.

Security Compliance Manager innehåller redo att distribuera principer och Desired Configuration Management configuration packs som testas. Dessa baslinjer baseras på [Microsoft säkerhetsvägledning](https://technet.microsoft.com/en-us/library/cc184906.aspx) rekommendationer och industrin bästa praxis. De hjälper dig att hantera konfigurationsavvikelser, adress efterlevnadskrav och minska säkerhetshot.

Du kan använda Security Compliance Manager tooimport hello aktuella konfigurationen av datorerna med hjälp av två olika metoder. Du kan först importera Active Directory-baserade grupprinciper. Därefter kan du importera hello konfigurationen av en ”golden master” referensdatorn med hjälp av hello [LocalGPO verktyget](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) tooback in hello lokal grupprincip. Du kan sedan importera hello lokal grupprincip i Security Compliance Manager.

Jämför din standarder tooindustry bästa praxis, anpassa dem och skapa nya principer och Desired Configuration Management-konfigurationspaket. Baslinjer har publicerats för alla operativsystem som stöds, inklusive Windows 10 årsdagar Update och Windows Server 2016.


### <a name="install-and-manage-antimalware"></a>Installera och hantera program mot skadlig kod

För miljöer som hanteras separat från produktionsmiljön, kan du använda ett program mot skadlig kod tillägget toohelp skydda dina virtuella datorer och molntjänster. Den kan integreras med [Azure Security Center](../security-center/security-center-intro.md).


[Microsoft Antimalware](azure-security-antimalware.md) innehåller funktioner som realtidsskydd, schemalagd genomsökning, åtgärder mot skadlig kod, Signaturuppdateringar, uppdateringar, exempel reporting undantag händelseinsamling och [stöd för PowerShell](https://msdn.microsoft.com/library/dn771715.aspx).

![Azure program mot skadlig kod](./media/azure-security-iaas/azantimalware.png)

### <a name="install-hello-latest-security-updates"></a>Installera hello senaste säkerhetsuppdateringarna
Hello första arbetsbelastningarna att kunder flytta tooAzure är labs och externa system. Om dina Azure-värdbaserade virtuella datorer värd för program eller tjänster som behöver toobe tillgänglig toohello Internet måste vara vaksam om korrigering. Korrigera utöver hello-operativsystemet. Okorrigerade säkerhetsproblem på program från tredje part kan det leda tooproblems kan undvikas om bra uppdateringshantering är på plats.

### <a name="deploy-and-test-a-backup-solution"></a>Distribuera och testa en lösning för säkerhetskopiering

Precis som säkerhetsuppdateringar, måste en säkerhetskopia toobe hanteras hello samma sätt som du hanterar alla andra åtgärder. Detta gäller för system som ingår i din produktionsmiljö utöka toohello moln. Testning och utveckling system måste följa säkerhetskopieringsstrategier som tillhandahåller funktioner som är liknande toowhat användare är vana, utifrån sina erfarenheter lokala miljöer.

Produktionsarbetsbelastningar flyttas tooAzure ska integrera med befintliga säkerhetskopiering när det är möjligt. Du kan också använda [Azure Backup](../backup/backup-azure-arm-vms.md) toohelp adressera dina behov för säkerhetskopiering.


## <a name="monitor"></a>Övervaka

[Security Center](../security-center/security-center-intro.md) ger pågående utvärdering av hello säkerhetstillståndet hos dina Azure-resurser tooidentify potentiella säkerhetsproblem. En lista över rekommendationer hjälper dig att hello konfigureringen av nödvändiga kontroller.

Exempel:

- Etablera program mot skadlig kod toohelp identifiera och ta bort skadlig programvara.
- Konfigurera säkerhet grupper och regler toocontrol trafik toovirtual datorer.
- Etablering web application brandväggar toohelp skydd mot angrepp riktade webbprogram.
- Systemuppdateringar som fattas.
- OS-konfigurationer som inte matchar hello-adressering rekommenderade baslinjer.

hello följande bild visar några av hello-alternativ som du kan aktivera i Security Center.

![Principer för Azure Security Center](./media/azure-security-iaas/security-center-policies.png)

[Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) är en Microsoft molnbaserade IT lösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur. Eftersom Operations Management Suite implementeras som en molnbaserad tjänst kan du distribuera den snabbt och med minimal investeringar i infrastrukturresurser.

Nya funktioner som levereras automatiskt, vilket sparar dig från löpande underhållet och uppgradera kostnader. Operations Management Suite är integrerat med System Center Operations Manager. Det har olika komponenter toohelp du hantera dina Azure arbetsbelastningar, inklusive en [säkerhet och efterlevnad](../operations-management-suite/oms-security-getting-started.md) modul.

Du kan använda hello funktioner för säkerhet och efterlevnad i Operations Management Suite tooview information om dina resurser. hello information är uppdelade i fyra huvudsakliga kategorier:

- **Säkerhetsdomäner**: Utforska ytterligare säkerhetsposter över tid. Kontroll av skadlig kod kan uppdatera assessment, information om nätverk, identitet och åtkomst och datorer med säkerhetshändelser. Dra nytta av instrumentpanelen för Snabbåtkomst toohello Azure Security Center.
- **Anmärkningsvärda problem**: snabbt identifiera hello antal aktiva problem och hello allvarlighetsgrad för de här problemen.
- **Identifieringar (förhandsgranskning)**: identifiera attack mönster av visualisera säkerhetsaviseringar när de görs mot dina resurser.
- **Hot intelligence**: identifiera attack mönster av visualisera hello Totalt antal servrar med utgående skadlig IP-trafik hello skadliga hot typ och en karta som visar var dessa IP-adresser kommer från.
- **Vanliga säkerhetsfrågor**: se en lista över de vanligaste hello-säkerhet frågor som du kan använda toomonitor din miljö. När du klickar på en av frågorna hello **Sök** blad öppnas och visar hello resultat för frågan.

hello visar följande skärmbild ett exempel på hello information som kan visa Operations Management Suite.

![Operations Management Suite baslinjer för säkerhet](./media/azure-security-iaas/oms-security-baseline.png)



## <a name="next-steps"></a>Nästa steg


* [Azure-säkerhetsteamets blogg](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)
* [Azure säkerhetsmetoder och mönster](security-best-practices-and-patterns.md)
