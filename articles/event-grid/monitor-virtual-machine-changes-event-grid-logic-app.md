---
title: "aaaMonitor virtuella ändringar - Azure händelse rutnätet & Logic Apps | Microsoft Docs"
description: "Kontrollera config ändringar i virtuella datorer (VM) med hjälp av Azure händelse rutnätet och Logic Apps"
keywords: "logikappar, händelse rutnät, virtuell dator, VM"
services: logic-apps
author: ecfan
manager: anneta
ms.assetid: 
ms.workload: logic-apps
ms.service: logic-apps
ms.topic: article
ms.date: 08/16/2017
ms.author: LADocs; estfan
ms.openlocfilehash: f0633e598be6e7880a310e6f8e64f6738cc692b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Övervaka ändringar av virtuella datorer med Azure händelse rutnätet och Logic Apps

Du kan starta en automatiserad [logik app arbetsflöde](../logic-apps/logic-apps-what-are-logic-apps.md) när specifika händelser inträffar i Azure-resurser eller resurser från tredje part. Dessa resurser kan publicera dessa händelser tooan [Azure händelse rutnätet](../event-grid/overview.md). I sin tur hello händelse rutnätet skickar dessa händelser toosubscribers som har köer, webhooks, eller [händelsehubbar](../event-hubs/event-hubs-what-is-event-hubs.md) som slutpunkter. Som en prenumerant kan din logikapp vänta på dessa händelser från hello händelse rutnätet innan du kan köra automatiska arbetsflöden tooperform uppgifter - utan att du skriva någon kod.

Här är till exempel vissa händelser som utgivare kan skicka toosubscribers via hello Azure händelse rutnätet tjänsten:

* Skapa, läsa, uppdatera eller ta bort en resurs. Du kan till exempel övervaka ändringar som kan avgifter på din Azure-prenumeration och påverkar fakturan. 
* Lägg till eller ta bort personer från en Azure-prenumeration.
* Din app utför en viss åtgärd.
* Ett nytt meddelande visas i en kö.

Den här guiden skapar en logikapp som övervakar ändringar tooa virtuella datorn och skickar e-postmeddelanden om dessa ändringar. När du skapar en logikapp med en händelseprenumeration för en Azure-resurs flöda händelser från den här resursen via en händelse rutnätet toohello logikapp. hello självstudier vägleder dig genom att skapa den här logikapp:

![Översikt – övervaka virtuella datorn med händelsen rutnätet och logik app](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Skapa en logikapp som övervakar händelser från ett rutnät för händelsen.
> * Lägg till ett villkor som specifikt söker efter ändringar av virtuell dator.
> * Skicka e-post när den virtuella datorn ändras.

## <a name="prerequisites"></a>Krav

* Ett e-postkonto från [valfri e-provider som stöds av Azure Logikappar](../connectors/apis-list.md), till exempel Office 365 Outlook, Outlook.com eller Gmail för att skicka meddelanden. Den här kursen använder Office 365 Outlook.

* En [virtuella](https://azure.microsoft.com/services/virtual-machines). Om du inte redan gjort det, skapar du en virtuell dator via en [skapa en VM-kursen](https://docs.microsoft.com/azure/virtual-machines/). toomake hello virtuella publicera händelser du [behöver inte toodo stor](../event-grid/overview.md).

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Skapa en logikapp som övervakar händelser från ett rutnät för händelse

Skapa en logikapp och lägga till en händelseutlösare rutnät som övervakar hello resursgruppen för den virtuella datorn först. 

1. Logga in toohello [Azure-portalen](https://portal.azure.com). 

2. Hello övre vänstra hörnet i hello Azure huvudmenyn, Välj **ny** > **Enterprise Integration** > **Logikapp**.

   ![Skapa logikapp](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. Skapa din logikapp med hello-inställningar som anges i följande tabell hello:

   ![Ange logiken information om appar](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Inställning | Föreslaget värde | Beskrivning | 
   | ------- | --------------- | ----------- | 
   | **Namn** | *{logik-appens-namn}* | Ange ett unikt logik appnamn. | 
   | **Prenumeration** | *{din Azure-prenumerationer}* | Välj hello samma Azure-prenumeration för alla tjänster i den här självstudiekursen. | 
   | **Resursgrupp** | *{din Azure-resurs-grupp}* | Välj hello samma Azure-resursgrupp för alla tjänster i den här självstudiekursen. | 
   | **Plats** | *{din Azure-region}* | Välj hello samma region för alla tjänster i den här självstudiekursen. | 
   | | | 

4. När du är klar, Välj **PIN-kod toodashboard**, och välj **skapa**.

   Du har nu skapat en Azure-resurs för din logikapp. 
   När Azure distribuerar din logikapp, visar hello Logic Apps Designer du mallar för vanliga mönster så kan du sätta igång snabbare.

   > [!NOTE] 
   > När du väljer **PIN-kod toodashboard**, logikappen öppnas automatiskt i Logic Apps Designer. I annat fall kan du manuellt hitta och öppna logikappen.

5. Nu välja en mall för logik-app. Under **mallar**, Välj **tom Logikapp** så att du kan bygga din logikapp från början.

   ![Välj logiska appmallen](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   hello Logic Apps Designer visas nu [ *kopplingar* ](../connectors/apis-list.md) och [ *utlösare* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) som du kan använda toostart logikapp och även åtgärder att du lägger till efter en utlösare tooperform uppgifter. En utlösare är en händelse som skapar en logik app-instansen och startar logik app arbetsflödet. 
   Din logikapp måste en utlösare som första hello-objektet.

6. Ange ”händelse rutnätet” i sökrutan hello som filter. Välj den här utlösaren: **Azure händelse rutnät - på en resurs-händelse**

   ![Välj den här utlösaren: ”Azure händelse rutnät - på en resurs händelse”](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. När du uppmanas att logga in tooAzure händelse rutnät med dina autentiseringsuppgifter för Azure.

   ![Logga in med dina autentiseringsuppgifter för Azure](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Om du har loggat in med ett personligt microsoftkonto som @outlook.com eller @hotmail.com, hello händelse rutnätet utlösaren kanske inte visas korrekt. Som en tillfällig lösning kan välja [Anslut med tjänstens huvudnamn](/azure-resource-manager/resource-group-create-service-principal-portal.md), eller autentisera sig som en medlem av hello Azure Active Directory som är kopplad till ditt Azure-prenumeration, till exempel *användarnamn* @emailoutlook.onmicrosoft.com.

8. Nu prenumerera logik app toopublisher händelserna. Ange hello information för din händelseprenumeration som anges i följande tabell hello:

   ![Ange detaljer för händelseprenumerationen](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Inställning | Föreslaget värde | Beskrivning | 
   | ------- | --------------- | ----------- | 
   | **Prenumeration** | *{virtual machine-Azure-prenumerationer}* | Välj hello händelse publisher Azure-prenumeration. Välj hello Azure-prenumeration för den virtuella datorn för den här självstudiekursen. | 
   | **Resurstyp** | Microsoft.Resources.resourceGroups | Välj hello händelse publisher resurstypen. För den här självstudiekursen hello väljer du det angivna värdet så att din logikapp övervakar enbart resursgrupper. | 
   | **Resursnamn** | *{virtuella-datorn-resource-group-name}* | Välj hello publisher resursnamnet. Välj hello namnet på hello resursgruppen för den virtuella datorn för den här självstudiekursen. | 
   | Valfria inställningar, väljer **visa avancerade alternativ**. | *{finns beskrivningar}* | * **Prefixet Filter**: den här kursen kan lämna inställningen tom. hello standardbeteendet matchar alla värden. Du kan dock ange prefix-strängen som ett filter, till exempel en sökväg och en parameter för en viss resurs. <p>* **Suffix Filter**: den här kursen kan lämna inställningen tom. hello standardbeteendet matchar alla värden. Du kan dock ange en suffixsträng som ett filter, till exempel ett filnamnstillägg, när du vill att endast specifika filtyper.<p>* **Prenumerationsnamn**: Ange ett unikt namn för din händelseprenumeration. |
   | | | 

   När du är klar kan din händelseutlösare rutnätet se ut det här exemplet:
   
   ![Exempel händelseinformationen rutnätet utlösare](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Spara din logikapp. Välj hello designer verktygsfältet **spara**. toocollapse och Dölj Välj information för en åtgärd i din logikapp hello åtgärd namnlist.

   ![Spara din logikapp](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   När du sparar din logikapp med en rutnätet händelseutlösare skapar Azure automatiskt ett händelseprenumerationen för din logik app tooyour markerad resurs. Så när hello resurs publicerar en händelse toohello händelse rutnät, skickar att händelsen rutnätet automatiskt hello händelse tooyour logikapp. Den här händelsen utlöser din logikapp, och sedan skapar och kör en instans av hello arbetsflöde som du definierar i dessa nästa steg.

Din logikapp har publicerats och lyssnar tooevents från hello händelse rutnät, men göra inte något tills du lägger till åtgärder toohello arbetsflöde. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Lägg till ett villkor som söker efter ändringar av virtuell dator

toorun logik app arbetsflödet endast när en viss händelse inträffar, lägga till ett villkor som kontrollerar för den virtuella datorn ”skrivåtgärder”. När det här villkoret är sant, skickar din logikapp du e-post med information om hello uppdatera virtuell dator.

1. I logik App Designer hello för rutnätet händelseutlösare Välj under **nytt steg** > **Lägg till ett villkor**.

   ![Lägg till ett villkor tooyour logikapp](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   hello logik App Designer lägger till en tom villkoret tooyour arbetsflöde, inklusive åtgärd sökvägar toofollow beroende på om hello villkoret är SANT eller FALSKT.

   ![Tom villkor](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. I hello **villkoret** väljer **redigera i Avancerat läge**.
Ange det här uttrycket:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   Villkoret nu ser ut som i det här exemplet:

   ![Tom villkor](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   Det här uttrycket kontrollerar hello händelse `body` för en `data` objekt där hello `operationName` egenskapen är hello `Microsoft.Compute/virtualMachines/write` igen. 
   Lär dig mer om [händelse rutnätet Händelseschema](../event-grid/event-schema.md).

3. tooprovide en beskrivning för hello villkoret väljer hello **ellipserna** (**...** ) knappen på hello villkoret form och välj sedan **Byt namn på**.

   > [!NOTE] 
   > hello nedan senare exemplen i den här kursen också beskrivs stegen i hello logik app arbetsflödet.

4. Nu välja **redigera i grundläggande läget** så att hello-uttrycket automatiskt löser som visas:

   ![Logik app villkor](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Spara din logikapp.

## <a name="send-email-when-your-virtual-machine-changes"></a>Skicka e-post när den virtuella datorn ändras

Lägg nu till en [ *åtgärd* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) så att du får ett e-postmeddelande när hello angetts villkoret är sant.

1. I hello villkor **om värdet är true** väljer **lägga till en åtgärd**.

   ![Lägga till åtgärden för när villkoret är SANT](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. Ange ”e” i sökrutan hello som filter. Baserat på din e-leverantör, hitta och välj hello matchande koppling. Välj sedan hello ”skicka e-post” åtgärd för din koppling. Exempel: 

   * En Azure arbets- eller skolkonto väljer hello Office 365 Outlook connector. 
   * Välj hello Outlook.com-anslutning för personliga Microsoft-konton. 
   * Välj hello Gmail-anslutning för Gmail-konton. 

   Vi toocontinue med hello Office 365 Outlook connector. 
   Om du använder en annan provider hello steg förblir hello detsamma, men ditt användargränssnitt kan visas olika. 

   ![Välj åtgärden ”Skicka e-post”](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. Om du inte redan har en anslutning för e-post-providern, logga in tooyour e-postkonto när du uppmanas för autentisering.

4. Ange information för hello e-post som anges i följande tabell hello:

   ![Tomt e-åtgärd](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > tooselect från fält som är tillgängliga i arbetsflödet, klickar du på i en redigeringsruta så att hello **dynamiskt innehåll** lista öppnas, eller välj **lägga till dynamiskt innehåll**. Fler fält, Välj **se mer** för varje avsnitt i hello-listan. tooclose hello **dynamiskt innehåll** Välj **lägga till dynamiskt innehåll**.

   | Inställning | Föreslaget värde | Beskrivning | 
   | ------- | --------------- | ----------- | 
   | **Att** | *{mottagaren-e-postadress}* |Ange hello mottagarens e-postadress. I testsyfte kan använda du din egen e-postadress. | 
   | **Ämne** | Resursen uppdateras: **ämne**| Ange hello innehåll för hello e-ämne. Den här självstudiekursen, ange hello förslag text och välj hello händelse **ämne** fältet. Här innehåller e-postmeddelandets ämne hello namnet hello uppdaterade resursen (virtuell dator). | 
   | **Brödtext** | Resursgrupp: **avsnittet** <p>Händelsetyp: **händelsetyp**<p>Händelse-ID: **ID**<p>Tid: **Händelsetid** | Ange hello innehåll för hello huvudtext. Den här självstudiekursen, ange hello förslag text och välj hello händelse **avsnittet**, **händelsetyp**, **ID**, och **Händelsetid** fält så att din e-post innehåller hello resursgruppens namn, händelsetyp, händelse tidsstämpel och händelse-ID för hello uppdatering. <p>tooadd tomma rader i ditt innehåll, trycka på SKIFT + RETUR. | 
   | | | 

   > [!NOTE] 
   > Om du väljer ett fält som representerar en matris, hello designer lägger automatiskt till en **för varje** cirkel runt hello-åtgärd som refererar till hello matris. På så sätt kan din logikapp åtgärden utförs på varje element i matrisen.

   Din e-åtgärd kan nu se ut det här exemplet:

   ![Välj utdata tooinclude i e-post](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   Och klar logikappen kan se ut som i det här exemplet:

   ![Klar logikapp](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Spara din logikapp. toocollapse och Dölj Välj information för varje åtgärd i din logikapp hello åtgärd namnlist.

   ![Spara din logikapp](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   Din logikapp nu är aktiv, men väntar på ändringar tooyour virtuella datorn innan du börjar. 
   tootest din logikapp nu fortsätta toohello nästa avsnitt.

## <a name="test-your-logic-app-workflow"></a>Testa arbetsflödet logik app

1. toocheck logikappen får hello vissa händelser, uppdatera den virtuella datorn. 

   Du kan till exempel ändra storlek den virtuella datorn i hello Azure-portalen eller [ändra storlek på den virtuella datorn med Azure PowerShell](../virtual-machines/windows/resize-vm.md). 

   Du bör få ett e-postmeddelande efter en liten stund. Exempel:

   ![E-post om uppdateringen av den virtuella datorn](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. tooreview hello körs och utlösare historik för din logikapp på logiken app-menyn väljer **översikt**. tooview mer information om hur du kör, välja hello raden för som körs.

   ![Logikapp körs historik](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. Expandera hello steg som du vill tooreview tooview hello indata och utdata för varje steg. Den här informationen kan hjälpa dig att diagnostisera och felsöka problem i din logikapp.
 
   ![Kör historikinformation logikapp](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Grattis, du skapat och kör en logikapp som övervakar resurs händelser via en händelse rutnätet och e-postmeddelanden när dessa händelser inträffar. Du också lära dig hur enkelt du kan skapa arbetsflöden som automatisera processer och integrera system och molntjänster.

## <a name="clean-up-resources"></a>Rensa resurser

Den här kursen använder resurser och utför åtgärder som avgifter på din Azure-prenumeration. Så när du är klar med hello självstudier och testning, se till att du inaktiverar eller ta bort resurser där du inte vill tooincur avgifter.

Du kan avbryta din logikapp köra och skicka e-post utan att ta bort hello app. Välj på appmenyn dina logic **översikt**. Välj hello verktygsfältet **inaktivera**.

![Inaktivera din logikapp](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

**Q**: vilka andra virtuella övervaka uppgifter kan jag utföra med händelsen rutnät och logic apps? </br>
**En**: du kan övervaka andra konfigurationsändringar, till exempel:

* En virtuell dator hämtar rollbaserad åtkomst (RBAC) behörighet.
* Ändringarna tooa nätverkssäkerhetsgrupp (NSG) för ett nätverksgränssnitt (NIC).
* Diskar för en virtuell dator läggs till eller tas bort.
* En offentlig IP-adress tilldelas tooa virtuella nätverkskort.

## <a name="next-steps"></a>Nästa steg

* [Översikt över Event rutnätet](../event-grid/overview.md)
* [Händelsen rutnätet begrepp](../event-grid/concepts.md)
* [Snabbstart: Skapa och dirigera anpassade händelser med händelse rutnätet](../event-grid/custom-event-quickstart.md)
* [Händelseschema rutnätet händelse](../event-grid/event-schema.md)
* [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)
* [Skapa logik app arbetsflöden med hjälp av fördefinierade mallar](../logic-apps/logic-apps-use-logic-app-templates.md)