#### <a name="toocreate-a-cloud-appliance"></a>toocreate en moln-installation

1. I hello Azure-portalen, går toohello **StorSimple Enhetshanteraren** service.
2. Gå toohello **enheter** bladet. Hello kommandofältet hello service sammanfattning-bladet, klickar du på **skapa moln installation**.
    ![Skapa StorSimple-molninstallation](./media/storsimple-8000-create-cloud-appliance-u2/sca-create1.png)
3. I hello **skapa moln installation** bladet ange hello följande information.
   
    ![Skapa StorSimple-molninstallation](./media/storsimple-8000-create-cloud-appliance-u2/sca-create2m.png)
   
   1. **Namn** – Ett unikt namn för molninstallationen.
   2. **Modellen** -Välj hello modell hello molnet enhetens. En 8010-enhet erbjuder 30 TB standardlagring, medan 8020 har 64 TB Premium Storage. Ange 8010 toodeploy hämtningsscenarier på objektsnivå från säkerhetskopior. Välj 8020 toodeploy hög prestanda, låg latens arbetsbelastningar, eller Använd som en sekundär enhet för katastrofåterställning.
   3. **Version** -Välj hello version av hello molnet installation. hello version motsvarar toohello version av hello virtuell diskavbildning som används toocreate hello molnet installation. Angivna hello version av hello molnet installation avgör vilka fysiska enheten du växla över eller klona från, är det viktigt att du skapar en lämplig version av hello molnet installation.
   4. **Virtuellt nätverk** – ange ett virtuellt nätverk som du vill toouse med den här utrustningen i molnet. Om du använder Premiumlagring, måste du välja ett virtuellt nätverk som stöds med hello Premium Storage-konto. hello stöds inte för virtuella nätverk är nedtonade i listrutan hello. Du får en varning om du väljer ett virtuellt nätverk som inte stöds.
   5. **Undernät** -baserat på hello virtuellt nätverk har valts, hello listrutan visar hello associerade undernät. Tilldela en undernät tooyour moln installation.
   6. **Storage-konto** – Välj en toohold hello lagringskontoavbildningen hello molnet enhetens under etableringen. Det här lagringskontot måste vara i hello samma region som hello molnet installation och virtuella nätverk. Det bör inte användas för datalagring av hello fysiska eller hello molnet installation. Som standard skapas ett nytt lagringskonto för detta ändamål. Men om du vet att du redan har ett lagringskonto som är lämpligt kan kan du välja den hello-listan. Om du skapar en premium molnet installation, visar hello listrutan endast Premium Storage-konton.
      
      > [!NOTE]
      > hello molnet enheten fungerar bara med hello Azure storage-konton.
    
   7. Välj hello kryssrutan tooindicate som du förstår att hello data som lagras på hello molnet installation finns i ett Microsoft-datacenter.
       * När du bara använder en fysisk enhet så sparas krypteringsnyckeln med din enhet. Microsoft kan därmed inte dekryptera den.

       * När du använder en moln-installation, lagras både hello krypteringsnyckeln och dekrypteringsnyckeln hello i Microsoft Azure. Mer information finns i [Säkerhetsöverväganden vid användning av en molninstallation](../articles/storsimple/storsimple-security.md#storsimple-virtual-device-security).
   8. Klicka på **skapa** tooprovision hello molnet installation. hello enheten kan ta cirka 30 minuter toobe etableras. Du meddelas när hello molnet installation har skapats. Gå tooDevices bladet och hello lista över enheter uppdaterar toodisplay hello molnet installation. hello status hello-enhetens är **klar tooset in**.
      
      ![StorSimple-enhet för molnet redo tooset in](./media/storsimple-8000-create-cloud-appliance-u2/sca-create3.png)

