1. Starta hello Azure Site Recovery UnifiedSetup.exe
2. I **innan du börjar**väljer **lägga till ytterligare processer servrar tooscale ut distribution**.

  ![Lägga till processerver](./media/site-recovery-add-process-server/ps-page-1.png)

3. I **Server konfigurationsinformation**anger hello IP-adressen för hello konfigurationsservern och lösenfrasen hello.

  ![Lägga till processerver 2](./media/site-recovery-add-process-server/ps-page-2.png)
4. I **Internetinställningar**, ange hur providern som körs på hello konfigurationsservern ansluter tooAzure Site Recovery via hello hello Internet.

  ![Lägga till processerver 3](./media/site-recovery-add-process-server/ps-page-3.png)

   * Om du vill tooconnect med hello-proxy som är för tillfället inställd på hello datorn markerar **Anslut med befintliga proxyinställningar**.
   * Om du vill hello providern tooconnect direkt markerar **Anslut direkt utan en proxy**.
   * Om hello befintliga proxyservern kräver autentisering, eller om du vill toouse en anpassad proxyserver för hello leverantörsanslutning, Välj **Anslut med anpassade proxyinställningar**.

     * Om du använder en anpassad proxyserver måste toospecify hello adress, port och autentiseringsuppgifter.
     * Om du använder en proxyserver bör du redan ha tillåtit åtkomst toohello URL: er.

5. I **Kravkontroll**, installationsprogrammet körs en kontroll toomake till att installationen kan köras. En varning visas om hello **Global kontroll när synkronisering**, kontrollera då hello på hello systemklockan (**datum och tid** inställningar) hello är samma som hello tidszon.

     ![Lägga till processerver 4](./media/site-recovery-add-process-server/ps-page-4.png)

6. I **miljö information**väljer du om du ska tooreplicate virtuella VMware-datorer. I så fall konfigurerar du kontroller som kontrollerar att PowerCLI 6.0 är installerat.

     ![Lägga till processerver 5](./media/site-recovery-add-process-server/ps-page-5.png)

7. I **installera platsen**väljer där tooinstall hello binärfiler och om du vill lagra hello cache. hello-enhet som du väljer måste ha minst 5 GB ledigt diskutrymme, men vi rekommenderar en cacheenhet med minst 600 GB ledigt utrymme.
     ![Lägga till processerver 5](./media/site-recovery-add-process-server/ps-page-6.png)

8. I **val av nätverk**anger hello lyssnare (nätverkskort och SSL-port) på vilken hello konfigurationsservern skickar och tar emot replikeringsdata. Port 9443 är hello standardport som används för att skicka och ta emot replikeringstrafik, men du kan ändra den här porten nummer toosuit krav för din miljö. I tillägg toohello port 9443 öppna vi också port 443, som används av en webbserver tooorchestrate replikeringsåtgärder. Använd inte Port 443 för att skicka eller ta emot replikeringstrafik.

     ![Lägga till processerver 6](./media/site-recovery-add-process-server/ps-page-7.png)
9. I **sammanfattning**, granska hello information och klickar på **installera**. När installationen är klar skapas en lösenfras. Du behöver den när du aktiverar replikering. Kopiera lösenfrasen och förvara den på en säker plats.

     ![Lägga till processerver 7](./media/site-recovery-add-process-server/ps-page-8.png)
