---
title: aaaIsolation i hello Azure offentligt moln | Microsoft Docs
description: "Läs mer om molnbaserad databearbetning tjänster som omfattar ett brett urval av compute-instanser och tjänster som kan skalas upp-och automatiskt toomeet hello behov program-eller enterprise."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: 271e5f0d00abcfd404ce6c50cfb7d1ac26360c90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="isolation-in-hello-azure-public-cloud"></a>Isolera i hello Azure offentligt moln
##  <a name="introduction"></a>Introduktion
### <a name="overview"></a>Översikt
tooassist aktuella och framtida Azure-kunder att förstå och använda hello olika säkerhetsrelaterade funktioner tillgängliga i och omgivande hello Azure-plattformen, Microsoft har utvecklat en serie faktablad, säkerhet översikter, bästa praxis och Checklistor.
Hej avsnitt angivet bredd och djup och uppdateras regelbundet. Det här dokumentet är en del av den serien som sammanfattas i hello abstrakt avsnittet efter.

### <a name="azure-platform"></a>Azure-plattformen
Azure är en öppen och flexibla molntjänstplattform som stöder hello stort urval av operativsystem, programmeringsspråk, ramverk, verktyg, databaser, och enheter. Du kan till exempel:
- Kör Linux behållare med Docker-integration.
- Skapa appar med JavaScript, Python, .NET, PHP, Java och Node.js; och
- Build-servrar för iOS, Android och Windows enheter.

Microsoft Azure stöder hello samma tekniker miljoner utvecklare och IT-proffs redan förlitar sig på och litar på.

När du bygger på eller migrera IT tillgångar till en offentlig molntjänstleverantör, måste den organisationens förmåga tooprotect dina program och data med hello tjänster och hello kontroller ger toomanage hello säkerheten för din molnbaserade tillgångar.

Azures infrastruktur har utformats från hello anläggning tooapplications som värd för miljontals kunder samtidigt och det ger en säker grund som företag uppfyller deras behov. Dessutom Azure ger dig en mängd olika konfigurerbara säkerhet alternativ och hello möjlighet toocontrol dem så att du kan anpassa toomeet hello unika säkerhetskrav för dina distributioner. Det här dokumentet hjälper dig att möta dessa krav.

### <a name="abstract"></a>Abstrakt

Microsoft Azure kan du toorun program och virtuella maskiner (VMs) på delade fysiska infrastrukturen. Ett av hello särskilda ekonomiska motiveringen toorunning program i en molnmiljö är hello möjlighet toodistribute hello kostnaden för delade resurser mellan flera kunder. Det här tillvägagångssättet för flera innehavare ökar effektiviteten genom multiplexering resurser mellan olika kunder låg kostnad. Tyvärr introducerar även hello risken för delning av fysiska servrar och andra infrastruktur resurser toorun känsliga program och virtuella datorer som kan höra tooan skadlig och potentiellt skadliga användare.

Den här artikeln beskrivs hur Microsoft Azure tillhandahåller isolering mot skadliga såväl som icke-skadliga användare och fungerar som en guide för att bygga om molnlösningar genom att erbjuda olika isolering val tooarchitects. I det här dokumentet fokuserar på hello teknik av Azure-plattformen och kunden vända säkerhetskontroller och försöker inte tooaddress SLA: er priser modeller och DevOps överväganden.

## <a name="tenant-level-isolation"></a>Nivån Klientisolering
En av hello primära fördelarna med cloud computing är konceptet för en delad infrastruktur som är vanliga i ett stort antal kunder samtidigt, inledande tooeconomies skalan. Detta kallas för flera innehavare. Microsoft arbetar ständigt tooensure att hello arkitektur med flera innehavare för Microsoft Cloud Azure stöder standarder för säkerhet, sekretess, sekretess, integritet och tillgänglighet.

En klient kan definieras som en klient eller organisation som äger och hanterar en specifik instans av Molntjänsten i hello moln-aktiverat arbetsplats. Med hello identitetsplattformen som tillhandahålls av Microsoft Azure, är en klient helt enkelt en dedikerad instans av Azure Active Directory (AD Azure) som din organisation tilldelas och äger när den registrerar sig för en Microsoft-molntjänst.

Varje Azure AD-katalog är separat och åtskild från andra Azure AD-kataloger. Precis som ett företags kontorsbyggnad är en säker resurs specifika tooonly din organisation, Azure AD-katalog har också utformad toobe en säker tillgång för exklusiv användning av din organisation. hello Azure AD-arkitekturen isolerar kunden kunddata och identitetsinformation. Det innebär att användare och administratörer av en Azure AD-katalog inte oavsiktligt eller illvilligt kan komma åt data i en annan katalog.

### <a name="azure-tenancy"></a>Azure innehavare
Azure innehavare (Azure-prenumeration) refererar tooa ”kund/billing” relationen och ett unikt [klient](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) i [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). Nivån klientisolering i Microsoft Azure uppnås med hjälp av Azure Active Directory och [rollbaserad kontroller](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) erbjuds av den. Varje Azure-prenumeration är associerad med en katalog i Azure Active Directory (AD).

Användare, grupper och program från katalogen kan hantera resurser i hello Azure-prenumeration. Du kan tilldela behörigheter med hello Azure-portalen, Azure kommandoradsverktyg och Azure Management-API: er. En Azure AD-klient är logiskt isolerade med säkerhetsgränser så att ingen kund kan komma åt eller kompromettera samtidigt hyresgäster medvetet eller av misstag. Azure AD som körs på ”bare metal”-servrar isolerade i ett separat nätverkssegment där värdnivå paketfilter och Windows-brandväggen blockerar oönskade anslutningar och trafik.

- Åtkomst toodata i Azure AD kräver användarautentisering via en [säkerhetstokentjänst (STS)](https://docs.microsoft.com/azure/app-service-web/web-sites-authentication-authorization). Information om hello användarens existens aktiverat läge och rollen används av hello auktorisering system toodetermine om hello begärda åtkomsten toohello mål klienten har behörighet för den här användaren i den här sessionen.

![Azure innehavare](./media/azure-isolation/azure-isolation-fig1.png)


- Klienter är diskreta behållare och det finns ingen relation mellan dessa.

- Ingen åtkomst över innehavare om innehavaradministration beviljar via federation eller etablering användarkonton från andra klienter.

- Fysisk åtkomst tooservers som utgör hello Azure AD-tjänsten och direktåtkomst tooAzure Annonsens backend-system är begränsad.

- Azure AD-användare har ingen åtkomst till toophysical tillgångar eller platser och är därför inte möjligt för dem toobypass hello logiska RBAC principkontroller anges efter.

För diagnostik- och behov av underhåll, krävs en funktionsmodell som använder en just-in-time-privilegium höjning system och användas. Azure AD Privileged Identity Management (PIM) introducerar hello begreppet en berättigad-administratör. [Berättigad administratörer](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) ska vara användare som behöver privilegierad åtkomst till då och då, men inte varje dag. hello-rollen är inaktiv tills hello användare behöver åtkomst, och sedan de slutföra aktiveringsprocessen och bli administratör active för en förinställd tidsperiod.

![Azure AD Privileged Identity Management](./media/azure-isolation/azure-isolation-fig2.png)

Azure Active Directory är värd för varje innehavare i sin egen skyddade behållaren med principer och behörigheter tooand i hello behållare enbart ägs och hanteras av hello-klient.

hello begreppet klient behållare är djupt ingrained i hello katalogtjänsten på alla skikt, från portaler alla hello sätt toopersistent lagring.

Även om metadata från flera Azure Active Directory-klienter som är lagrad på hello samma fysiska disk, det finns ingen relation mellan hello behållare än vad som definieras av hello directory-tjänsten, som i sin tur beror hello Innehavaradministratör.

### <a name="azure-role-based-access-control-rbac"></a>Azure rollbaserad åtkomstkontroll (RBAC)
[Azure rollbaserad åtkomstkontroll (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) hjälper du tooshare olika komponenter som är tillgängliga i en Azure-prenumeration genom att tillhandahålla detaljerad åtkomsthantering för Azure. Azure RBAC kan du toosegregate uppgifter i din organisation och bevilja åtkomst baserat på vad användarna behöver tooperform sitt arbete. Istället för att ge alla obegränsad behörighet i Azure-prenumeration eller resurser, kan du tillåta endast vissa åtgärder.

Azure RBAC har tre grundläggande roller som gäller tooall resurstyper:

- **Ägare** har fullständig åtkomst tooall resurser inklusive hello rätt toodelegate åtkomst tooothers.

- **Deltagare** kan skapa och hantera alla typer av Azure-resurser, men det går inte att bevilja åtkomst tooothers.

- **Läsaren** kan visa befintliga Azure-resurser.

![Rollbaserad åtkomstkontroll i Azure](./media/azure-isolation/azure-isolation-fig3.png)

hello resten av hello RBAC-roller i Azure kan hanteringen av specifika Azure-resurser. Hello virtuella deltagarrollen tillåter hello användaren toocreate och hantera virtuella datorer. Det ger dem åtkomst toohello Azure Virtual Network eller hello undernät som hello virtuella datorn ansluter till.

[Inbyggda RBAC-roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) listan hello-roller som är tillgängliga i Azure. Det anger hello åtgärder och att varje inbyggda rollen ger toousers omfång. Om du tittar toodefine egna roller för ännu mer kontroll, se hur toobuild [anpassade roller i Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

Vissa funktioner för Azure Active Directory är:
- Azure AD kan SSO tooSaaS program, oavsett var de finns. Vissa program federeras med Azure AD och andra använder enkel inloggning med lösenord. Federerade program kan också använda användaretablering och [lösenordsvalv](https://www.techopedia.com/definition/31415/password-vault).

- Åtkomst toodata i [Azure Storage](https://azure.microsoft.com/services/storage/) styrs via autentisering. Varje lagringskonto har en primär nyckel ([lagringskontonyckel](https://docs.microsoft.com/azure/storage/storage-create-storage-account), eller SAK) och en sekundär hemlig nyckel (signatur för delad åtkomst i hello eller SAS).

- Azure AD innehåller identitet som en tjänst via federation med hjälp av [Active Directory Federation Services](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs), synkronisering och replikering med lokala kataloger.

- [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) är hello Multi-Factor authentication-tjänsten som kräver att användare tooverify inloggningar med hjälp av en mobilapp, telefonsamtal eller SMS. Den kan användas med Azure AD toohelp säker lokala resurser med hello Azure Multi-Factor Authentication-servern, och anpassade program och genom att ange hello SDK.

- [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kan du ansluta virtuella datorer i Azure tooan Active Directory-domän utan att distribuera domänkontrollanter. Du kan logga in toothese virtuella datorer med företagets Active Directory-autentiseringsuppgifter och administrera domänanslutna virtuella datorer med hjälp av Grupprincip tooenforce baslinjer för säkerhet på alla dina virtuella Azure-datorer.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) ger en hög tillgänglighet globala identity management-tjänst för konsumentinriktade program som kan skalas toohundreds miljoner identiteter. Den kan integreras över mobila plattformar och webbaserade plattformar. Dina användare kan logga in tooall programmen via anpassningsbara upplevelser genom att använda sina befintliga sociala konton eller genom att skapa autentiseringsuppgifter.

### <a name="isolation-from-microsoft-administrators--data-deletion"></a>Isolering från Microsoft administratörer & Databorttagning
Microsoft tar starkt åtgärder tooprotect informationen från oönskad åtkomst eller används av obehöriga. Dessa operativa processer och kontroller backas upp av hello [Online Services-villkoren](http://aka.ms/Online-Services-Terms), som erbjuder avtal åtaganden som styr åtkomst till tooyour data.

-   Microsoft-tekniker behöver inte standarddata åtkomst tooyour i hello molnet. I stället ges de tillgång under hantering av tillsyn, bara när det behövs. Att åtkomsten är noggrant kontrolleras loggas och återkallas när den inte längre behövs.

-   Microsoft kan anlita andra företag tooprovide begränsade tjänster i dess ställe. Underleverantörer kan komma åt data endast toodeliver hello tjänster för som vi har anlitat dem tooprovide och de är förbjudna att använda informationen i något annat syfte. Dessutom är de enligt avtal bundna toomaintain hello sekretess för våra kunder information.

Företagstjänster med granskad certifieringar granska t.ex. ISO/IEC 27001 regelbundet verifieras av Microsoft och ackrediterade företag, som utför exempel granskningar tooattest den åtkomsten endast för legitima affärsändamål. Du kan alltid komma åt dina egna kunddata när som helst och av någon anledning.

Om du tar bort alla data som Microsoft Azure tar du bort hello data, inklusive eventuella cachelagrade eller säkerhetskopiering kopior. Borttagningen sker för omfattas tjänster inom 90 dagar efter hello slutet av hello kvarhållningsperiod. (I omfånget tjänster definieras under hello databearbetning villkoren i vår [Online Services-villkoren](http://aka.ms/Online-Services-Terms).)

Om en diskenhet som används för lagring av drabbas av ett maskinvarufel, är det på ett säkert sätt [raderas eller förstörs](https://www.microsoft.com/trustcenter/Privacy/You-own-your-data) innan Microsoft returnerar den toohello tillverkare för ersättning eller reparation. hello data på hello enhet skrivs över tooensure som hello data kan inte återställas på något sätt.

## <a name="compute-isolation"></a>Compute-isolering
Microsoft Azure tillhandahåller olika molnbaserade databehandling tjänster som omfattar ett brett urval av compute-instanser och tjänster som kan skalas upp-och automatiskt toomeet hello behov program-eller enterprise. Dessa beräknings-instans och tjänsten erbjuder isolering på flera nivåer toosecure data utan att kompromissa hello flexibilitet i konfigurationen som kunder kräver.

### <a name="hyper-v--root-os-isolation-between-root-vm--guest-vms"></a>Hyper-V & rot OS-isolering mellan rot VM & gäst-VM
Azures beräkningsplattform baseras på datorn virtualisering, vilket innebär att alla kunden koden körs i en virtuell dator för Hyper-V. Det finns en Hypervisor som körs direkt via hello maskinvara och delar en nod i variabelnummer av gäst virtuella maskiner (VMs) på varje nod i Azure (eller slutpunkter i nätverket).


![Hyper-V & rot OS-isolering mellan rot VM & gäst-VM](./media/azure-isolation/azure-isolation-fig4.jpg)


Varje nod har också en särskild rot VM, som körs hello värd-OS. En kritisk gräns är hello isolering av hello rot VM från hello gäst virtuella datorer och hello gäst virtuella datorer från varandra, hanteras av hello hypervisor och hello rot OS. länkning av hello hypervisor/root OS utnyttjar Microsofts åren av operativsystemet säkerhet erfarenhet och nyare learning från Microsoft Hyper-V, tooprovide stark isolering av gäst-VM.

hello Azure-plattformen använder en virtualiserad miljö. Användarinstanser fungerar som fristående virtuella datorer som inte har åtkomst till tooa fysiska värdserver och denna isolering tillämpas med behörighetsnivåer för fysisk processor (ring-0/ring-3).

Ring 0 är hello mest Privilegierade och 3 är hello minst. hello gästoperativsystemet som körs i en mindre privilegierad Ring 1 och program som körs i hello lägsta möjliga Ring 3. Den här virtualisering av fysiska resurser leder tooa klar åtskillnad mellan gäst OS- och hypervisor, vilket resulterar i ökad säkerhet åtskillnad mellan hello två.

hello Azure hypervisor fungerar som en micro kernel och skickar alla maskinvara förfrågningar från gästen virtuella datorer toohello värden för bearbetning med hjälp av ett delat minne gränssnitt som heter VMBus. Detta hindrar användare från att erhålla rådata Läs/Skriv/kör åtkomst toohello system och minskar hello risken för delning av systemresurser.

### <a name="advanced-vm-placement-algorithm--protection-from-side-channel-attacks"></a>Avancerade VM placeringsalgoritmen och skydd mot attacker, kanal på klientsidan
Mellan VM angrepp består av två steg: att placera en VM som styrs av angriparen på hello samma värden som en hello drabbade virtuella datorer och brott mot hello isolering gräns tooeither stjäla känslig drabbade information eller påverka prestandan för greed eller vandalism. Microsoft Azure ger skydd på båda dessa steg med hjälp av en avancerad VM placeringsalgoritmen och skydd mot alla kända sida kanal attacker, inklusive störningar granne virtuella datorer.

### <a name="hello-azure-fabric-controller"></a>hello Azure-Infrastrukturkontrollanten
hello Azure-Infrastrukturkontrollanten ansvarar för att tilldela infrastrukturresurser tootenant arbetsbelastningar och den hanterar enkelriktad kommunikation från hello toovirtual värddatorerna. hello VM att placera algoritmen hello Azure-infrastrukturkontrollanten är mycket avancerade och nästan omöjligt toopredict som fysisk värdnivå.

![hello Azure-Infrastrukturkontrollanten](./media/azure-isolation/azure-isolation-fig5.png)

hello Azure hypervisor tvingar minne och processen åtskillnad mellan virtuella datorer och dirigerar säkert nätverk trafik tooguest OS klienter. Detta eliminerar möjligheten att och sidan kanal attack på VM-nivå.

I Azure, hello rot VM är speciell: ett strikt operativsystem kallas hello rot OS som är värd för en fabric-agent (MI) körs. FAs används i tur toomanage gästagenter (GA) gästen OS på kundens virtuella datorer. FAs också hantera lagringsnoder.

hello Azure hypervisor samling rot OS/MI och kundens virtuella datorer/GAs omfattar en beräkningsnod. FAs hanteras av en fabric-controller (FC) som finns utanför beräkning och lagring noder (beräkning och lagring klustren hanteras av separata FCs). Om en kund uppdaterar sina programkonfigurationsfilen medan den körs, hello FC som kommunicerar med hello mi, som sedan kontaktar GAs, som meddelar hello tillämpningen av hello konfigurationsändring. Ett maskinvarufel hello för händelsen, kommer hello FC automatiskt hitta tillgänglig maskinvara och starta om Hej VM.

![Azure-Infrastrukturkontrollanten](./media/azure-isolation/azure-isolation-fig6.jpg)

Kommunikation från en Infrastrukturkontrollanten tooan agent är enkelriktad. hello agent implementerar en SSL-skyddad tjänst som bara svarar toorequests från hello-styrenhet. Den kan inte initiera anslutningar toohello domänkontrollant eller andra Privilegierade interna noder. hello FC behandlar alla svar som om de vore inte är betrodd.


![Fabric-styrenhet](./media/azure-isolation/azure-isolation-fig7.png)

Isolering sträcker sig från hello rot-VM från gäst-VM och hello gäst-VM från varandra. Compute-noderna är också isolerade från lagringsnoder för bättre skydd.


hello hypervisor och hello värdens operativsystem ger nätverkspaket - filter toohelp garantera att den inte är betrodd virtuella datorer inte kan skapa falska trafik eller ta emot trafik som inte åtgärdas toothem trafiken tooprotected infrastruktur slutpunkter eller skicka och ta emot olämpliga broadcast-trafik.


### <a name="additional-rules-configured-by-fabric-controller-agent-tooisolate-vm"></a>Ytterligare regler som konfigurerats av Fabric-styrenhet Agent tooIsolate VM
Som standard blockeras all trafik när en virtuell dator skapas och hello fabric controller agenten konfigurerar sedan hello paket filtrera tooadd regler och undantag tooallow behörighet trafik.

Det finns två typer av regler som är programmerade:

-   **Datorn konfiguration eller infrastruktur regler:** som standard all kommunikation är blockerad. Det finns undantag tooallow toosend för en virtuell dator och ta emot DHCP och DNS-trafik. Virtuella datorer kan också skicka trafik toohello ”offentliga” internet och skicka trafik tooother virtuella datorer i hello samma virtuella Azure-nätverk och hello OS Aktiveringsservern. hello virtuella datorer lista över tillåtna utgående mål innehåller inte Azure router undernät, hantering av Azure och andra Microsoft-egenskaper.

-   **Konfigurationsfilen för rollen:** detta definierar hello inkommande åtkomstkontrollistor (ACL) baserat på hello klient modell.

### <a name="vlan-isolation"></a>VLAN-isolering
Det finns tre VLAN i varje kluster:

![VLAN-isolering](./media/azure-isolation/azure-isolation-fig8.jpg)


-   hello anslutningar huvudsakliga VLAN – obetrodda kunden noder

-   hello innehåller FC VLAN – betrodda FCs och ge support för system

-   Hej enheten VLAN – innehåller betrott nätverk och andra infrastruktur-enheter

Kommunikation tillåts från hello FC VLAN toohello huvudsakliga VLAN, men kan inte initieras från hello huvudsakliga VLAN toohello FC VLAN. Kommunikationen är också blockeras från hello VLAN toohello Huvudenhet VLAN. Detta säkerställer att även om en nod som kör Kundkod äventyras, kan den angrepp noder på hello FC eller enhet VLAN.

## <a name="storage-isolation"></a>Isolering av lagring
### <a name="logical-isolation-between-compute-and-storage"></a>Logisk isolering mellan beräkning och lagring
Som en del av dess grundläggande design separerar Microsoft Azure VM-baserad beräkning från lagringsplatsen. Den här separationen kan beräkning och lagring tooscale oberoende av varandra, vilket gör det enklare tooprovide multitenans och isolering.

Därför Compute Azure Storage körs på separata maskinvara med ingen network connectivity tooAzure-utom logiskt. [Detta](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf) innebär att när en virtuell disk skapas inte tilldelat diskutrymme för den totala kapaciteten. I stället skapas en tabell som mappar adresser på hello virtuell disk tooareas på hello fysisk disk och tabellen är tom från början. **hello första gången en kund skriver data på hello virtuell disk, allokeras utrymmet på hello fysisk disk och en pekare tooit placeras i hello tabell.**
### <a name="isolation-using-storage-access-control"></a>Åtkomstkontroll för isolering med lagring
**Åtkomstkontroll i Azure Storage** har en enkel modellen för åtkomstkontroll. Varje Azure-prenumeration kan skapa en eller flera Lagringskonton. Varje Lagringskonto har en hemlig nyckel som används toocontrol åtkomst tooall data i detta Lagringskonto.

![Åtkomstkontroll för isolering med lagring](./media/azure-isolation/azure-isolation-fig9.png)

**Dataåtkomst tooAzure lagring (inklusive tabeller)** kan styras via en [SAS (signatur för delad åtkomst)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) token, vilket ger begränsad åtkomst. hello SAS skapas via en fråga mall (URL) som signerats med hello [SAK (Lagringskontonyckel)](https://msdn.microsoft.com/library/azure/ee460785.aspx). Som [signerade URL](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) kan få tooanother processen (som delegerats), vilket kan sedan fylla i hello information om hello frågan och begära hello av hello storage-tjänst. En SAS kan du toogrant tidsbaserade åtkomst tooclients utan att avslöja hello lagringskontots hemlig nyckel.

hello SAS innebär att vi ger en klient begränsad behörighet, tooobjects i vår storage-konto för en angiven tidsperiod och med en angiven uppsättning behörigheter. Vi kan bevilja dessa begränsade behörigheter utan tooshare åtkomstnycklarna för ditt konto.

### <a name="ip-level-storage-isolation"></a>IP-nivån lagring isolering
Du kan fastställa brandväggar och definiera ett intervall med IP-adresser för dina betrodda klienter. Med ett IP-adressintervall, ansluta endast klienter som har en IP-adress inom intervallet hello som definierats för[Azure Storage](https://docs.microsoft.com/azure/storage/storage-security-guide).

IP-storage-data kan skyddas från obehöriga användare via en mekanism för nätverk som har använt tooallocate en dedikerad eller dedikerade tunnel trafik tooIP lagring.

### <a name="encryption"></a>Kryptering
Azure erbjuder följande typer av kryptering tooprotect data:
-   Kryptering under överföring

-   Vilande kryptering

#### <a name="encryption-in-transit"></a>Kryptering under överföring
Kryptering under överföring är en mekanism för att skydda data när de skickas över nätverk. Du kan skydda data med hjälp av med Azure Storage:

-   [Kryptering på transportnivå](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), till exempel HTTPS när du överför data till eller från Azure Storage.

-   [Tråd kryptering](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), till exempel SMB 3.0-kryptering för Azure-filresurser.

-   [Kryptering på klientsidan](https://docs.microsoft.com/azure/storage/storage-security-guide#using-client-side-encryption-to-secure-data-that-you-send-to-storage), tooencrypt hello data innan den överförs till lagring och toodecrypt hello data när det överförs utanför lagring.

#### <a name="encryption-at-rest"></a>Kryptering i vila
I många organisationer [datakryptering i viloläge](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) är ett obligatoriskt steg mot data sekretess-, efterlevnads- och suveränitet. Det finns tre Azure-funktioner som tillhandahåller kryptering av data som är ”i vila”:

-   [Lagringstjänstens kryptering](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-at-rest) kan du toorequest att hello lagringstjänsten automatiskt kryptera data när du skriver den tooAzure lagring.

-   [Kryptering på klientsidan](https://docs.microsoft.com/azure/storage/storage-security-guide#client-side-encryption) ger också hello-funktionen för kryptering i vila.

-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) kan du tooencrypt hello OS diskar och datadiskar som används av en virtuell IaaS-dator.

#### <a name="azure-disk-encryption"></a>Azure Disk Encryption
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) för virtuella datorer (VM) hjälper dig att organisationens säkerhets- och efterlevnadskrav genom att kryptera dina Virtuella diskar (inklusive start- och datadiskar) med nycklar och principer som du styr i [Azure nyckel Valvet](https://azure.microsoft.com/services/key-vault/).

hello diskkryptering lösning för Windows är baserad på [Microsoft BitLocker-diskkryptering](https://technet.microsoft.com/library/cc732774.aspx), och hello Linux-lösning baserad på [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

hello-lösningen stöder hello följande scenarier för virtuella IaaS-datorer när de är aktiverade i Microsoft Azure:
-   Integrering med Azure Key Vault

-   Standardnivån VMs: A, D, DS, G, GS och så vidare, serien IaaS-VM

-   Aktivera kryptering på Windows och Linux virtuella IaaS-datorer

-   Om du inaktiverar kryptering på Operativsystemet och enheter för Windows IaaS-VM

-   Om du inaktiverar kryptering på dataenheter för Linux virtuella IaaS-datorer

-   Aktivera kryptering på virtuella IaaS-datorer som kör windowsklient-OS

-   Aktivera kryptering på volymer med monteringssökvägar

-   Aktivera kryptering på den virtuella Linux-datorer som är konfigurerade med disk-striping (RAID) med hjälp av [mdadm](https://en.wikipedia.org/wiki/Mdadm)

-   Aktivera kryptering på den virtuella Linux-datorer med hjälp av [LVM (logisk volymhanterare)](https://msdn.microsoft.com/library/windows/desktop/bb540532) för datadiskar

-   Aktivera kryptering på virtuella Windows-datorer som har konfigurerats med hjälp av lagringsutrymmen

-   Alla offentliga Azure-regioner som stöds

hello-lösningen stöder inte hello följande scenarier, funktioner och teknik i hello versionen:

-   Grundnivån IaaS-VM

-   Om du inaktiverar kryptering på en OS-enhet för Linux virtuella IaaS-datorer

-   Virtuella IaaS-datorer som skapas med hjälp av hello klassiska Virtuella metod för skapande av

-   Integrering med din lokala nyckelhanteringstjänst

-   Azure Files (delade filsystem), Network File System (NFS), dynamiska volymer och virtuella Windows-datorer som är konfigurerade med programvarubaserad RAID-system

## <a name="sql-azure-database-isolation"></a>SQL Azure Database isolering
SQL-databasen är en relationsdatabastjänst i Microsoft hello molnet baserat på hello marknadsledande Microsoft SQL Server-databasmotorn och kan hantera verksamhetskritiska arbetsbelastningar. SQL-databas erbjuder förutsägbar data isolering på kontot, geography / region och baserade på nätverk – allt detta med nästan obefintlig administration.

### <a name="sql-azure-application-model"></a>SQL Azure programmodell

[Microsoft SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started) databasen är en molnbaserad relationsdatabstjänst bygger på SQL Server-teknik. Det ger en hög tillgänglighet, skalbar, flera innehavare databastjänst hos Microsoft i molnet.

Ett program ur SQL Azure tillhandahåller hello följande hierarki: varje nivå har en-till-många inneslutning av nivåer nedan.

![SQL Azure programmodell](./media/azure-isolation/azure-isolation-fig10.png)

hello-konto och prenumeration är Microsoft Azure-plattformen begrepp tooassociate fakturering och hantering.

Logiska servrar och databaser är SQL Azure-specifika begrepp och hanteras med hjälp av SQL Azure angivna OData- och TSQL-gränssnitt eller via SQL Azure-portalen som integrerat i Azure-portalen.

SQL Azure-servrarna är inte fysiska eller Virtuella instanser, i stället de är samlingar av databaser, delning och hantering av säkerhetsprinciper som lagras i så kallade ”logiska master” databas.

![SQL Azure](./media/azure-isolation/azure-isolation-fig11.png)

Logiska master-databaser är:

-   SQL-inloggningar används tooconnect toohello server

-   Brandväggsregler

Fakturerings- och användning-relaterad information för SQL Azure-databaser från hello samma logiska server inte är garanterat toobe på hello samma fysiska instansen i SQL Azure-klustret i stället program måste ange hello mål databasnamn vid anslutning.

Ur customer skapas en logisk server i en grafisk geo region medan hello faktiska skapandet av hello server sker i en hello kluster på hello region.

### <a name="isolation-through-network-topology"></a>Isolering via nätverkets topologi

När en logisk server skapas och DNS-namnet har registrerats, pekar toohello så kallade ”Gateway” adressen i hello specifika datacenter där hello server placerades hello DNS-namn.

Vi har en samling tillståndslös gateway tjänster bakom hello VIP (virtuell IP-adress). I allmänhet delta gateways när samordning behövs mellan flera datakällor (master-databasen, användardatabas osv.). Gateway-tjänster implementera hello följande:
-   **Proxy för TDS-anslutningen.** Detta inkluderar hitta användardatabas i hello backend-kluster, implementera hello inloggningssekvensen och vidarebefordran hello TDS-paket toohello serverdelen och tillbaka.

-   **Databashantering av.** Detta inkluderar att implementera en samling arbetsflöden toodo CREATE/ALTER/DROP databasåtgärder. hello databasåtgärder kan anropas av kontroll TDS-paket eller explicit OData APIs.

-   CREATE/ALTER/DROP inloggning åtgärder

-   Logisk server hanteringsåtgärder via OData-API

![Isolering via nätverkets topologi](./media/azure-isolation/azure-isolation-fig12.png)

hello nivå bakom hello gateways kallas ”backend”. Detta är där alla hello data lagras på ett sätt som hög tillgänglighet. Varje datadel är nämnda toobelong tooa ”partition” eller ”redundansenhet”, var och en av dem har minst tre repliker. Replikerna lagras och replikeras av SQL Server-motorn och hanteras av en växling vid fel system som ofta kallas tooas ”infrastruktur”.

I allmänhet kommunicerar hello backend-systemet inte utgående tooother system som en säkerhetsåtgärd. Det här är reserverade toohello system i hello frontend (gateway) nivå. hello gateway skiktdatorer har begränsad behörighet för hello backend-datorer toominimize hello risken för angrepp som en mekanism för skydd på djupet.

### <a name="isolation-by-machine-function-and-access"></a>Isolering av datorn funktion och åtkomst
SQL Azure (består av tjänster som körs på en annan dator funktioner. SQL Azure är uppdelat i ”backend” moln databasen och ”klientdels” (Gatewayhantering) miljöer med hello allmän princip trafik endast växla till backend- och inte ut. hello frontend-miljö kan kommunicera toohello utanför värld av andra tjänster och i allmänhet endast har begränsad behörighet i hello serverdel (tillräckligt med toocall hello genvägar den behov tooinvoke).

## <a name="networking-isolation"></a>Isolering av nätverk
Azure-distribution har flera lager för isolering av nätverk. hello visar följande diagram olika lager i Azure tillhandahåller toocustomers isolering av nätverk. Dessa lager är både intern i hello Azure-plattformen och kunddefinierade funktioner. Inkommande från hello Internet, Azure DDoS ger isolering mot storskaliga attacker mot Azure. hello nästa isoleringsskikt är kunddefinierade offentliga IP-adresser (slutpunkter) som används toodetermine vilken trafik kan passera hello cloud service toohello virtuellt nätverk. Interna Azure virtuell nätverksisolering garanterar fullständig isolering från andra nätverk och att trafiken endast flödar via konfigurerade användaren sökvägar och metoder. Dessa sökvägar och metoder är hello nästa lager, där NSG: er, UDR och virtuella nätverksenheter kan vara används toocreate isolering gränser tooprotect hello programdistributioner i hello skyddat nätverk.

![Isolering av nätverk](./media/azure-isolation/azure-isolation-fig13.png)

**Trafik isolering:** A [virtuellt nätverk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) är hello trafik isoleringsgräns på hello Azure-plattformen. Virtuella datorer (VM) i ett virtuellt nätverk kan inte kommunicera direkt tooVMs i ett annat virtuellt nätverk, även om båda virtuella nätverken skapas av hello samma kund. Isolering är en kritisk egenskap som garanterar kundens virtuella datorer och kommunikation förblir privat inom ett virtuellt nätverk.

[Undernät](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#subnets) ger ett ytterligare lager med isolering med i det virtuella nätverket baserat på IP-intervall. IP-adresser i hello virtuella nätverk, du kan dela ett virtuellt nätverk i flera undernät av organisations- och säkerhetsskäl. VM: ar och PaaS roll instanser distribuerade toosubnets (samma eller olika) inom ett VNet kan kommunicera med varandra utan övrig konfiguration. Du kan också konfigurera [nätverkssäkerhetsgrupp (NSG: er)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#network-security-groups-nsg) tooallow eller neka nätverket trafik tooa VM-instans baserat på konfigurerad i åtkomstkontrollistan (ACL) för NSG-regler. NSG:er kan antingen associeras med undernät eller individuella VM-instanser inom det undernätet. När en NSG är associerad med ett undernät, gäller hello ACL-regler tooall hello VM-instanser i det undernätet.

## <a name="next-steps"></a>Nästa steg

- [Isolering av nätverk som alternativ för datorer i Windows Azure-nätverk](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/)

Detta inkluderar hello klassiska frontend och backend-scenario där datorer i ett visst backend-nätverk eller undernätverk kanske bara tillåter vissa klienter eller andra datorer tooconnect tooa speciella slutpunkten baserat på en godkänd lista över IP-adresser.

- [Compute-isolering](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure tillhandahåller en olika molnbaserade databehandling tjänster som omfattar ett brett urval av compute-instanser och tjänster som kan skalas upp-och automatiskt toomeet hello behov program-eller enterprise.

- [Isolering av lagring](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

Microsoft Azure separerar kunden VM-baserad beräkning från lagringsplatsen. Den här separationen kan beräkning och lagring tooscale oberoende av varandra, vilket gör det enklare tooprovide multitenans och isolering. Därför Compute Azure Storage körs på separata maskinvara med ingen network connectivity tooAzure-utom logiskt. Alla förfrågningar kör via HTTP eller HTTPS enligt kundens val.

