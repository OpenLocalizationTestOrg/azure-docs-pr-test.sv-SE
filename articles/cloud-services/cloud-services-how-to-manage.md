---
title: "aaaCommon moln Tjänstehantering (klassisk) | Microsoft Docs"
description: "Lär dig hur toomanage cloud services i hello klassiska Azure-portalen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 41a30e50-067c-485b-96fd-434a6d057abc
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 03b1d632f1480d0f65c87b7f8ffc747417acf7b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-cloud-services"></a>Hur tooManage Cloud Services
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-manage-portal.md)
> * [Klassisk Azure-portal](cloud-services-how-to-manage.md)
>
>

I hello **molntjänster** område i hello Azure klassiska portal, du kan uppdatera en rolltjänst eller en distribution, befordra en stegvis distribution tooproduction, länka resurser tooyour molntjänst så att du kan se hello resurs beroenden och skala hello resurser tillsammans, och ta bort en molnbaserad tjänst eller en distribution.

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Så här: uppdatera en rolltjänst för molnet eller distribution
Om du behöver tooupdate hello programkod för din molntjänst använder **uppdatering** på instrumentpanelen hello **molntjänster** sidan eller **instanser** sidan. Du kan uppdatera en roll eller alla roller. Du behöver tooupload ett nytt tjänstepaket och konfigurationsfilen för tjänsten.

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), på instrumentpanelen hello **molntjänster** sidan eller **instanser** klickar du på **uppdatering**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. I **distributionsetiketten**, ange ett namn tooidentify hello distribution (till exempel mycloudservice4). Du hittar hello distributionsetiketten under **Snabbstart** på hello instrumentpanel.
3. I **paketet**, använda **Bläddra** tooupload hello servicepaketfil (.cspkg).
4. I **Configuration**, använda **Bläddra** tooupload hello tjänstekonfigurationsfilen (.cscfg).
5. I **rollen**väljer **alla** om du vill tooupgrade alla roller i hello Molntjänsten. tooperform en enda roll uppdatera väljer hello roll som du vill använda tooupdate. Även om du väljer en viss roll tooupdate är hello uppdateringar i hello tjänstkonfigurationsfilen tillämpade tooall roller.
6. Om hello uppdateringsändringar hello antal roller eller hello storleken på någon roll, väljer hello **Tillåt uppdateringen om rollstorlekar eller antalet roller ändras** kryssrutan tooenable hello uppdatering tooproceed.

    Tänk på att om du ändrar hello storleken på en roll (det vill säga hello storleken för en virtuell dator som är värd för en rollinstans) eller hello antal roller varje instans av serverroll (virtuell dator) måste vara återställts från en avbildning och lokala data går förlorade.

7. Om några service-roller har endast en rollinstans väljer hello **uppdatera även om en eller flera roller innehåller en enda instans kryssruta** tooenable hello uppgradera tooproceed.

    Azure kan bara garantera tjänsttillgänglighet 99,95% under en molnet tjänstuppdatering om rollerna har minst två rollinstanser (virtuella datorer). Som gör att en virtuell dator tooprocess klientbegäranden medan hello andra uppdateras.

8. Klicka på **OK** (markering) toobegin uppdatera hello-tjänst.

## <a name="how-to-swap-deployments-toopromote-a-staged-deployment-tooproduction"></a>Så här: växla distributioner toopromote tooproduction en stegvis distribution
Använd **växla** toopromote en fristående distribution av en cloud service tooproduction. När du bestämmer toodeploy en ny version av en molnbaserad tjänst kan mellanlagra och testa den nya versionen i din fristående molntjänstmiljö medan dina kunder använder hello aktuella versionen i produktion. När du är klar toopromote hello den nya versionen tooproduction kan du använda **växla** tooswitch hello URL: er med vilka hello två distributioner behandlas.

Du kan växla distributioner från hello **molntjänster** sida eller hello instrumentpanelen.

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), klickar du på **molntjänster**.
2. Hello molntjänster, klicka på hello cloud service tooselect den.
3. Klicka på **växla**.

    hello öppnas följande bekräftelse.

    ![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. När du har kontrollerat hello distributionsinformation klickar du på **Ja** tooswap hello distributioner.

    hello distribution växlingen sker snabbt eftersom hello enda som ändras är hello virtuella IP-adresser (VIP) för hello-distributioner.

    toosave beräkning kostnader, kan du ta bort hello distribution i hello mellanlagring miljön när du är säker hello nya Produktionsdistribution fungerar som förväntat.

### <a name="common-questions-about-swapping-deployments"></a>Vanliga frågor om växling distributioner

**Vad är hello förutsättningarna för att byta distributioner?**

Det finns två viktiga krav för en lyckad distribution växling:

- Om du vill att toouse en statisk IP-adress för din produktionsplatsen, måste du reservera en för din mellanlagringsplatsen. Annars misslyckas hello växlingen.

- Alla instanser av dina roller måste köras innan du kan utföra hello växlingen. Du kan kontrollera hello status för dina instanser hello klassiska Azure-portalen eller med hjälp av [hello Get-AzureRole kommando i Windows PowerShell](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0).

Observera att uppdateringar för gäst-OS och återställning operations-tjänsten kan också orsakas av distribution växlingar toofail. Se [distribution felsöka cloud service](cloud-services-troubleshoot-deployment-problems.md) för mer information.

**Medför en växling driftstopp för Mina program? Hur får jag hantera den?**

Enligt beskrivningen i hello sista avsnittet är en distribution växling vanligtvis mycket snabbt eftersom det är bara en konfigurationsändring i hello Azure belastningsutjämnare. I vissa fall kan det dock ta tio eller fler sekunder och resultera i tillfälliga anslutningsfel. toolimit påverkan tooyour kunder, Överväg att implementera [klienten logik](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-tooa-cloud-service"></a>Så här: länka en resurs tooa molntjänst
tooshow din molntjänst beroenden för andra resurser, du kan länka en Azure SQL Database-instans eller en molntjänst för lagring konto toohello. Du kan länka och Avlänka resurser på hello **länkade resurser** sidan och övervaka deras användning på hello cloud service-instrumentpanelen. Om ett länkade storage-konto har övervakning aktiverat kan övervaka du Totalt antal begäranden på hello cloud service-instrumentpanelen.

Använd **länk** toolink en ny eller befintlig SQL Database-instans eller lagring konto tooyour tjänst i molnet. Du kan sedan skala hello databasen tillsammans med rolltjänst för hello moln som använder den på hello **skala** sidan. (Ett lagringskonto skalas automatiskt som Minnesanvändningen ökar). Mer information finns i [hur tooScale en tjänst i molnet och länkade resurser](cloud-services-how-to-scale.md).

Du också kan övervaka, hantera och skala hello databasen i hello **databaser** nod i hello klassiska Azure-portalen.

”Länka” en resurs på detta sätt ansluta inte din app toohello resurs. Om du skapar en ny databas med hjälp av **länk**, behöver du tooadd hello anslutning strängar tooyour programkod och uppgradera sedan hello-Molntjänsten. Du måste också tooadd anslutningssträngar om din app använder resurser i ett länkat lagringskonto.

hello följande procedur beskriver hur toolink en ny SQL Database-instans distribueras på en ny SQL Database-server, tooa Molntjänsten.

### <a name="toolink-a-sql-database-instance-tooa-cloud-service"></a>toolink en molntjänst för SQL Database-instans tooa
1. I hello [klassiska Azure-portalen](http://manage.windowsazure.com/), klickar du på **molntjänster**. Klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.
2. Klicka på **länkade resurser**.

    Hej **länkade resurser** öppnas.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klicka på antingen **länka en resurs** eller **länk**.

    Hej **länk resurs** startas guiden.

    ![Länken Sida1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klicka på **skapar en ny resurs** eller **länka en befintlig resurs**.
5. Välj hello typ av resurs toolink. I hello [klassiska Azure-portalen](http://manage.windowsazure.com/), klickar du på **SQL-databas**. (Endast hello klassiska Azure-portalen stöder länka en molntjänst för lagring konto tooa.)
6. toocomplete hello databaskonfiguration, följ instruktionerna i hjälpen för hello **SQL-databaser** område i hello klassiska Azure-portalen.

    Du kan följa hello fortskrider hello länka igen i området för hello-meddelande.

    När länkar är klar, kan du övervaka hello status för hello länkade resurs på hello cloud service-instrumentpanelen. Information om att skala en länkad SQL-databas finns [hur tooScale en tjänst i molnet och länkade resurser](cloud-services-how-to-scale.md).

### <a name="toounlink-a-linked-resource"></a>toounlink länkade resurser
1. I hello [klassiska Azure-portalen](http://manage.windowsazure.com/), klickar du på **molntjänster**. Klicka sedan på hello namnet på hello cloud service tooopen hello instrumentpanelen.
2. Klicka på **länkade resurser**, och välj sedan hello resurs.
3. Klicka på **Avlänka**. Klicka på **Ja** på hello bekräftelse.

    Bryta länken till en SQL-databas har ingen effekt på hello eller hello programmet anslutningar toohello databasen. Du kan hantera hello databasen i hello **SQL-databaser** område i hello klassiska Azure-portalen.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Så här: ta bort distributioner och en tjänst i molnet
Innan du kan ta bort en molnbaserad tjänst, måste du ta bort alla befintliga distributionen.

toosave beräkning kostnader, kan du ta bort fristående distributionen när du har kontrollerat att din Produktionsdistribution fungerar som förväntat. Du är fakturerade beräkning kostnader för rollinstanser även om en molnbaserad tjänst inte körs.

Använd följande procedur toodelete hello en distribution eller Molntjänsten.

1. I hello [klassiska Azure-portalen](http://manage.windowsazure.com/), klickar du på **molntjänster**.
2. Välj hello molntjänst och klicka sedan på **ta bort**. (tooselect en tjänst i molnet utan att öppna hello instrumentpanelen, klicka var som helst utom hello namn i hello cloud service posten.)

    Om du har en distribution i Förproduktion eller produktion, visas en meny med alternativ liknande toohello efter längst hello hello-fönstret. Innan du kan ta bort hello Molntjänsten, måste du ta bort alla befintliga distributioner.

    ![Ta bort meny](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)

3. toodelete en distribution, klickar du på **ta bort Produktionsdistribution** eller **ta bort distributionen av fristående**. Klicka sedan på hello bekräftelse på **Ja**.
4. Om du planerar toodelete hello-Molntjänsten, Upprepa steg 3, om det behövs, toodelete andra distributionen.
5. toodelete hello-Molntjänsten, klicka på **ta bort Molntjänsten**. Klicka sedan på hello bekräftelse på **Ja**.

> [!NOTE]
> Utförlig övervakning har konfigurerats för din molntjänst kan tas Azure inte bort om hello övervakningsdata från storage-konto när du tar bort hello-Molntjänsten. Du behöver toodelete hello data manuellt. Information om där toofind hello mått tabeller finns i ”så här: komma åt utförlig övervakningsdata utanför hello klassiska Azure-portalen” i [hur tooMonitor Cloud Services](cloud-services-how-to-monitor.md).
>
>

## <a name="next-steps"></a>Nästa steg
* [Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure.md).
* Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy.md).
* Konfigurera en [domännamn](cloud-services-custom-domain-name.md).
* Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate.md).
