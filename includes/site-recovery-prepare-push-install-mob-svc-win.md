### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Förbereda för en push-installation på en Windows-dator

1. Kontrollera att det finns en nätverksanslutning mellan hello Windows-dator och hello processervern.
2. Skapa ett konto som hello processervern kan använda tooaccess hello-dator. hello kontot ska ha administratörsbehörighet (lokalt eller via domänadministratör). (Använd det här kontot bara för hello push-installation och agentuppdateringar.)

   > [!NOTE]
   > Om du inte använder ett domänkonto, inaktivera fjärråtkomst till användaren kontrollen på hello lokala datorn. toodisable fjärranslutna användare åtkomstkontroll hello HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System registernyckel, lägga till ett nytt DWORD-värde: **LocalAccountTokenFilterPolicy**. Ange hello värdet för**1**. toodo detta på ett kommando kommandotolk kör hello följande kommando:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
2. I Windows-brandväggen på datorn hello du tooprotect, Välj **tillåta en app eller funktion via brandväggen**. Aktivera **File and Printer Sharing** och **Windows Management Instrumentation (WMI)**. Du kan konfigurera brandväggsinställningar för hello med hjälp av ett grupprincipobjekt (GPO) för datorer som tillhör tooa domän.

   ![Brandväggsinställningar](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

3. Lägg till hello-konto som du skapade i CSPSConfigtool.
    1.  Logga in tooyour konfigurationsservern.
    2.  Öppna **cspsconfigtool.exe**. (Det är tillgängligt som en genväg på skrivbordet hello och i hello %ProgramData%\home\svsystems\bin mapp.)
    3.  På hello **hantera konton** väljer **Lägg till konto**.
    4.  Lägg till hello-konto som du skapade.
    5.  Ange hello autentiseringsuppgifter som du använder när du aktiverar replikering för en dator.
