---
title: "aaaConfigure rapporter för Azure Backup"
description: "Den här artikeln handlar om hur du konfigurerar Power BI-rapporter för Azure Backup med Recovery Services-valvet."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 86e465f1-8996-4a40-b582-ccf75c58ab87
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 503a240b36ea999e0fea434b6a50d26ddf7677bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-backup-reports"></a>Konfigurera Azure Backup-rapporter
Den här artikeln handlar om steg tooconfigure rapporter för Azure Backup med hjälp av Recovery Services-valvet och tooaccess rapporterna med Power BI. När du utför dessa åtgärder kan du direkt gå tooPower BI tooview alla hello rapporter, anpassa och skapa rapporter. 

## <a name="supported-scenarios"></a>Scenarier som stöds
1. Azure Backup-rapporter har stöd för virtuell dator i Azure backup och filen/mappen säkerhetskopiering toocloud med hjälp av Azure Recovery Services-agenten.
2. Azure SQL, DPM och Azure Backup Server stöds inte just nu.
3. Du kan visa rapporter över valv och över prenumerationer, om samma lagringskonto har konfigurerats för varje hello valv. Lagringskonto har valts måste vara i hello samma region som recovery services-valvet.
4. hello frekvensen för schemalagd uppdatering för hello rapporter är 24 timmar i Power BI. Du kan också utföra en ad hoc-uppdatering av hello rapporter i Power BI, där case senaste data i kundlagringskontot används för att återge rapporter. 

## <a name="prerequisites"></a>Krav
1. Skapa en [Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account) tooconfigure för rapporteras. Det här lagringskontot används för att lagra rapporter relaterade data.
2. [Skapa en Power BI-konto](https://powerbi.microsoft.com/landing/signin/) tooview, anpassa och skapa egna rapporter med Power BI-portalen.
3. Registerresursleverantören har hello **Microsoft.insights** om inte registrerad redan, med hello prenumeration av lagringskonto och Recovery Services-hello prenumeration valvet tooenable reporting data tooflow toohello Storage-konto. toodo Hej samma, måste du gå tooAzure portal > prenumeration > resursproviders och Sök efter den här providern tooregister den. 

## <a name="configure-storage-account-for-reports"></a>Konfigurera lagringskonto för rapporter
Använd hello följande steg tooconfigure hello storage-konto för recovery services-ventilen med hjälp av Azure portal. Detta är en engångskonfiguration och när lagringskontot har konfigurerats kan du gå tooPower BI direkt tooview Innehållspaketet och använda rapporter.
1. Om du redan har ett Recovery Services-valv öppna fortsätta toonext steg. Om du inte behöver öppna en Recovery Services-valv men i hello Azure-portalen på hello hubbmenyn, klicka på **Bläddra**.

   * Skriv i hello lista över resurser, **återställningstjänster**.
   * När du börjar skriva in hello listan filtreras baserat på dina indata. När du ser **Recovery Services-valv** klickar du på det.

      ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     hello visas Recovery Services-valv. Välj ett valv hello listan av Recovery Services-valv.

     hello valda valvet instrumentpanelen öppnas.
2. Hello listan med objekt som visas under valv, klickar du på **säkerhetskopiering rapporter** under övervakning och rapporter avsnittet tooconfigure hello storage-konto för rapporter.

      ![Välj säkerhetskopiering rapporter menyn objektet steg 2](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. På hello säkerhetskopiering rapporter bladet, klickar du på **konfigurera** knappen. Detta öppnar hello Azure Application Insights blad som används för att överföra data toocustomer storage-konto.

      ![Konfigurera lagring konto steg 3](./media/backup-azure-configure-reports/configure-storage-account.PNG)
4. Ange hello Status växlingsknappen för**på** och välj **Arkivera tooa Lagringskonto** kryssrutan så att reporting data kan börjar flöda i toohello storage-konto.

      ![Aktivera diagnostik steg 4](./media/backup-azure-configure-reports/set-status-on.png)
5. Klicka på Lagringskontot objektväljaren och väljer hello storage-kontot hello listan för att lagra data och klicka på **OK**.

      ![Välj kontot steg 5](./media/backup-azure-configure-reports/select-storage-account.png)
6. Välj **AzureBackupReport** kryssrutan och även flytta skjutreglaget hello tooselect kvarhållningsperiod för detta rapportdata. Rapportdata i hello storage-konto sparas av hello perioden som valts med hjälp av skjutreglaget.

      ![Välj kontot steg 6](./media/backup-azure-configure-reports/save-configuration.png)
7. Granska alla hello ändringar och klicka på **spara** knappen överst, som visas i hello bild ovan. Den här åtgärden säkerställer att alla dina ändringar sparas och storage-konto har nu konfigurerats för att lagra rapportinformationen.

> [!NOTE]
> När du konfigurerar rapporter genom att spara storage-konto, bör du **vänta 24 timmar** för inledande data push toocomplete. Efter denna tid bör du importera Azure Backup-Innehållspaketet i Power BI. Se [frågor och svar](#frequently-asked-questions) mer information. 
>
>

## <a name="view-reports-in-power-bi"></a>Visa rapporter i Power BI 
När du konfigurerar lagringskonto för rapporter med hjälp av recovery services-ventilen, tar cirka 24 timmar för att rapportera data toostart flödar i. Använd hello följande steg efter 24 timmar för att skapa lagringskontot tooview rapporter i Power BI:
1. [Logga in](https://powerbi.microsoft.com/landing/signin/) tooPower BI.
2. Klicka på **hämta Data** och klickar på Hämta under **Services** i innehållsbiblioteket Pack. Använd steg som nämns i [Power BI-dokumentationen tooaccess content pack](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-packs-services/).

     ![Importera Innehållspaketet](./media/backup-azure-configure-reports/content-pack-import.png)
3. Typen **Azure Backup** i sökfältet och klicka på **blir det nu**.

      ![Hämta Innehållspaketet](./media/backup-azure-configure-reports/content-pack-get.png)
4. Ange hello lagringskontonamnet konfigurerade i steg 5 ovan och klicka på **nästa** knappen.

    ![Ange namnet på lagringskontot](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Ange hello lagringskontonyckel för det här lagringskontot. Du kan [visa och kopiera åtkomstnycklar för lagring](../storage/common/storage-create-storage-account.md#manage-your-storage-account) genom att gå tooyour storage-konto i Azure-portalen. 

     ![Ange storage-konto](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>
     
6. Klicka på **inloggning** knappen. När inloggningen är klar, hämta **dataimport** meddelande.

    ![Importera Innehållspaketet](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>
    
    När du har tid, **lyckade** meddelande när hello importen är klar. Det kan ta lite längre tooimport hello Innehållspaketet, om det finns stora mängder data i hello storage-konto.
    
    ![Importera lyckade Innehållspaketet](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>
    
7. När data har importerats **Azure Backup** Innehållspaketet är synliga i **appar** i hello navigeringsfönstret. hello listan visas nu Azure Backup-instrumentpanelen, rapporter och dataset med en gul stjärna som anger nyligen importerade rapporter. 

     ![Azure Backup-Innehållspaketet](./media/backup-azure-configure-reports/content-pack-azure-backup.png) <br/>
     
8. Klicka på **Azure Backup** under instrumentpaneler, vilket visar att en uppsättning fästa viktiga rapporter.

      ![Azure Backup-instrumentpanelen](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. tooview hello fullständig uppsättning rapporter, klicka på rapport hello instrumentpanelen.

      ![Azure Backup-jobbet hälsa](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Klicka på varje flik i hello rapporter tooview rapporter i detta område.

      ![Azure Backup rapporter flikar](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
1. **Hur jag kontrollera om reporting data har börjat flödar i toostorage konto?**
    
    Du kan gå toohello lagring konto konfigurerade och Välj behållare. Om hello behållaren har en post för insights-loggar-azurebackupreport, innebär det att rapportera data har startats flödar i.

2. **Vad är hello ofta toostorage kontot för push-data och Azure Backup-Innehållspaketet i Power BI?**

   För användare i dag 0, tar det cirka 24 timmar toopush data toostorage konto. När den här första push compelete kan uppdateras data med hello följande frekvens som visas i hello bilden nedan. 
      * Data som är relaterade för**jobb, aviseringar, säkerhetskopiering objekt, valv, skyddade servrar och principer** pushas toocustomer storage-konto som och när du är inloggad.
      * Data som är relaterade för**lagring** pushas toocustomer lagringskonto var 24: e timme.
   
    ![Azure Backup rapporter data push frekvens](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  Powerbi har en [en gång om dagen för schemalagd uppdatering](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Du kan utföra en manuell uppdatering av hello data i Power BI för hello-Innehållspaketet.

3. **Hur länge behålla hello rapporter?** 

   Du kan välja period av rapportdata i hello storage-konto (med steg 6 i Konfigurera storage-konto för rapporter ovan) när du konfigurerar storage-konto. Förutom att du kan [analysera rapporter i excel](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) och spara dem under en längre period, enligt dina behov. 

4. **Ser jag mina data i rapporter när du har konfigurerat hello storage-konto?**

   Alla hello data som genereras när **”konfigurera storage-konto”** pushas toohello storage-konto och kommer att vara tillgängliga i rapporter. Dock **inte push-pågående jobb** för rapportering. När hello jobbet har slutförts eller misslyckas, skickas tooreports.

5. **Om det redan har konfigurerat hello lagringsrapporter konto tooview kan jag ändra hello configuration toouse ett annat lagringskonto?** 

   Ja, du kan ändra hello configuration toopoint tooa olika storage-konto. Vid anslutning tooAzure säkerhetskopiering Innehållspaketet bör du använda hello nyligen konfigurerade storage-konto. Dessutom när ett annat lagringskonto har konfigurerats kan skulle nya dataflöde i det här lagringskontot. Men äldre data (innan du ändrar hello configuration) skulle kvar i hello äldre storage-konto.

6. **Kan jag visa rapporter över valv och över prenumerationer?** 

   Ja, du kan konfigurera hello samma lagringskonto över olika valv tooview mellan valvet rapporter. Du kan också konfigurera hello samma lagringskonto för valv över prenumerationer. Du kan sedan använda det här lagringskontot vid anslutning tooAzure säkerhetskopiering Innehållspaketet i Power BI tooview hello-rapporter. Hej lagringskonto har valts bör dock i hello samma region som recovery services-valvet.
   
## <a name="next-steps"></a>Nästa steg
Nu när du har konfigurerat hello storage-konto och importerade Azure Backup-Innehållspaketet, hello nästa steg är toocustomize dessa rapporter och använda reporting modellen toocreate rapporter. Läs hello följande artiklar för mer information.

* [Med Azure Backup reporting datamodellen](backup-azure-reports-data-model.md)
* [Filtrera rapporter i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Skapa rapporter i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)

