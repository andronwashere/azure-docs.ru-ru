---
title: Перенос данных в службу синхронизации файлов Azure с помощью Azure Data Box, а также другие методы
description: Перенесите данные массового способом, который совместим со службой синхронизации файлов Azure.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 02/12/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d1ec5168b898d0aa75c12e6eb435e20c09de1929
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64700268"
---
# <a name="migrate-bulk-data-to-azure-file-sync"></a>Перенос массовых данных в службу синхронизации файлов Azure
Службы синхронизации файлов Azure можно перенести массовых данных двумя способами:

* **Передача файлов с помощью службы синхронизации файлов Azure.** Это самый простой способ. Переместите файлы локально, чтобы Windows Server 2012 R2 или более поздней версии и установите агент службы синхронизации файлов Azure. После настройки синхронизации файлов будет загружен с сервера. (В настоящее время наши клиенты пользовались частотой среднее передачи 1 Тиб о каждые два дня.) Чтобы убедиться, что сервер не использует слишком много пропускной способности для вашего центра обработки данных, может потребоваться настроить [расписание полосы пропускания](storage-sync-files-server-registration.md#ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter).
* **Перенести файлы в автономном режиме.** Если у вас недостаточно пропускной способности, может не появиться возможность передачи файлов в Azure в течение приемлемого промежутка времени. Задача — начальная синхронизация из всего набора файлов. Чтобы преодолеть эту проблему, используйте массового автономного средства миграции, такие как [семейства Azure Data Box](https://azure.microsoft.com/services/storage/databox). 

В этой статье описывается процедура переноса файлов в автономном режиме способом, который совместим со службой синхронизации файлов Azure. Выполните эти инструкции во избежание конфликтов файл и сохранить в файл и папку списки управления доступом (ACL) и метки времени, после включения синхронизации.

## <a name="online-migration-tools"></a>Инструменты оперативного перемещения
Процесс, описываемый в этой статье работает не только для поля данных, но также и для других средств автономной миграции с нами. Он также подходит для интерактивных средах, например AzCopy, Robocopy, или средства партнеров и служб. Тем не менее преодолеть первоначального отправить запрос, выполните действия, описанные в этой статье, чтобы использовать эти средства способом, который совместим со службой синхронизации файлов Azure.


## <a name="benefits-of-using-a-tool-to-transfer-data-offline"></a>Преимущества использования это средство для передачи данных в автономном режиме
Ниже приведены основные преимущества использования средство передачи как поле данных для автономной миграции.

- Отправка всех файлов по сети не нужно. Для больших пространств имен это средство может сохранить пропускную способность сети и время.
- При использовании службы синхронизации файлов Azure, независимо от того, какое средство передачи вы используете (Data Box, служба импорта и экспорта Azure и т. д.), к активному серверу отправляет только файлы, которые изменяются после перемещения данных в Azure.
- Служба синхронизации файлов Azure выполняет синхронизацию файлов и папок списки управления доступом, даже если средство миграции автономных массового не транспорта списки управления доступом.
- Data Box и синхронизации файлов Azure без простоев. При использовании Data Box для передачи данных в Azure, вы эффективно использовать пропускную способность сети и сохраняет файл точность. Вы также постоянно пространства имен в актуальном состоянии, отправлять только файлы, которые изменяются после перемещения данных в Azure.

## <a name="prerequisites-for-the-offline-data-transfer"></a>Необходимые условия для передачи данных в автономном режиме
Не следует включать синхронизации на сервере, который вы переносите до завершения передачи данных в автономном режиме. Ниже приведены другие действия, которые следует учитывать перед началом работы.

- Если вы планируете использовать Data Box для массового перемещения, просмотрите [предварительные требования к развертыванию для Data Box](../../databox/data-box-deploy-ordered.md#prerequisites).
- Планирование топологии окончательный службы синхронизации файлов Azure: [Планирование развертывания службы синхронизации файлов Azure](storage-sync-files-planning.md)
- Выберите учетные записи хранения Azure, в которых будут храниться общие папки, с которыми нужно выполнить синхронизацию. Убедитесь, что массовое перемещение выполняется во временные общие папки промежуточного хранения в одной и той же учетной записи хранения. Массовое перемещение может быть включено только с использованием конечной общей папки и общей папки промежуточного хранения, которые находятся в одной учетной записи хранения.
- Массовое перемещение возможно только при создании новых связей синхронизации с расположением на сервере. Нельзя включить массовое перемещение с существующими связями синхронизации.


## <a name="process-for-offline-data-transfer"></a>Процесс для передачи данных в автономном режиме
Вот как настроить службу синхронизации файлов Azure способом, который совместим с помощью средств переноса bulk, таких как Azure Data Box:

![Схема показывает, как настроить службу синхронизации файлов Azure](media/storage-sync-files-offline-data-transfer/data-box-integration-1-600.png)

| Шаг | Описание |
|---|---------------------------------------------------------------------------------------|
| ![Шаг 1](media/storage-sync-files-offline-data-transfer/bullet_1.png) | [Заказ Data Box](../../databox/data-box-deploy-ordered.md). Предложения семейства Data Box [нескольких продуктов](https://azure.microsoft.com/services/storage/databox/data) в соответствии с требованиями. При получении данных Box, выполните его [документации для копирования данных](../../databox/data-box-deploy-copy-data.md#copy-data-to-data-box) это UNC-путь в поле:  *\\< DeviceIPAddres\>\<StorageAccountName_AzFile\> \<ShareName\>* . Здесь *ShareName* — это имя промежуточной папки. Отправьте Data Box обратно в Azure. |
| ![Шаг 2](media/storage-sync-files-offline-data-transfer/bullet_2.png) | Подождите, пока файлы отображаются в файловые ресурсы Azure, которые были выбраны в качестве временного размещения общих ресурсов. *Не включайте синхронизации для этих общих папках.* |
| ![Шаг 3.](media/storage-sync-files-offline-data-transfer/bullet_3.png) | Создайте новую пустую общую папку для каждой общей папки, созданное поле данных. Этот новый ресурс должен быть в той же учетной записи хранения общего ресурса Data Box. [Создание общей папки в службе файлов Azure](storage-how-to-create-file-share.md). |
| ![Шаг 4.](media/storage-sync-files-offline-data-transfer/bullet_4.png) | [Создать группу синхронизации](storage-sync-files-deployment-guide.md#create-a-sync-group-and-a-cloud-endpoint) в службе синхронизации хранилища. Ссылка на пустой ресурс в качестве облачной конечной точкой. Повторите этот шаг для каждой общей папки Data Box. [Настройка синхронизации файлов Azure](storage-sync-files-deployment-guide.md). |
| ![Шаг 5](media/storage-sync-files-offline-data-transfer/bullet_5.png) | [Добавьте каталог своего активного сервера в качестве конечной точки сервера](storage-sync-files-deployment-guide.md#create-a-server-endpoint). В процессе укажите, что переместить файлы в Azure и ссылаться на промежуточном общие папки. Можно включать и отключать распределение по уровням, при необходимости. При создании конечной точки сервера на сервере в реальном времени, ссылаться на промежуточную общую папку. На **Добавление конечной точки сервера** колонке в разделе **автономный режим передачи данных**выберите **включено**и затем выберите промежуточную общую папку, должны находиться в той же учетной записи хранения облака Конечная точка. Здесь список доступных общих папок фильтруется по учетной записи хранения и общие папки, которые уже не синхронизируются. |

![Снимок экрана пользовательского интерфейса портала Azure, показывающий, как включить перемещение данных в автономном режиме при создании новой конечной точки сервера](media/storage-sync-files-offline-data-transfer/data-box-integration-2-600.png)

## <a name="syncing-the-share"></a>Синхронизация общих папок
После создания конечной точки сервера, запустится синхронизация. Процесс синхронизации определяет, существует ли также каждого файла на сервере в промежуточную общую папку, где поле данных их сохранение файлов. Если файл существует, процесс синхронизации копирует файл из промежуточной папки, а не отправкой с сервера. Если файл не существует в общей папке промежуточного или более новая версия доступна на локальном сервере, процесс синхронизации передает файл с локального сервера.

> [!IMPORTANT]
> Вы можете включить режим миграции массового только тогда, когда вы создаете конечную точку сервера. После установки конечной точки сервера нельзя интегрировать массового переноса данных с уже выполняется синхронизация сервера в пространство имен.

## <a name="acls-and-timestamps-on-files-and-folders"></a>Списки управления доступом и метки времени для файлов и папок
Служба синхронизации файлов Azure гарантирует, что списки ACL файлов и папок синхронизируются из активному серверу, даже если средство массового миграции, которое вы использовали не изначально транспорта списки управления доступом. По этой причине промежуточной папки не должно содержать все списки управления доступом для файлов и папок. Когда вы включаете средство переноса данных в автономном режиме, при создании новой конечной точки сервера, на сервере, синхронизируются все списки управления доступом файла. Вновь созданные и измененные отметки времени, также синхронизируются.

## <a name="shape-of-the-namespace"></a>Схема пространства имен
При включении синхронизации, содержимое сервера определить фигуры пространства имен. Если файлы удаляются с локального сервера, после моментального снимка Data Box и миграции, эти файлы не перемещаются в реальном времени выполняется синхронизация пространство имен. Они остаются в общей папке промежуточного, но они не копируются. Это необходимо, так как синхронизация сохраняет пространство имен в соответствии с сервером. Поле *моментального снимка* является просто промежуточной нуля для эффективного копирования. Это не полномочия для фигуры динамической пространства имен.

## <a name="cleaning-up-after-bulk-migration"></a>После миграции массового 
Как сервер завершает его начальной синхронизации пространства имен, файлы массового переноса данных поле использовать промежуточную общую. На **свойства конечной точки сервера** колонка на портале Azure в **автономный режим передачи данных** раздела, состояние изменится с **In Progress** для **завершено** . 

![Снимок экрана колонки свойства конечной точки сервера, где находятся элементы управления состояние и disable для передачи данных в автономном режиме](media/storage-sync-files-offline-data-transfer/data-box-integration-3-444.png)

Теперь можно выполнить очистку промежуточную общую папку для сокращения затрат:

1. На **свойства конечной точки сервера** колонке, если состояние имеет значение **завершено**выберите **отключить передачу данных в автономном режиме**.
2. Рассмотрите возможность удаления промежуточную общую папку для сокращения затрат. Промежуточную общую папку, возможно, не содержат ACL для файлов и папок, поэтому не очень полезно. В целях резервного копирования в определенный момент времени создать настоящие [моментального снимка общего ресурса синхронизации файлов Azure](storage-snapshots-files.md). Вы можете [Настройка архивации Azure для создания моментальных снимков]( ../../backup/backup-azure-files.md) по расписанию.

Отключить режим передачи данных в автономном режиме, только в том случае, если состояние — **завершено** или если вы хотите отменить из-за неправильной конфигурации. Если отключить режим во время развертывания, файлы запустится для отправки с сервера, даже если папке промежуточного по-прежнему доступен.

> [!IMPORTANT]
> После отключения режима передачи данных в автономном режиме, вы не удается включить снова, даже если промежуточную общую папку из процесса миграции массового по-прежнему доступен.

## <a name="next-steps"></a>Дальнейшие действия
- [Планирование развертывания службы синхронизации файлов Azure](storage-sync-files-planning.md)
- [Как развернуть службу синхронизации файлов Azure (предварительная версия)](storage-sync-files-deployment-guide.md)
