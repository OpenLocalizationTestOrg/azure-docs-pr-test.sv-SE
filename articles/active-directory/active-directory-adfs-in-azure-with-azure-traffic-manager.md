---
title: "aaaHigh tillgänglighet geografiska mellan AD FS-distribution i Azure med Azure Traffic Manager | Microsoft Docs"
description: "I det här dokumentet får du lära dig hur toodeploy AD FS i Azure för hög tillgänglighet."
keywords: AD fs med Azure traffic manager, AD FS med Azure Traffic Manager, geografiska, flera datacenter, geografiska datacenter, flera geografiska datacenter, distribuera AD FS i azure, distribuera azure AD FS, azure AD FS, azure ad fs, distribuera AD FS, distribuera ad fs, adfs i azure. Distribuera AD FS i azure, distribuera AD FS i azure, azure AD FS, introduktion tooAD FS, Azure, AD FS i Azure, iaas, ADFS, flytta adfs tooazure
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: c5838d749cdc5c8aabbe62b255d568525da747ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>AD FS-distribution med hög tillgänglighet över geografiska områden i Azure med Azure Traffic Manager
[AD FS-distribution i Azure](active-directory-aadconnect-azure-adfs.md) innehåller steg för steg som toohow kan du distribuera en enkel AD FS-infrastrukturen för din organisation i Azure. Den här artikeln innehåller hello nästa steg toocreate cross-geografisk distribution av AD FS i Azure med hjälp av [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Azure Traffic Manager hjälper dig att skapa ett geografiskt spridning hög tillgänglighet och hög prestanda AD FS-infrastrukturen för din organisation genom att göra användningen av intervallet för routningsmetoder tillgängliga toosuit olika måste från hello-infrastruktur.

En AD FS-infrastruktur med hög tillgänglighet och geografisk spridning möjliggör:

* **Eliminering av enskild felpunkt:** med funktioner för redundans i Azure Traffic Manager, du kan få en AD FS-infrastruktur med hög tillgänglighet även om en av hello datacenter i en del av hello globen kraschar
* **Bättre prestanda:** du kan använda hello förslag distribution i den här artikeln tooprovide en högpresterande AD FS-infrastruktur som hjälper användare att autentisera snabbare. 

## <a name="design-principles"></a>Designprinciper
![Övergripande design](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

hello grundläggande principer ska vara detsamma som anges i Design-principerna i hello artikeln AD FS-distribution i Azure. hello diagrammet ovan visar en enkel förlängning av hello grundläggande distribution tooanother geografisk region. Nedan visas några punkter tooconsider när du utökar din distribution toonew geografisk region

* **Virtuellt nätverk:** bör du skapa ett nytt virtuellt nätverk i hello geografisk region du toodeploy ytterligare AD FS-infrastruktur. I hello diagrammet ovan visas Geo1 VNET och Geo2 VNET som hello två virtuella nätverk i varje geografisk region.
* **Domänkontrollanter och AD FS-servrarna i nya geografiska VNET:** rekommenderas toodeploy domänkontrollanter i hello nya geografisk region så att hello AD FS-servrar i hello ny region inte har toocontact en domänkontrollant i en annan långt bort nätverket toocomplete en autentisering och därigenom förbättra hello prestanda.
* **Lagringskonton:** Lagringskonton som är associerade med ett område. Eftersom du ska distribuera datorer i en ny geografisk region, har du toocreate nya lagringskonton toobe används i hello region.  
* **Nätverkssäkerhetsgrupper:** Liksom lagringskonton kan nätverkssäkerhetsgrupper som skapas i ett område inte kan användas i ett annat geografiskt område. Därför behöver toocreate nya network security grupper liknande toothose i hello första geografisk region för INT och DMZ undernät i hello nya geografisk region.
* **DNS-etiketter för offentliga IP-adresser:** Azure Traffic Manager kan referera tooendpoints bara via DNS-etiketter. Det är därför krävs toocreate DNS-etiketter för hello externa belastningsutjämnare offentliga IP-adresser.
* **Azure Traffic Manager:** Microsoft Azure Traffic Manager kan du toocontrol användaren trafik tooyour Tjänsteslutpunkter körs i olika Datacenter runt hello world hello distributionen. Azure Traffic Manager fungerar på hello DNS-nivå. Den använder DNS-svar toodirect slutanvändarens trafik tooglobally spridd slutpunkter. Klienter ansluter sedan toothose slutpunkter som är direkt. Med olika alternativ för routning av prestanda, viktat och prioritet, kan du enkelt välja hello dirigeringsalternativet som passar bäst för din organisations behov. 
* **V net tooV-net-anslutningsbarhet mellan två regioner:** behöver du inte toohave anslutningen mellan virtuella nätverk för hello sig själv. Eftersom varje virtuellt nätverk har åtkomst toodomain styrenheter och har en AD FS- och WAP-server i sig själv, kan de arbeta utan någon anslutning mellan hello virtuella nätverk i olika regioner. 

## <a name="steps-toointegrate-azure-traffic-manager"></a>Steg toointegrate Azure Traffic Manager
### <a name="deploy-ad-fs-in-hello-new-geographical-region"></a>Distribuera AD FS i hello nya geografisk region
Följ steg hello och riktlinjerna i [AD FS-distribution i Azure](active-directory-aadconnect-azure-adfs.md) toodeploy hello samma topologi i hello nya geografisk region.

### <a name="dns-labels-for-public-ip-addresses-of-hello-internet-facing-public-load-balancers"></a>DNS-etiketter för offentliga IP-adresser för hello Internet Facing (offentliga) belastningsutjämnare
Som nämnts ovan är hello Azure Traffic Manager kan endast referera tooDNS etiketter som slutpunkter och det är därför viktigt toocreate DNS-etiketter för hello externa belastningsutjämnare offentliga IP-adresser. Nedan skärmbild som visar hur du kan konfigurera DNS-etikett för hello offentlig IP-adress. 

![DNS-etikett](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Distribuera Azure Traffic Manager
Följ hello steg nedan toocreate traffic manager-profil. Mer information kan du också läsa för[hantera en Azure Traffic Manager-profil](../traffic-manager/traffic-manager-manage-profiles.md).

1. **Skapa en Traffic Manager-profil:** Ge din Traffic Manager-profil ett unikt namn. Den här hello-profilens namn är en del av hello DNS-namn och fungerar som ett prefix för hello Traffic Manager-domännamnet. hello namn / prefix läggs too.trafficmanager.net toocreate en DNS-etikett för din traffic manager. hello skärmbilden nedan visar hello traffic manager DNS-prefix som har angetts som mysts och resulterande DNS-etikett ska vara mysts.trafficmanager.net. 
   
    ![Skapa Traffic Manage-profil](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Trafikroutningmetod:** Det finns tre routningalternativ i Traffic Manager:
   
   * Prioritet 
   * Prestanda
   * Viktad
     
     **Prestanda** rekommenderas hello alternativet tooachieve mycket dynamisk AD FS-infrastruktur. Du kan dock välja den routningmetod som bäst passar för dina distributionsbehov. hello AD FS-funktioner påverkas inte av hello routning alternativ som valts. Se [Traffic Manager trafikroutningsmetoder](../traffic-manager/traffic-manager-routing-methods.md) för mer information. I hello exempel skärmbild ovanför du ser hello **prestanda** vald metod.
3. **Konfigurera slutpunkter:** i hello traffic manager-sidan klickar du på slutpunkter och välj Lägg till. Då öppnas en Lägg till slutpunkten sidan liknande toohello skärmbilden nedan
   
   ![Konfigurera slutpunkter](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Hello så olika indata hello Riktlinje nedan:
   
   **Typ:** Välj Azure slutpunkt som vi ska peka tooan Azure offentlig IP-adress.
   
   **Namn:** skapar du ett namn som du vill tooassociate med hello slutpunkt. Detta är inte hello DNS-namn och påverkar inte DNS-poster.
   
   **Resurs-måltyp:** Välj offentlig IP-adress som hello toothis egenskapen. 
   
   **Rikta resurs:** så får du ett alternativ toochoose från hello olika DNS-etiketter som finns tillgängliga i din prenumeration. Välj hello DNS-etikett motsvarande toohello slutpunkt som du konfigurerar.
   
   Lägg till slutpunkt för varje geografisk region som du vill hello Azure Traffic Manager tooroute trafik till.
   Mer information och detaljerade anvisningar om hur tooadd / konfigurera slutpunkter i traffic manager finns för[Lägg till, inaktivera, aktivera eller ta bort slutpunkter](../traffic-manager/traffic-manager-endpoints.md)
4. **Konfigurera avsökning:** i hello traffic manager-sidan klickar du på konfiguration. I hello konfigurationssidan måste toochange hello övervakaren inställningar tooprobe via HTTP port 80 och relativ sökväg /adfs/probe
   
    ![Konfigurera avsökning](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Se till att hello status hello slutpunkter är ONLINE när hello konfigurationen är klar**. Om alla slutpunkter i 'försämrad' tillstånd, gör Azure Traffic Manager en bästa försök tooroute hello trafik förutsatt att hello diagnostik är felaktig och alla slutpunkter kan nås.
   > 
   > 
5. **Ändra DNS-poster för Azure Traffic Manager:** federationstjänsten ska vara ett CNAME toohello Azure Traffic Manager DNS-namn. Skapa en CNAME-post i hello offentliga DNS-poster så att den som försöker tooreach hello federationstjänsten faktiskt når hello Azure Traffic Manager.
   
    Till exempel toopoint hello federation service fs.fabidentity.com toohello Traffic Manager måste tooupdate din DNS-resurs poster toobe hello följande:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-hello-routing-and-ad-fs-sign-in"></a>Testa hello Routning och AD FS-inloggning
### <a name="routing-test"></a>Routningtest
En grundläggande för hello routning skulle vara tootry ping hello federationstjänstens DNS-namn från en dator i varje geografisk region. Beroende på hello routningsmetoden valt, återspeglas hello endpoint faktiskt pingar den i hello ping visas. Till exempel om du har valt hello prestanda kommer routning, hello endpoint närmsta toohello regionens hello klienten att nå. Nedan visas hello ögonblicksbild av två ping från två olika region-klientdatorer, en i EastAsia region och en i USA, västra. 

![Routningtest](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS-inloggningstest
hello enklaste sättet tootest AD FS är att använda hello IdpInitiatedSignon.aspx sidan. Hej IdpInitiatedSignOn i ordning toobe kan toodo att det är obligatoriskt tooenable för hello AD FS-egenskaper. Gör hello nedan tooverify din AD FS-konfiguration

1. Kör hello nedan cmdlet hello AD FS-servern med hjälp av PowerShell tooset som tooenabled. 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. Gå till https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx från valfri extern dator
3. Du bör se hello AD FS-sidan som nedan:
   
    ![AD FS-test - autentiseringsfråga](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    och om inloggningen lyckas visas ett meddelande som det här nedan:
   
    ![AD FS-test - lyckad autentisering](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Relaterade länkar
* [Grundläggande AD FS-distribution i Azure](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
* [Traffic Manager routningsmetoder](../traffic-manager/traffic-manager-routing-methods.md)

## <a name="next-steps"></a>Nästa steg
* [Hantera en Azure Traffic Manager-profil](../traffic-manager/traffic-manager-manage-profiles.md)
* [Lägga till, inaktivera, aktivera eller ta bort slutpunkter](../traffic-manager/traffic-manager-endpoints.md) 

