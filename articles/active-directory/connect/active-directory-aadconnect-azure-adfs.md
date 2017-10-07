---
title: aaaActive Directory Federation Services i Azure | Microsoft Docs
description: "I det här dokumentet får du lära dig hur toodeploy AD FS i Azure för hög tillgänglighet."
keywords: Distribuera AD FS i azure, distribuera azure AD FS, azure AD FS, azure ad fs, distribuera AD FS, distribuera ad fs, AD FS i azure, distribuera AD FS i azure, distribuera AD FS i azure, azure AD FS, introduktion tooAD FS, Azure, AD FS i Azure, flytta iaas, ADFS, adfs tooazure
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2c39271f7569b9ce395dce2f53f5ba5a4897b132
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Distribuera Active Directory Federation Services i Azure
AD FS tillhandahåller förenklad, säker identitetsfederation och funktioner för enkel inloggning (SSO). Federation med Azure AD eller O365 kan användare tooauthenticate med lokala autentiseringsuppgifter och åtkomst till alla resurser i molnet. Därför är det viktigare toohave en högtillgänglig AD FS-infrastrukturen tooensure åt tooresources både lokalt och i hello molnet. Distribuera AD FS i Azure kan du få hello hög tillgänglighet som krävs med minimalt arbete.
Det finns flera fördelar med att distribuera AD FS i Azure, några av dem anges nedan:

* **Hög tillgänglighet** -med hello kraften i Azure-Tillgänglighetsuppsättningar är du säkerställa att en infrastruktur med hög tillgänglighet.
* **Enkelt tooScale** – behöver bättre prestanda? Enkelt migrera toomore kraftfulla datorer med ett par klick i Azure
* **Mellan Geo-redundans** – med Azure Geo-redundans du vara säker på att din infrastruktur har hög tillgänglighet över hela världen hello
* **Enkelt tooManage** – med hög förenklad hanteringsalternativ i Azure-portalen, hanteringen av din infrastruktur är det mycket enkelt och problemfri 

## <a name="design-principles"></a>Designprinciper
![Distributionsdesign](./media/active-directory-aadconnect-azure-adfs/deployment.png)

hello diagrammet ovan visar hello rekommenderas grundläggande topologi toostart Distribuera AD FS-infrastrukturen i Azure. hello principerna bakom hello olika komponenter i hello topologin visas nedan:

* **DC/ADFS-servrar**: Om du har färre än 1 000 användare behöver du bara installera AD FS-rollen på domänkontrollanterna. Om du inte vill att alla prestandapåverkan på hello domänkontrollanter eller om du har fler än 1 000 användare kan sedan distribuera AD FS på separata servrar.
* **Server för WAP** – det är nödvändigt toodeploy webbprogramproxyservrar, så att användare kan nå hello AD FS när de inte är på företagsnätverket hello också.
* **DMZ**: hello webbprogramproxyservrarna placeras i hello DMZ och endast TCP/443 åtkomst tillåts mellan hello DMZ och hello interna undernätet.
* **Belastningsutjämnare**: tooensure hög tillgänglighet av AD FS och Webbprogramproxy-servrar, bör du använda en intern belastningsutjämnare för AD FS-servrarna och Azure belastningsutjämnare för Web Application Proxy-servrar.
* **Tillgänglighetsuppsättningar**: tooprovide redundans tooyour AD FS-distribution, rekommenderas att du grupperar två eller flera virtuella datorer i en Tillgänglighetsuppsättning för liknande arbetsbelastningar. Den här konfigurationen garanterar att minst en virtuell dator är tillgänglig under planerat eller oplanerat underhåll.
* **Storage-konton**: rekommenderas toohave två storage-konton. Med ett enda storage-konto kan leda till toocreation av en enskild felpunkt och kan orsaka hello distribution toobecome är inte tillgänglig i ett troligt scenario där hello lagringskonto kraschar. Två lagringskonton innebär att ett lagringskonto kan associeras med varje felrad.
* **Nätverkssegregering**: WAP-servrar bör distribueras i ett separat DMZ-nätverk. Du kan dela upp ett virtuellt nätverk i två undernät och därefter distribuera hello Web Application Proxy-servrar i ett isolerat undernät. Bara kan du konfigurera hello grupp för nätverkssäkerhet för varje undernät och tillåta endast obligatoriska kommunikation mellan hello två undernät. Mer information finns i distributionsscenarierna nedan.

## <a name="steps-toodeploy-ad-fs-in-azure"></a>Steg toodeploy AD FS i Azure
hello anvisningarna i det här avsnittet disposition hello guiden toodeploy hello nedan beskrivs AD FS-infrastrukturen i Azure.

### <a name="1-deploying-hello-network"></a>1. Distribuera hello nätverk
Som vi nämnt ovan kan du antingen skapa två undernät i ett enda virtuellt nätverk eller skapa två olika virtuella nätverk (VNet). I den här artikeln fokuserar vi på distributionen av ett virtuellt nätverk som delas in i två undernät. Det här är en enklare metod som två separata Vnet kräver en gateway för virtuellt nätverk tooVNet för kommunikation.

**1.1 Skapa det virtuella nätverket**

![Skapa det virtuella nätverket](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)

I hello Azure-portalen, kan Välj virtuella nätverk och du distribuera hello virtuella nätverk och ett undernät omedelbart med ett enda klick. INT-undernätet har definierats och är nu klar för virtuella datorer toobe lagts till.
hello nästa steg är tooadd ett annat undernät toohello nätverk, d.v.s. hello DMZ undernät. toocreate hello DMZ undernät, bara

* Välj hello nyskapad nätverk
* Välj undernät i hello egenskaper
* I hello undernät Kontrollpanelen klickar du på hello knappen Lägg till
* Ange hello undernät namn och adress utrymme information toocreate hello undernät

![Undernät](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)

![DMZ-undernät](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. Skapa hello nätverk säkerhetsgrupper**

En nätverkssäkerhetsgrupp (NSG) innehåller en lista över regler för åtkomstkontrollistan (ACL) som tillåter eller nekar nätverkstrafik tooyour VM-instanser i ett virtuellt nätverk. NSG:er kan antingen associeras med undernät eller individuella VM-instanser inom det undernätet. När en NSG är associerad med ett undernät, gäller hello ACL-regler tooall hello VM-instanser i det undernätet.
För hello syftet med den här vägledningen, skapar vi två NSG: er: en för ett internt nätverk och en DMZ. De tilldelas namnen NSG_INT och NSG_DMZ.

![Skapa en nätverkssäkerhetsgrupp](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Efter hello NSG har skapats, kan det finnas 0 inkommande och 0 utgående regler. När hello roller på respektive hello-servrar är installerat och fungerar, kan sedan hello inkommande och utgående regler göras bl.a toohello önskad nivå av säkerhet.

![Initiera nätverkssäkerhetsgruppen](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

När hello NSG: er har skapats kan du associera NSG_INT med undernät INT och NSG_DMZ med undernät DMZ. Här är ett skärmbildsexempel:

![Konfigurera nätverkssäkerhetsgruppen](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Klicka på undernät tooopen hello panelen för undernät
* Välj hello undernätet tooassociate med hello NSG 

Efter konfigurationen kan hello panelen för undernät bör se ut som nedan:

![Undernät efter NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. Skapa anslutning tooon lokala**

Vi behöver en anslutning tooon lokala i ordning toodeploy hello-domänkontrollant (DC) i azure. Azure erbjuder olika anslutningen alternativ tooconnect din lokala infrastruktur tooyour Azure-infrastrukturen.

* Punkt-till-plats
* Plats-till-plats för Virtual Network
* ExpressRoute

Det rekommenderas toouse ExpressRoute. Med ExpressRoute kan du skapa privata anslutningar mellan Azures datacenter och infrastruktur som finns lokalt eller i en samplaceringsmiljö. ExpressRoute-anslutningar inte överskrider hello offentliga Internet. De erbjuder flera tillförlitlighet, högre hastighet, lägre latens och högre säkerhet än vanliga anslutningar över hello Internet.
Du kan välja alla anslutningsmetod som passar bäst för din organisation medan toouse ExpressRoute rekommenderas. Mer information om ExpressRoute- och hello toolearn olika anslutningsalternativ med ExpressRoute, läsa [teknisk översikt för ExpressRoute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Skapa lagringskonton
I ordning toomaintain hög tillgänglighet och undvika beroende av ett enda storage-konto kan du skapa två storage-konton. Dela hello datorer i varje tillgänglighetsuppsättning i två grupper och tilldelar sedan varje grupp ett separat lagringskonto.

![Skapa lagringskonton](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Skapa tillgänglighetsuppsättningar
Skapa tillgänglighetsuppsättningar som innehåller 2 datorer varje hello minsta för varje roll (DC/AD FS och WAP). På så sätt kan du uppnå högre tillgänglighet för varje roll. Skapa hello tillgänglighetsuppsättningar, men det är viktigt toodecide hello följande:

* **Fault domäner**: virtuella datorer i hello samma fel domän delar hello samma kraftkälla och fysisk nätverksväxel. Minst två feldomäner rekommenderas. hello standardvärdet är 3 och du kan lämna den som den är hello syftet med den här distributionen
* **Uppdatera domäner**: datorer som tillhör toohello samma uppdateringsdomän startas om tillsammans under en uppdatering. Vill du toohave minst 2 uppdatering domäner. hello standardvärdet är 5 och du kan lämna den som den är hello syftet med den här distributionen

![Tillgänglighetsuppsättningar](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Skapa hello efter tillgänglighetsuppsättningar

| Tillgänglighetsuppsättning | Roll | Feldomäner | Uppdateringsdomäner |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Distribuera virtuella datorer
hello nästa steg är toodeploy virtuella datorer som är värd för hello olika roller i din infrastruktur. Minst två datorer rekommenderas i varje tillgänglighetsuppsättning. Skapa fyra virtuella datorer för grundläggande hello-distribution.

| Dator | Roll | Undernät | Tillgänglighetsuppsättning | Lagringskonto | IP-adress |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Statisk |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Statisk |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Statisk |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Statisk |

Som du kanske har märkt har ingen nätverkssäkerhetsgrupp (NSG) angetts. Det beror på att azure kan du använda NSG på hello undernätverksnivå. Sedan kan du styra nätverkstrafiken för datorn med hjälp av hello enskilda NSG tillhör antingen hello undernät annars hello NIC-objekt. Läs mer i [Vad är en nätverkssäkerhetsgrupp (NSG)?](https://aka.ms/Azure/NSG)
Statisk IP-adress rekommenderas om du hanterar hello DNS. Du kan använda Azure DNS och i stället referera toohello nya datorer med sina Azure FQDN: er i hello DNS-poster för din domän.
Virtuell dator-fönstret bör se ut som nedan när hello distributionen är klar:

![Distribuerade virtuella datorer](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-hello-domain-controller--ad-fs-servers"></a>5. Konfigurera hello domänkontrollant / AD FS-servrar
 I ordning tooauthenticate behöver varje inkommande begäran, AD FS toocontact hello-domänkontrollant. toosave hello kostsamma resa från Azure tooon lokal Domänkontrollant för autentisering, rekommenderas toodeploy en replik av hello domänkontrollanten i Azure. I ordning tooattain hög tillgänglighet rekommenderas toocreate en tillgänglighetsuppsättning minst 2-domänkontrollanter.

| Domänkontrollant | Roll | Lagringskonto |
|:---:|:---:|:---:|
| contosodc1 |Replik |contososac1 |
| contosodc2 |Replik |contososac2 |

* Flytta upp hello två servrar som domänkontrollanter för repliken med DNS
* Konfigurera hello AD FS-servrar genom att installera hello AD FS-serverrollen med hjälp av Serverhanteraren hello.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. Distribuera en intern belastningsutjämnare (ILB)
**6.1. Skapa hello ILB**

toodeploy en ILB väljer belastningsutjämnare i hello Azure-portalen och klicka på Lägg till (+).

> [!NOTE]
> Om du inte ser **belastningsutjämnare** i din-menyn klickar du på **Bläddra** i hello nedre vänstra hello-portalen och Bläddra tills du ser **belastningsutjämnare**.  Klicka på hello gul stjärna tooadd den tooyour-menyn. Nu välja hello ny ikon tooopen hello panelen toobegin belastningsutjämnarkonfiguration av hello belastningsutjämnare.
> 
> 

![Bläddra till belastningsutjämnaren](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Namnet**: ge alla lämpligt namn toohello belastningsutjämnare
* **Schemat**: eftersom den här belastningsutjämnaren placeras framför hello AD FS-servrar och är avsett för internt nätverksanslutningar endast kan du välja ”interna”
* **Virtuellt nätverk**: Välj hello virtuellt nätverk där du distribuerar AD FS
* **Undernät**: Välj hello här internt undernät
* **IP-adresstilldelning**: Statisk

![Intern belastningsutjämnare](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)

När du klickar på Skapa och hello ILB har distribuerats, bör du se den i hello lista över belastningsutjämnare:

![Belastningsutjämnare efter ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)

Nästa steg är tooconfigure hello serverdelspool och hello backend-avsökning.

**6.2. Konfigurera serverdelspoolen för den interna belastningsutjämnaren**

Välj hello nyskapad ILB hello belastningsutjämnare Kontrollpanelen. Hello Inställningar Kontrollpanelen öppnas. 

1. Välj serverdelspooler hello inställningar panelen
2. Lägg till backend-pool-panelen i hello, klicka på Lägg till virtuell dator
3. Nu visas en panel där du kan välja tillgänglighetsuppsättning.
4. Välj hello AD FS tillgänglighetsuppsättning

![Konfigurera serverdelspoolen för den interna belastningsutjämnaren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)

**6.3. Konfigurera avsökning**

Markera avsökningar hello ILB inställningar på panelen.

1. Klicka på Lägg till.
2. Ange information för avsökningen a. **Namn**: Avsökningens namn. b. **Protokoll**: TCP. c. **Port**: 443 (HTTPS). d. **Intervallet**: 5 (standardvärdet) – detta är hello intervallet då ILB ska söka hello datorer i serverdelspoolen hello e. **Tröskelvärde för ohälsosamt värde gränsen**: 2 (standard val ue) – detta är avsökningsfel efter vilken ILB deklarerar en dator i hello backend poolen inte svarar och stoppa skicka trafik tooit hello tröskelvärdet.

![Konfigurera ILB-avsökning](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)

**6.4. Skapa regler för belastningsutjämning**

Hej ILB ska konfigureras med regler för belastningsutjämning i ordning tooeffectively Utjämna hello trafiken. I ordning toocreate en belastningsutjämningsregel 

1. Välj regel hello inställningar panelen av hello ILB för belastningsutjämning
2. Klicka på Lägg till i hello belastningen belastningsutjämning regeln Kontrollpanelen
3. Läsa in belastningsutjämning regeln panelen i hello Lägg till en. **Namnet**: Ange ett namn för regeln hello b. **Protokoll**: Välj TCP. c. **Port**: 443. d. **Serverport**: 443. e. **Serverdelspool**: Välj hello-pool som du skapade för hello AD FS tidigare f. **Avsökningen**: Välj hello avsökningen skapade tidigare för AD FS-servrar

![Konfigurera ILB-belastningsutjämningsregler](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. Uppdatera DNS med ILB**

Gå tooyour DNS-servern och skapa en CNAME-post för hello ILB. hello CNAME ska vara för hello federationstjänsten med hello IP-adress som pekar toohello hello ILB IP-adress. Till exempel om hello ILB DIP-adressen är 10.3.0.8 och hello federationstjänsten som installeras är fs.contoso.com, skapar du en CNAME-post för fs.contoso.com pekar too10.3.0.8.
Se till att alla kommunikation angående fs.contoso.com hamnar på hello ILB och är korrekt vidare.

### <a name="7-configuring-hello-web-application-proxy-server"></a>7. Konfigurera hello webbprogramproxyservern
**7.1. Konfigurera hello Web Application Proxy-servrar tooreach AD FS-servrar**

I ordning tooensure som webbprogramproxyservrarna skapa kan tooreach hello AD FS-servrar bakom hello ILB, en post i hello %systemroot%\system32\drivers\etc\hosts för hello ILB. Observera att hello unikt namn (DN) ska hello federationstjänstens namn, till exempel fs.contoso.com. Och hello IP-post ska vara som hello ILB IP-adress (10.3.0.8 som hello exempel).

**7.2. Installera rollen för hello Web Application Proxy**

När du har kontrollerat att webbprogramproxyservrarna är kan tooreach hello AD FS-servrar bakom ILB kan du sedan installera hello Web Application Proxy-servrar. Web Application Proxy-servrar behöver inte vara domänansluten toohello domän. Installera hello Web Application Proxy roller på hello två Web Application Proxy-servrar genom att välja hello fjärråtkomst-rollen. hello Serverhanteraren vägleder dig toocomplete hello WAP installation.
Mer information om hur toodeploy WAP, läsa [installera och konfigurera hello Webbprogramproxyservern](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-hello-internet-facing-public-load-balancer"></a>8.  Distribuera hello Internet Facing (offentlig) belastningsutjämnare
**8.1.  Skapa en Internetuppkopplad (offentlig) belastningsutjämnare**

Välj belastningsutjämnare i hello Azure-portalen, och klicka sedan på Lägg till. Ange hello följande information i hello skapa belastningen belastningsutjämnaren Kontrollpanelen

1. **Namnet**: namn för hello belastningsutjämnare
2. **Schema**: Offentligt – det här alternativet anger att belastningsutjämnaren behöver en offentlig adress.
3. **IP-adress**: Skapa en ny IP-adress (dynamisk).

![Internetuppkopplad belastningsutjämnare](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Efter distributionen visas hello belastningsutjämnare i hello belastningen belastningsutjämnare lista.

![Lista med belastningsutjämnare](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)

**8.2. Tilldela en DNS-etikett toohello offentliga IP-adress**

Klicka på hello nyskapad belastningen belastningsutjämnaren post i hello belastningen belastningsutjämnare panelen toobring in hello panelen för konfigurationen. Följ under steg tooconfigure hello DNS-etikett för hello offentlig IP-adress:

1. Klicka på hello offentlig IP-adress. Då öppnas hello panelen för hello offentlig IP-adress och dess inställningar
2. Klicka på Konfiguration.
3. Ange en DNS-etikett. Detta blir hello offentliga DNS-etikett som du kan komma åt från var som helst, till exempel contosofs.westus.cloudapp.azure.com. Du kan lägga till en post i hello extern DNS för hello federationstjänsten (till exempel fs.contoso.com) som löser toohello DNS-etikett hello extern belastningsutjämnare (contosofs.westus.cloudapp.azure.com).

![Konfigurera en Internetuppkopplad belastningsutjämnare](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Konfigurera en Internetuppkopplad belastningsutjämnare (DNS)](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. Konfigurera serverdelspoolen för den Internetuppkopplade (offentliga) belastningsutjämnaren** 

Följ hello samma steg som skapar hello intern belastningsutjämnare tooconfigure hello serverdelspool för Internet Facing (offentlig) belastningsutjämnare som hello tillgänglighet som angetts för hello WAP-servrar. Till exempel contosowapset.

![Konfigurera serverdelspoolen för den Internetuppkopplade belastningsutjämnaren](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)

**8.4. Konfigurera avsökning**

Följ samma steg som konfigurerar hello interna tooconfigure hello belastningsutjämningsavsökning för hello serverdelspool för WAP-servrar hello.

![Konfigurera avsökningen för den Internetuppkopplade belastningsutjämnaren](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)

**8.5. Skapa belastningsutjämningsregler**

Följ samma steg som ILB tooconfigure hello belastningsutjämning regel för TCP 443 hello.

![Konfigurera belastningsutjämningsregler för den Internetuppkopplade belastningsutjämnaren](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)

### <a name="9-securing-hello-network"></a>9. Säkra hello nätverk
**9.1. Att säkra hello internt undernät**

Generellt sett måste hello enligt reglerna för tooefficiently skydda din interna undernätet (i hello ordning enligt nedan)

| Regel | Beskrivning | Flöde |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Tillåt hello HTTPS-kommunikation från DMZ |Inkommande |
| DenyInternetOutbound |Ingen åtkomst toointernet |Utgående |

![INT-åtkomstregler (inkommande)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[kommentar]: <> (![INT-åtkomstregler (inkommande)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [kommentar]: <> (![INT-åtkomstregler (utgående)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))

**9.2. Skydda hello DMZ undernät**

| Regel | Beskrivning | Flöde |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Tillåt HTTPS från internet toohello DMZ |Inkommande |
| DenyInternetOutbound |Något annat än HTTPS toointernet är blockerad |Utgående |

![EXT-åtkomstregler (inkommande)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[kommentar]: <> (![EXT-åtkomstregler (inkommande)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [kommentar]: <> (![EXT-åtkomstregler (utgående)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

> [!NOTE]
> Om autentisering med klientanvändarcertifikat (clientTLS-autentisering med X509-användarcertifikat) krävs så kräver AD FS att TCP-port 49443 är aktiverad för inkommande åtkomst.
> 
> 

### <a name="10-test-hello-ad-fs-sign-in"></a>10. Testa hello AD FS-inloggning
hello enklast tootest AD FS är hello IdpInitiatedSignon.aspx sidan. Hej IdpInitiatedSignOn i ordning toobe kan toodo att det är obligatoriskt tooenable för hello AD FS-egenskaper. Gör hello nedan tooverify din AD FS-konfiguration

1. Kör hello nedan cmdlet hello AD FS-servern med hjälp av PowerShell tooset som tooenabled.
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. Gå till https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx från valfri extern dator.  
3. Du bör se hello AD FS-sidan som nedan:

![Testa inloggningssidan](./media/active-directory-aadconnect-azure-adfs/test1.png)

Om inloggningen lyckas visas ett meddelande som det nedan:

![Lyckat test](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Mall för att distribuera AD FS i Azure
hello mallen distribuerar en installation av 6 machine, 2 för domänkontrollanter, AD FS och WAP.

[Distributionsmall för AD FS i Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Du kan använda ett befintligt virtuellt nätverk eller skapa ett nytt VNET när du distribuerar här mallen. hello olika parametrar som är tillgängliga för att anpassa hello distribution nedan med hello beskrivning av användning av hello-parametern i hello distributionsprocessen. 

| Parameter | Beskrivning |
|:--- |:--- |
| Plats |hello region toodeploy hello resurser till, t.ex. östra USA. |
| StorageAccountType |hello typ av hello Lagringskonto som skapats |
| VirtualNetworkUsage |Anger om ett nytt virtuellt nätverk ska skapas eller om ett befintligt ska användas |
| VirtualNetworkName |hello namnet på hello virtuellt nätverk tooCreate, obligatorisk på både befintliga eller nya användning av virtuellt nätverk |
| VirtualNetworkResourceGroupName |Anger hello hello resursgruppen där hello befintligt virtuellt nätverk finns. När du använder ett befintligt virtuellt nätverk kan blir detta en obligatorisk parameter så hello distribution kan hitta hello-ID för hello befintligt virtuellt nätverk |
| VirtualNetworkAddressRange |Hej adressintervall hello nya VNET, obligatoriska om att skapa ett nytt virtuellt nätverk |
| InternalSubnetName |hello namnet på hello internt undernät, obligatorisk på båda användningsalternativ för virtuella nätverk (ny eller befintlig) |
| InternalSubnetAddressRange |hello adressintervall hello internt undernät, som innehåller hello domänkontrollanter och ADFS-servrar, obligatorisk om att skapa ett nytt virtuellt nätverk. |
| DMZSubnetAddressRange |hello adressintervall hello dmz undernät som innehåller hello Windows application proxy-servrar, om hur du skapar ett nytt virtuellt nätverk. |
| DMZSubnetName |hello namnet på hello internt undernät, obligatorisk på båda användningsalternativ för virtuella nätverk (ny eller befintlig). |
| ADDC01NICIPAddress |hello interna IP-adress Hej första domänkontrollant, IP-adressen ska tilldelas statiskt toohello DC och måste vara en giltig ip-adress inom hello internt undernät |
| ADDC02NICIPAddress |hello interna IP-adress Hej andra domänkontrollanten, IP-adressen ska tilldelas statiskt toohello DC och måste vara en giltig ip-adress inom hello internt undernät |
| ADFS01NICIPAddress |hello interna IP-adressen hello första AD FS-servern, IP-adressen ska tilldelas statiskt toohello AD FS-servern och måste vara en giltig ip-adress inom hello internt undernät |
| ADFS02NICIPAddress |hello interna IP-adressen hello andra AD FS-servern, IP-adressen ska tilldelas statiskt toohello AD FS-servern och måste vara en giltig ip-adress inom hello internt undernät |
| WAP01NICIPAddress |hello interna IP-adress hello första WAP servern denna IP-adress ska tilldelas statiskt toohello WAP servern och måste vara en giltig ip-adress inom hello DMZ undernät |
| WAP02NICIPAddress |hello interna IP-adressen för hello andra WAP servern denna IP-adress ska tilldelas statiskt toohello WAP servern och måste vara en giltig ip-adress inom hello DMZ undernät |
| ADFSLoadBalancerPrivateIPAddress |hello interna IP-adress hello ADFS belastningsutjämnare, IP-adressen ska tilldelas statiskt toohello belastningsutjämnare och måste vara en giltig ip-adress inom hello internt undernät |
| ADDCVMNamePrefix |Virtual Machine-namnprefixet för domänkontrollanter |
| ADFSVMNamePrefix |Virtual Machine-namnprefixet för AD FS-servrar |
| WAPVMNamePrefix |Virtual Machine-namnprefixet för WAP-servrar |
| ADDCVMSize |hello vm-storlek för hello-domänkontrollanter |
| ADFSVMSize |hello vm-storlek för hello ADFS-servrar |
| WAPVMSize |hello vm-storlek för hello WAP-servrar |
| AdminUserName |hello namnet på hello lokal administratör hello virtuella datorer |
| AdminPassword |hello lösenordet för hello lokala administratörskontot för hello virtuella datorer |

## <a name="additional-resources"></a>Ytterligare resurser
* [Tillgänglighetsuppsättningar](https://aka.ms/Azure/Availability) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [Interna belastningsutjämnare](https://aka.ms/Azure/ILB/Internal)
* [Internetuppkopplad belastningsutjämnare](https://aka.ms/Azure/ILB/Internet)
* [Lagringskonton](https://aka.ms/Azure/Storage)
* [Azure Virtual Networks](https://aka.ms/Azure/VNet)
* [AD FS och WAP-länkar (webbprogramproxy)](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Nästa steg
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
* [Konfigurera och hantera AD FS med Azure AD Connect](active-directory-aadconnectfed-whatis.md)
* [AD FS-distribution med hög tillgänglighet över geografiska områden i Azure med Azure Traffic Manager](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)

