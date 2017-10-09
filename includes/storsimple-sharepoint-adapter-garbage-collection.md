<!--author=SharS last changed: 9/17/15-->

I den här proceduren ska du:

1. [Förbereda toorun hello Underhållaren körbara](#to-prepare-to-run-the-maintainer) .
2. [Förbereda hello innehållsdatabasen och Papperskorgen för omedelbar borttagning av överblivna Blobbar](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Kör Maintainer.exe](#to-run-the-maintainer).
4. [Återställa hello innehållsdatabasen och inställningar för Papperskorgen](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="tooprepare-toorun-hello-maintainer"></a>tooprepare toorun hello Underhållaren
1. Öppna hello SharePoint 2013 Management Shell på hello Web front-end-server, som en administratör.
2. Navigera toohello mappen *starta enheten*: \Program\Microsoft SQL Remote Blob Storage 10.50\Maintainer\.
3. Byt namn på **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** för**web.config**.
4. Använd `aspnet_regiis -pdf connectionStrings` toodecrypt hello web.config-filen.
5. I hello dekrypterade web.config-filen under hello `connectionStrings` , lägga till hello anslutningssträngen för SQL server-instans och hello-innehållsdatabasens namn. Se följande exempel hello.
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Använd `aspnet_regiis –pef connectionStrings` toore-kryptera hello web.config-filen. 
7. Byt namn på web.config tooMicrosoft.Data.SqlRemoteBlobs.Maintainer.exe.config. 

#### <a name="tooprepare-hello-content-database-and-recycle-bin-tooimmediately-delete-orphaned-blobs"></a>ta bort överblivna Blobbar tooprepare hello innehållsdatabasen och Papperskorgen tooimmediately
1. Kör hello följande uppdateringsfrågor för hello innehåll måldatabasen på hello SQL Server i SQL Management Studio: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. På hello web front-end-server under **Central Administration**, redigera hello **Web Application allmänna inställningar för** för hello önskad innehållsdatabas tootemporarily inaktivera hello Papperskorgen. Den här åtgärden kommer också att tom hello Papperskorgen om eventuella relaterade webbplatssamlingar. toodo, genom att välja **Central Administration** -> **programhantering** -> **webbprogram (hantera webbprogram)**  ->  **SharePoint - 80** -> **allmänna programinställningar**. Ange hello **Återanvänd Bin Status** för**OFF**.
   
    ![Web Application allmänna inställningar](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="toorun-hello-maintainer"></a>toorun hello Underhållaren
* På hello web front-end-server, hello SharePoint 2013 Management Shell, kör hello Underhållaren på följande sätt:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Endast hello `GarbageCollection` åtgärden stöds för StorSimple just nu. Observera också att hello parametrar som utfärdats för Microsoft.Data.SqlRemoteBlobs.Maintainer.exe är skiftlägeskänsliga. 
  > 
  > 

#### <a name="toorevert-hello-content-database-and-recycle-bin-settings"></a>toorevert hello innehållsdatabasen och inställningar för Papperskorgen
1. Kör hello följande uppdateringsfrågor för hello innehåll måldatabasen på hello SQL Server i SQL Management Studio:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. På hello web front-end-server, i **Central Administration**, redigera hello **Web Application allmänna inställningar för** för hello önskad innehållsdatabas toore aktivera hello Papperskorgen. toodo, genom att välja **Central Administration** -> **programhantering** -> **webbprogram (hantera webbprogram)**  ->  **SharePoint - 80** -> **allmänna programinställningar**. Ange hello Återanvänd Bin Status för**på**.

