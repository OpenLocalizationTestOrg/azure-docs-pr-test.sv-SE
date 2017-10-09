---
title: aaaUse en lokal SQL Server i Azure Machine Learning | Microsoft Docs
description: "Använda data från en lokal SQL Server-databasen tooperform advanced analytics med Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 08e4610d-02b6-4071-aad7-a2340ad8e2ea
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: garye;krishnan
ms.openlocfilehash: c0e9908e296b97b31611ef0192744a59073acd80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Utföra avancerade analyser med Azure Machine Learning med hjälp av data från en lokal SQL Server-databas

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Ofta företag som arbetar med lokala data vill tootake nytta av hello skala och flexibilitet för hello moln för sina maskininlärning arbetsbelastningar. Men de vill inte toodisrupt deras aktuella affärsprocesser och arbetsflöden genom att flytta sina lokala data toohello moln. Azure Machine Learning har nu stöd för att läsa data från en lokal SQL Server-databasen och sedan utbildning och poängsättning av en modell med dessa data. Du har inte längre toomanually och synkronisera hello av data mellan hello molnet och lokala servern. I stället hello **importera Data** modul i Azure Machine Learning Studio kan nu läsa direkt från din lokala SQL Server-databasen för din utbildning och bedömningen jobb.

Den här artikeln innehåller en översikt över hur tooingress lokala SQL server-data i Azure Machine Learning. Det förutsätts att du är bekant med Azure Machine Learning-begrepp som arbetsytor, moduler, datauppsättningar, experiment, *etc.*.

> [!NOTE]
> Den här funktionen är inte tillgänglig för kostnadsfria arbetsytor. Mer information om Machine Learning-priser och nivåer finns [Azure Machine Learning-priser](https://azure.microsoft.com/pricing/details/machine-learning/).
>
>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="install-hello-microsoft-data-management-gateway"></a>Installera hello Microsoft Data Management Gateway
tooaccess en lokal SQL Server-databas i Azure Machine Learning, behöver du toodownload och installera hello Microsoft Data Management Gateway. När du konfigurerar hello gateway-anslutningen i Machine Learning Studio har hello möjlighet att hämta och installera hello gateway med hello **hämtningen och registrera datagatewayen** dialogrutan som beskrivs nedan.

Du kan också installera hello Data Management Gateway i förväg genom att hämta och köra hello MSI-installationspaketet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).
Välj hello senaste versionen, välja 32-bitars eller 64-bitars som passar din dator. hello MSI kan också vara används tooupgrade en befintlig Data Management Gateway toohello senaste version, med alla inställningar som bevaras.

hello gateway har hello följande krav:

* versioner av hello stöds Windows-operativsystemet är Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 och Windows Server 2012 R2.
* hello bör konfigurationen för hello gateway-datorn är minst 2 GHz 4 kärnor, 8GB RAM-minne och 80GB disk.
* Om värddatorn hello viloläge svara hello gateway inte toodata begäranden. Därför konfigurera en lämplig energischema på hello datorn innan du installerar hello gateway. Om hello datorn är konfigurerade toohibernate, visas ett meddelande i hello gateway-installationen.
* Eftersom kopieringsaktiviteten sker på en specifik frekvens, följer hello Resursanvändning (CPU, minne) på hello datorn också hello samma mönster med belastning och ledig tid. Resursutnyttjande beror också kraftigt på hello mängden data som flyttas. När flera kopiera jobb pågår, ska du se Resursanvändning gå upp under tider med låg belastning. Hello lägsta konfigurationen som anges ovan är tekniskt tillräckligt, vill du kanske toohave en konfiguration med mer resurser än hello minimikonfiguration beroende på dina specifika belastningen för dataförflyttning.

Tänk hello följande när du konfigurerar och använder en Data Management Gateway:

* Du kan installera bara en instans av Data Management Gateway på en dator.
* Du kan använda en enda gateway för flera lokala datakällor.
* Du kan ansluta flera gateways på olika datorer toohello samma lokala datakällan.
* Du kan konfigurera en gateway för endast en arbetsyta i taget. Gateways kan för närvarande inte delas mellan arbetsytorna.
* Du kan konfigurera flera gateways för en enda arbetsyta. Exempelvis kanske toouse en gateway som är anslutna tooyour test datakällor under utveckling och en gateway för produktionen när du är klar toooperationalize.
* hello gateway behöver inte toobe på hello detsamma datorn som hello datakälla. Men används närmare toohello datakällan minskar hello tid för hello gateway tooconnect toohello datakälla. Vi rekommenderar att du installerar hello gateway på en dator som skiljer sig från hello en värdar hello lokala datakällan så att hello gateway och datakälla inte konkurrerar om resurser.
* Om du redan har en gateway som har installerats på datorn som betjänar Power BI eller Azure Data Factory scenarier, installera en separat gateway för Azure Machine Learning på en annan dator.

  > [!NOTE]
  > Du kan inte köra Data Management Gateway och Power BI Gateway på hello samma dator.
  >
  >
* Du behöver toouse hello Data Management Gateway för Azure Machine Learning, även om du använder Azure ExpressRoute för andra data. Datakällan ska behandlas som en lokal datakälla (som finns bakom en brandvägg) även om du använder ExpressRoute. Använd hello Data Management Gateway tooestablish anslutningen mellan Machine Learning och hello-datakälla.

Du hittar detaljerad information om installationskraven och installationssteg felsökningstips i hello artikel [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Ingång data från din lokala SQL Server-databas till Azure Machine Learning
I den här genomgången ska du ställa in en Data Management Gateway i en Azure Machine Learning-arbetsytan, konfigurera den och läsa data från en lokal SQL Server-databas.

> [!TIP]
> Innan du börjar inaktivera webbläsarens popup-blockerare för `studio.azureml.net`. Om du använder hello webbläsaren Google Chrome, hämta och installera en av hello flera plugin-program finns på Google Chrome vi besvarar [klickar du på en gång App Extension](https://chrome.google.com/webstore/search/clickonce?_category=extensions).
>
>

### <a name="step-1-create-a-gateway"></a>Steg 1: Skapa en gateway
hello första steget är toocreate och ställa in hello gateway tooaccess lokal SQL-databasen.

1. Logga in för[Azure Machine Learning Studio](https://studio.azureml.net/Home/) och välj hello-arbetsyta som du vill toowork i.
2. Klicka på hello **inställningar** bladet på hello vänster och klicka sedan på hello **DATAGATEWAYAR** fliken hello överst.
3. Klicka på **nya DATA GATEWAY** längst hello hello-skärmen.

    ![Nya Datagateway](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. I hello **nya datagateway** dialogrutan Ange hello **Gatewaynamnet** och eventuellt lägga till en **beskrivning**. Klicka på pilen hello hello nedre högra hörnet toogo toohello nästa steg i hello-konfigurationen.

    ![Ange gatewaynamn och beskrivning](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. Hämta och registrera data gateway dialogrutan Kopiera hello GATEWAY REGISTRERINGSNYCKEL toohello Urklipp i hello.

    ![Hämta och registrera gateway](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>Om du ännu inte har hämtat och installerat hello Microsoft Data Management Gateway och sedan på **Download data management gateway**. Det tar toothe Microsoft Download Center där du kan välja hello gatewayversionen du behöver, hämta och installera den. Du hittar detaljerad information om installationskraven och installationssteg felsökningstips i hello början avsnitt i hello artikel [flytta data mellan lokala källor och moln med Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).
7. När hello gateway har installerats hello Konfigurationshanteraren för Data Management Gateway öppnar och hello **registrera gateway** dialogrutan visas. Klistra in hello **Gateway registreringsnyckel** du kopierade toohello Urklipp och klicka på **registrera**.
8. Om du redan har en installerad gateway köra hello Data Management Gateway Configuration Manager. Klicka på **Byt nyckel**, klistra in den **Gateway registreringsnyckel** som du kopierade toohello Urklipp i hello föregående steg och klickar på **OK**.
9. När hello installationen är klar, hello **registrera gateway** visas dialogrutan för Microsoft Data Management Gateway Configuration Manager. Klistra in hello GATEWAY REGISTRERINGSNYCKEL som du kopierade till hello Urklipp i föregående steg och klickar på **registrera**.

    ![Registrera gateway](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. hello gateway-konfigurationen är klar när hello följande värden är inställda på hello **Start** fliken i Microsoft Data Management Gateway Configuration Manager:

    * **Gatewaynamnet** och **instansnamn** ställs toohello namnet på hello gateway.
    * **Registrering** har angetts för**registrerade**.
    * **Status för** har angetts för**igång**.
    * hello nedre visar hello statusfältet **anslutna tooData Management Gateway Cloud Service** tillsammans med en grön bock.

      ![Data Management Gateway Manager](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      Azure Machine Learning Studio också uppdateras när hello registreringen lyckas.

    ![Gateway-registrering lyckades](media/machine-learning-use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. I hello **ladda ned och registrera gateway** dialogrutan, klicka på kryssmarkeringen toocomplete hello installationsprogrammet. Hej **inställningar** visar sidan gatewayens status som ”Online”. I högra fönstret hello hittar du status och annan användbar information.

    ![Inställningar för gateway](media/machine-learning-use-data-from-an-on-premises-sql-server/gateway-status.png)
12. Växla toohello i hello Microsoft Data Management Gateway Configuration Manager **certifikat** fliken hello certifikatet som anges på den här fliken är används tooencrypt/dekryptera autentiseringsuppgifterna för hello lokalt datalager som du anger hello-portalen. Det här certifikatet är hello standardcertifikatet. Microsoft rekommenderar att du ändrar tooyour egna certifikatet som du säkerhetskopierar det certifikat hanteringssystemet. Klicka på **ändra** toouse ditt eget certifikat i stället.

    ![Ändra gateway-certifikat](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. (valfritt) Om du vill tooenable utförlig loggning för att felsöka problem med hello-gateway i hello Microsoft Data Management Gateway Configuration Manager växla toothe **diagnostik** och kontrollerar hello **aktivera utförlig loggning i felsökningssyfte** alternativet. hello loggningsinformation finns i hello Windows Loggboken under hello **program- och tjänstloggar**  - &gt; **Data Management Gateway** nod. Du kan också använda hello **diagnostik** fliken tootest hello anslutning tooan lokal datakälla med hjälp av hello gateway.

    ![Aktivera utförlig loggning](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

Detta avslutar hello gateway-installationen i Azure Machine Learning.
Du är nu redo toouse dina lokala data.

Du kan skapa och konfigurera flera gateways i Studio för varje arbetsyta. Du kan till exempel ha en gateway som du vill tooconnect tooyour test datakällor under utveckling och en annan gateway för dina datakällor för produktion. Azure Machine Learning ger dig flexibilitet tooset in flera gateways beroende på din företagsmiljö. För närvarande kan du dela inte en gateway mellan arbetsytorna och bara en gateway kan installeras på en dator. Mer information finns i [flytta data mellan lokala källor och moln med Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-hello-gateway-tooread-data-from-an-on-premises-data-source"></a>Steg 2: Använd hello gateway tooread data från en lokal datakälla
När du ställer in hello gateway kan du lägga till en **importera Data** modulen till ett experiment som anger hello data från hello lokala SQL Server-databas.

1. Välj hello i Machine Learning Studio **EXPERIMENT** klickar du på **+ ny** i hello nedre vänstra hörnet och välj **tomt Experiment** (eller välj en av flera exempel experiment tillgänglig).
2. Leta upp och dra hello **importera Data** modulen toohello experimentera arbetsytan.
3. Klicka på **Spara som** nedan hello arbetsytan. Ange ”Azure lokala SQL Server självstudie om Maskininlärning” för hello experiment namn, väljer arbetsytan och på hello **OK** är markerat.

   ![Spara experiment med ett nytt namn](media/machine-learning-use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. Klicka på hello **importera Data** modulen tooselect, i den **egenskaper** fönstret toohello höger i hello arbetsytan och välj ”lokal SQL-databas” i hello **datakällan** listrutan.
5. Välj hello **datagatewayen** du installerad och registrerad. Du kan ställa in en annan gateway genom att välja ”(lägga till nya Datagateway...)”.

   ![Välj datagateway för modulen importera Data](media/machine-learning-use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. Ange hello SQL **Databasservernamnet** och **databasnamnet**, tillsammans med hello SQL **databasfrågan** du vill tooexecute.
7. Klicka på **ange värden** under **användarnamn och lösenord** och ange dina Databasautentiseringsuppgifter. Du kan använda Windows-integrerad autentisering eller beroende på hur dina lokala SQL Server är konfigurerat för SQL Server-autentisering.

   ![Ange autentiseringsuppgifter på databasen](media/machine-learning-use-data-from-an-on-premises-sql-server/database-credentials.png)

   hello-meddelande ”värden som krävs” ändringar för ”värden set” med en grön bock. Du behöver bara tooenter hello autentiseringsuppgifter en gång om hello databasinformation eller lösenord ändras. Azure Machine Learning använder hello-certifikat som du angav när du installerade hello gateway tooencrypt hello autentiseringsuppgifter i hello molnet. Azure lagrar aldrig lokala autentiseringsuppgifter utan kryptering.

   ![Importera Data modulen egenskaper](media/machine-learning-use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. Klicka på **kör** toorun hello experiment.

När hello experiment är klar kan du visualisera hello data som du har importerat från hello databasen genom att klicka på hello utdataporten för hello **importera Data** modulen och välja **visualisera**.

När du är klar med att utveckla ditt experiment kan du distribuera och driftsätta modellen. Med hjälp av hello Batch Execution Service, data från hello lokala SQL Server-databas som konfigurerats i hello **importera Data** modulen läses och används för resultatfunktioner. Du kan använda hello begäran-tjänsten för poängsättning av lokala data, Microsoft rekommenderar att du använder den [Excel-tillägget](machine-learning-excel-add-in-for-web-services.md) i stället. För närvarande kan skriva tooan lokala SQL Server-databas genom **exportera Data** är inte stöds antingen i experimenten eller publicerade webbtjänster.
