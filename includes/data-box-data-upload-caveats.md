---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: b159ec8620fa8c93e4917f73be9b9898e1b4fbcc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244586"
---
- Не копируйте файлы напрямую к любому из предварительно созданных общих папок. Необходимо создать папку в общей папке и скопируйте файлы в эту папку.
- Папка в разделах *StorageAccount_BlockBlob* и *StorageAccount_PageBlob* — это контейнер. Например, контейнеры создаются в виде *StorageAccount_BlockBlob/container* и *StorageAccount_PageBlob/container*.
- Каждая папка, созданная непосредственно в *StorageAccount_AzureFiles*, преобразуется в общую папку Azure.
- При наличии объекта Azure (например, BLOB-объекта или файла) в облаке с тем же именем, что и у копируемого объекта, Data Box перезапишет файл в облако.
- Каждый файл, записанный в общие папки *StorageAccount_BlockBlob* и *StorageAccount_PageBlob*, отправляется в виде блочного и страничного BLOB-объекта соответственно.
- Хранилище больших двоичных объектов не поддерживает каталоги. Если вы создаете папку в папке *StorageAccount_BlockBlob*, тогда виртуальные папки создаются в имени большого двоичного объекта. Для файлов Azure поддерживается фактическая структура каталогов.
- Любые пустые Иерархия каталогов (без файлов), созданная в разделе *StorageAccount_BlockBlob* и *StorageAccount_PageBlob* папки не отправлен.
- Если при отправке данных в Azure возникают ошибки, в целевой учетной записи хранения создается журнал ошибок. Путь к этому журналу ошибок доступен после завершения передачи, чтобы вы могли просмотреть журнал и предпринять корректирующие действия. Не удаляйте данные из источника без проверки загруженные данные.
