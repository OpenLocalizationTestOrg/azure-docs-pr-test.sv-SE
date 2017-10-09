<!--author=alkohli last changed:01/14/2016-->


#### <a name="toocreate-a-new-service"></a>toocreate en ny tjänst
1. Med autentiseringsuppgifterna för ditt Microsoft-konto kan logga in toohello klassiska Azure-portalen på denna URL: [https://manage.windowsazure.com/](https://manage.windowsazure.com/).
2. I hello klassiska Azure-portalen klickar du på **ny** > **datatjänster** > **StorSimple Manager** > **snabb Skapa**.
3. Hello formuläret som visas och hello följande:
   
   1. Ange ett unikt **namn** för din tjänst. Detta är ett eget namn som kan vara används tooidentify hello-tjänsten. hello namnet kan innehålla mellan 2 och 50 tecken som kan vara bokstäver, siffror och bindestreck. hello-namnet måste börja och sluta med en bokstav eller en siffra.
   2. Ange en **plats** för din tjänst. Välj en plats närmaste toohello geografisk region där du vill att toodeploy i allmänhet enheten. Du kan också toofactor i hello följande: 
      
      * Om du har befintliga arbetsbelastningar i Azure som du också avser toodeploy med din StorSimple-enhet bör du använda det datacentret.
      * Din StorSimple Manager-tjänst och Azure-lagring kan finnas på två olika platser. I sådana fall är du nödvändiga toocreate hello StorSimple Manager och Azure-lagringskontot separat. toocreate ett Azure storage-konto går toohello Azure Storage-tjänst i hello klassiska Azure-portalen och följer hello stegen i [skapa ett Azure Storage-konto](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account). När du skapar detta konto Lägg till den toohello StorSimple Manager-tjänsten genom att följa stegen hello i [konfigurera ett nytt lagringskonto för tjänsten hello](../articles/storsimple/storsimple-deployment-walkthrough.md#configure-a-new-storage-account-for-the-service).
   3. Välj en **prenumeration** hello nedrullningsbara listan. hello prenumerationen är länkade tooyour fakturering konto. Det här fältet syns inte om du bara har en prenumeration.
   4. Välj **skapa ett nytt lagringskonto** tooautomatically skapa ett lagringskonto med hello-tjänsten. Det här lagringskontot har ett särskilt namn, till exempel "storsimplebwv8c6dcnf." Avmarkera kryssrutan om du behöver ha din data på en annan plats. 
   5. Klicka på **skapa StorSimple Manager** toocreate hello-tjänsten.
   
   ![Skapa StorSimple Manager](./media/storsimple-create-new-service/HCS_CreateAService-include.png)
   
   Du kommer att dirigerad toohello **Service** landningssida. hello-tjänsten kommer att ta en stund. När hello-tjänsten har skapats kan du kommer att meddelas och hello hello tjänstens status kommer att ändras för**Active**.
   
   ![Skapa en tjänst](./media/storsimple-create-new-service/HCS_StorSimpleManagerServicePage-include.png)

![Video tillgänglig](./media/storsimple-create-new-service/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur toocreate en ny StorSimple Manager-tjänst klickar du på [här](https://azure.microsoft.com/documentation/videos/create-a-storsimple-manager-service/).

