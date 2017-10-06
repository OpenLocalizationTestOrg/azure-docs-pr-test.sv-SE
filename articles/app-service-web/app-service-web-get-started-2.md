---
title: "aaaAdd funktioner tooyour första webbapp | Microsoft Docs"
description: "Lägg till smarta funktioner tooyour första webbapp på några minuter."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 542671c2-22f0-4f20-8b4b-fa477264c492
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2016
ms.author: cephalin
ms.openlocfilehash: 46c9b118c2c188508cb0a369c691a79073b7d7b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-functionality-tooyour-first-web-app"></a>Lägg till funktioner tooyour första webbapp
I [distribuera din första web app tooAzure på fem minuter](app-service-web-get-started-dotnet.md), du har distribuerat en exempelwebbapp till [Azure App Service](../app-service/app-service-value-prop-what-is.md). I den här artikeln, ska du snabbt lägga till några bra funktioner tooyour distribueras webbprogrammet. På några minuter gör du följande:

* ställer in användarautentisering
* ställer in automatisk skalning av appen
* ta emot meddelanden på hello prestandan för din app

Oavsett vilket exempel du distribuerade i hello föregående artikel, kan du följa med i hello kursen.

hello tre aktiviteter i den här kursen är bara några exempel på hello många användbara funktioner som du får när du lägger din webbapp i App Service. Många av hello funktioner är tillgängliga i hello **lediga** nivå (vilket är program som din första webbapp körs), och du kan använda dina utvärderingskrediter tootry ut funktioner som kräver högre prisnivåer. Vara säker på att ditt webbprogram finns kvar i **lediga** tjänstnivån såvida du inte uttryckligen ändrar tooa olika prisnivån.

> [!NOTE]
> hello webbprogram som du skapade med Azure CLI kör **lediga** nivå där endast en delad VM-instans med resurskvoter. Mer information om vad du får i den **kostnadsfria nivån** finns i avsnittet om [gränser för Apptjänst](../azure-subscription-service-limits.md#app-service-limits).
> 
> 

## <a name="authenticate-your-users"></a>Autentisera användarna
Nu ska vi se hur lätt det är tooadd autentisering tooyour app (kan läsa [autentisering/auktorisering i Apptjänst](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)).

1. I hello portalbladet för din app som du just öppnade klickar du på **inställningar** > **autentisering / auktorisering**.  
    ![Autentisera – inställningsbladet](./media/app-service-web-get-started/aad-login-settings.png)
2. Klicka på **på** tooturn autentisering.  
3. Gå till **Autentiseringsprovidrar** och klicka på **Azure Active Directory**.  
    ![Autentisera – välj Azure AD](./media/app-service-web-get-started/aad-login-config.png)
4. I hello **inställningarna för Azure Active Directory** bladet, klickar du på **Express**, klicka på **OK**. hello standardinställningarna skapas en ny Azure AD-program i din standardkatalog.  
    ![Autentisera – expresskonfigurering](./media/app-service-web-get-started/aad-login-express.png)
5. Klicka på **Spara**.  
    ![Autentisera – spara konfiguration](./media/app-service-web-get-started/aad-login-save.png)
   
    När hello ändringen är genomförd visas hello notification bell blir grön tillsammans med ett eget meddelande.
6. Igen hello portalbladet för appen och klicka på hello **URL** länk (eller **Bläddra** i hello menyraden). hello länken är en HTTP-adress.  
    ![Autentisera – Leta fram tooURL](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Men när den öppnas hello app i en ny flik hello omdirigeringar för URL-rutan flera gånger och har slutförts på din app med en HTTPS-adress. Vad det uppstår är att du redan är inloggad tooyour Azure-prenumeration och du är därmed automatiskt autentiserad i hello app.  
    ![Autentisera – logga in](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Så om du nu öppnar en oautentiserad session i en annan webbläsare, visas en inloggningsskärm när du navigerar toohello samma URL.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
    Om du aldrig har gjort något med Azure Active Directory tidigare finns det kanske inga Azure AD-användare i din standardkatalog. I så fall är förmodligen hello endast kontot i dit hello Microsoft-konto med din Azure-prenumeration. Som har varför du blev automatiskt inloggad i toohello app i hello samma webbläsare tidigare.
    Du kan använda den samma Microsoft-konto toolog i på den här inloggningssidan.

Grattis, nu autentiseras all trafik tooyour webbprogram.

Du har tänkt hello **autentisering / auktorisering** bladet som du kan göra mycket mer, såsom:

* aktivera inloggning via sociala medier
* aktivera flera olika inloggningsalternativ
* Ändra hello standardbeteendet när personer först gå tooyour app

App Service tillhandahåller en nyckelfärdig lösning för några vanliga hello-autentisering måste så inte behöver du tooprovide hello autentiseringslogiken själv.
Mer information om autentisering/auktorisering i Apptjänst finns [här](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Automatisk skalning av appen utifrån efterfrågan
Nästa vi autoskalning appen så att de justeras automatiskt den kapacitet toorespond toouser begäran (kan läsa [skala upp din app i Azure](web-sites-scale.md) och [skala instansantalet manuellt eller automatiskt](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Webbappar kan i princip skalas på två sätt:

* [Skala upp](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Du får mer processorkapacitet, minne och diskutrymme och extra funktioner såsom dedikerade virtuella datorer, anpassade domäner och certifikat, mellanlagringsplatser, autoskalning med mera. Du skalar upp genom att ändra hello prisnivån för App Service-plan som tillhör din app.
* [Skala ut](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): öka hello antal VM-instanser som appen körs.
  Du kan skala ut tooas många som 50 instanser, beroende på din prisnivå.

Nu kommer vi till saken och ställer in autoskalningen.

1. Först ska vi skala upp tooenable autoskalning. Hej portalbladet för appen och klicka på **inställningar** > **skala upp (Apptjänstplan)**.  
    ![Skala upp – inställningsbladet](./media/app-service-web-get-started/scale-up-settings.png)
2. Bläddra och välj hello **S1 Standard** tjänstnivån, hello lägsta nivån som autoskalning (inringad i skärmbilden) och klicka sedan på **Välj**.  
    ![Skala upp – välj nivå](./media/app-service-web-get-started/scale-up-select.png)
   
    Så där, nu har du skalat upp.
   
   > [!IMPORTANT]
   > Med den här nivån förbrukas dina kostnadsfria utvärderingskrediter. Om du har en per tillfälle-konto har avgifter tooyour konto.
   > 
   > 
3. Då ska vi ställa in autoskalning. Hej portalbladet för appen och klicka på **inställningar** > **skala ut (Apptjänstplan)**.  
    ![Skala ut – inställningsbladet](./media/app-service-web-get-started/scale-out-settings.png)
4. Ändra **skala genom** för**CPU-procent**. hello skjutreglagen under listrutan hello uppdateras. Ange sedan antal **Instanser** mellan **1** och **2** och ett **Målområde** mellan **40** och **80**. Gör det genom att skriva i rutorna hello eller flytta Hej reglage.  
    ![Skala ut – konfigurera autoskalning](./media/app-service-web-get-started/scale-out-configure.png)
   
    Med den här inställningen skalas appen ut automatiskt när processoranvändningen går över 80 procent och skalas in när processoranvändningen går under 40 procent.
5. Klicka på **spara** i hello menyraden.

Vad bra! Nu autoskalas appen.

Du har tänkt hello **skalinställningar** bladet som du kan göra mycket mer, såsom:

* Skala tooa specifikt antal instanser manuellt
* skala efter andra prestandavärden, till exempel minnesprocent eller diskkö
* anpassa skalningsuppförande när en prestandaregel löser ut
* autoskala enligt schema
* ställa in autoskalningsuppförande för framtida händelser

Mer information om att skala upp appar finns i [Skala upp din app i Azure](web-sites-scale.md). Mer information om att skala ut finns i avsnittet om att [skala antalet instanser manuellt eller automatiskt ](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Varningar om appen
Nu när autoskalas appen, vad som händer när den når hello maximalt instansantal (2) uppnås och Processorn går över inställd användningsgrad (80 procent)?
Du kan ställa in en avisering (kan läsa [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)) tooinform du den här situationen så att du kan ytterligare skala upp eller ut appen, till exempel. Vi ställer snabbt in en varning för en sådan händelse.

1. Hej portalbladet för appen och klicka på **verktyg** > **aviseringar**.  
    ![Varningar – inställningsbladet](./media/app-service-web-get-started/alert-settings.png)
2. Klicka på **Lägg till avisering**. Sedan hello **resurs** rutan, Välj hello-resurs som slutar med **(serverfarms)**. Det är din apptjänstplan.  
    ![Varningar – lägg till avisering för App Service-plan](./media/app-service-web-get-started/alert-add.png)
3. I rutan **Namn** anger du `CPU Maxed`, i rutan **Mått** anger du **CPU-procent** och i rutan **Tröskelvärde** `90`. Markera sedan alternativet **E-postägare, deltagare och läsare** och klicka sedan på **OK**.   
    ![Varningar – ställ in varning](./media/app-service-web-get-started/alert-configure.png)
   
    När Azure har skapat hello avisering, visas den i hello **aviseringar** bladet.  
    ![Varningar – färdig varning visas](./media/app-service-web-get-started/alert-done.png)

Vad bra, nu får du varningar.

Med den här varningen kontrolleras processoranvändningen med några minuters mellanrum. Om användningen går över 90 procent får du, liksom de som har rätt behörighet, en varning via e-post. toosee alla som är auktoriserade tooreceive hello aviseringar, gå tillbaka toohello portalbladet för appen och klicka på hello **åtkomst** knappen.  
![Se vilka som får varningar](./media/app-service-web-get-started/alert-rbac.png)

Du bör se som **prenumerationsadministratörer** är redan hello **ägare** hello-appen. Den här gruppen inkluderar du om du är hello kontoadministratör för din Azure-prenumeration (säga utvärderingsprenumerationen). Mer information om rollbaserad åtkomstkontroll i Azure finns [här](../active-directory/role-based-access-control-configure.md).

> [!NOTE]
> Varningsregler är en funktion i Azure. Mer information om varningsaviseringar finns [här](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).
> 
> 

## <a name="next-steps"></a>Nästa steg
På ditt sätt tooconfigure hello aviseringen kanske såg du en omfattande uppsättning verktyg i hello **verktyg** bladet. Här kan du felsöka problem, övervaka prestanda, testa säkerhetsrisker, hantera resurser, interagera med hello VM-konsolen och lägga till användbara tillägg. Vi erbjuder tooclick på var och en av dessa verktyg toodiscover hello enkelt men kraftfullt verktyg till hands.

Ta reda på hur toodo mer med den distribuerade appen. Här är bara några exempel:

* [Köpa och konfigurera ett eget domännamn](custom-dns-web-site-buydomains-web-app.md) – Du kan köpa en snygg domän för din webbapp att använda istället för domänen *.azurewebsites.net. Eller så kan du använda en domän som du redan har.
* [Skapa mellanlagringsmiljöer](web-sites-staged-publishing.md) -distribuera din app tooa mellanlagring URL innan du placerar den i produktion. På så sätt kan du uppdatera livewebbappar utan att behöva oroa dig. Du kan skapa en utförlig DevOps-lösning med flera olika distributionsplatser.
* [Ställa in kontinuerlig distribution](app-service-continuous-deployment.md) – Du kan integrera appdistribution i ditt källkontrollsystem. Distribution till Azure sker vid varje incheckning.
* [Få tillgång till lokala resurser](web-sites-hybrid-connection-get-started.md) – Du kan nå en befintlig lokal databas eller lokalt CRM-system.
* [Säkerhetskopiera appar](web-sites-backup.md) – Du kan ställa in säkerhetskopiering och återställning av webbappar. På så sätt är du förberedd för oväntade fel och kan återställa vid behov.
* [Aktivera diagnostikloggar](web-sites-enable-diagnostic-log.md) -Läs hello IIS loggar från Azure eller programspår. Du kan läsa dem i ett flöde, ladda ned dem eller portera dem till [Application Insights](../application-insights/app-insights-overview.md) för nyckelfärdig analys.
* [Söka igenom appen efter säkerhetsrisker](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
  söka igenom dina webbappar mot aktuella hot genom en tjänst som tillhandahålls av [Tinfoil Security](https://www.tinfoilsecurity.com/).
* [Köra bakgrundsjobb](../azure-functions/functions-overview.md) – Du kan köra jobb för databehandling, rapportering med mera.
* [Lär dig hur App Service fungerar](../app-service/app-service-how-works-readme.md)

