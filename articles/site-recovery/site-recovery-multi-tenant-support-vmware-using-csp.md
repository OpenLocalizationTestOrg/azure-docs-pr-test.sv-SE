---
title: "aaaMulti klient stöd för VMware VM replikering tooAzure (CSP-program) | Microsoft Docs"
description: "Beskriver hur toodeploy Azure Site Recovery i en klientmiljön tooorchestrate replikering, redundans och återställning av lokal VMware virtuella maskiner (VMs) tooAzure via hello CSP-programmet med hjälp av hello Azure-portalen"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: manayar
ms.openlocfilehash: 9be555c9a438f66e6d3dfcdc9f507a84763846d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-tooazure-through-csp"></a>Stöd för flera innehavare i Azure Site Recovery för att replikera VMware virtuella datorer tooAzure via CSP

Azure Site Recovery har stöd för miljöer med flera innehavare för klient-prenumerationer. Det stöder också flera innehavare för klient-prenumerationer som skapas och hanteras via hello Microsoft Cloud Solution Providers (CSP). Den här artikeln beskrivs hello vägledning för att implementera och hantera flera innehavare VMware-Azure-scenarier. Den omfattar också skapa och hantera innehavare prenumerationer via CSP.

Den här vägledningen ritar kraftigt från hello befintliga dokumentationen för att replikera VMware tooAzure för virtuella datorer. Mer information finns i [replikera VMware virtuella datorer tooAzure med Site Recovery](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Miljöer med flera innehavare
Det finns tre huvudsakliga modeller för flera innehavare:

* **Delade värd Services Provider (HSP)**: hello partner äger hello fysisk infrastruktur och använder delade resurser (vCenter, Datacenter, fysisk lagring och så vidare) toohost flera innehavare virtuella datorer på hello samma infrastruktur. hello partner kan tillhandahålla katastrofåterställning hantering som en hanterad tjänst eller hello-klient kan äga katastrofåterställning som en lösning för självbetjäning.

* **Dedikerad värd tjänsteleverantör**: hello partner äger hello fysisk infrastruktur men använder dedicerade resurser (flera Vcenter fysiska datastores och så vidare) toohost varje klients virtuella datorer på en separat infrastruktur. hello partner kan tillhandahålla katastrofåterställning hantering som en hanterad tjänst eller hello-klient kan äga som en lösning för självbetjäning.

* **Hanterade Services Provider (MSP)**: hello kunden äger hello fysisk infrastruktur som är värd för virtuella datorer hello och hello partner tillhandahåller katastrofåterställning aktivering och hantering.

## <a name="shared-hosting-multi-tenant-guidance"></a>Delade riktlinjer för flera innehavare
Det här avsnittet beskriver hello delade scenariot i detalj. hello andra två scenarier är delmängder av hello delade scenariot, och de använder hello samma principer. hello skillnaderna beskrivs hello slutet av hello delade vägledning.

hello grundläggande krav i ett scenario med flera innehavare är tooisolate hello olika klienter. En klient får inte vara kan tooobserve vad en annan innehavare har värdet hosted. Det här kravet är inte lika viktigt i en självbetjäning miljö där det kan vara avgörande i en partner-hanterad miljö. Dessa riktlinjer förutsätter att klientisolering krävs.

hello arkitektur visas i följande diagram hello:

![Delad HSP med en vCenter](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**Delade scenario med en vCenter**

Som det visas i föregående diagram hello har varje kund en separat hanteringsserver. Den här konfigurationen gränser innehavaren åtkomst till tootenant-specifika virtuella datorer och möjliggör klientisolering. Ett scenario med VMware replikeringen av den virtuella datorn använder hello configuration server toomanage konton toodiscover virtuella datorer och installera agenter. Vi följer hello samma principer för miljöer med flera innehavare med hello begränsa VM identifiering via vCenter åtkomstkontroll.

hello krav för isolering av data kräver att alla infrastruktur för känslig information (till exempel autentiseringsuppgifter) hålls hemlig tootenants. Därför rekommenderar vi att alla komponenter i hello hanteringsservern förblir under hello exklusiv kontroll av hello partner. hello management server-komponenter är:
* Konfigurationsservern (CS)
* Processervern (PS)
* Huvudmålservern (Huvudmålservern) 

En skalbar PS är också under hello partner kontroll.

### <a name="every-cs-in-hello-multi-tenant-scenario-uses-two-accounts"></a>Varje CS i hello flera innehavare scenariot använder två konton

- **konto för vCenter**: använda kontot toodiscover innehavaren virtuella datorer. Den har vCenter åtkomstbehörigheter tooit (som beskrivs i nästa avsnitt om hello). toohelp undvika oavsiktlig åtkomst minnesläckor, rekommenderar vi att partner anger autentiseringsuppgifterna sig själva i hello-konfigurationsverktyget.

- **Virtuella åtkomstkonto**: använda detta konto tooinstall hello mobility agent hello innehavaren virtuella datorer via en automatisk push. Det är vanligtvis ett domänkonto som en klient kan ge tooa partner eller att du kan också hello partner kan hantera direkt. Om en klient inte vill tooshare hello information med hello partner direkt, kan han eller hon tillåtas tooenter hello autentiseringsuppgifter via begränsad tid åt toohello CS eller hello partner hjälp att installera mobility agenter manuellt.

### <a name="requirements-for-a-vcenter-access-account"></a>Krav för ett konto för vCenter

Som nämns i föregående avsnitt hello, måste du konfigurera hello CS med ett konto som har en särskild roll som tilldelats tooit. hello rolltilldelning måste vara tillämpas toohello vCenter åtkomstkonto för varje vCenter-objekt och inte sprids toohello underordnade objekt. Den här konfigurationen garanterar klientisolering, eftersom åtkomst spridningen kan resultera i oavsiktlig åtkomst tooother objekt.

![hello alternativ för sprida tooChild objekt](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

hello alternativ är tooassign hello användarkontot och rollen på hello datacenter objekt och sprider dem toohello underordnade objekt. Ge hello konto en *ingen åtkomst* rollen för alla objekt (till exempel andra klienter VM: ar) som ska vara tillgänglig tooa viss klient. Den här konfigurationen är besvärlig och den exponerar oavsiktliga åtkomstkontroller, eftersom alla nya underordnade objekt automatiskt att beviljas åtkomst som ärvs från hello överordnade. Därför rekommenderar vi att du använder hello första tillvägagångssättet.

Hej vCenter-kontoåtkomst procedur är följande:

1. Skapa en ny roll genom att klona hello fördefinierad *skrivskyddad* roll, och ge det ett enkelt namn (till exempel Azure_Site_Recovery som visas i det här exemplet).

2. Tilldela följande behörigheter toothis roll hello:

    * **DataStore**: allokera utrymme, bläddra datalagret, låg nivå filåtgärder, ta bort filen, uppdateringsfiler för virtuell dator

    * **Nätverket**: tilldela nätverk

    * **Resursen**: tilldela VM tooresource poolen, migrera avstängda VM, migrera påslagen VM

    * **Uppgifter**: skapa uppdatera aktiviteten

    * **Virtuella**: 
        * Configuration > alla
        * Interaktion > besvara frågan, enhetsanslutning, konfigurera CD-skivor, konfigurera diskettenheter media, Stäng av slå på Installera för VMware-verktyg
        * Inventering > från befintliga, skapa nya, registrera, avregistrera
        * Etablering > Tillåt virtuella hämtning, Tillåt virtuella filer överför
        * Hantering av ögonblicksbilder > Ta bort ögonblicksbilder

    ![dialogrutan Redigera roll för hello](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Tilldela nivåer toohello vCenter åtkomstkonto (används i hello klient CS) för olika objekt på följande sätt:

>| Objekt | Roll | Kommentarer |
>| --- | --- | --- |
>| vCenter | Skrivskyddad | Krävs endast tooallow vCenter komma åt för att hantera olika objekt. Du kan ta bort den här behörigheten om hello kontot ska aldrig toobe tillhandahålls tooa klient eller användas för alla hanteringsåtgärder på hello vCenter. |
>| Datacenter | Azure_Site_Recovery |  |
>| Värd- och -värdkluster | Azure_Site_Recovery | Nytt garanterar att åtkomst på hello objektnivå, så att endast tillgänglig värdar har klient virtuella datorer före redundans och efter återställning efter fel. |
>| DataStore-kluster med datalagret | Azure_Site_Recovery | Samma som föregående. |
>| Nätverk | Azure_Site_Recovery |  |
>| Hanteringsserver | Azure_Site_Recovery | Innehåller åtkomst tooall komponenter (CS PS och Huvudmålservern) om några utanför hello CS-datorn. |
>| Klient virtuella datorer | Azure_Site_Recovery | Säkerställer att alla nya innehavaren virtuella datorer i en viss klient också få åtkomst eller inte är tillgängligt via hello Azure-portalen. |

Hej vCenter kontoåtkomst är slutförd. Det här steget uppfyller hello minsta möjliga behörigheter krav toocomplete återställning åtgärder. Du kan också använda dessa åtkomstbehörigheter med din befintliga principer. Ändra bara din befintliga behörigheter set tooinclude rollbehörigheter från steg 2, detaljerad tidigare.

toorestrict katastrofåterställning åtgärder förrän hello redundansläge (som är utan funktioner för återställning efter fel), följ hello föregående procedur med ett undantag: i stället för att tilldela hello *Azure_Site_Recovery* roll toohello vCenter kontot, tilldela bara en *skrivskyddad* rollen toothat konto. Den här behörighetsgruppen låter VM-replikering och redundans, vilket tillåter inte återställning efter fel. Allt annat i hello föregående process blir kvar som är. tooensure klientisolering och begränsa VM identifiering, varje tillstånd fortfarande är tilldelad vid hello nivån endast och inte spridda toochild objekt.

## <a name="other-multi-tenant-environments"></a>Andra miljöer med flera innehavare

hello föregående avsnitt beskrivs hur tooset upp en miljö med flera innehavare för en delad värd-lösning. hello är andra två större lösningar värd för dedikerade och hanteringstjänster. hello-arkitekturen för dessa lösningar beskrivs i följande avsnitt hello.

### <a name="dedicated-hosting-solution"></a>Dedikerad värd

I följande diagram hello visas hello arkitektur skillnaden i en dedikerad värd-lösning är att varje klient infrastrukturen har ställts in för att klienten endast. Eftersom klienter är isolerade via separat Vcenter hello värdleverantör fortfarande måste följa hello CSP stegen som värd för delade men behöver inte tooworry om klientisolering. CSP inställningar ändras inte.

![arkitektur för delade hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Dedikerad värd scenario med flera Vcenter**

### <a name="managed-service-solution"></a>Hanterad tjänst-lösning

Som visas i hello är följande diagram, hello arkitektur skillnaden i en hanterad tjänst-lösning att varje klient infrastruktur är fysiskt avskild från andra klienter infrastruktur. Det här scenariot finns vanligtvis när hello klient äger hello infrastruktur och vill ha en lösning providern toomanage katastrofåterställning. Igen, eftersom klienterna är fysiskt isolerade via olika nätverksinfrastrukturer, hello partner måste toofollow hello CSP stegen som värd för delade men behöver inte tooworry om klientisolering. CSP etablering ändras inte.

![arkitektur för delade hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**Hanterad service scenario med flera Vcenter**

## <a name="csp-program-overview"></a>Översikt över CSP-programmet
Hej [CSP-programmet](https://partner.microsoft.com/en-US/cloud-solution-provider) bättre tillsammans artiklar som erbjuder partners alla Microsoft-molntjänster, inklusive Office 365 Enterprise Mobility Suite och Microsoft Azure kan åstadkomma. Med CSP våra samarbetspartners äger hello slutpunkt till slutpunkt-relation med kunder och blir kontaktpunkt för hello primära relationen. Partners kan distribuera Azure-prenumerationer för kunder och kombinera hello prenumerationer med sina egna mervärde, anpassade erbjudanden.

Med Azure Site Recovery kan partners hantera hello komplett lösning för katastrofåterställning för kunder direkt via CSP. Eller så kan du använda CSP tooset in Site Recovery-miljöer och informera kunderna hantera sina egna katastrofåterställning behov på ett sätt för självbetjäning. I båda fallen är partners hello samverkan mellan Site Recovery och sina kunder. Partners tjänsten hello kundrelation och debitera kunder för användning i Site Recovery.

## <a name="create-and-manage-tenant-accounts"></a>Skapa och hantera innehavarens konton

### <a name="step-0-prerequisite-check"></a>Steg 0: Kontrollen av förutsättningar

hello VM förutsättningar är hello densamma som beskrivs i hello [dokumentation för Azure Site Recovery](site-recovery-vmware-to-azure.md). I tillägg toothose krav bör du ha hello tidigare nämnda åtkomstkontroller på plats innan du fortsätter med klient-hantering via CSP. Skapa en separat hanteringsserver som kan kommunicera med hello klient virtuella datorer och partnerns vCenter för varje klient. Endast hello partnern har åtkomst till rättigheter toothis servern.

### <a name="step-1-create-a-tenant-account"></a>Steg 1: Skapa ett klient-konto

1. Via [Microsoft Partner Center](https://partnercenter.microsoft.com/), logga in tooyour CSP-konto. 
 
2. På hello **instrumentpanelen** väljer du **kunder**.

    ![hello Microsoft Partner Center kunder länk](./media/site-recovery-multi-tenant-support-vmware-using-csp/csp-dashboard-display.png)

3. På hello som öppnas klickar du på hello **Lägg till kunden** knappen.

    ![hello lägga till kundens knapp](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-new-customer.png)

4. På hello **ny kund** sidan, Fyll i hello information kontoinformation för hello-klient och klicka sedan på **nästa: prenumerationer**.

    ![hello kontoinformation sida](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-add-filled.png)

5. På sidan för val av hello prenumerationer, Välj hello **Microsoft Azure** kryssrutan. Du kan lägga till andra prenumerationer nu eller vid ett senare tillfälle.

    ![kryssrutan för hello Microsoft Azure-prenumeration](./media/site-recovery-multi-tenant-support-vmware-using-csp/azure-subscription-selection.png)

6. På hello **granska** sidan Bekräfta hello klient information och klicka sedan på **skicka**.

    ![Hej granskningssidan](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-summary-page.png)  

    När du har skapat hello innehavarkonto, visas en bekräftelsesida, visar hello information av hello standardkonto och hello lösenordet för den prenumerationen. 

7. Spara hello information och ändra lösenord hello senare vid behov via hello Azure portal-inloggningssida.  
 
    Du kan dela information med hello innehavaren är eller du kan skapa och dela ett särskilt konto om det behövs.

### <a name="step-2-access-hello-tenant-account"></a>Steg 2: Åtkomstkonto hello-klient

Du kan använda hello innehavarens prenumeration via hello instrumentpanelen för Microsoft-Partner Center, som beskrivs i ”steg 1: skapa ett klient-konto”. 

1. Gå toohello **kunder** , och klickar sedan på hello namnet på hello innehavarkonto.

2. På hello **prenumerationer** sidan av hello innehavarkonto som du kan övervaka hello befintliga prenumerationer för kontot och lägga till flera prenumerationer efter behov. toomanage hello klient katastrofåterställning åtgärder, Välj **alla resurser (Azure portal)**.

    ![länka hello alla resurser](./media/site-recovery-multi-tenant-support-vmware-using-csp/all-resources-select.png)  
    
    Klicka på **alla resurser** ger dig åtkomst toohello klients Azure-prenumerationer. Du kan kontrollera åtkomsten genom att klicka på hello Azure Active Directory-länken längst hello uppifrån höger om hello Azure-portalen.

    ![Azure Active Directory-länk](./media/site-recovery-multi-tenant-support-vmware-using-csp/aad-admin-display.png)

Du kan nu utföra alla site recovery-åtgärder för hello klienten via hello Azure-portalen och hantera hello disaster recovery-åtgärder. tooaccess hello klientprenumeration via CSP för hanterade katastrofåterställning, följ hello som beskrevs tidigare processen.

### <a name="step-3-deploy-resources-toohello-tenant-subscription"></a>Steg 3: Distribuera resurser toohello klientprenumeration
1. Skapa en resursgrupp på hello Azure-portalen, och sedan distribuera Recovery Services-valvet per hello normala process. 
 
2. Hämta hello valvregistreringsnyckel.

3. Registrera hello CS för hello klienten med hjälp av hello valvregistreringsnyckel.

4. Ange hello autentiseringsuppgifter för hello två konton: vCenter åtkomstkonto och virtuell dator åtkomst till kontot.

    ![Serverkonton med configuration Manager](./media/site-recovery-multi-tenant-support-vmware-using-csp/config-server-account-display.png)

### <a name="step-4-register-site-recovery-infrastructure-toohello-recovery-services-vault"></a>Steg 4: Registrera Site Recovery-infrastruktur toohello Recovery Services-valvet
1. I hello Azure-portalen på hello valvet som du skapade tidigare, registrera hello vCenter server toohello CS som du har registrerat i ”steg 3: distribuera resurser toohello klientprenumeration”. Använd hello vCenter åtkomstkonto för det här ändamålet.
2. Avsluta hello ”Förbered infrastruktur”-processen för Site Recovery per hello normala process.
3. hello virtuella datorer är nu redo toobe replikeras. Kontrollera att endast hello innehavarens virtuella datorer visas på hello **Välj virtuella datorer** bladet under hello **replikera** alternativet.

    ![Klient VMs lista på bladet för hello väljer virtuella datorer](./media/site-recovery-multi-tenant-support-vmware-using-csp/tenant-vm-display.png)

### <a name="step-5-assign-tenant-access-toohello-subscription"></a>Steg 5: Tilldela klienten åtkomst toohello prenumeration

För självbetjäning katastrofåterställning ge toohello innehavare hello kontoinformation som anges i steg 6 i hello ”steg 1: skapa ett klient-konto” avsnittet. Utföra denna åtgärd när hello partner har konfigurerat hello disaster recovery-infrastruktur. Om hello katastrofåterställning är hanterade eller självbetjäning, partners måste komma åt klienten prenumerationer via hello CSP-portalen. De konfigurera hello ägs av partner valvet och registrera infrastruktur toohello klient prenumerationer.

Partners kan också lägga till en ny användare toohello klient-prenumeration hello CSP-portalen genom att göra hello följande:

1. Gå toohello klient CSP prenumeration sida och välj sedan hello **användare och licenser** alternativet.

    ![Hej klients CSP prenumerationssidan](./media/site-recovery-multi-tenant-support-vmware-using-csp/users-and-licences.png)

    Du kan nu skapa en ny användare genom att ange hello relevant information och välja behörigheter eller genom att överföra hello listan över användare i en CSV-fil.

2. När du har skapat en ny användare kan gå tillbaka toohello Azure portal, och klickar sedan på hello **prenumeration** bladet, Välj hello relevanta prenumeration.

3. På hello bladet som öppnas väljer **Access Control (IAM)**, och klicka sedan på **Lägg till** tooadd en användare med hello relevanta åtkomstnivå.      
    hello-användare som har skapats via hello CSP portal visas automatiskt på hello bladet som öppnas när du klickar på åtkomstnivå.

    ![Lägga till en användare](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-user-subscription.png)

    De flesta hanteringsåtgärder hello *deltagare* roll är tillräckliga. Användare med den här åtkomstnivån kan göra allt på en prenumeration förutom ändra åtkomstnivåerna (som *ägare*-åtkomst krävs). Du kan även finjustera hello åtkomstnivåer som krävs.
