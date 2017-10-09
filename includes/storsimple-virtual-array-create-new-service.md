#### <a name="toocreate-a-new-service"></a>toocreate en ny tjänst

1.  Med autentiseringsuppgifterna för ditt Microsoft-konto kan logga in toohello Azure-portalen på denna URL: <https://portal.azure.com/>. Om du distribuerar hello-enhet i Government portal, logga in på: <https://portal.azure.us/>

2.  I hello Azure-portalen klickar du på **+ ny** &gt; **lagring** &gt; **virtuella StorSimple-serien**.

    ![Skapa ny tjänst](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  I hello **StorSimple Enhetshanteraren** bladet som öppnas, hello följande:

    1.  Ange ett unikt **resursnamn** för tjänsten. hello resursnamnet är ett eget namn som kan vara används tooidentify hello-tjänsten. hello namnet kan innehålla mellan 2 och 50 tecken som kan vara bokstäver, siffror och bindestreck. hello-namnet måste börja och sluta med en bokstav eller en siffra.

    2.  Välj en **prenumeration** hello nedrullningsbara listan. hello prenumerationen är länkade tooyour fakturering konto. Det här fältet syns inte om du bara har en prenumeration.

    3.  För **resursgruppen**väljer du en befintlig eller skapa en ny grupp. Mer information finns i avsnittet om [Azure-resursgrupper](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

    4.  Ange en **plats** för din tjänst. Se [Azure-regioner](https://azure.microsoft.com/regions/#services) för mer information om vilka tjänster som är tillgängliga i vilken region. I allmänhet ska du välja en **plats** närmaste toohello-geografisk region där du vill att toodeploy din enhet. Du kan också toofactor i hello följande:

        -   Om du har befintliga arbetsbelastningar i Azure som du också avser toodeploy med din StorSimple-enhet, rekommenderar vi att du använder det datacentret.

        -   Din StorSimple-Enhetshanteraren och Azure-lagring kan vara två olika platser. I så fall kan är du obligatoriska toocreate hello StorSimple Enhetshanteraren och Azure storage-konto separat. toocreate ett Azure storage-konto går toohello Azure Storage-tjänst i hello Azure-portalen och följer hello stegen i [skapa ett Azure Storage-konto](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account). När du har skapat det här kontot, lägga till den toohello StorSimple enheten Manager-tjänsten genom att följa hello stegen i [konfigurera ett nytt lagringskonto för tjänsten hello](https://azure.microsoft.com/en-us/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service).

        -   Om distribuerar hello-enhet i hello Government Portal är hello StorSimple enheten Manager-tjänsten tillgänglig i Iowa för oss och vi Virginia platser.

    5.  Välj **skapar ett nytt Azure storage-konto** tooautomatically skapa ett lagringskonto med hello-tjänsten. Ange en **lagringskontonamnet**. Avmarkera kryssrutan om du behöver ha din data på en annan plats.

    6.  Kontrollera **PIN-kod toodashboard** om du vill ha en snabb länk toothis tjänst på instrumentpanelen.

    7.  Klicka på **skapa** toocreate hello StorSimple Enhetshanteraren.

        ![Skapa ny tjänst](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

Du är riktat toohello **Service** landningssida. Hej tjänstgenereringen tar några minuter. När hello-tjänsten har skapats kan du kommer att meddelas och hello hello tjänstens status kommer att ändras för**Active**.


