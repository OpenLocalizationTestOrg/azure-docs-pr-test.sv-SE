<!--author=alkohli last changed: 01/12/17-->

#### <a name="toocomplete-hello-minimum-storsimple-device-setup"></a>toocomplete hello minsta StorSimple-enhetsinställningen

   > [!NOTE]
   > Du kan inte ändra hello enhetsnamn när hello minimala Enhetsinstallationen har slutförts.
   
1. Från hello tabular lista över enheter i hello **enheter** bladet Välj och klicka på enheten. hello-enheten är i ett **klar tooset in** tillstånd. Hej **enhetskonfiguration** blad öppnas.

     ![Nätverksgränssnitt för minimala StorSimple-enhetsinställningar](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig1.png)

2. I hello **enhetskonfiguration** bladet:
   
   1. Ange ett **eget namn** för din enhet. hello standard-enhetsnamnet har information som enhetsmodell hello och serienummer. Du kan tilldela ett eget namn för in too64 tecken toomanage din enhet.
   2. Ange hello **tidszon** baserat på hello geografiska plats i vilka hello enheten ska distribueras. Enheten använder den här tidszonen för alla schemalagda åtgärder.
   3. Under hello **DATA 0 inställningar**:

       1. DATA 0-nätverksgränssnittet visas som aktiverat med hello nätverksinställningar (IP, undernät, gateway) konfigurerats via hello installationsguiden. DATA 0 aktiveras också automatiskt för molnet samt iSCSI.

       2. Ange hello fasta IP-adresser för styrenhet 0 och 1. **hello styrenhets-fästa IP-adresser måste toobe frigöra IP-adresser inom hello undernät tillgänglig genom hello enhetens IP-adress.** Om hello fasta DATA 0 gränssnittet har konfigurerats för IPv4, hello IP-adresser måste toobe enligt hello IPv4-format. Om du angav ett prefix för IPv6-konfigurationen, fylls hello fasta IP-adresser automatiskt i de här fälten.

            ![Nätverksgränssnitt för minimala StorSimple-enhetsinställningar](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig2.png)

            hello fasta IP-adresser för hello controller används för att underhålla hello uppdateringar toohello enhet. Hello statiska IP-adresser måste därför vara dirigerbara och kunna tooconnect toohello Internet. Du kan kontrollera att dina fasta styrenhets-IP-adresser är dirigerbara genom att använda hello [Test-HcsmConnection] [ Test] cmdlet. följande exempel visar fast domänkontrollant IP-adresser är routade toohello Internet och kan komma åt hello hello Microsoft Update-servrar.

            ![Test-HcsmConnection visar dirigerbara IP-adresser](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig3.png)

1. Klicka på **OK**. hello enhetskonfiguration startar. När hello enhetskonfigurationen är klar, visas ett meddelande. Hej enhetens status ändras för**Online** i hello **enheter** bladet.

    ![Nätverksgränssnitt för minimala StorSimple-enhetsinställningar](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig4.png)

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx
