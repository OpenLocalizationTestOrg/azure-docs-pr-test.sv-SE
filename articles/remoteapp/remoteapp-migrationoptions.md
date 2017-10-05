---
title: "Alternativ för att migrera utanför Azure RemoteApp | Microsoft Docs"
description: "Läs mer om alternativen för att migrera utanför Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c4e0e5bc-5c13-4487-b1b6-ebf2a5edc1f0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9ab63124e2521ee1922d15c1e388c54d50eb8301
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Alternativ för att migrera utanför Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.


Om du har slutat använda Azure RemoteApp eftersom den [pensionering meddelande](https://go.microsoft.com/fwlink/?linkid=821148) eller eftersom du är klar med utvärderingen, måste du migrera från Azure RemoteApp till en annan app service. Det finns två olika metoder för att migrera: en självhanterad (kallas ofta infrastruktur som en tjänst [IaaS]) distribution eller en helt hanterad (ofta kallade plattform som en tjänst) eller programvara som en tjänst [PaaS/SaaS] erbjudande. 

Självbetjäning IaaS är ett själv distribution som är hanterade drivs och ägs av du distribuerats direkt på virtuella datorer (VM) eller fysiska system. I den andra änden en helt hanterad PaaS/SaaS erbjudande är mer som Azure RemoteApp - partner som tillhandahåller en tjänstnivå ovanpå en lösning för fjärrkommunikation som hanterar operativa och behandlingen när du som kund, göra vissa avbildningen och apphantering.

[Visa Azure RemoteApp-Webbseminarier på migreringsalternativ](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), eller Läs på för mer information (inklusive exempel på olika alternativ som värd).

## <a name="self-managed-iaas-solutions"></a>Fristående (IaaS) lösningar
### <a name="rds-on-iaas"></a>**RDS på IaaS**
Du kan distribuera en intern sessionsbaserade Fjärrskrivbordstjänster (i Windows Server) distribution med RemoteApp eller skrivbord lokalt eller i en värdmiljö (t.ex. på virtuella Azure-datorer). RDS på IaaS-distributioner är bäst för kunder som redan är bekant med och som har befintliga teknisk expertis med RDS-distribution. 

> [!NOTE]
> Du måste volymlicensiering med Software Assurance (SA) för klientåtkomstlicenser för Fjärrskrivbordstjänster använder det här distributionsalternativet.

Distribution av Fjärrskrivbordstjänster på Azure Virtual Machines är enklare än någonsin när du använder distributions- och korrigering mallar (läsa ett [översikt](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) och sedan [hämta dem](https://aka.ms/rdautomation)). Du kan få samma elastiska skalning funktioner med Azure klassisk distribution modellen resurser (inte Resursmodell för Azure-resurser) i Azure RemoteApp med hjälp av den [automatisk skalning skriptet](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), även om det finns fler anpassningar och konfigurationer. När du distribuerar RDS på Azure Virtual Machines support tillhandahålls via [Azure stöder](https://azure.microsoft.com/support/plans/), samma supportpersonal som stöds av du med Azure RemoteApp. Du kan hämta cost beräknar baserat på din användning av befintlig genom att kontakta [Azure-supporten](https://azure.microsoft.com/support/plans/), eller du kan utföra beräkningar själv med hjälp av en snart att vara publicerat kostnaden Kalkylatorn.  Dessutom N-serien virtuella datorer (för tillfället i privat förhandsvisning) kan du lägga till vGPU - höra mer om att lägga till vGPU och hur du [utnyttjar Fjärrskrivbordstjänster förbättringar i Windows Server 2016](https://myignite.microsoft.com/videos/2794) i vår Ignite-sessionen.   

Vi har steg för steg distributionsguider för [Windows Server 2012 R2](http://aka.ms/rdsonazure) och [Windows Server 2016](http://aka.ms/rdsonazure2016) att hjälpa dig med distributionen. Kolla in den [fjärrskrivbord blogg](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) för de senaste nyheterna.

### <a name="citrix-on-iaas"></a>**Citrix på IaaS**
En intern Citrix sessionsbaserade XenApp eller XenDesktop kan vara distribueras lokalt eller i en värdmiljö (som på Azure Virtual Machines). 

Kolla in distributionsguiden stegvisa [Citrix XA 7.6 på Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), mer information. Läs mer om [Citrix på Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), inklusive ett pris Kalkylatorn. Du kan också hitta en [Citrix Kontakta](http://citrix.com/English/contact/index.asp) att diskutera alternativen med.

## <a name="fully-managed-paassaas-offerings"></a>Helt hanterad PaaS/SaaS ()-erbjudanden

### <a name="citrix-xenapp-essentials-released-april-2017"></a>Citrix XenApp Essentials (utgiven April 2017)
Nu tillgängligt på den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials är den nya application virtualization tjänsten kombinerar prestanda och flexibilitet för Citrix molnplattform med enkla, normativ och enkelt att använda Bild av Microsoft Azure RemoteApp. 

Befintliga Azure RemoteApp-kunder kan [registrera dig för en kostnadsfri utvärderingsversion](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).  Obs: Endast Citrix användaren avgift är ledig gäller Azure kostnader för beräkning och lagring

Lära sig mer:
- [Migrera från Azure RemoteApp till Citrix XenApp Essentials](remoteapp-migrate-citrix.md)
- [Citrix och Microsoft](https://www.citrix.com/global-partners/microsoft/remote-app.html)
- [Citrix XenApp Essentials presentation](https://www.youtube.com/watch?v=91Z7CCfQ-9k).  

### <a name="citrix-cloud-xenapp-service-and-xendesktop-service"></a>Citrix XenApp Molntjänsten och XenDesktop Service 

[Citrix XenApp Molntjänsten och XenDesktop Service](https://www.citrix.com/products/citrix-cloud/services.html) är den bästa lösningen för leverans av både appar och stationära datorer, plus avancerad hantering och övervakning av funktioner. 

#### <a name="conexlink-platform-name-mycloudit"></a>Conexlink (plattformsnamnet: MyCloudIT)
[MyCloudIT](https://mycloudit.com) är en automatiseringsplattform för IT-företag att förenkla, optimera och skala migrering och leverans av fjärrskrivbord fjärrprogram och infrastrukturen i Microsoft Azure-molnet. 

Plattformens MyCloudIT minskar tidpunkten för distribution av 95%, Azure kostnaden med 30% och flyttar deras klient hela IT-infrastruktur till molnet i en fråga om några viktiga linjer. Partners kan nu hantera kunder från en global instrumentpanelen, service slutanvändare runtom i världen som aldrig förr och öka intäkterna utan att lägga till ytterligare kostnader eller omfattande Azure utbildning.  

> Primär plats: Dallas, TX, USA
> 
> Åtgärden region: över hela världen
> 
> Samarbeta status: [guld](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)
> 
> Microsoft Cloud-leverantör: Ja
> 
> Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både
> 
> Lösningar för migrering till Azure RemoteApp: Ja, [Läs mer](https://mycloudit.com/remote-app-microsoft/)
> 
> Brian Garoutte, VD för företag-utveckling
> 
> Telefon: 972-218-0741
>   
> E-post:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

### <a name="frame"></a>RAM

IT-organisationer i enterprise och myndigheter, hanterade leverantörer och inledande programleverantörer välja ram för att skapa och hantera sina säker, programvarudefinierade arbetsytor i molnet. Från små och stora företag, ram enkelt otroligt så att användare kan komma åt Windows-program på en webbläsare från valfri enhet. RAM-plattformen innehåller allt administratör behöver för att distribuera program från molnet, till exempel Azure-infrastrukturen och RDS-licenser (när Azure-konto och licenser är valfri). 

Lär dig mer om [ram på Azure](https://www.fra.me/ara). 

> Primär plats: San Mateo, CA, USA
>
> Åtgärden region: över hela världen
>
> Microsoft-Partner: Ja
> 
> Telefon: 1-480-269-4668

### <a name="awingu"></a>Awingu
Awingu innehåller en enkel arbetsyta online-lösning som kör äldre appar, SaaS och dokument från en html5-webbläsare. Därför gör alla program på ett säkert sätt tillgängligt på någon typ av enhet. En mängd op Single-Sign-On-alternativ är tillgänglig för SaaS-tjänster. Också kan olika (moln) filer system vara djupt integrerad på din arbetsyta. Bredvid full mobilitet ger Awingus omfattande online arbetsytan optimala säkerhet med detaljerade kontroller (t.ex. Hämta/överföring), fullständig användning granskning, Multifaktorautentisering (t.ex. Azure MFA), session inspelning och mycket mer. Out-of-the-box, Awingu kan dokumentet och programdelning sessionen för optimerad och säkert samarbete.
Awingus lösning är flera innehavare, flera AD och öppna API. Den används av små och stora företag och leverantörer av molntjänster och [ISV: er](http://www.isv2saas.com). Dessa kunder uppskattar särskilt enkelt för användning, enkel att installera och TCO.

Allt-i-ett-Awingu är [tillgängliga i Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) med 2 inbyggda samtidiga användare. Ytterligare licenser är tillgängliga via en [mängd distributörer och återförsäljare](http://www.awingu.com/reseller).

Lär dig mer om [Awingu på som ett alternativ till Azure RemoteApp](http://alternative-for-azure-remoteapp.awingu.com/).


> Primär plats: Belgien
> 
> Operativsystemet regioner: EMEA, Nordamerika och Brasilien
> 
> Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både 
> 
> **Globala**:
> 
> Arnaud Marlière, CMO
> 
> E-post:[arnaud@awingu.com](mailto:arnaud@awingu.com)
> 
> Telefon: +1 646 583 3025
> 
> **Belgien HQ**:
> 
> Ottergemsesteenweg Zuid 808 B44
> 
> 9000 Speditörs
> 
> E-post:[info@awingu.com](mailto:info@awingu.com) 
> 
> Telefon: + 32 9 296 40 11
> 
> **USA**:
> 
> 7 våning, 1177 Ave av Nord,
> 
> New York, NY 10036
> 
> E-post:[info.us@awingu.com](mailto:info.us@awingu.com)

### <a name="microsoft-hosted-service-provider"></a>Microsoft som värd-leverantör
Värd partner har vanligtvis en helt hanterad värdbaserade Windows-skrivbordet och programtjänsten, vilket kan omfatta hantering av Azure-resurser, operativsystem, program och supportavdelningen med partnern licensavtal med Microsoft och andra programvaruleverantörer tillsammans med som en Tjänstleverantörslicensavtalet att återförsäljning av prenumeranten Access License (SAL). Följande information innehåller information och kontaktinformation för några av de värdar som specialisering bistå kunder med Azure RemoteApp-migreringen. Checka ut [den aktuella listan över värdbaserade leverantörer](http://aka.ms/rdsonazurecertified) som slutförts RDS på IaaS sökväg och assessment.  

### <a name="citrix-service-provider-program"></a>Citrix Service Provider Program
Citrix Service Provider Program gör det enkelt för leverantörer att leverera enkelhet av virtuella molntjänster till små och medelstora företag, erbjuda dem till de tjänster som de vill använda i en enkel, betalning per användning modell. Citrix-leverantörer utöka sina Microsoft SPLA-företag och expandera deras RDS plattform investeringar med vilken enhet som helst, åtkomst överallt, bredaste programmets support, en innehållsrik, extra säkerhet och ökad skalbarhet. I sin tur Citrix-leverantörer ger dig flera prenumeranter ökar nöjda och minska sina driftskostnader. [Lär dig mer](http://www.citrix.com/products/service-providers.html) eller [hitta en partner](https://www.citrix.com/buy/partnerlocator.html).

#### <a name="acuutech"></a>Acuutech
[Acuutech](http://www.acuutech.com) specialiserat sig på att tillhandahålla värdbaserad skrivbord lösningar som ger fullständig skrivbord och program för ISV upplevelser som bygger på Microsoft-teknik till en global klient grundläggande från Azure och egna datacenter.

> Primär plats: London, Storbritannien; Singapore; Houston, TX
> 
> Åtgärden region: över hela världen
> 
> Samarbeta status: Guld
> 
> Microsoft Cloud-leverantör: Ja
> 
> Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både
> 
> Lösningar för migrering till Azure RemoteApp: Ja, [Läs mer](http://www.acuutech.com/ara-migration/)
> 
> **Storbritannien**:
>   
> 5/6 York House, Langston väg
>   
> Loughton, Essex IG10 3TQ
>   
> Telefon: + 44 (0) 20 8502 2155
> 
> **Singapore**:
>   
> 100 Cecil gata, #09-02 
>   
> Globalt Singapore 069532
> 
> Telefon: + 65 6709 4933
>   
> **Nordamerika**:
>   
> 3601 S. Sandman St.
>   
> Suite 200, Houston, TX 77098
>   
> Telefon: +1 713 691 0800

#### <a name="aspex"></a>ASPEX
[ASPEX](http://www.aspex.be/en) specialiserat sig på ISV övergå till molnet och ISV-vill optimera deras aktuella inställningar för molnet. ASPEX erbjuder en mängd olika hanterade tjänster, devops och rådgivning.  

> Primär plats: Antwerpen, Belgien
> 
> Åtgärden region: västra Europa
> 
> Samarbeta status: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)
> 
> Microsoft Cloud-leverantör: Ja
> 
> Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både
> 
> Lösningar för migrering till Azure RemoteApp: Ja, [Läs mer](https://www.aspex.be/en/azure-remote-apps)
> 
> Telefon: +3232202198
> 
> E-post:[info@aspex.be](mailto:info@aspex.be)
> 
> Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="caasecom"></a>Caase.com
[Caase.com](http://www.caase.com/) hjälper företag, lokala myndigheter, icke-statliga organ och sjukvården institutioner med transporten mot ett effektivare sätt för arbete i Microsoft Cloud. Ha produktiva och säker var som helst, med vilken enhet som helst och låg IT-kostnader. Caase.com är true specialist för Microsoft Office365, Azure, Enterprise Mobility och säkerhet och Windows. Med våra konsulter, migreringstjänster, införande program, träning skapar hantering och support Caase.com du en optimerad och säker plattform för samarbete för kunders anställda, partners och leverantörer.
Caase.com är mastermind arbetsytan Azure Remote (mobila arbetsyta) och digitala arbetsplatsen (sociala intranät). Båda lösningarna – åstadkommas införs – är grunden som användarna av dessa lösningar har mest angenäm, lyckad och effektiv upplevelsen i sina vägen till Microsoft Cloud.
Nederländska översättning ánd en stödjande film över här: http://caase.com/over-ons/

> Åtgärden region: nederländska baserat globalt omfattande
> 
> Samarbeta status: [guld](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)
> 
> Microsoft Cloud-leverantör: Ja
> 
> Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både
> 
> Lösningar för migrering till Azure RemoteApp: Ja, [mer](http://caase.com/diensten/microsoft-azure/).
> 
> 
> Nederländerna:
> 
> Rigtersbleek Zandvoort 10 (Tyskland Spinnerij)
> 
> 7521 BE, Enschede
> 
> Telefon: +31 (0) 88 4320 000


#### <a name="nerdio"></a>Nerdio
[Nerdio för Azure](http://getnerdio.com/nfa/) är en IT-automatiseringsplattform som levererar löjligt enkelt etablering, hantering och optimering av fullständig IT-miljöer i Microsoft-molnet. Klara av virtuella skrivbord, fjärrprogram och servrar i ett par timmar. Administrera miljön i tre gånger eller mindre med Nerdio Admin Portal. Använd intelligent automatisk skalning och spara 40 till 60% i Azure IaaS-resurser.

> Primär plats: Chicago, IL åtgärden region: Worldwide Partner status: [guld](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft Molntjänstleverantör: Ja
> 
> Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både
> 
> Lösningar för migrering till Azure RemoteApp: Ja
> 
> 
> 8001 Lincoln Ave
> 
> Suite 212
> 
> Skokie, IL 60077
> 
> USA
> 
> (844) 4NERDIO externt 6
> 
> [sayhello@getnerdio.com](mailto:sayhello@getnerdio.com)

#### <a name="saasplaza"></a>**SaaSplaza**
[SaaSplaza](http://www.saasplaza.com/) erbjuder fullständigt Microsoft Dynamics portfölj (NAV, AX, GP, SL, CRM) privata och offentliga moln (Azure).

> Primär plats: Nederländerna
> 
> Åtgärden Region: över hela världen
> 
> Samarbeta status: [guld](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)
> 
> Microsoft Cloud-leverantör: Ja
> 
> Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både
> 
> **EMEA**:
> 
> Prins Mauritslaan 29 35
> 
> 71 LP Badhoevedorp
> 
> Nederländerna
> 
> Telefon: +31 20 547 8060 
> 
>  **Americas**:
> 
> 171 NIEDERSACHSEN väg, Suite 105
> 
> Encinitas, CA 92024
> 
> San Diego
> 
> USA
> 
> Telefon: +1 858 385 8900 
> 
> **APAC**:
> 
> 105 Cecil gata
>    
> \#11-08, Åttahörning
> 
> Singapore 069534
> 
> Singapore
>   
> Telefon - Singapore: + 65 6222 6591
> 
> Telefon - Australien: +61 2 8310 5568 
>    
> Telefon - Nya Zeeland: +64 4 488 0321
> 
## <a name="need-more-help"></a>Behöver du mer hjälp?
Fortfarande behöver hjälp att välja eller har fler frågor? Använd någon av följande metoder för att få hjälp. 

1. Skicka e-post på [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).
2. Kontakta [Azure-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Starta genom att öppna en [Azure supportärende](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3. Ring oss. [Hitta ett lokalt försäljning nummer](https://azure.microsoft.com/overview/sales-number/).

