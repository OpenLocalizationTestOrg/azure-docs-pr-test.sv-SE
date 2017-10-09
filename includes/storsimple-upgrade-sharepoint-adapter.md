<!--author=SharS last changed: 9/17/15-->

### <a name="upgrade-sharepoint-2010-toosharepoint-2013-and-then-install-hello-storsomple-adapter-for-sharepoint"></a>Uppgradera SharePoint 2010 tooSharePoint 2013 och installera sedan hello StorSomple nätverkskort för SharePoint
> [!IMPORTANT]
> Alla filer som flyttats tidigare tooexternal lagring via RBS inte är tillgängliga förrän hello uppgraderingen är klar och hello RBS funktionen aktiveras igen. toolimit användare påverkas, utföra en uppgradering eller ominstallation under ett planerat underhåll.
> 
> 

#### <a name="tooupgrade-sharepoint-2010-toosharepoint-2013-and-then-install-hello-adapter"></a>tooupgrade SharePoint 2010 tooSharePoint 2013 och sedan installera hello-kort
1. I hello SharePoint 2010-servergruppen externalized Obs hello BLOB lagra sökvägen för hello Blobbar och hello innehållsdatabaser som RBS är aktiverad. 
2. Installera och konfigurera hello nya SharePoint 2013-grupp. 
3. Flytta databaser, program och webbplatssamlingar från hello SharePoint 2010-servergruppen toohello nya SharePoint 2013-grupp. Anvisningar finns för[översikt över hello uppgraderingsprocessen tooSharePoint 2013](https://technet.microsoft.com/library/cc262483.aspx).
4. Installera hello StorSimple nätverkskort för SharePoint på nya hello-grupp. Gå för[installera hello StorSimple-kortet för SharePoint](#install-the-storsimple-adapter-for-sharepoint) för procedurer.
5. Med hjälp av hello information som du antecknade i steg 1, aktivera RBS för hello samma uppsättning innehållsdatabaser och ange hello samma BLOB lagra sökväg som användes i hello SharePoint 2010-installationen. Gå för[konfigurera RBS](#configure-rbs) för procedurer. När du har slutfört det här steget ska tidigare externalized filer vara tillgänglig från nya hello-grupp. 

### <a name="upgrade-hello-storsimple-adapter-for-sharepoint"></a>Uppgradera hello StorSimple nätverkskort för SharePoint
> [!IMPORTANT]
> Du bör schemalägga den här uppgraderingen toooccur under en planerad underhållsperiod för hello följande orsaker:
> 
> * Tidigare blir externalized innehåll inte tillgängliga förrän hello nätverkskort installeras.
> * Allt innehåll överförs toohello plats när du avinstallerar hello tidigare version av hello StorSimple nätverkskort för SharePoint, men innan du installerar hello ny version, kommer att lagras i hello innehållsdatabasen. Du behöver toomove innehåll toohello StorSimple enheten när du installerar nya hello-kort. Du kan använda hello Microsoft` RBS Migrate()` PowerShell-cmdlet som ingår i SharePoint toomigrate hello-innehåll. Mer information finns i [migrerar innehåll till eller från RBS](https://technet.microsoft.com/library/ff628255.aspx). 
> 
> 

#### <a name="tooupgrade-hello-storsimple-adapter-for-sharepoint"></a>tooupgrade hello StorSimple-kortet för SharePoint
1. Avinstallera hello tidigare version av StorSimple-kortet för SharePoint.
   
   > [!NOTE]
   > Detta inaktiverar automatiskt RBS på hello innehållsdatabaserna. Befintliga Blobbar finns dock kvar på hello StorSimple-enhet. Eftersom RBS är inaktiverad och hello Blobbar inte har migrerat tillbaka toohello innehållsdatabaser, misslyckas alla förfrågningar för dessa BLOB. 
   > 
   > 
2. Installera hello nytt StorSimple-kort för SharePoint. hello nytt kort hello innehållsdatabaser som tidigare aktiverat eller inaktiverat för RBS identifierar automatiskt och använder hello tidigare inställningar.

