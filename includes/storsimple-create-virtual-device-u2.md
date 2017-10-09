#### <a name="toocreate-a-virtual-device"></a>toocreate en virtuell enhet
1. I hello Azure-portalen, går toohello **StorSimple Manager** service.
2. Gå toohello **enheter** sidan. Klicka på **Skapa virtuell enhet** längst hello hello **enheter** sidan.
3. I hello **dialogrutan Skapa virtuell enhet**, ange hello följande information.
   
    ![StorSimple skapa virtuell enhet](./media/storsimple-create-virtual-device-u2/CreatePremiumsva1.png)
   
   1. **Namn** – ett unikt namn för din virtuella enhet.
   2. **Modellen** -Välj hello modell för hello virtuell enhet. Det här fältet visas bara om du kör uppdatering 2 eller senare. Enhetsmodellen 8010 erbjuder 30 TB standardlagring medan 8020 har 64 TB premiumlagring. Välj 8010
   3. toodeploy hämtningsscenarier på objektsnivå från säkerhetskopior. Välj 8020 toodeploy hög prestanda, Låg fördröjning av arbetsbelastning eller som en sekundär enhet för katastrofåterställning.
   4. **Version** -Välj hello version av hello virtuell enhet. Om en 8020 enhetsmodell väljs, sedan visas hello versionsfältet inte toohello användare. Det här alternativet saknas om alla hello fysiska enheter som har registrerats med den här tjänsten kör uppdatering 1 (eller senare). Det här fältet visas bara om du har en blandning av före uppdatering 1 och uppdatering 1 fysiska enheter registrerade hello samma tjänst. Den angivna hello version av hello virtuella enheten avgör vilken fysisk enhet som du kan växling vid fel eller klona från så det är viktigt att du skapar en lämplig version av hello virtuell enhet. Välj:
      
      * Versionsuppdatering 0.3 om du växlar eller katastrofåterställer från en fysisk enhet som kör uppdatering 0.3 eller tidigare. 
      * Versionsuppdatering 1 om du växlar eller klonar från en fysisk enhet som kör uppdatering 1 (eller senare). 
   5. **Virtuellt nätverk** – ange ett virtuellt nätverk som du vill toouse med den här virtuella enheten. Om du använder Premiumlagring (uppdatering 2 eller senare), måste du välja ett virtuellt nätverk som stöds med hello Premium Storage-konto. hello stöds inte för virtuella nätverk är nedtonade i listrutan hello. Du får en varning om du väljer ett virtuellt nätverk som inte stöds. 
   6. **Lagringskonto för skapande av virtuell enhet** – Välj en toohold hello lagringskontoavbildningen av hello virtuella enheten under etableringen. Det här lagringskontot måste vara i hello samma region som hello virtuella enheten och virtuella nätverk. Det bör inte användas för datalagring av hello fysiska eller virtuella hello-enheten. Ett nytt lagringskonto kommer som standard att skapas för det här ändamålet. Men om du vet att du redan har ett lagringskonto som är lämpligt kan kan du välja den hello-listan. Om hur du skapar en premium virtuell enhet visas endast hello listrutan Premium Storage-konton. 
      
      > [!NOTE]
      > hello virtuella enheten fungerar bara med hello Azure storage-konton. Andra molntjänstleverantörer som Amazon, HP och OpenStack (som stöds för hello fysiska enheten) stöds inte för hello virtuella StorSimple-enheten.
      > 
      > 
   7. Klicka på hello markerat tooindicate som du förstår att hello data som lagras på hello virtuella enheten kommer att finnas i ett Microsoft-datacenter. När du bara använder en fysisk enhet så sparas krypteringsnyckeln med din enhet. Microsoft kan därmed inte dekryptera den. 
      
       När du använder en virtuell enhet lagras både hello krypteringsnyckeln och dekrypteringsnyckeln hello i Microsoft Azure. Mer information finns i [Säkerhetsöverväganden vid användning av en virtuell enhet](../articles/storsimple/storsimple-security.md#storsimple-virtual-device-security).
   8. Klicka på hello Kontrollera ikonen toocreate hello virtuell enhet. hello enheten kan ta cirka 30 minuter toobe etableras.
      
      ![Skapandefas för virtuell StorSimple-enhet](./media/storsimple-create-virtual-device-u2/StorSimple_VirtualDeviceCreating1M.png)

