---
title: "aaaMy första grafisk runbook i Azure Automation | Microsoft Docs"
description: "Självstudiekurs med stegvisa hello skapa, testa och publicering av en enkel grafisk runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: runbook, runbook-mall, runbook-automation, azure runbook
ms.assetid: dcb88f19-ed2b-4372-9724-6625cd287c8a
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/17/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 964cf8bae75ca597959bfc39b2b07c22bbc9acb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="my-first-graphical-runbook"></a>Min första grafiska runbook

> [!div class="op_single_selector"]
> * [Grafisk](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-arbetsflöde](automation-first-runbook-textual.md)
> 
> 

Den här självstudiekursen vägleder dig genom hello skapandet av en [grafisk runbook](automation-runbook-types.md#graphical-runbooks) i Azure Automation.  Vi börjar med en enkel runbook som testar och publicerar medan vi förklarar hur tootrack hello hello runbook jobbets status.  Vi ändra hello runbook tooactually hantera Azure-resurser, i det här fallet startar en virtuell Azure-dator.  Sedan kursen vi hello genom att göra hello runbook stabilare genom att lägga till runbook-parametrar och villkorliga länkar.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du följande hello.

* En Azure-prenumeration.  Om du inte redan har ett konto kan du [aktivera dina MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller <a href="/pricing/free-account/" target="_blank">[registrera dig för ett kostnadsfritt konto](https://azure.microsoft.com/free/).
* [Azure Automation-konto](automation-sec-configure-azure-runas-account.md) toohold hello runbook och autentisera tooAzure resurser.  Det här kontot måste ha behörighet toostart och stoppa hello virtuella datorn.
* En virtuell dator i Azure.  Eftersom vi ska stoppa och starta den här datorn bör den inte finnas i produktionsmiljön.

## <a name="step-1---create-runbook"></a>Steg 1 – Skapa en runbook
Vi börjar med att skapa en enkel runbook som matar ut hello text *Hello World*.

1. Hello Azure-portalen, öppna ditt Automation-konto.  
   sidan för hello Automation-konto får du en snabb överblick över hello resurser i det här kontot.  Du bör redan ha vissa tillgångar.  De flesta av dessa är hello-moduler som ingår automatiskt i ett nytt Automation-konto.  Du bör också ha hello autentiseringsuppgiftstillgång som nämns i hello [krav](#prerequisites).
2. Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.<br> ![Runbook-kontroll](media/automation-first-runbook-graphical/runbooks-resources-tile.png)
3. Skapa en ny runbook genom att klicka på hello **lägga till en runbook** knappen och sedan **skapa en ny runbook**.
4. Namnge hello runbook hello *MyFirstRunbook grafiska*.
5. I det här fallet ska vi toocreate en [grafisk runbook](automation-graphical-authoring-intro.md) så väljer **grafisk** för **runbooktyp**.<br> ![Ny runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Klicka på **skapa** toocreate hello runbook och öppna hello grafiska redigerare.

## <a name="step-2---add-activities-toohello-runbook"></a>Steg 2 – Lägg till aktiviteter toohello runbook
hello biblioteket kontrollen hello vänster på hello editor kan tooselect aktiviteter tooadd tooyour runbook.  Vi tooadd en **Write-Output** cmdlet toooutput text från hello runbook.

1. I hello biblioteket kontroll, klickar du på hello Sök textruta och skriv **Write-Output**.  hello sökresultat visas nedan. <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
2. Rulla ned toohello längst ned i listan över hello.  Du kan antingen högerklickar du på **Write-Output** och välj **lägga till toocanvas** Klicka hello ellips nästa toohello cmdlet och sedan välja **lägga till toocanvas**.
3. Klicka på hello **Write-Output** aktivitet på hello arbetsytan.  Då öppnas hello Configuration kontroll bladet, vilket gör att du tooconfigure hello aktivitet.
4. Hej **etikett** toohello namnet på hello cmdlet som standard, men vi kan ändra den toosomething mer användarvänlig. Ändra också*skriva Hello World toooutput*.
5. Klicka på **parametrar** tooprovide värden för hello cmdlet-parametrar.  
   Vissa cmdletar har flera parameteruppsättningar, och du behöver tooselect vilka du en toouse. I det här fallet **Write-Output** har endast en parameter, så inte behöver du tooselect en. <br> ![Write-Output-egenskaper](media/automation-first-runbook-graphical/write-output-properties-b.png)
6. Välj hello **InputObject** parameter.  Det här är hello parameter där vi ange hello text toosend toohello utdataströmmen.
7. I hello **datakällan** listrutan **PowerShell-uttryck**.  Hej **datakällan** listrutan innehåller olika källor att du använder toopopulate ett parametervärde.  
   Du kan använda utdata från sådana källor som en annan aktivitet, en Automation-tillgång eller ett PowerShell-uttryck.  I det här fallet vi vill bara toooutput hello text *Hello World*. Vi kan använda ett PowerShell-uttryck och ange en sträng.
8. I hello **uttryck** skriver *”Hello World”* och klicka sedan på **OK** två gånger tooreturn toohello arbetsytan.<br> ![PowerShell-uttryck](media/automation-first-runbook-graphical/expression-hello-world.png)
9. Spara hello runbook genom att klicka på **spara**.<br> ![Spara runbook](media/automation-first-runbook-graphical/runbook-toolbar-save-revised20165.png)

## <a name="step-3---test-hello-runbook"></a>Steg 3 – runbook-testet hello
Innan vi publicerar hello runbook toomake den tillgänglig i produktion, vi vill tootest den toomake till att den fungerar korrekt.  När du testar en runbook kör du dess **utkastversion** och visar dess utdata interaktivt.

1. Klicka på **Test fönstret** tooopen hello Test bladet.<br> ![Testfönstret](media/automation-first-runbook-graphical/runbook-toolbar-test-revised20165.png)
2. Klicka på **starta** toostart hello test.  Detta bör vara hello endast aktiverade alternativet.
3. En [runbook-jobbet](automation-runbook-execution.md) skapas och dess status visas i fönstret hello.  
   hello jobbstatus startar som *i kö* som anger att den väntar på en runbook worker i hello molnet toobecome tillgängliga.  Sedan flyttas för*Start* när en arbetsprocess anspråk hello jobb, och sedan *kör* när hello runbook faktiskt börjar köras.  
4. När hello runbook-jobbet är klart visas dess utdata. I detta fall bör du se *Hello World*.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
5. Stäng hello Test bladet tooreturn toohello arbetsytan.

## <a name="step-4---publish-and-start-hello-runbook"></a>Steg 4 – publicera och starta hello runbook
hello-runbook som vi skapade är fortfarande i utkastläge. Vi behöver toopublish den innan vi kan köra den i produktion.  När du publicerar en runbook kan du skriva över hello befintliga publicerade versionen med hello utkastet.  I vårt fall har vi inte en publicerad version ännu eftersom vi just skapade hello runbook.

1. Klicka på **publicera** toopublish hello runbook och sedan **Ja** när du tillfrågas.<br> ![Publicera](media/automation-first-runbook-graphical/runbook-toolbar-publish-revised20166.png)
2. Om du bläddrar vänstra tooview hello runbook i hello **Runbooks** bladet visar en **redigering Status** av **publicerade**.
3. Rulla tillbaka toohello rätt tooview hello bladet för **MyFirstRunbook**.  
   hello alternativ överst hello kan vi toostart hello runbook, schemalägga toostart någon gång hello framtida eller skapa en [webhook](automation-webhooks.md) så att den kan startas via en HTTP-anrop.
4. Vi vill bara toostart hello runbook så klicka på **starta** och sedan **Ja** när du tillfrågas.<br> ![Starta runbook](media/automation-first-runbook-graphical/runbook-controls-start-revised20165.png)
5. Ett jobb blad öppnas för hello runbook-jobb som vi skapade.  Du kan stänga det här bladet, men i det här fallet vi lämna den öppen så att vi kan titta på hello jobb pågår.
6. hello jobbstatus visas i **jobbsammanfattning** och matchar hello status som vi såg när vi testade hello runbook.<br> ![Jobbsammanfattning](media/automation-first-runbook-graphical/runbook-job-summary.png)
7. En gång hello runbook status visas *slutförd*, klickar du på **utdata**. Hej **utdata** öppnas bladet och vi ser vår *Hello World* hello i fönstret.<br> ![Jobbsammanfattning](media/automation-first-runbook-graphical/runbook-job-output.png)  
8. Stäng hello utdata-bladet.
9. Klicka på **alla loggar** tooopen hello dataströmmar bladet för hello runbook-jobbet.  Vi ska bara se *Hello World* i hello utdata stream, men detta kan du visa andra dataströmmar för en runbook-jobb, till exempel utförlig och fel om hello runbook skriver toothem.<br> ![Jobbsammanfattning](media/automation-first-runbook-graphical/runbook-job-AllLogs.png)
10. Stäng hello alla loggar bladet och hello jobbet bladet tooreturn toohello MyFirstRunbook bladet.
11. Klicka på **jobb** tooopen hello jobb-bladet för runbook.  Detta visar alla hello jobben som skapats av denna runbook. Vi ska bara se ett jobb visas eftersom vi bara körde hello jobbet en gång.<br> ![Jobb](media/automation-first-runbook-graphical/runbook-control-jobs.png)
12. Du kan klicka på det här jobbet tooopen hello samma jobb-fönstret som vi visas när vi startade hello runbook.  Detta ger dig toogo bakåt i tiden och visa hello-information för alla jobb som har skapats för en viss runbook.

## <a name="step-5---create-variable-assets"></a>Steg 5 – Skapa variabler för tillgångar
Vi har testat och publicerat vår runbook, men hittills gör den egentligen inget användbart. Vi vill toohave den hantera Azure-resurser.  Innan vi konfigurerar hello runbook tooauthenticate vi skapa en variabel toohold hello prenumerations-ID och referera till den när vi har skapat hello aktiviteten tooauthenticate i steg 6 nedan.  Inklusive en kontext som referens toohello prenumeration kan du tooeasily arbete mellan flera prenumerationer.  Kopiera ditt prenumerations-ID från hello prenumerationer alternativet hello navigeringsfönstret innan du fortsätter.  

1. I bladet för hello Automation-konton klickar du på hello **tillgångar** panelen och hello **tillgångar** blad öppnas.
2. Klicka på hello i hello tillgångar bladet **variabler** panelen.
3. På bladet för hello variabler, klickar du på **lägga till en variabel**.<br>![Automation-variabel](media/automation-first-runbook-graphical/create-new-subscriptionid-variable.png)
4. I hello ny variabel bladet i hello **namn** ange **AzureSubscriptionId** och i hello **värdet** ange ditt prenumerations-ID.  Behåll *sträng* för hello **typen** och hello standardvärde för **kryptering**.  
5. Klicka på **skapa** toocreate hello variabeln.  

## <a name="step-6---add-authentication-toomanage-azure-resources"></a>Steg 6 – Lägg till autentisering toomanage Azure-resurser
Nu när vi har en variabel toohold våra prenumerations-ID kan vi konfigurera vår runbook tooauthenticate med hello kör som-autentiseringsuppgifter som är refererad tooin hello [krav](#prerequisites).  Vi gör det genom att lägga till hello Azure kör som anslutningen **tillgången** och **Add-AzureRMAccount** cmdlet toohello arbetsytan.  

1. Öppna hello grafiska redigerare genom att klicka på **redigera** på hello MyFirstRunbook blad.<br> ![Redigera runbook](media/automation-first-runbook-graphical/runbook-controls-edit-revised20165.png)
2. Vi behöver inte hello **skriva Hello World toooutput** längre så Högerklicka och välj **ta bort**.
3. Hello biblioteket kontroll, expandera **anslutningar** och Lägg till **AzureRunAsConnection** toohello arbetsytan genom att välja **lägga till toocanvas**.
4. På arbetsytan hello och väljer **AzureRunAsConnection** och Skriv i Kontrollpanelen för hello Configuration **hämta kör som-anslutning** i hello **etikett** textruta.  Det här är hello-anslutning
5. Skriv i hello biblioteket kontroll, **Add-AzureRmAccount** hello Sök textrutan med.
6. Lägg till **Add-AzureRmAccount** toohello arbetsytan.<br> ![Add-AzureRMAccount](media/automation-first-runbook-graphical/search-powershell-cmdlet-addazurermaccount.png)
7. Hovra över **hämta kör som-anslutning** tills en cirkel visas på hello längst ned på hello formen. Klicka på hello cirkeln och dra hello pilen för**Add-AzureRmAccount**.  hello pilen som du skapade är en *länk*.  Hej runbook startas med **hämta kör som-anslutning** och kör sedan **Add-AzureRmAccount**.<br> ![Skapa länk mellan aktiviteter](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
8. På arbetsytan hello och väljer **Add-AzureRmAccount** och styra rutan typ i hello Configuration **inloggning tooAzure** i hello **etikett** textruta.
9. Klicka på **parametrar** och hello aktivitet parametern Configuration bladet visas.
10. **Lägg till AzureRmAccount** har flera parameteruppsättningar, så vi måste tooselect något innan vi kan ange parametervärden.  Klicka på **parameterinställning** och välj sedan hello **ServicePrincipalCertificatewithSubscriptionId** parameteruppsättning.
11. När du har valt hello parameteruppsättning visas hello parametrar i hello aktivitet parametern Configuration bladet.  Klicka på **APPLICATIONID**.<br> ![Lägg till Azure RM-kontoparametrar](media/automation-first-runbook-graphical/add-azurermaccount-params.png)
12. Hello parametervärdet bladet välj **aktivitetsutdata** för hello **datakällan** och välj **hämta kör som-anslutning** hello listan i hello **fält sökvägen** textruta typen **ApplicationId**, och klicka sedan på **OK**.  Vi anger hello namnet på hello egenskap för hello fältet sökvägen eftersom hello aktivitet matar ut ett objekt med flera egenskaper.
13. Klicka på **CERTIFICATETHUMBPRINT**, och hello parametervärdet bladet välj **aktivitetsutdata** för hello **datakällan**.  Välj **hämta kör som-anslutning** hello listan i hello **fältet sökväg** textruta typen **CertificateThumbprint**, och klicka sedan på **OK**.
14. Klicka på **SERVICEPRINCIPAL**, och i hello parametervärdet bladet välj **ConstantValue** för hello **datakällan**, klickar du på alternativet för hello **True**, och klicka sedan på **OK**.
15. Klicka på **TENANTID**, och hello parametervärdet bladet välj **aktivitetsutdata** för hello **datakällan**.  Välj **hämta kör som-anslutning** hello listan i hello **fältet sökväg** textruta typen **TenantId**, och klicka sedan på **OK** två gånger.  
16. Skriv i hello biblioteket kontroll, **Set-AzureRmContext** hello Sök textrutan med.
17. Lägg till **Set-AzureRmContext** toohello arbetsytan.
18. På arbetsytan hello väljer **Set-AzureRmContext** och styra rutan typ i hello Configuration **ange prenumerations-Id** i hello **etikett** textruta.
19. Klicka på **parametrar** och hello aktivitet parametern Configuration bladet visas.
20. **Set-AzureRmContext** har flera parameteruppsättningar, så vi måste tooselect något innan vi kan ange parametervärden.  Klicka på **parameterinställning** och välj sedan hello **SubscriptionId** parameteruppsättning.  
21. När du har valt hello parameteruppsättning visas hello parametrar i hello aktivitet parametern Configuration bladet.  Klicka på **SubscriptionID**
22. Hello parametervärdet bladet välj **Variabeltillgång** för hello **datakällan** och välj **AzureSubscriptionId** hello listan och klicka sedan på **OK**  två gånger.   
23. Hovra över **inloggning tooAzure** tills en cirkel visas på hello längst ned på hello formen. Klicka på hello cirkeln och dra hello pilen för**ange prenumerations-Id**.

Din runbook ska se ut så Hej då följande: <br>![Konfiguration av runbook-autentisering](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="step-7---add-activity-toostart-a-virtual-machine"></a>Steg 7 – Lägg till aktivitet toostart en virtuell dator
Här vi lägga till en **Start AzureRmVM** aktiviteten toostart en virtuell dator.  Du kan välja en virtuell dator i din Azure-prenumeration och för nu du hårdkoda som namn till hello cmdlet.

1. Skriv i hello biblioteket kontroll, **Start-AzureRm** hello Sök textrutan med.
2. Lägg till **Start AzureRmVM** toohello arbetsytan och klicka och dra den under **ange prenumerations-Id**.
3. Hovra över **ange prenumerations-Id** tills en cirkel visas på hello längst ned på hello formen.  Klicka på hello cirkeln och dra hello pilen för**Start AzureRmVM**.
4. Välj **Start-AzureRmVM**.  Klicka på **parametrar** och sedan **parameterinställning** tooview hello anger för **Start AzureRmVM**.  Välj hello **ResourceGroupNameParameterSetName** parameteruppsättning. Observera att **ResourceGroupName** och **Name** visas med utropstecken.  Det betyder att de är obligatoriska parametrar.  Observera också att båda förväntar strängvärden.
5. Välj **Name**.  Välj **PowerShell-uttryck** för hello **datakällan** och anger hello namn för hello virtuella omges av dubbla citattecken som vi börjar med denna runbook.  Klicka på **OK**.<br>![Parametervärde för Name i Start-AzureRmVM](media/automation-first-runbook-graphical/runbook-startvm-nameparameter.png)
6. Välj **ResourceGroupName**. Använd **PowerShell-uttryck** för hello **datakällan** och Skriv hello namn hello resursgruppens omges av dubbla citattecken.  Klicka på **OK**.<br> ![Start-AzureRmVM-parametrar](media/automation-first-runbook-graphical/startazurermvm-params.png)
7. Klicka på rutan testa så att vi kan testa hello runbook.
8. Klicka på **starta** toostart hello test.  När den är klar kan du kontrollera att hello den virtuella datorn har startats.

Din runbook ska se ut så Hej då följande: <br>![Konfiguration av runbook-autentisering](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="step-8---add-additional-input-parameters-toohello-runbook"></a>Steg 8 – Lägg till ytterligare indataparametrar toohello runbook
Vår runbook startar hello virtuell dator i hello resursgruppen som vi angav i hello **Start AzureRmVM** cmdlet.  Vår runbook är mer användbar om vi kan ange både när hello runbook startas.  Vi nu lägga till indataparametrar toohello runbook tooprovide funktionen.

1. Öppna hello grafiska redigerare genom att klicka på **redigera** på hello **MyFirstRunbook** fönstret.
2. Klicka på **ingående och utgående** och sedan **lägga till indata** tooopen hello Runbookinmatningsparameter fönstret.<br> ![Indata och utdata för runbook](media/automation-first-runbook-graphical/runbook-toolbar-InputandOutput-revised20165.png)
3. Ange *VMName* för hello **namn**.  Behåll *sträng* för hello **typen**, men ändra **obligatoriska** för*Ja*.  Klicka på **OK**.
4. Skapa en andra obligatorisk indataparameter kallas *ResourceGroupName* och klicka sedan på **OK** tooclose hello **indata och utdata** fönstret.<br> ![Indataparametrar för Runbook](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
5. Välj hello **Start AzureRmVM** aktivitet och klicka sedan på **parametrar**.
6. Ändra hello **datakällan** för **namn** för**Runbook indata** och välj sedan **VMName**.<br>
7. Ändra hello **datakällan** för **ResourceGroupName** för**Runbook indata** och välj sedan **ResourceGroupName**.<br> ![Start-AzureVM-parametrar](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
8. Spara hello runbook och öppna hello Test.  Observera att du kan nu ange värden för hello två indatavariabler som du använder i hello test.
9. Stäng hello Test-fönstret.
10. Klicka på **publicera** toopublish hello ny version av hello runbook.
11. Stoppa hello virtuell dator som du startade i hello föregående steg.
12. Klicka på **starta** toostart hello runbook.  Typen i hello **VMName** och **ResourceGroupName** för hello virtuell dator som du ska toostart.<br> ![Starta runbook](media/automation-first-runbook-graphical/runbook-start-inputparams.png)
13. När hello runbook är klar kontrollerar du att hello den virtuella datorn har startats.

## <a name="step-9---create-a-conditional-link"></a>Steg 9 – Skapa en villkorlig länk
Vi kan nu ändra hello runbook så att den endast försöker toostart hello virtuella datorn om den inte redan har startats.  Det gör du genom att lägga till en **Get-AzureRmVM** cmdlet toohello runbook som hämtar hello instans nivån hello virtuella datorns status för. Du lägga till en PowerShell-arbetsflöde kodmodulen kallas **Erhåll Status för** med ett fragment av PowerShell code toodetermine om hello virtuella datorns tillstånd är igång eller stoppad.  En villkorlig länk från hello **Erhåll Status för** modulen körs bara **Start AzureRmVM** om hello aktuella körtillstånd stoppas.  Slutligen utdata vi ett meddelande tooinform du om hello VM har startats eller inte med hello PowerShell Write-Output-cmdlet.

1. Öppna **MyFirstRunbook** i hello grafiska redigerare.
2. Ta bort hello länk mellan **ange prenumerations-Id** och **Start AzureRmVM** genom att klicka på den och sedan trycka på hello *ta bort* nyckel.
3. Skriv i hello biblioteket kontroll, **Get-AzureRm** hello Sök textrutan med.
4. Lägg till **Get-AzureRmVM** toohello arbetsytan.
5. Välj **Get-AzureRmVM** och sedan **parameterinställning** tooview hello anger för **Get-AzureRmVM**.  Välj hello **GetVirtualMachineInResourceGroupNameParamSet** parameteruppsättning.  Observera att **ResourceGroupName** och **Name** visas med utropstecken.  Det betyder att de är obligatoriska parametrar.  Observera också att båda förväntar strängvärden.
6. Under **Datakälla** för **Namn** väljer du **Indata för runbook** och väljer sedan **VMName**.  Klicka på **OK**.
7. Under **Datakälla** för **ResourceGroupName** väljer du **Indata för runbook** och väljer sedan **ResourceGroupName**.  Klicka på **OK**.
8. Under **Datakälla** för **Status** väljer du **Konstant värde** och klickar sedan på **Sant**.  Klicka på **OK**.  
9. Skapa en länk från **ange prenumerations-Id** för**Get-AzureRmVM**.
10. Hello biblioteket kontrollen Expandera **Runbook-kontroll** och Lägg till **kod** toohello arbetsytan.  
11. Skapa en länk från **Get-AzureRmVM** för**kod**.  
12. Klicka på **kod** och i hello Configuration fönstret Ändra etikett för**Erhåll Status för**.
13. Välj **kod** parametern och hello **redigerare** bladet visas.  
14. Klistra in följande kodavsnittet hello i Redigeraren för hello kod:
    
     ```
     $StatusesJson = $ActivityOutput['Get-AzureRmVM'].StatusesText
     $Statuses = ConvertFrom-Json $StatusesJson
     $StatusOut =""
     foreach ($Status in $Statuses){
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"}
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"}
     }
     $StatusOut
     ```
15. Skapa en länk från **Erhåll Status för** för**Start AzureRmVM**.<br> ![Runbook med kodmodul](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
16. Markera hello länk och i hello Configuration fönstret Ändra **använda villkor** för**Ja**.   Obs hello länken aktiverar tooa streckad linje vilket betyder att hello-målaktiviteten körs bara om hello villkoret löser tootrue.  
17. För hello **villkor uttryck**, typen *$ActivityOutput ['Hämta Status'] - eq ”stoppad”*.  **Start-AzureRmVM** körs nu bara om hello virtuella datorn har stoppats.
18. Hello biblioteket kontroll, expandera **Cmdlets** och sedan **Microsoft.PowerShell.Utility**.
19. Lägg till **Write-Output** toohello arbetsytan två gånger.<br> ![Runbook med Write-Output](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
20. På hello första **Write-Output** styra klickar du på **parametrar** och ändra hello **etikett** värdet för*meddela VM igång*.
21. För **InputObject**, ändra **datakällan** för**PowerShell-uttryck** och anger hello uttryck *”$VMName har startat”.* .
22. På hello andra **Write-Output** styra klickar du på **parametrar** och ändra hello **etikett** värdet för*meddela kunde inte starta VM*
23. För **InputObject**, ändra **datakällan** för**PowerShell-uttryck** och anger hello uttryck *”$VMName kunde inte starta”.* .
24. Skapa en länk från **Start AzureRmVM** för**meddela VM igång** och **meddela kunde inte starta VM**.
25. Välj hello länk för**meddela VM igång** och ändra **använda villkor** för**SANT**.
26. För hello **villkor uttryck**, typen *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - eq $true*.  Den här Write-Output styra körs nu bara om hello virtuella datorn har startats.
27. Välj hello länk för**meddela kunde inte starta VM** och ändra **använda villkor** för**SANT**.
28. För hello **villkor uttryck**, typen *$ActivityOutput ['Start-AzureRmVM']. IsSuccessStatusCode - ne $true*.  Den här Write-Output styra nu körs bara om hello virtuella datorn inte har startats.
29. Spara hello runbook och öppna hello Test.
30. Starta hello runbook med hello virtuella datorn stoppas och den ska starta.

## <a name="next-steps"></a>Nästa steg
* toolearn mer information om hur du grafiskt redigering finns [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md)
* tooget igång med PowerShell runbooks finns [min första PowerShell-runbook](automation-first-runbook-textual-powershell.md)
* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md)

