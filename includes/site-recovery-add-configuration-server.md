1. Kör installationsfilen för hello Unified installationen.
2. I **innan du börjar**väljer **installera hello konfigurationsservern och processervern**.

    ![Innan du börjar](./media/site-recovery-add-configuration-server/combined-wiz1.png)

3. I **licensvillkoren för programvara från tredje part**, klickar du på **jag accepterar** toodownload och installera MySQL.

    ![Tredjepartsprogram](./media/site-recovery-add-configuration-server/combined-wiz2.png)
4. I **registrering**väljer hello registreringsnyckel som du hämtade från hello-valvet.

    ![Registrering](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. I **Internetinställningar**, ange hur providern som körs på konfigurationsservern hello ansluter tooAzure Site Recovery via hello hello Internet.

   a. Om du vill tooconnect med hello-proxy som är för tillfället inställd på hello datorn markerar **ansluta tooAzure Platsåterställningen genom att använda en proxyserver**.

   b. Om du vill hello providern tooconnect direkt markerar **ansluter direkt tooAzure Site Recovery utan en proxyserver**.

   c. Om hello befintliga proxyservern kräver autentisering, eller om du vill toouse en anpassad proxyserver för hello leverantörsanslutning, Välj **Anslut med anpassade proxyinställningar**.

     * Om du använder en anpassad proxyserver måste toospecify hello adress, port och autentiseringsuppgifter.
     * Om du använder en proxyserver du bör redan ha tillåtit hello webbadresser som beskrivs i [krav](#prerequisites).

     ![Brandvägg](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. I **Kravkontroll**, installationsprogrammet körs en kontroll toomake till att installationen kan köras. En varning visas om hello **Global kontroll när synkronisering**, kontrollera då hello på hello systemklockan (**datum och tid** inställningar) hello är samma som hello tidszon.

    ![Krav](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. I **MySQL Configuration**, skapa autentiseringsuppgifter för inloggning toohello MySQL server-instans som är installerad.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. I **miljö information**väljer du om du ska tooreplicate virtuella VMware-datorer. Om du kan sedan kontrolleras att PowerCLI 6.0 är installerad.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz7.png)

9. I **installera platsen**väljer där tooinstall hello binärfiler och om du vill lagra hello cache. hello-enhet som du väljer måste ha minst 5 GB ledigt diskutrymme, men vi rekommenderar en cacheenhet med minst 600 GB ledigt utrymme.

    ![Installationsplats](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. I **val av nätverk**anger hello lyssnare (nätverkskort och SSL-port) på vilken hello configuration server skickar och tar emot replikeringsdata. Port 9443 är hello standardport som används för att skicka och ta emot replikeringstrafik, men du kan ändra den här porten nummer toosuit krav för din miljö. I tillägg toohello port 9443 öppna vi också port 443, som används av en webbserver tooorchestrate replikeringsåtgärder. Använd inte port 443 för att skicka eller ta emot replikeringstrafik.

    ![Val av nätverk](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. I **sammanfattning**, granska hello information och klickar på **installera**. När installationen är klar skapas en lösenfras. Du behöver den när du aktiverar replikering. Kopiera lösenfrasen och förvara den på en säker plats.

    ![Sammanfattning](./media/site-recovery-add-configuration-server/combined-wiz10.png)

När registreringen är klar hello servern visas på hello **inställningar** > **servrar** bladet i hello-valvet.
