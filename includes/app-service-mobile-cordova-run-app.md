
1. Besök hello [Azure-portalen].
2. Klicka på **Apptjänster** > Hej backend som du skapade.
3. I inställningarna för hello mobilappar klickar du på **Quickstart** > **Cordova**.
![Azure-portalen med snabbstart för mobila appar markerad][quickstart]
4. Under **Konfigurera klientprogrammet**, väljer du **Skapa en ny app**, och klickar sedan på **Hämta**.
2. Packa upp hello hämtade ZIP tooa katalog på hårddisken, navigera toohello lösningsfilen (.sln) och öppna den med hjälp av Visual Studio.
3. Välj hello lösning plattform (Android, iOS eller Windows) hello nästa toothe start listrutepilen i Visual Studio. Välj en specifik distribution enhet eller emulator genom att klicka på hello nedrullningsbara på hello grön pil. Du kan använda hello standard Android-plattformen och flytta efterföljande emulatorn. Mer avancerade självstudier (t.ex, push-meddelanden) kräver tooselect en stödd enhet eller emulator.
4. toobuild och kör din Cordova-app, tryck på F5 eller klicka på hello grön pil. Om du ser en dialogruta för säkerhet i hello emulatorn begär åtkomst toohello nätverk, acceptera den.
5. När hello appen startas på hello enhet eller emulator, ange en beskrivande text i **ange ny text**, som *fullständig hello kursen* och klicka sedan på hello **Lägg till** knappen.

hello serverdelen infogar data från hello begäran i hello TodoItem-tabellen i hello SQL-databas och returnerar information om hello nyligen lagrade objektet tillbaka toohello mobila appar. Hej mobilappen visar dessa data i hello lista.

Du kan upprepa steg 3 till 5 för andra plattformar.

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png

<!-- URLs -->
[Azure-portalen]: https://portal.azure.com/
