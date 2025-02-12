---
title: Интеграция функций Azure с виртуальной сетью Azure
description: Пошаговое руководство, в котором показано, как подключиться к виртуальной сети Azure функции
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: article
ms.date: 5/03/2019
ms.author: alkarche
ms.reviewer: glenga
ms.openlocfilehash: 0a31b58a3c843a2add0c84dc1a3ad4ab6417815e
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612887"
---
# <a name="tutorial-integrate-functions-with-an-azure-virtual-network"></a>Руководство по интеграции функции с виртуальной сетью Azure

Этом руководстве показано, как с помощью функций Azure для подключения к ресурсам в виртуальной сети Azure. Вы создадите функцию, которая имеет доступ как к Интернету и к виртуальной Машине под управлением WordPress в виртуальной сети.

> [!div class="checklist"]
> * Создание приложения-функции в плане "премиум"
> * Развертывание веб-сайта WordPress в виртуальной Машине в виртуальной сети
> * Подключение к виртуальной сети приложение-функция
> * Создание функции прокси-сервер для доступа к ресурсам WordPress
> * Запросить файл WordPress из внутри виртуальной сети

> [!NOTE]  
> В плане "премиум" в этом руководстве создается приложение-функцию. Этот план размещения доступна в предварительной версии. Дополнительные сведения см. в разделе [план "премиум"].

## <a name="topology"></a>Топология

Следующей схеме показана архитектура решения, которое создается.

 ![Пользовательский Интерфейс для интеграции виртуальной сети](./media/functions-create-vnet/topology.png)

Функций, выполняемых в план "премиум" имеют те же возможности размещения веб-приложений в службе приложений Azure, включая интеграции с виртуальной сетью. Дополнительные сведения об интеграции с виртуальной сетью, включая устранение неполадок и расширенная настройку, см. в разделе [интеграция приложения с виртуальной сетью Azure](../app-service/web-sites-integrate-with-vnet.md).

## <a name="prerequisites"></a>Предварительные требования

Для этого руководства очень важно, что вы понимаете IP-адресацию и выделение. Вы можете начать с [этой статьи, где рассматриваются основы адресации и подсети](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). Многие дополнительные статьи и видео доступны через Интернет.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="create-a-function-app-in-a-premium-plan"></a>Создание приложения-функции в плане "премиум"

Во-первых, создать приложение-функцию в [план "премиум"]. Эта схема обеспечивает масштабирование без сервера поддерживая Интеграция виртуальной сети.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

Вы можете Закрепить панель мониторинга в приложении-функции, выбрав значок булавки в правом верхнем углу. Закрепление облегчает вернуться к этому приложению-функции, после создания виртуальной Машины.

## <a name="create-a-vm-inside-a-virtual-network"></a>Создание виртуальной Машины в виртуальной сети

Создайте предварительно настроенной виртуальной Машины, работающей WordPress внутри виртуальной сети ([WordPress LEMP7 Max Performance](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure) по Jetware). Виртуальной Машины WordPress используется из-за низкой эффективности и удобства. Этот же сценарий работает с любого ресурса в виртуальной сети, например REST API, сред службы приложений и других служб Azure. 

1. На портале выберите **+ создать ресурс** в области навигации слева в поле поиска введите `WordPress LEMP7 Max Performance`, и нажмите клавишу ВВОД.

1. Выберите **Wordpress LEMP Max Performance** в результатах поиска. Выберите план программного обеспечения из **Wordpress LEMP Max Performance для CentOS** как **план программного обеспечения** и выберите **создать**.

1. В **основы** вкладке, используйте параметры виртуальной Машины, как указано в таблице под изображением:

    ![Вкладка основы для создания виртуальной Машины](./media/functions-create-vnet/create-vm-1.png)

    | Параметр      | Предлагаемое значение  | Описание      |
    | ------------ | ---------------- | ---------------- |
    | **Подписка** | Ваша подписка | Подписка, в котором создаются ресурсы. | 
    | **[Группа ресурсов](../azure-resource-manager/resource-group-overview.md)**  | myResourceGroup | Выберите `myResourceGroup`, или группу ресурсов, созданные с помощью приложения-функции. Используя ту же группу ресурсов для приложения-функции, виртуальной Машины WordPress и план размещения упрощает для очистки ресурсов, когда вы закончите с этим руководством. |
    | **Имя виртуальной машины** | Wordpress виртуальной сети | Имя виртуальной Машины должно быть уникальным в группе ресурсов |
    | **[Регион](https://azure.microsoft.com/regions/)** | "Западная Европа" (Европа). | Выберите регион, рядом с вами или рядом с ним функций, которые обращаются к виртуальной Машине. |
    | **Размер** | B1s | Выберите **изменить размер** и выберите B1s стандартного образа, который содержит 1 виртуальный процессор и 1 ГБ памяти. |
    | **Тип проверки подлинности** | Пароль | Для использования проверки подлинности пароля, необходимо также указать **Username**, защищенный **пароль**, а затем **подтверждение пароля**. Для этого руководства не нужно будет войти на виртуальную машину, если вам не требуются для устранения неполадок. |

1. Выберите **сети** и в разделе Настройка виртуальных сетей выберите **создать**.

1. В **Создание виртуальной сети**, используйте параметры в таблице под изображением:

    ![Вкладка "Сеть" для создания виртуальной Машины](./media/functions-create-vnet/create-vm-2.png)

    | Параметр      | Предлагаемое значение  | Описание      |
    | ------------ | ---------------- | ---------------- |
    | **Name** | myResourceGroup — виртуальная сеть | Можно использовать имя по умолчанию, созданный для виртуальной сети. |
    | **Диапазон адресов** | 10.10.0.0/16 | Используйте диапазон один адрес для виртуальной сети. |
    | **Имя подсети** | Учебник Net | Имя подсети. |
    | **Диапазон адресов** (подсеть) | 10.10.1.0/24   | Размер подсети определяет, сколько интерфейсов, которые могут добавляться к подсети. Эта подсеть используется сайтом WordPress.  Объект `/24` подсети предоставляет 254 адреса узла. |

1. Выберите **ОК** для создания виртуальной сети.

1. Вернитесь в **сети** вкладке, выберите **None** для **общедоступный IP-адрес**.

1. Выберите **управления** вкладку, а затем в **учетная запись хранения диагностики**, выберите учетную запись хранилища, созданные с помощью приложения-функции.

1. Выберите **Review + create** (Просмотреть и создать). После завершения проверки выберите **создать**. Виртуальная машина создать процесс занимает несколько минут. Созданной виртуальной Машины имеет доступ только к виртуальной сети.

1. После создания виртуальной Машины выберите **перейти к ресурсу** для просмотра страницы для новой виртуальной Машины, затем выберите **сети** под **параметры**.

1. Убедитесь, что не **общедоступный IP-адрес**. Запишите **частный IP-адрес**, используемая для подключения к виртуальной Машине из приложения-функции.

    ![Параметры сети на виртуальной машине](./media/functions-create-vnet/vm-networking.png)

Теперь у вас есть веб-сайта WordPress развернут целиком в пределах виртуальной сети. Этот сайт недоступен из Интернета.

## <a name="connect-your-function-app-to-the-virtual-network"></a>Подключение к виртуальной сети приложение-функция

С помощью веб-сайта WordPress на виртуальной Машине в виртуальной сети теперь можно подключиться к этой виртуальной сети приложения-функции.

1. В новом приложении-функции, выберите **функции платформы** > **сети**.

    ![Выберите сеть в приложении-функции](./media/functions-create-vnet/networking-0.png)

1. В разделе **интеграции с виртуальной сетью**выберите **щелкните здесь для настройки**.

    ![Состояние для Настройка сети](./media/functions-create-vnet/Networking-1.png)

1. На странице интеграции с виртуальной сети, выберите **Добавление виртуальной сети (Предварительная версия)** .

    ![Добавление предварительной версии интеграции с виртуальной сетью](./media/functions-create-vnet/networking-2.png)

1. В **состояния сетевых компонентов**, используйте параметры в таблице под изображением:

    ![Определить виртуальную сеть функции приложения](./media/functions-create-vnet/networking-3.png)

    | Параметр      | Предлагаемое значение  | Описание      |
    | ------------ | ---------------- | ---------------- |
    | **Виртуальная сеть** | MyResourceGroup — виртуальная сеть | Этой виртуальной сети является то, что вы создали ранее. |
    | **Подсеть** | Создайте новую подсеть. | Создайте подсеть в виртуальной сети для приложения-функции для использования. Интеграция виртуальной сети должен быть настроен на использование пустую подсеть. Не важно, что ваши функции использовать в разных подсетях вашей виртуальной Машины. Виртуальная сеть автоматически направляет трафик между двумя подсетями. |
    | **Имя подсети** | Функция Net | Имя новой подсети. |
    | **Блок адресов виртуальной сети** | 10.10.0.0/16 | Выберите одинаковые блоки адресов, используемых сайтом WordPress. Следует устанавливать только один блок адресов, определенный. |
    | **Диапазон адресов** | 10.10.2.0/24   | Размер подсети ограничивает общее число экземпляров, которые можно масштабировать для приложения-функции плана "премиум". В этом примере используется `/24` подсети с 254 адресов, доступных на узле. Эта подсеть является избыточной, но просто вычислить. |

1. Выберите **ОК** Чтобы добавить подсеть. Закройте страницы интеграции виртуальной сети и состояния сетевых компонентов, чтобы вернуться к странице вашего приложения-функции.

Приложения-функции теперь можно получить доступ к виртуальной сети, где выполняется на сайте WordPress. Далее используется [прокси функций Azure](functions-proxies.md) для возврата в файл с сайта WordPress.

## <a name="create-a-proxy-to-access-vm-resources"></a>Создание прокси-сервер для доступа к ресурсам виртуальной Машины

С включенной интеграцией виртуальной сети можно создать учетную запись-посредник в приложении-функции для перенаправления запросов к виртуальной Машине, работающих в виртуальной сети.

1. В приложении-функции выберите **учетные записи-посредники** >  **+** , затем использовать параметры прокси-сервера в таблице под изображением:

    ![Определение параметров прокси-сервера](./media/functions-create-vnet/create-proxy.png)

    | Параметр  | Предлагаемое значение  | Описание      |
    | -------- | ---------------- | ---------------- |
    | **Name** | Предприятия в хронологии | Имя может быть любое значение. Он используется для идентификации прокси-сервер. |
    | **Шаблон маршрута** | /plant | Маршрут, который сопоставляется ресурса виртуальной Машины. |
    | **Внутренний URL-адрес** | http://<YOUR_VM_IP>/wp-content/themes/twentyseventeen/assets/images/header.jpg | Замените `<YOUR_VM_IP>` IP-адресом виртуальной машины WordPress, который был создан ранее. Это сопоставление возвращает один файл с сайта. |

1. Выберите **создать** для добавления прокси-сервер для приложения-функции.

## <a name="try-it-out"></a>Попробуйте сейчас

1. В браузере, попробуйте получить доступ к URL-адрес, используемый как **URL-адрес внутреннего**. Как и ожидалось, тайм-аута запроса. Время ожидания возникает, поскольку сайт WordPress подключен только к виртуальной сети, а не через Интернет.

1. Копировать **URL-адрес прокси-сервера** из нового прокси-сервер и вставьте его в адресную строку браузера. Возвращаемый изображение находится сайт WordPress, работающий внутри виртуальной сети.

    ![Файл изображения предприятия в хронологии, возвращенных с сайта WordPress](./media/functions-create-vnet/plant.png)

Приложения-функции подключен к Интернету и виртуальной сетью. Прокси-сервер получает запрос через общедоступный Интернет и затем действуя как простой прокси-сервер HTTP для пересылать запрос подключенной виртуальной сети. Прокси-сервера, затем передает ответ обратно, публично через Интернет.

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Следующие шаги

В этом руководстве на сайте WordPress служит в качестве API, который вызывается при использовании прокси-сервера в приложении-функции. Этот сценарий делает хороший учебник, так как очень легко настроить и визуализации. Можно использовать любой другой интерфейс API, развернутым в виртуальной сети. Возможно, также были созданы функции с кодом, который вызывает API-интерфейсы, развернутые в виртуальной сети. Более реалистичный сценарий — функция, которая использует данные клиента API-интерфейсы для вызова экземпляра SQL Server, развернутых в виртуальной сети.

Функций, выполняемых в план "премиум" совместное использование одной базовой инфраструктуре службы приложений как веб-приложения на планы премиум v2. Документацию по [веб-приложения в службе приложений Azure](../app-service/overview.md) применяется к функции плана "премиум".

> [!div class="nextstepaction"]
> [Дополнительные сведения о сетевых параметрах в функциях](./functions-networking-options.md)

[План "премиум"]: functions-scale.md#premium-plan
