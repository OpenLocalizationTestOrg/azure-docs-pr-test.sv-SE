<!--author=alkohli last changed:02/10/2017-->


#### <a name="toocreate-a-new-service"></a>toocreate en ny tjänst

1. Använda ditt Microsoft-konto autentiseringsuppgifter toolog på toohello [Azure-portalen](https://portal.azure.com/).

2. I hello Azure-portalen klickar du på  **+**  och klicka sedan på hello marketplace **se alla**.

    ![Skapa StorSimple Device Manager](./media/storsimple-8000-create-new-service/createssdevman1.png)

    Sök efter _StorSimple Physical_. Markera och klicka på **StorSimple Physical Device Series** och sedan på **Skapa**. Du kan också i hello Azure-portalen klickar du på  **+**  och sedan under **lagring**, klickar du på **StorSimple fysisk enhet serien**.

    ![Skapa StorSimple Device Manager](./media/storsimple-8000-create-new-service/createssdevman11.png)

3. I hello **StorSimple Enhetshanteraren** bladet hello följande steg:
   
   1. Ange ett unikt **resursnamn** för tjänsten. Det här namnet är ett eget namn som kan vara används tooidentify hello-tjänsten. hello namnet kan innehålla mellan 2 och 50 tecken som kan vara bokstäver, siffror och bindestreck. hello-namnet måste börja och sluta med en bokstav eller en siffra.

   2. Välj en **prenumeration** hello nedrullningsbara listan. hello prenumerationen är länkade tooyour fakturering konto. Det här fältet syns inte om du bara har en prenumeration.

   3. Välj **Använd befintlig grupp** eller **Skapa ny grupp** för **Resursgrupp**. Mer information finns i avsnittet om [Azure-resursgrupper](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).
   
   4. Ange en **plats** för din tjänst. Välj en plats närmaste toohello geografisk region där du vill att toodeploy i allmänhet enheten. Du kan också toofactor i hello följande överväganden: 
      
      * Om du har befintliga arbetsbelastningar i Azure som du också avser toodeploy med din StorSimple-enhet bör du använda det datacentret.
      * StorSimple Device Manager-tjänsten och Azure-lagring kan finnas på två olika platser. I så fall kan är du obligatoriska toocreate hello StorSimple Enhetshanteraren och Azure storage-konto separat. toocreate ett Azure storage-konto går toohello Azure Storage-tjänst i hello Azure-portalen och följer hello stegen i [skapa ett Azure Storage-konto](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account). När du har skapat det här kontot, lägga till den toohello StorSimple enheten Manager-tjänsten genom att följa hello stegen i [konfigurera ett nytt lagringskonto för tjänsten hello](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#configure-a-new-storage-account-for-the-service).

   5. Välj **skapa ett nytt lagringskonto** tooautomatically skapa ett lagringskonto med hello-tjänsten. Ange ett namn för lagringskontot. Avmarkera kryssrutan om du behöver ha din data på en annan plats.

   6. Kontrollera **PIN-kod toodashboard** om du vill ha en snabb länk toothis tjänst på instrumentpanelen.
      
   7. Klicka på **skapa** toocreate hello StorSimple Enhetshanteraren.

       ![Skapa StorSimple Device Manager](./media/storsimple-8000-create-new-service/createssdevman2.png)
   
Hej tjänstgenereringen tar några minuter. När hello-tjänsten har skapats, visas ett meddelande och hello nytt tjänsten blad öppnas.
   
![Skapa StorSimple Device Manager](./media/storsimple-8000-create-new-service/createssdevman5.png)


