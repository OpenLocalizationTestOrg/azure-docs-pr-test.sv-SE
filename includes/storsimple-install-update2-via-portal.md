<!--author=alkohli last changed: 02/06/17-->

#### <a name="tooinstall-an-update-from-hello-azure-portal"></a>tooinstall en uppdatering från hello Azure-portalen

1. Välj enheten hello StorSimple-tjänsten på sidan. Navigera för**enheter** > **Underhåll**.
2. Hello längst hello-sidan, klickar du på **Sök efter uppdateringar**. Ett jobb skapas tooscan efter uppdateringar. Du meddelas när hello jobbet slutförs ordentligt.
3. I hello **programuppdateringar** avsnittet hello samma sidan hello nya programuppdateringar är tillgängliga. Vi rekommenderar att du läser hello viktig information innan du installerar en uppdatering på enheten.
4. Hello längst hello-sidan, klickar du på **installera uppdateringar**, och sedan **OK**.
5. I hello **installera uppdateringar** dialogrutan, se till att du har följt hello rekommendationer och välj sedan **jag förstår hello ovan krav och är redo tooupgrade enheten** och klicka på Kontrollera hello knappen.
   
    ![Bekräftelsemeddelande](./media/storsimple-install-update2-via-portal/InstallUpdate12_2M.png)
6. En uppsättning nödvändiga kontrollerar startar. Dessa kontroller omfattar följande:
   
   * **Domänkontrollanten hälsokontroller** tooverify både hello styrenheter är felfri och online.
   * **Maskinvara komponenten hälsokontroller** tooverify alla hello maskinvarukomponenter på StorSimple-enheten är felfria.
   * **DATA 0 kontrollerar** tooverify att DATA 0 är aktiverad på enheten. Om inte det här gränssnittet är aktiverat måste du aktivera det och sedan försöka igen.
   * **DATA 2 och DATA 3 kontrollerar** tooverify att nätverksgränssnitten DATA 2 och DATA 3 inte har aktiverats. Om dessa gränssnitt är aktiverade, måste du inaktivera de här och försök sedan tooupdate din enhet. Den här kontrollen utförs enbart om du uppdaterar från en enhet som kör GA-programvara. Enheter som kör versionerna 0.1, 0.2 eller 0.3 behöver inte den här kontrollen.
   * **Gateway-Kontrollera** på alla enheter som kör en tidigare tooUpdate för version 1. Den här kontrollen utförs på alla hello-enhet som kör före uppdateringen 1 programvara men misslyckas på hello-enheter som har en gateway som konfigurerats för ett nätverksgränssnitt än DATA 0.
     
     hello uppdateringen har genomförts om alla kontroller har slutförts. Du meddelas när hello kontrollerar pågår.
     
     ![Förkontrollsmeddelande](./media/storsimple-install-update2-via-portal/InstallUpdate12_3M.png)
     
     hello följande är ett exempel där hello kontroller misslyckades. Du måste kontrollera att båda hello-styrenheter är felfri och online. Du måste också toocheck hello hälsotillstånd hello maskinvarukomponenter. I det här exemplet behöver komponenterna i Styrenhet 0 och Styrenhet 1 åtgärdas. Du kanske måste toocontact Microsoft-supporten om du kan lösa dessa problem själv.
     
       ![Kontrollerna misslyckades](./media/storsimple-install-update2-via-portal/HCS_PreUpgradeChecksFailed-include.png)
7. När hello kontroller har slutförts, skapas ett uppdateringsjobb. Du meddelas när hello Uppdateringsjobbet har skapats.
   
    ![Uppdatera skapande av jobb](./media/storsimple-install-update2-via-portal/InstallUpdate12_44M.png)
   
    hello uppdateringen tillämpas sedan på enheten.
    
8. toomonitor hello fortskrider hello uppdateringsjobb, klicka på **visa jobb**. På hello **jobb** sidan som du kan se hello uppdatera status.
9. Det tar några timmar toocomplete hello uppdatering. Välj hello Uppdateringsjobbet och klicka på **information** tooview hello informationen om hello jobb när som helst.
10. När hello jobbet har slutförts, går toohello **Underhåll** sidan och rulla ned för**programuppdateringar**.

