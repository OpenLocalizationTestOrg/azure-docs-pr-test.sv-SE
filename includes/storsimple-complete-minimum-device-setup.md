<!--author=alkohli last changed: 9/17/15-->

#### <a name="toocomplete-hello-minimum-storsimple-device-setup"></a>toocomplete hello minsta StorSimple-enhetsinställningen
1. I hello **enheter** väljer hello enhet, klickar du på pilen hello mot hello enheten namn toogo toohello specifika enhetens sida. 
   
    ![Enhetssida med enhet online](./media/storsimple-complete-minimum-device-setup/HCS_DevicesPageM-include.png) 
2. Klicka på snabbstartsikonen ![Snabbstart-ikon](./media/storsimple-complete-minimum-device-setup/HCS_QuickStartIcon-include.png) tooaccess hello enhetens snabbstartsida. Klicka på **Slutför Enhetsinställningar** toostart hello **enhetskonfiguration** guiden.
   
    ![Snabbstartsida för enheten](./media/storsimple-complete-minimum-device-setup/Device_Quick_Start_page_1M.png)
3. På hello **grundläggande inställningar** sidan, hello följande:
   
   1. Ange ett **eget namn** för din enhet. hello standard-enhetsnamnet har information som enhetsmodell hello och serienummer. Du kan tilldela ett eget namn för in too64 tecken toomanage din enhet.
   2. Ange hello **tidszon** baserat på hello geografiska plats i vilka hello enheten ska distribueras. Enheten använder den här tidszonen för alla schemalagda åtgärder.
   3. Under **DNS-inställningar**, anger du en adress för din **sekundära DNS-server**. Om du använder IPv6 fylls hello fältet i baserat på hello IPv6-prefix som ges i hello Windows PowerShell-gränssnittet. 
      Om hello sekundära DNS-servern inte är konfigurerad, du är inte tillåtna toosave enhetskonfigurationen.
   4. Aktivera minst ett nätverk för iSCSI under iSCSI-aktiverade gränssnitt. Minst ett nätverksgränssnitt måste toobe moln-aktiverat och ett måste toobe iSCSI-aktiverade. DATA 0 är automatiskt moln-aktiverat.
      
      ![Grundläggande inställningar för minimala StorSimple-enhetsinställningar](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupBasicSettings1-include.png)
4. Klicka på pilikonen hello. ![StorSimple-pilikonen](./media/storsimple-complete-minimum-device-setup/HCS_ArrowIcon-include.png)
5. På hello **nätverksgränssnitt** anger hello fasta IP-adresser för styrenhet 0 och 1. Om hello fasta DATA 0 gränssnittet har konfigurerats för IPv4, hello IP-adresser måste toobe enligt hello IPv4-format. Om du angav ett prefix för IPv6-konfigurationen, fylls hello fasta IP-adresser automatiskt i de här fälten.

    > [!NOTE] 
    > - hello styrenhets-fästa IP-adresser måste toobe frigöra IP-adresser inom hello undernät tillgänglig genom hello enhetens IP-adress.
    > - hello fasta IP-adresser för hello controller används för att underhålla hello uppdateringar toohello enhet och hello statiska IP-adresser måste därför vara dirigerbara och kunna tooconnect toohello Internet.

    ![Nätverksgränssnitt för minimala StorSimple-enhetsinställningar](./media/storsimple-complete-minimum-device-setup/HCS_MinDeviceSetupNetworkInterfaces2-include.png)

1. Klicka på kryssikonen hello ![StorSimple-kryssikon](./media/storsimple-complete-minimum-device-setup/HCS_CheckIcon-include.png).
   Du kommer tillbaka toohello enhet **Snabbstart** sidan.
   
   > [!NOTE]
   > Du kan ändra alla hello andra Enhetsinställningar när som helst genom att öppna hello **konfigurera** sidan.
   > 
   > 

![Video tillgänglig](./media/storsimple-complete-minimum-device-setup/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur toocomplete hello minimala Enhetsinstallationen klickar du på [här](https://azure.microsoft.com/documentation/videos/minimum-storsimple-device-setup/).

