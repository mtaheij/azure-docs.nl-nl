---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: 9e9057073c8a661e2f3382333abc7ac2778c4ee3
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/10/2020
ms.locfileid: "84680344"
---
### <a name="install-via-composer"></a>Installeren via Composer
1. Maak een bestand met de naam **composer.json** in de hoofdmap van uw project en voeg de volgende code toe:
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "*"
      }
    }
    ```
2. Download **[composer.phar][composer-phar]** naar de hoofdmap van uw project.
3. Open een opdrachtprompt en voer de volgende opdracht uit in de hoofdmap van uw project
   
    ```
    php composer.phar install
    ```

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: https://github.com/Azure/azure-sdk-for-php
[composer-phar]: http://getcomposer.org/composer.phar
