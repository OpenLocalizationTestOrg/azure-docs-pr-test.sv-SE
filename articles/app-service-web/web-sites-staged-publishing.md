---
title: "aaaSet in mellanlagringsmiljöer för web apps i Azure App Service | Microsoft Docs"
description: "Lär dig hur toouse mellanlagrad publicering för web apps i Azure App Service."
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: 338424100a20bf823323313fb6699e439f367421
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Skapa mellanlagringsmiljöer i Azure App Service
<a name="Overview"></a>

När du distribuerar ditt webbprogram, webbprogram på Linux, mobila serverdel och API-app för[Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714), du kan distribuera tooa separat distributionsplats i stället för hello standard produktionsplatsen vid körning i hello **Standard**eller **Premium** läge för App Service-plan. Distributionsplatser är faktiskt live appar med sina egna värdnamn. Appen innehåll och konfigurationer element kan växlas mellan två distributionsplatser, inklusive hello produktionsplatsen. Distribuera ditt program tooa distributionsplatsen har hello följande fördelar:

* Du kan verifiera app-ändringar i en distribution mellanlagringsplatsen innan växling med hello produktionsplatsen.
* Distribuera en app tooa plats först och växla till produktion säkerställer att alla instanser av hello fack varmkörts innan som ska växlas över till produktion. Detta eliminerar avbrott när du distribuerar din app. hello trafik omdirigering sker sömlös och inga begäranden tas bort på grund av byte åtgärder. Hela arbetsflödet kan automatiseras genom att konfigurera [automatiskt växla](#Auto-Swap) när före växlingen verifiering inte behövs.
* Efter en växling har hello fack med tidigare mellanlagrade appen nu hello tidigare produktionsprogrammet. Om hello växlas över till hello produktionsplatsen ändras inte som väntat, kan du utföra hello samma Växla omedelbart tooget ”senaste kända fungerande webbplatsen” tillbaka.

Varje App Service-plan läge stöder olika antal distributionsplatser. toofind hello antal fack appens läge stöder, se [priser för Apptjänst](https://azure.microsoft.com/pricing/details/app-service/).

* När appen har flera platser, kan du inte ändra hello-läge.
* Skalning är inte tillgänglig för icke-produktoionsplats.
* Länkade resurshantering stöds inte för icke-produktoionsplats. I hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) , du kan undvika den här potentiella effekten på en produktionsplatsen genom att tillfälligt flytta hello icke produktionsplatsen tooa annat App Service-plan läge. Observera att hello icke-produktionsplatsen igen måste dela hello samma läge med hello produktionsplatsen innan du kan växla hello två platser.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Lägga till en distributionsplats
hello app måste köras i hello **Standard** eller **Premium** läge i ordning för du tooenable flera distributionsplatser.

1. I hello [Azure Portal](https://portal.azure.com/), öppna appens [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Välj hello **distributionsfack** alternativ och klicka sedan på **Lägg till plats**.
   
    ![Lägg till en ny distributionsplats][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Om hello app inte är redan i hello **Standard** eller **Premium** läge, visas ett meddelande som anger hello stöds lägen för att aktivera stegvis publicering. Nu har du hello alternativet tooselect **uppgradera** och navigera toohello **skala** fliken i din app innan du fortsätter.
   > 
   > 
3. I hello **lägga till en plats** bladet namnge hello fack och välj om tooclone appkonfiguration från en annan befintlig distributionsplatsen. Klicka på hello markerat toocontinue.
   
    ![Konfigurationskälla][ConfigurationSource1]
   
    hello första gången som du lägger till en plats du bara har två alternativ: klona konfigurationen från hello standard plats i produktion eller inte alls.
    När du har skapat flera platser, kommer du att kunna tooclone konfigurationen från en plats än hello något i produktion:
   
    ![Konfiguration av datakällor][MultipleConfigurationSources]
4. I resursbladet för din app, klickar du på **distributionsfack**, klicka på en distribution fack tooopen som sätts resursbladet med en uppsättning mått och konfiguration precis som alla andra appar. hello hello fack visas överst hello i hello bladet tooremind du du visar hello distributionsplatsen.
   
    ![Distribution fack rubrik][StagingTitle]
5. Klicka på hello app-URL i hello fack-bladet. Observera hello distributionsplatsen har sin egen värdnamn och är också en live-app. toolimit offentlig åtkomst toohello distributionsplatsen, se [App Service Web App – blockera web access toonon produktion distributionsplatser](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Det finns inget innehåll efter distributionen fack har skapats. Du kan distribuera toohello plats från en annan databas gren eller en helt annan databas. Du kan också ändra hello platskonfigurationen. Använd hello Publicera profil eller distribution av autentiseringsuppgifterna som associeras med hello distributionsplatsen för uppdateringar av innehållet.  Du kan till exempel [publicera toothis fack med git](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a>Konfigurationen för distributionsplatser
När du klonar konfigurationen från en annan distributionsplats kan hello klonade konfiguration redigeras. Dessutom följer vissa konfigurationselement hello innehåll över växlingsutrymme (inte port specifika) medan andra konfigurationselement behålls i hello samma fack efter en växling (specifika-port). hello visar följande lista hello-konfiguration som ändras när du växlar platser.

**Inställningar som bytts**:

* Allmänna inställningar – till exempel framework-version, 32/64-bitars Web sockets
* App-inställningar (kan vara konfigurerade toostick tooa plats)
* Anslutningssträngar (kan vara konfigurerade toostick tooa plats)
* Hanterarmappningar
* Inställningar för övervakning och diagnostik
* WebJobs innehåll

**Inställningar som inte har bytts**:

* Publishing slutpunkter
* Anpassade domännamn
* SSL-certifikat och bindningar
* Skalinställningarna
* WebJobs planeringsprogram

tooconfigure en app inställning eller ett anslutningsfel sträng toostick tooa plats (inte växlas) åtkomst hello **programinställningar** bladet för en viss plats, och sedan väljer hello **fack inställningen** för hello konfigurationselement som ska behålla hello plats. Observera att markera en konfigurationselement som fack specifika påverkar hello för att fastställa att element som inte kan över alla hello-distributionsplatser som är associerade med hello app.

![Platsinställningar][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Växla distributionsplatser 
Du kan växla distributionsplatser i hello **översikt** eller **distributionsplatser** vy över resursbladet för din app.

> [!IMPORTANT]
> Innan du byter en app från en distributionsplats till produktion, se till att alla icke-specifika platsinställningar konfigureras precis som du vill toohave i hello växlingen mål.
> 
> 

1. tooswap distributionsplatser, klicka på hello **växla** i hello kommandofältet hello appen eller i hello kommandofältet för en distributionsplats.
   
    ![Växla knapp][SwapButtonBar]

2. Kontrollera att växlingen hello riktade käll- och växling är korrekt. Hello växlingen mål är vanligtvis hello produktionsplatsen. Klicka på **OK** toocomplete hello igen. Hello distributionsplatser har bytts när hello-åtgärden har slutförts.

    ![Fullständig växling](./media/web-sites-staged-publishing/SwapImmediately.png)

    För hello **växlingen med preview** växla typ, se [växlingen med preview (flera fasen swap)](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>Växla med förhandsgranskning (flera fasen swap)

Växlingen med Förhandsgranska eller flera fasen växlingen förenkla verifieringen av fack konfigurationselement, till exempel anslutningssträngar.
För verksamhetskritiska arbetsbelastningar, som du vill toovalidate som hello appen fungerar som förväntat när hello produktion platsens konfiguration används och du måste utföra dessa verifiering *innan* hello app växlas till produktionen. Växlingen med preview är vad du behöver.

> [!NOTE]
> Växlingen med preview stöds inte i web apps i Linux.

När du använder hello **växla med förhandsgranskning** alternativet (se [växla distributionsplatser](#Swap)), Apptjänst hello följande:

- Behåller hello mål fack oförändrat så befintliga arbetsbelastningen på platsen (t.ex. produktion) påverkas inte.
- Tillämpar hello konfigurationselement för hello fack toohello källa destinationsplatsen, inklusive hello fack-specifika anslutningssträngar och app-inställningar.
- Startar om hello arbetsprocesser på hello källplatsen med hjälp av dessa ovannämnda konfigurationselement.
- När du har slutfört hello växlingsutrymme: flyttar hello Förproduktion warmed upp källplatsen till hello destinationsplatsen. hello destinationsplatsen flyttas till hello källplatsen som en manuell växling.
- När du avbryter hello växlingsutrymme: tillämpa hello konfigurationselement för hello fack toohello källa källplatsen.

Du kan förhandsgranska exakt hur hello app att fungera med hello destinationsplatsens konfiguration. När du slutför verifieringen Slutför hello växling i ett separat steg. Det här steget har hello fördel att hello källplatsen är redan varmkörts med hello önskad konfiguration och klienter får inte någon avbrottstid.  

Prover för hello Azure PowerShell-cmdlets som är tillgängliga för flera fasen växlingen ingår i hello Azure PowerShell-cmdlets för distribution av platser.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Konfigurera automatisk växling
Automatisk växling förenklar DevOps-scenarier där du vill att toocontinuously distribuera din app med noll kallstart och driftstopp för slutanvändare och kunder hello-appen. När en distributionsplats konfigureras för automatisk växling till produktion, varje gång du överför din kod uppdatering toothat fack ska Apptjänst automatiskt växla hello app till produktion när det redan har varmkörts hello plats.

> [!IMPORTANT]
> När du aktiverar automatisk växling för en plats, kontrollera hello platskonfigurationen exakt hello-konfiguration som är avsedd för hello mål fack (vanligtvis hello produktionsplatsen).
> 
> 

> [!NOTE]
> Automatisk växling stöds inte i web apps i Linux.

Det är enkelt att konfigurera automatisk växling för en plats. Följ hello stegen nedan:

1. I **Distributionsfack**, Välj en produktionsplatsen och välj **programinställningar** i resursbladet för den platsen.  
   
    ![][Autoswap1]
2. Välj **på** för **automatiskt växla**väljer hello önskat mål plats i **automatiskt växla fack**, och klicka på **spara** i hello kommandofältet. Kontrollera att har konfigurationen för hello fack exakt hello avsedd för hello mål plats.
   
    Hej **meddelanden** fliken blinkar en grön **lyckade** när hello åtgärden är klar.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > tootest automatiskt växla för din app, kan du först välja en plats för icke-produktion mål i **automatiskt växla fack** toobecome bekant med hello-funktionen.  
   > 
   > 
3. Köra en kod push toothat distributionsplats. Automatisk växling sker efter en kort tid och hello update visas på mål-platsens URL.

<a name="Rollback"></a>

## <a name="toorollback-a-production-app-after-swap"></a>toorollback en produktionsapp efter växling
Om eventuella fel identifieras i produktion efter en växling fack återställa du hello fack tillbaka tootheir före växlingen tillstånd genom att byta hello samma två platser direkt.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Anpassade värmts före växlingen
Vissa appar kan kräva anpassade värmts åtgärder. Hej `applicationInitialization` configuration-elementet i web.config kan du toospecify initiering av anpassade åtgärder toobe utförs innan en begäran tas emot. hello växlingen väntar på den här anpassade värmts toocomplete. Här är ett exempel web.config-fragment.

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="toodelete-a-deployment-slot"></a>toodelete en distributionsplats
I hello bladet för en distributionsplats, öppna hello distribution fack-bladet klickar du på **översikt** (hello standardsidan) och klicka på **ta bort** i hello kommandofältet.  

![Ta bort en distributionsplats][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a>Azure PowerShell-cmdlets för distributionsplatser
Azure PowerShell är en modul som tillhandahåller cmdlets toomanage Azure via Windows PowerShell, bland annat stöd för att hantera distributionsplatser i Azure App Service.

* Information om att installera och konfigurera Azure PowerShell och på autentisera Azure PowerShell med Azure-prenumerationen finns [hur tooinstall och konfigurera Microsoft Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Skapa en webbapp
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Skapa en distributionsplats
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-toosource-slot"></a>Påbörja en växling med granskning (med swap) och tillämpa toosource destinationsplatsen fack konfiguration
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Avbryta en väntande växling (swap med granskning) och återställa platskonfigurationen för källa
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Växla distributionsplatser
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a>Ta bort distributionsplatsen
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a>Azure-kommandoradsgränssnittet (Azure CLI)-kommandon för distributionsplatser
hello Azure CLI finns plattformsoberoende kommandon för att arbeta med Azure, inklusive stöd för att hantera App Service-distributionsplatser.

* För instruktioner om hur du installerar och konfigurerar hello Azure CLI, inklusive information om hur tooconnect Azure CLI tooyour Azure-prenumeration, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md).
* toolist hello kommandon för Azure App Service i hello Azure CLI, anropa `azure site -h`.

> [!NOTE] 
> För [Azure CLI 2.0](https://github.com/Azure/azure-cli) kommandon för distributionsplatser, se [az apptjänst web distributionsplatsen](/cli/azure/appservice/web/deployment/slot).

- - -
### <a name="azure-site-list"></a>Azure platslista
Information om hello appar i hello nuvarande prenumeration anropa **azure platslista**, som i följande exempel hello.

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a>Skapa Azure-webbplats
toocreate en distributionsplats anropa **azure plats skapa** och ange hello namn för en befintlig app och hello på hello fack toocreate, som i följande exempel hello.

`azure site create webappslotstest --slot staging`

tooenable källkontrollen för hello ny plats, Använd hello **--git** enligt följande exempel hello.

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a>Azure site växlingsutrymme
toomake hello uppdaterade distribution fack hello produktionsprogrammet använder hello **azure plats växlingen** kommandot tooperform en växlingsåtgärd, som i följande exempel hello. hello produktionsprogrammet får inte någon stillestånd eller ska genomgå kallstart.

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a>ta bort Azure-webbplats
toodelete en distributionsplats som inte längre behövs, Använd hello **ta bort azure site** kommandot, som i följande exempel hello.

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> Se hur en webbapp fungerar i praktiken. [Prova App Service](https://azure.microsoft.com/try/app-service/) omedelbart och skapa en tillfällig startapp – inget kreditkort behövs, inga åtaganden.
> 
> 

## <a name="next-steps"></a>Nästa steg
[Azure App Service Web App – blockera web access toonon produktion distributionsplatser](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[introduktion tooApp-tjänsten på Linux](./app-service-linux-intro.md)
[kostnadsfri utvärderingsversion av Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

