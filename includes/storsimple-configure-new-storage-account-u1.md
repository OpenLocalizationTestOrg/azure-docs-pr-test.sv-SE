<!--author=alkohli last changed: 9/17/15-->

#### <a name="tooadd-a-storage-account-in-storsimple-8000-series-update-10"></a>tooadd ett lagringskonto i StorSimple 8000 Series uppdatering 1.0
1. På StorSimple Manager-hello tjänstens startsida väljer du din tjänst och dubbelklicka på den. Detta tar du toohello **Snabbstart** sidan. Välj hello **konfigurera** sidan.
2. Klicka på **Lägg till/redigera lagringskonto**.
3. I hello **Lägg till/redigera Lagringskonto** dialogrutan klickar du på **Lägg till ny**.
4. I hello **Provider** , markera hello lämplig molntjänstleverantör. hello stöds providers är Azure, Amazon S3, Amazon S3 med RRS, HP och OpenStack. Ange hello autentiseringsuppgifter och hello plats som är associerade med hello lagringskontot för dina molntjänstleverantörer. hello fält visas för autentiseringsuppgifter kommer att vara olika beroende på hello molntjänstleverantör du angett. 
   
   * Om du har valt Azure som din molntjänstleverantör, anger du hello **namn** och hello primära **åtkomstnyckeln** för ditt Microsoft Azure storage-konto. För en Azure-konto fylls hello plats automatiskt i.
     
        ![Lägg till ett Azure Storage-konto](./media/storsimple-configure-new-storage-account-u1/AddAzureStorageaccount-include.png)
   * Om du har valt Amazon S3 eller Amazon S3 med RRS, anger du ett eget **namn på lagringskonto**, **åtkomstnyckel** och **hemlig nyckel**. För Amazon S3 och Amazon S3 med RRS stöds hello följande platser:
     
     * Standard för USA
     * USA, Väst (Oregon)
     * USA, Väst (norra Kalifornien)
     * EU (Ireland)
     * Asien och stillahavsområdet (Singapore)
     * Asien och stillahavsområdet (Sydney)
     * Asien och stillahavsområdet (Tokyo)
     * Sydamerika (Sao Paulo)
       
       ![Lägg till Amazon-lagringskonto](./media/storsimple-configure-new-storage-account-u1/AddAmazonStorageaccount-include.png)
   * Om du valt HP som din molntjänstleverantör, anger du ett eget **namn på lagringskonto**, **klient-ID**, **användarnamn** och **lösenord**. Hello följande platser stöds för HP:
     
     * USA, Östra
     * USA, Västra
       
       ![Lägg till HP-lagringskonto](./media/storsimple-configure-new-storage-account-u1/AddHPStorageaccount-include.png)
   * Om du har valt **Openstack** som din molntjänstleverantör, anger du **värdnamn**, **åtkomstnyckel** och **hemlig nyckel**.
     
     > [!NOTE]
     > Ett eget namn är tillåtet för alla hello molntjänstleverantörer förutom Azure. Du kan använda olika egna namn och skapa mer än ett lagringskonto med hello samma uppsättning autentiseringsuppgifter.
     > 
     > 
     
        ![Lägg till Openstack-lagringskonto](./media/storsimple-configure-new-storage-account-u1/AddOpenstackStorageaccount-include.png)
5. Välj **aktivera SSL-läge** toocreate en säker kanal för nätverkskommunikation mellan din enhet och hello moln. Rensa hello **aktivera SSL-läge** bara kryssrutan om du arbetar inom ett privat moln.
   
   > [!NOTE]
   > Om du använder HP som leverantör är SSL alltid aktiverat.
   > 
   > 
6. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png). Du meddelas när hello storage-konto har skapats.
7. hello nyligen skapade lagringskontot kommer att visas på hello **konfigurera** sidan **lagringskonton**. Klicka på **spara** toosave hello nytt lagringskonto. Klicka på **OK** när du uppmanas att bekräfta.

