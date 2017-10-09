### <a name="install-via-composer"></a>Installera via Composer
1. [Installera Git][install-git]. Observera att i Windows, måste du också lägga till hello Git körbara tooyour PATH-miljövariabeln. 
2. Skapa en fil med namnet **composer.json** i hello roten av projektet och Lägg till följande kod tooit hello:
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "^0.4"
      }
    }
    ```
3. Hämta  **[composer.phar] [ composer-phar]**  i projektroten.
4. Öppna en kommandotolk och kör följande kommando i projektroten hello
   
    ```
    php composer.phar install
    ```

[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
