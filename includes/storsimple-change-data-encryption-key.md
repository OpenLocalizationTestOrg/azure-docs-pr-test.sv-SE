<!--author=SharS last changed: 12/01/15-->

### <a name="step-1-authorize-a-device-toochange-hello-service-data-encryption-key-in-hello-azure-classic-portal"></a>Steg 1: Ge en enhet toochange hello krypteringsnyckel för tjänstdata i hello klassiska Azure-portalen
Vanligtvis begär hello enhetsadministratör hello service administratören godkänna en enhet toochange service datakrypteringsnycklar. Hej administratör sedan godkänner hello enhetsnyckel toochange hello.

Det här steget utförs i hello klassiska Azure-portalen. Hej administratör kan välja en enhet från listan hello-enheter som är berättigade toobe behörighet. hello enhet är sedan behöriga toostart hello krypteringsnyckel för tjänstdata ändra processen.

#### <a name="which-devices-can-be-authorized-toochange-service-data-encryption-keys"></a>Vilka enheter kan auktoriseras toochange service datakrypteringsnycklar?
En enhet måste uppfylla följande villkor innan det kan behöriga tooinitiate service encryption key dataändringar hello:

* hello enheten måste vara online toobe som är berättigade till auktoriseringen av tjänsten data kryptering ändringen.
* Du kan tillåta hello samma enhet igen efter 30 minuter ändrar hello nyckeln inte har initierats.
* Du kan tillåta en annan enhet, förutsatt att hello ändringen inte har initierats av hello tidigare godkänd enhet. När hello ny enhet har auktoriserats kan inte hello gamla enheten initiera hello ändringen.
* Du kan inte godkänna en enhet när hello förnyelse av krypteringsnyckel för tjänstdata hello pågår.
* Du kan godkänna en enhet när några av hello enheter som har registrerats med hello-tjänsten har flyttats över hello kryptering medan andra inte har. I sådana fall är hello berättigade enheter hello de som har slutfört hello tjänsten data encryption key ändras.

> [!NOTE]
> I hello klassiska Azure-portalen behörig StorSimple virtuella enheter inte visas i hello lista över enheter som kan vara toostart hello ändringen.
> 
> 

Utför följande steg tooselect hello och auktorisera en enhet tooinitiate hello tjänsten data kryptering ändringen.

#### <a name="tooauthorize-a-device-toochange-hello-key"></a>tooauthorize en enhet toochange hello nyckel
1. På hello service instrumentpanelen klickar du på **ändra krypteringsnyckel för tjänstdata**.
   
    ![Ändra service krypteringsnyckeln](./media/storsimple-change-data-encryption-key/HCS_ChangeServiceDataEncryptionKey-include.png)
2. I hello **ändra krypteringsnyckel för tjänstdata** dialogrutan väljer och auktorisera en enhet tooinitiate hello tjänsten data kryptering ändringen. hello listrutan har alla kvalificerade hello-enheter som kan godkännas.
3. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-change-data-encryption-key/HCS_CheckIcon-include.png).

### <a name="step-2-use-windows-powershell-for-storsimple-tooinitiate-hello-service-data-encryption-key-change"></a>Steg 2: Använda Windows PowerShell för StorSimple tooinitiate hello tjänsten data encryption key ändras
Det här steget utförs i hello Windows PowerShell för StorSimple-gränssnittet på hello behörighet StorSimple-enhet.

> [!NOTE]
> Inga åtgärder kan utföras i hello klassiska Azure-portalen för din StorSimple Manager-tjänsten tills hello nyckelförnyelse har slutförts.
> 
> 

Om du använder hello enhetens seriekonsol tooconnect toohello Windows PowerShell-gränssnittet, utföra hello följande steg.

#### <a name="tooinitiate-hello-service-data-encryption-key-change"></a>Ändra tooinitiate hello krypteringsnyckel för tjänstdata
1. Välj alternativ 1 toolog med fullständig åtkomst.
2. I hello kommandotolk, skriver du:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. När hello cmdleten har slutförts får du en ny krypteringsnyckel för tjänstdata. Kopiera och spara den här nyckeln för användning i steg 3 i den här processen. Den här nyckeln kommer att användas tooupdate alla hello återstående enheter som har registrerats med hello StorSimple Manager-tjänsten.
   
   > [!NOTE]
   > Den här processen måste initieras inom fyra timmar för att auktorisera en StorSimple-enhet.
   > 
   > 
   
   Den här nya nyckeln skickas toohello service toobe pushas tooall hello enheter som är registrerade med hello-tjänsten. En avisering visas sedan på instrumentpanelen för hello-tjänsten. hello-tjänsten kommer att inaktivera alla hello åtgärder på hello registrerade enheter och hello enhetsadministratör måste sedan tooupdate hello krypteringsnyckel för tjänstdata på hello andra enheter. Dock kommer hello I/o (värdar skickar data toohello moln) inte att avbrytas.
   
   Om du har en enda enhet registrerats tooyour service, hello förnyelse processen är slutförd och du kan hoppa över hello nästa steg. Om du har flera enheter registrerade tooyour tjänsten fortsätta toostep 3.

### <a name="step-3-update-hello-service-data-encryption-key-on-other-storsimple-devices"></a>Steg 3: Uppdatera hello krypteringsnyckel för tjänstdata på andra StorSimple-enheter
Dessa steg måste utföras i hello Windows PowerShell-gränssnittet för din StorSimple-enhet om du har flera enheter registrerade tooyour StorSimple Manager-tjänsten. hello-nyckel som du fick i steg 2: använda Windows PowerShell för StorSimple tooinitiate hello tjänsten data encryption key ändringen måste vara används tooupdate alla hello återstående StorSimple-enheten registrerad med hello StorSimple Manager-tjänsten.

Utföra hello följande steg tooupdate hello service datakryptering på enheten.

#### <a name="tooupdate-hello-service-data-encryption-key"></a>tooupdate hello krypteringsnyckel för tjänstdata
1. Använda Windows PowerShell för StorSimple tooconnect toohello-konsolen. Välj alternativ 1 toolog med fullständig åtkomst.
2. I hello kommandotolk, skriver du:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Ange hello krypteringsnyckel för tjänstdata som du fick i [steg 2: använda Windows PowerShell för StorSimple tooinitiate hello tjänsten data encryption key ändringen](#to-initiate-the-service-data-encryption-key-change).

