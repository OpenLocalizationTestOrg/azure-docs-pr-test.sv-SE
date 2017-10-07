---
title: "aaaReplicate en flera nivåer IIS baserade webbapp med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooreplicate IIS web grupp virtuella datorer med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 1974265b3cb05f6dc57049876306d2e08424bb97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-iis-based-web-application-using-azure-site-recovery"></a>Replikera en IIS-baserad webbtjänst flernivåapp med hjälp av Azure Site Recovery

## <a name="overview"></a>Översikt


Programvara är hello motor för företagsproduktiviteten i en organisation. Olika program kan ha olika syften i en organisation. Vissa av dessa som löneuppgifter bearbetning, ekonomiprogram och kundinriktade webbplatser kan vara ytterst viktigt för organisationen. Ska det vara viktigt för organisationen hello toohave dem upp och körs vid alla tidpunkter tooprevent förlust av produktivitet och mer förhindra allt eventuella skador toohello varumärken bild av hello organisation.

Kritiska webbprogram anges vanligtvis som program på flera nivåer med hello webb-, databas och program på olika nivåer. Förutom att sprida över olika nivåer, kan hello program också använda flera servrar i varje nivå tooload Utjämna hello-trafiken. Dessutom kan hello mappningar mellan olika nivåer och på hello webbservern vara baserad på statiska IP-adresser. På redundanskluster måste vissa av dessa mappningar toobe uppdateras, särskilt om du har flera webbplatser som är konfigurerade på hello webbserver. Vid webbprogram med hjälp av SSL måste-certifikatbindningar toobe uppdateras.

Traditionella icke-baserat replikeringsåterställning metoder innebär säkerhetskopiering för olika configuration-filer, registerinställningar, bindningar, anpassade komponenter (COM eller .NET), innehåll och även certifikat och återställs hello filer via en uppsättning manuella steg. Dessa tekniker är tydligt besvärlig fel felbenägna och inte skalbara. Det är till exempel enkelt möjlig för tooforget säkerhetskopiera certifikat och lämnas nya certifikat för hello server med inga val men toobuy efter växling vid fel.

En bra lösning för haveriberedskap, ska tillåta modellering av återställningsplaner runt hello ovan programarkitekturer för komplexa och har även hello möjlighet tooadd anpassade steg toohandle mappning mellan olika nivåer därför att tillhandahålla en enkelklickning att som lösning för en katastrofåterställning inledande tooa händelsen hello lägre Återställningstidsmål.


Den här artikeln beskriver hur tooprotect en IIS baserade webbprogram som använder en [Azure Site Recovery](site-recovery-overview.md). Den här artikeln beskriver bästa praxis för att replikera en tre nivå IIS baserat web application tooAzure, hur du kan göra en katastrof återställningsgranskning och hur du kan redundans hello programmet tooAzure.


## <a name="prerequisites"></a>Krav

Innan du börjar, kontrollera att du förstår hello följande:

1. [Replikera en virtuell dator tooAzure](site-recovery-vmware-to-azure.md)
1. Hur för[utforma ett nätverk för återställning](site-recovery-network-design.md)
1. [Gör en testa redundans tooAzure](./site-recovery-test-failover-to-azure.md)
1. [Gör en tooAzure för växling vid fel](site-recovery-failover.md)
1. Hur för[replikera en domänkontrollant](site-recovery-active-directory.md)
1. Hur för[replikera SQL Server](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Mönster för distribution
Ett IIS-baserade webbprogram följer vanligtvis en hello efter distributionen mönster:

** Distribution mönstret 1 ** ett IIS-baserade webbgrupp med programmet begär Routing(ARR), IIS-Server och Microsoft SQL Server.

![Mönster för distribution](./media/site-recovery-iis/deployment-pattern1.png)

**Distribution av mönstret 2** en IIS baserade webbservergrupp med programmet begär Routing(ARR), IIS-servern, programserver och Microsoft SQL Server.


![Mönster för distribution](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Site Recovery-stöd

För hello syftet med att skapa den här artikeln virtuella VMware-datorer med IIS-servern version 7.5 på Windows Server 2012 R2 Enterprise användes. Eftersom site recovery replikering oberoende av programmet hello rekommendationer som anges här förväntade toohold på följande scenarier samt och för olika versioner av IIS.

### <a name="source-and-target"></a>Källa och mål

**Scenario** | **tooa sekundär plats** | **tooAzure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysisk server** | Nej | Ja

## <a name="replicate-virtual-machines"></a>Replikera virtuella datorer

Följ [vägledningen](site-recovery-vmware-to-azure.md) toostart replikerar alla hello IIS web grupp virtuella datorer tooAzure.

Om du använder en statisk IP-adress och sedan ange hello IP-adress som du vill hello virtuella tootake i hello [ **mål-IP** ](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties) i beräknings-och nätverksinställningar.

![Mål-IP](./media/site-recovery-active-directory/dns-target-ip.png)


## <a name="creating-a-recovery-plan"></a>Skapa en återställningsplan

En återställningsplan kan ordningsföljd hello växling vid fel på olika nivåer i en flernivåapp, därför kan upprätthålla programkonsekvens. Följ hello nedanstående steg när du skapar en återställningsplan för ett webbprogram med flera nivåer.  [Lär dig mer om hur du skapar en återställningsplan](./site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-toofailover-groups"></a>Lägga till virtuella datorer toofailover grupper
Ett typiskt flera nivåer IIS-webbprogram utgörs av en databasnivå med SQL virtuella datorer, hello webbnivå utgörs av en IIS-server och en program-nivå. Lägg till alla dessa virtuella datorer toodifferent-grupp baserat på nivå som nedan. [Mer information om att anpassa återställningsplan](site-recovery-runbook-automation.md#customize-the-recovery-plan).

1. Skapa en återställningsplan. Lägg till hello databasen virtuella skiktdatorer under grupp 1 tooensure att de är avstängning senast och tas först.

1. Lägg till programmet hello nivå virtuella datorer under Grupp2 så att de förs upp efter hello databasnivå har trätt.

1. Lägg till hello web nivå virtuella datorer i gruppen 3 så att de förs upp efter hello programmet nivå har trätt.

1. Lägg till belastningen belastningsutjämna virtuella datorer i gruppen 4 så att de förs upp efter hello webbnivå har trätt.


### <a name="adding-scripts-toohello-recovery-plan"></a>Lägga till skript toohello återställningsplan
Du kan behöva toodo vissa åtgärder på hello Azure virtuella datorer efter redundans och testning redundans toomake IIS web servergruppen funktionen korrekt. Du kan automatisera hello post Redundansåtgärden som uppdaterar DNS-posten ändrar webbplatsbindning, ändra i anslutningssträngen genom att lägga till motsvarande skript i hello återställningsplan enligt nedan. [Lär dig mer om att lägga till skript återställningsplan](./site-recovery-create-recovery-plans.md#add-scripts).

#### <a name="dns-update"></a>DNS-uppdatering
Om hello DNS har konfigurerats för dynamisk DNS-uppdatering och virtuella datorer vanligtvis uppdatera hello DNS med nya IP-hello när de startar. Om du vill tooadd explicit steg-tooupdate DNS med hello nya IP-adresser hello virtuella datorer lägger du till detta [skript tooupdate IP-Adressen i DNS-](https://aka.ms/asr-dns-update) som en post-åtgärd på recovery planeringsgrupper.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Anslutningssträngen i web.config för ett program
hello anslutningssträngen anger hello-databas som hello webbplats kommunicerar med.

Om hello-anslutningssträngen innehåller hello hello databasen virtuella datorns namn, inga ytterligare steg krävs efter växling vid fel och hello programmet ska kunna kommunicera tooautomatically toohello DB. Även om hello IP-adress för hello databasen virtuella datorn sparas den inte behövs tooupdate hello anslutningssträngen. Om hello anslutningssträngen refererar toohello databasen virtuell dator med en IP-adress, måste toobe uppdateras efter växling vid fel. T.ex. hello nedan anslutningssträngen pekar toohello DB med IP-127.0.1.2

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Du kan uppdatera hello anslutningssträngen i webbnivå genom att lägga till [skript för IIS-anslutning att uppdatera](https://aka.ms/asr-update-webtier-script-classic) efter grupp 3 i hello återställningsplan.

#### <a name="site-bindings-for-hello-application"></a>Bindningar för webbplats för hello program
Varje plats består av bindning information som innehåller hello typ av bindning, hello IP-adress på vilken hello IIS-servern lyssnar toohello begäranden för hello plats, hello portnummer och hello värdnamn för hello plats. När hello en växling vid fel måste dessa bindningar kanske toobe uppdateras om det finns en ändring i hello IP-adress som är kopplade till dem.

> [!NOTE]
>
> Om du har markerat 'alla otilldelade' för hello webbplatsbindning som hello exemplet nedan kan behöver du inte tooupdate denna bindning efter växling vid fel. Även om hello IP-adress som är kopplad till en plats inte ändras efter växling vid fel, hello webbplatsbindning behöver inte har uppdaterats (kvarhållning av hello IP-adress beror på hello nätverksarkitektur och undernät tilldelas toohello primära platsen och återställningsplatsen och därför kan eller inte vara möjligt för din organisation.)

![SSL-bindning](./media/site-recovery-iis/sslbinding.png)

Om du har associerat hello IP-adress med en plats, måste tooupdate alla bindningar för webbplats med hello nya IP-adressen. Du kan lägga till [webbserver nivå uppdateringsskriptet](https://aka.ms/asr-web-tier-update-runbook-classic) efter grupp 3 i recovery plan toochange hello platsbindningar.


#### <a name="update-load-balancer-ip-address"></a>Uppdatera IP-adressen för belastningsutjämnaren
Om du har routning av programbegäran virtuell dator, lägger du till [IIS ARR redundans skriptet](https://aka.ms/asr-iis-arrtier-failover-script-classic) efter grupp 4 tooupdate hello IP-adress.

#### <a name="hello-ssl-cert-binding-for-an-https-connection"></a>hello SSL-certifikat-bindning för en https-anslutning
Webbplatser kan ha ett associerat SSL-certifikat som hjälper till att säkerställa en säker kommunikation mellan hello webbserver och hello användarens webbläsare. Om hello webbplats har en https-anslutning och associerade https plats bindning toohello IP-adressen hello IIS-server med en SSL-certifikat-bindning, måste en ny plats-bindning toobe som lagts till för hello cert med hello IP för hello IIS virtuella datorn efter redundans.

hello SSL-certifikat kan utfärdas mot-

en) hello fullständigt kvalificerade domännamnet för hello webbplats<br>
b) hello server hello namn<br>
c) ett jokerteckencertifikat för hello domännamn<br>
d) en IP-adress – om hello SSL-certifikat utfärdas mot hello IP hello IIS-servern, en annan behov toobe för SSL-certifikat som utfärdats för hello IP-adressen för hello IIS-servern på hello Azure site och en ytterligare SSL-bindning för det här certifikatet måste toobe skapas. Det är därför lämpligt toonot används ett SSL-certifikat som utfärdats för IP. Detta är ett alternativ för mindre vanliga och snart kommer att inaktualiseras enligt nya CA/webbläsare forum ändringar.

#### <a name="update-hello-dependency-between-hello-web-and-hello-application-tier"></a>Uppdatera hello beroende mellan hello webb- och hello programmet nivå
Om du har ett specifikt tillämpningsprogramberoende baserat på hello IP-adressen för hello virtuella datorer måste tooupdate detta beroende efter växling vid fel.

## <a name="doing-a-test-failover"></a>Gör ett redundanstest
Följ [vägledningen](site-recovery-test-failover-to-azure.md) toodo testa redundans.

1.  Gå tooAzure portal och välj Recovery-tjänsten-valvet.
1.  Klicka på hello återställningsplan som skapats för IIS-webbservergrupp.
1.  Klicka på Testa redundans.
1.  Välj återställningspunkten och den virtuella Azure-nätverket toostart hello test failover-processen.
1.  Du kan utföra din verifieringar när hello sekundära miljön är upp.
1.  När hello verifieringar har slutförts, kan du välja verifieringar slutföra och hello redundanstestmiljön kommer att rensas.

## <a name="doing-a-failover"></a>Genomför en redundansväxling enligt
Följ [vägledningen](site-recovery-failover.md) när du gör en redundansväxling.

1.  Gå tooAzure portal och välj Recovery-tjänsten-valvet.
1.  Klicka på hello återställningsplan som skapats för IIS-webbservergrupp.
1.  Klicka på 'Redundans'.
1.  Välj återställningsprocessen punkt toostart hello växling vid fel.

## <a name="next-steps"></a>Nästa steg
Du kan lära dig mer om [replikera andra program](site-recovery-workload.md) med Site Recovery.
