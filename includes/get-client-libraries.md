### <a name="install-via-composer"></a><span data-ttu-id="d8fd8-101">Installera via Composer</span><span class="sxs-lookup"><span data-stu-id="d8fd8-101">Install via Composer</span></span>
1. <span data-ttu-id="d8fd8-102">[Installera Git][install-git].</span><span class="sxs-lookup"><span data-stu-id="d8fd8-102">[Install Git][install-git].</span></span> <span data-ttu-id="d8fd8-103">Observera att i Windows, måste du också lägga till hello Git körbara tooyour PATH-miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="d8fd8-103">Note that on Windows, you must also add hello Git executable tooyour PATH environment variable.</span></span> 
2. <span data-ttu-id="d8fd8-104">Skapa en fil med namnet **composer.json** i hello roten av projektet och Lägg till följande kod tooit hello:</span><span class="sxs-lookup"><span data-stu-id="d8fd8-104">Create a file named **composer.json** in hello root of your project and add hello following code tooit:</span></span>
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "^0.4"
      }
    }
    ```
3. <span data-ttu-id="d8fd8-105">Hämta  **[composer.phar] [ composer-phar]**  i projektroten.</span><span class="sxs-lookup"><span data-stu-id="d8fd8-105">Download **[composer.phar][composer-phar]** in your project root.</span></span>
4. <span data-ttu-id="d8fd8-106">Öppna en kommandotolk och kör följande kommando i projektroten hello</span><span class="sxs-lookup"><span data-stu-id="d8fd8-106">Open a command prompt and execute hello following command in your project root</span></span>
   
    ```
    php composer.phar install
    ```

[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
