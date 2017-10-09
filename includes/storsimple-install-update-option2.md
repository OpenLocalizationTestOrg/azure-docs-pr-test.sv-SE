<!--author=SharS last changed: 03/17/2016-->

#### <a name="tooinstall-update-12-from-hello-azure-classic-portal"></a>tooinstall Update 1.2 från hello klassiska Azure-portalen
1. I hello klassiska Azure-portalen, går toohello **enheter** och välja din enhet.
2. Navigera för**enheter** > **konfigurera**.
3. Under **nätverksgränssnitt**, kontrollera först att du har minst ett nätverksgränssnitt som är iSCSI-aktiverade. Leta upp hello nätverksgränssnitt (förutom DATA 0) som har en gateway som har tilldelats.
4. Inaktivera hello nätverksgränssnitt som har en tilldelad gateway och spara hello ändrade konfigurationen. Observera hello gränssnitt för nätverksinställningar bevaras och så när du återaktivera det här nätverksgränssnittet senare, hello portal återställs toohello ursprungliga inställningarna.
5. Du kan nu [använda hello Azure klassiska portal tooinstall Update 1.2](#install-update-12-via-the-azure-classic-portal). Följ instruktionerna för hello från steg 3 i den här proceduren. När du har installerat alla hello uppdateringar kan återaktivera du hello nätverksgränssnittet som du inaktiverade.

