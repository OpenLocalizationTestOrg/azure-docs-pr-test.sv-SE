<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> När du gör ändringar toohello StorSimple nätverkskort för SharePoint RBS konfigurationen måste du vara inloggad med ett konto som tillhör toohello gruppen Domänadministratörer. Dessutom måste du komma åt hello konfigurationssidan från en webbläsare som körs på hello samma värden som Central Administration.
> 
> 

#### <a name="tooconfigure-rbs"></a>tooconfigure RBS
1. Öppna hello Central Administration av SharePoint-sida och Bläddra för**systeminställningar**. 
2. I hello **Azure StorSimple** klickar du på **konfigurera StorSimple nätverkskort**.
   
    ![Konfigurera hello StorSimple-kort](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. På hello **konfigurera StorSimple nätverkskort** sidan:
   
   1. Kontrollera att hello **Aktivera redigering sökväg** är markerad.
   2. Hello i textrutan Ange hello Universal Naming Convention (UNC) för hello blobstore.
      
      > [!NOTE]
      > hello BLOB store volym måste finnas på en iSCSI-volymen som konfigurerats på hello StorSimple-enhet.

   3. Klicka på hello **aktivera** knapp under varje hello innehållsdatabaser som du vill tooconfigure för Fjärrlagring.
      
      > [!NOTE]
      > hello blobstore måste vara delad och kan nås av alla frontend (WFE) webbservrar och hello-användarkonto som har konfigurerats för hello SharePoint-servergrupp måste ha åtkomst toohello resursen.
      
      ![Aktivera hello RBS provider](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      När du aktiverar eller inaktiverar RBS dessutom visas hello följande meddelande.
      
      ![Konfigurera StorSimple nätverkskort aktivera inaktivera](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Klicka på hello **uppdatering** tooapply hello konfiguration. När du klickar på hello **uppdatering** knappen hello RBS Konfigurationsstatus kommer att uppdateras på alla WFE-servrar och hello hela servergruppen kommer att RBS-aktiverade. hello följande meddelande visas.
      
      ![Kortet configuration meddelande](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > Om du konfigurerar RBS för en SharePoint-grupp med ett mycket stort antal databaser (större än 200), kan hello Central Administration av SharePoint-webbsida timeout. Om det inträffar kan du uppdatera hello-sidan. Detta påverkar inte hello konfigurationsprocessen.

4. Verifiera hello konfiguration:
   
   1. Logga in toohello Central Administration av SharePoint-webbplats och bläddra toohello **konfigurera StorSimple nätverkskort** sidan.
   2. Kontrollera hello configuration information toomake till att de matchar hello-inställningar som du angav. 
5. Kontrollera att RBS fungerar korrekt:
   
   1. Ladda upp ett dokument tooSharePoint. 
   2. Bläddra toohello UNC-sökväg som du har konfigurerat. Se till att hello RBS katalogstruktur har skapats och att den innehåller hello upp objektet.
6. (Valfritt) Du kan använda hello Microsoft RBS `Migrate()` PowerShell-cmdlet som ingår i SharePoint toomigrate befintlig BLOB innehåll toohello StorSimple-enhet. Mer information finns i [migrerar innehåll till eller från RBS i SharePoint 2013] [ 6] eller [migrerar innehåll till eller från RBS (SharePoint Foundation 2010)] [7].
7. (Valfritt) Test-installationer Kontrollera du att hello-BLOB har flyttats utanför hello innehållsdatabas på följande sätt: 
   
   1. Starta SQL Management Studio.
   2. Köra hello ListBlobsInDB_2010.sql eller ListBlobsInDB_2013.sql, enligt följande.
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      Om RBS är korrekt konfigurerad, visas ett nullvärde i hello SizeOfContentInDB kolumn för alla objekt som har överförts och har externalized med RBS.
8. (Valfritt) När du konfigurerar RBS och flytta alla BLOB innehåll toohello StorSimple-enhet kan du flytta hello innehållsdatabasen toohello enhet. Om du väljer toomove hello innehållsdatabasen, rekommenderar vi att du konfigurerar hello innehållsdatabasen lagring på hello enhet som en primär volym. Använd upprätta sedan SQL Server best practices toomigrate hello innehållsdatabasen toohello StorSimple-enhet. 
   
   > [!NOTE]
   > Flytta hello innehållsdatabasen toohello enheten stöds endast för hello StorSimple 8000-serien (den inte stöds för hello 5000 och 7000-serien).
   
   Om du sparar Blobbar och hello innehållsdatabasen i separata volymer på hello StorSimple-enhet, rekommenderar vi att du konfigurerar dem i hello samma volymbehållare. Detta säkerställer att de ska säkerhetskopieras tillsammans.
   
   > [!WARNING]
   > Om du inte har aktiverat RBS rekommenderar vi inte flytta hello innehållsdatabasen toohello StorSimple-enhet. Detta är en otestade konfiguration.
   
9. Gå toohello nästa steg: [konfigurera skräpinsamling](#configure-garbage-collection).

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
