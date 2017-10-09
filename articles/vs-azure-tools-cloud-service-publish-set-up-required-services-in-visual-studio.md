---
title: "aaaPrepare toopublish eller distribuera ett Azure-program från Visual Studio | Microsoft Docs"
description: "Lär dig hello procedurer tooset in molnet och storage services-konto och konfigurera Azure-program."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: b5231d400e2ad9e20c3f21bad48a77c328b1f7a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-toopublish-or-deploy-an-azure-application-from-visual-studio"></a>Förbereda tooPublish eller distribuera ett Azure-program från Visual Studio
## <a name="overview"></a>Översikt
Innan du kan publicera ett molntjänstprojekt, måste du ställa in hello följande tjänster:

* En **Molntjänsten** toorun rollerna i hello Azure-miljön
* En **lagringskonto** som ger åtkomst till toohello Blob, köer och tabellen tjänster.

Använd hello följande procedurer tooset upp dessa tjänster och konfigurera ditt program

## <a name="create-a-cloud-service"></a>Skapa en molntjänst
toopublish en cloud service tooAzure, måste du först skapa en molntjänst som kör dina roller i hello Azure-miljön. Du kan skapa en molntjänst i hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), enligt beskrivningen i avsnittet hello **toocreate en tjänst i molnet med hjälp av hello klassiska Azure-portalen**senare i det här avsnittet. Du kan också skapa en molntjänst i Visual Studio med hjälp av hello Publiceringsguiden.

### <a name="toocreate-a-cloud-service-by-using-visual-studio"></a>toocreate en molntjänst med hjälp av Visual Studio
1. Öppna hello snabbmenyn för hello Azure-projekt och välj **publicera**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)
2. Om du inte har loggat in, logga in med ditt användarnamn och lösenord för hello Microsoft-konto eller organisationskonto som är associerad med din Azure-prenumeration.
3. Välj hello **nästa** knappen tooadvance toohello **inställningar** sidan.

    ![Guiden vanliga publiceringsinställningarna](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)
4. I hello **molntjänster** Välj **Skapa nytt**. Hej **skapa Azure-tjänster** visas.
5. Ange hello namnet på Molntjänsten. hello namn utgör en del av hello URL för din tjänst och måste därför vara globalt unika. hello namn är inte skiftlägeskänsligt.

### <a name="toocreate-a-cloud-service-by-using-hello-azure-classic-portal"></a>toocreate en molntjänst med hjälp av hello klassiska Azure-portalen
1. Logga in toohello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkId=253103) på hello Microsofts webbplats.
2. (valfritt) toodisplay en lista över molntjänster som du redan har skapat väljer hello molntjänster länk på hello vänster hello-sidan.
3. Välj hello  **+**  ikon i hello nedre vänstra hörn och välj sedan **Molntjänsten** på hello-menyn som visas. En annan skärm med två alternativ **Snabbregistrering** och **skapa anpassade**, visas. Om du väljer **Snabbregistrering**, du kan skapa en tjänst i molnet genom att bara ange dess URL och hello region där den ska vara fysiskt värd. Om du väljer **skapa anpassade**, du kan publicera en tjänst i molnet omedelbart genom att ange ett paket (.cspkg-fil), konfigurationsfilen (.cscfg) och ett certifikat. Skapa anpassade krävs inte om du avser toopublish Molntjänsten med hjälp av hello **publicera** i ett Azure-projekt. Hej **publicera** kommandot är tillgängligt på hello snabbmenyn för ett Azure-projekt.
4. Välj **Snabbregistrering** toolater publicera din tjänst i molnet med hjälp av Visual Studio.
5. Ange ett namn för din molntjänst. hello fullständiga URL: en visas nästa toohello namn.
6. Välj hello region där de flesta användare finns i hello-listan.
7. Välj hello hello längst ned i fönstret hello i **skapa molntjänst** länk.

## <a name="create-a-storage-account"></a>skapar ett lagringskonto
Ett lagringskonto tillhandahåller åtkomst toohello Blob, köer och tabellen tjänster. Du kan skapa ett lagringskonto med hjälp av Visual Studio eller hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="toocreate-a-storage-account-by-using-visual-studio"></a>toocreate ett lagringskonto med hjälp av Visual Studio
1. I **Solution Explorer**öppnar hello snabbmenyn för hello **lagring** nod, och välj sedan **skapa Lagringskonto**.

    ![Skapa ett nytt Azure storage-konto](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)
2. Välj eller ange följande information för hello nytt lagringskonto i hello hello **skapa Lagringskonto** dialogrutan.

   * hello Azure-prenumeration toowhich som du vill tooadd hello storage-konto.
   * hello namn toouse för hello nytt lagringskonto.
   * hello region eller affinitetsgrupp (till exempel västra USA eller Östasien).
   * Hej typ av replikering du vill använda toouse för hello storage-konto, till exempel Geo-Redundant.
3. När du är klar väljer **skapa**.hello nytt lagringskonto visas i hello **lagring** lista i **Server Explorer**.

### <a name="toocreate-a-storage-account-by-using-hello-azure-classic-portal"></a>toocreate ett lagringskonto med hjälp av hello klassiska Azure-portalen
1. Logga in toohello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkId=253103) på hello Microsofts webbplats.
2. (Valfritt) tooview storage-konton, Välj hello **lagring** länk hello Kontrollpanelen på vänster sida av hello sidan hello.
3. Välj hello i hello nedre vänstra hörnet på sidan hello  **+**  ikon.
4. Välj hello-menyn som visas **lagring**, och välj sedan **Snabbregistrering**.
5. Namnge hello storage-konto som leder till en unik url.
6. Namnge din tjänst i molnet. hello fullständiga URL: en visas nästa toohello namn.
7. Välj en region där de flesta användare finns i hello lista över regioner.
8. Om du vill tooenable geo-replikering. Om du aktiverar geo-replikering, kommer dina data att sparas i flera fysiska platser tooreduce hello risk för dataförlust. Lagring som den här funktionen gör dyrare, men du kan minska hello kostnader genom att aktivera geografiska plats när du skapar hello storage-konto i stället för att lägga till funktionen hello senare. Mer information finns i [georeplikering](http://go.microsoft.com/fwlink/?LinkId=253108).
9. Välj hello hello längst ned i fönstret hello i **skapa Lagringskonto** länk.

När du har skapat ditt lagringskonto visas hello URL: er som du kan använda tooaccess resurser i varje hello Azure storage-tjänster och även hello primära och sekundära åtkomstnycklarna för ditt konto. Du använder dessa nycklar tooauthenticate begäranden som görs mot hello lagringstjänster.

> [!NOTE]
> hello sekundära åtkomstnyckeln ger hello samma åtkomst tooyour lagringskontot som hello primärnyckeln och genereras som en säkerhetskopia bör din primärnyckeln äventyras. Vi rekommenderar dessutom att du återskapar dina åtkomstnycklar regelbundet. Du kan ändra en sträng inställningen toouse hello sekundära anslutningsnyckel medan du återskapar hello primärnyckel, kan du ändra den toouse hello återskapas primärnyckel medan du återskapar hello sekundärnyckeln.
>
>

## <a name="configure-your-app-toouse-services-provided-by-hello-storage-account"></a>Konfigurera din app toouse tjänster som tillhandahålls av hello storage-konto
Du måste konfigurera en roll som har åtkomst till storage services toouse hello Azure storage-tjänster som du har skapat. toodo, du kan använda flera konfigurationer för din Azure-projekt. Som standard skapas två i Azure-projekt. Du kan använda hello samma anslutning sträng i koden, men har ett annat värde för en anslutningssträng i varje tjänstkonfiguration av med hjälp av flera konfigurationer. Du kan exempelvis använder en service configuration toorun och felsöka programmet lokalt med hjälp av hello Azure storage-emulatorn och en annan tjänst configuration toopublish tooAzure ditt program. Mer information om konfigurationer finns [konfigurera din Azure-projekt med flera tjänstkonfiguration](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="tooconfigure-your-application-toouse-services-that-hello-storage-account-provides"></a>tooconfigure din programtjänster toouse som hello lagringskonto tillhandahåller
1. Öppna din Azure-lösning i Visual Studio. Öppna hello snabbmenyn för varje roll i din Azure-projekt som har åtkomst till lagringstjänster hello i Solution Explorer och välj **egenskaper**. En sida med hello namnet på hello roll visas i hello Visual Studio-redigeraren. hello sidan visar hello fält för hello **Configuration** fliken.
2. I hello egenskapssidor för hello roll, väljer **inställningar**.
3. I hello **tjänstkonfiguration** Välj hello namnet på tjänstkonfigurationen för hello som du vill tooedit. Om du vill toomake ändringar tooall av hello tjänstkonfiguration för den här rollen kan du välja **alla konfigurationer av**.  Mer information om hur tjänsten tooupdate konfigurationer finns i avsnittet hello **hantera anslutningssträngar för Lagringskonton** i hello avsnittet [konfigurerar hello roller för en Azure Cloud Service med Visual Studio ](vs-azure-tools-configure-roles-for-cloud-service.md).
4. toomodify anslutning sträng inställningar, väljer hello **...** knappen Nästa toohello inställning. Hej **skapa Lagringsanslutningssträng** dialogrutan visas.
5. Under **ansluta med**, Välj hello **prenumerationen** alternativet.
6. I hello **prenumeration** väljer din prenumeration. Om hello listan över prenumerationer inte innehåller hello något som du vill kan du välja hello **hämta Publiceringsinställningar** länk.
7. I hello **kontonamn** väljer du namnet på ditt lagringskonto. Azure-verktyg hämtar lagringskontouppgifter automatiskt med hjälp av hello .publishsettings-fil. toospecify ditt lagringskonto autentiseringsuppgifter manuellt, Välj hello **autentiseringsuppgifterna anges manuellt** alternativ och sedan fortsätta med den här proceduren. Du kan hämta din lagringskontonamn och primärnyckel från hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885). Om du inte vill toospecify kontoinställningarna lagring manuellt, Välj hello **OK** knappen tooclose hello dialogrutan.
8. Välj hello **ange lagringskonto** autentiseringsuppgifter länk.
9. I hello **kontonamn** ange hello namn för ditt lagringskonto.

   > [!NOTE]
   > Logga in på hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), och välj sedan hello **lagring** knappen. hello portal visar en lista över storage-konton. Om du väljer ett konto öppnar en sida för den. Du kan kopiera hello namnet på hello lagringskonto från den här sidan. Om du använder en tidigare version av hello klassiska portalen hello namnet på ditt lagringskonto visas i hello **Lagringskonton** vyn. toocopy detta namn, markera den i hello **egenskaper** fönster för detta visa och välj sedan hello Ctrl-C-nycklar. toopaste hello namn i Visual Studio väljer hello **kontonamn** text och välj hello Ctrl + V nycklar.
   >
   >
10. I hello **kontonyckel** anger primärnyckel, eller kopiera och klistra in den från hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).
     Det här viktiga toocopy:

    1. Välj hello längst hello hello sidan för hello lämpliga lagringskontot, **hantera nycklar** knappen.
    2. På hello **hantera nycklar åtkomst** sidan, markera hello texten i hello primärnyckeln och välj sedan hello Ctrl + C nycklar.
    3. Klistra in hello nyckel i Azure-verktyg i hello **kontonyckel** rutan.
    4. Du måste välja ett av följande alternativ toodetermine hello hur hello tjänsten kommer åt hello storage-konto:

       * **Använd HTTP**. Detta är standard hello-alternativet. Till exempel `http://<account name>.blob.core.windows.net`.
       * **Använda HTTPS** för en säker anslutning. Till exempel `https://<accountname>.blob.core.windows.net`.
       * **Ange anpassade slutpunkter** för varje hello tre tjänster. Du kan sedan ange dessa slutpunkter i hello fält för hello specifik tjänst.

         > [!NOTE]
         > Om du skapar anpassade slutpunkter, kan du skapa en mer komplex anslutningssträng. Du kan ange slutpunkter lagring som innehåller ett domännamn som du har registrerat för ditt lagringskonto med hello Blob-tjänsten när du använder den här strängformat. Du kan också ge åtkomst endast tooblob resurser i en enskild behållare via en signatur för delad åtkomst. Mer information om hur toocreate anpassade slutpunkter, se [konfigurera Azure Storage-anslutningssträngar](storage/common/storage-configure-connection-string.md).
         >
         >
11. toosave ändringarna anslutning sträng väljer hello **OK** knappen och välj sedan hello **spara** hello i verktygsfältet. När du har sparat ändringarna kan du hämta hello värdet för den här anslutningssträngen i koden med [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). När du publicerar program-tooAzure välja hello tjänstkonfiguration som innehåller hello Azure storage-konto för hello anslutningssträngen. När programmet publiceras, kontrollerar du att hello programmet fungerar som förväntat mot hello Azure storage-tjänster

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du publicerar appar tooAzure från Visual Studio finns [publicering av en tjänst i molnet med hjälp av hello Azure verktyg](vs-azure-tools-publishing-a-cloud-service.md).
