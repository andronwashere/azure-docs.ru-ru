---
title: Обучение и развертывание моделей Машинного обучения в Azure IoT Edge | Документация Майкрософт
description: Обучите модель машинного обучения с помощью Службы машинного обучения Azure, а затем упакуйте модель в виде образа контейнера, который можно развернуть как модуль Azure IoT Edge.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: acf0b1984eb3e68858be6ed68731612448e672f4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67432704"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Руководство по Обучение и развертывание модели машинного обучения Azure

> [!NOTE]
> Эта статья входит в серию учебников по использованию Машинного обучения Azure в IoT Edge. Если вы перешли к этой статье по прямой ссылке, для оптимальных результатов рекомендуем начать с изучения [первой статьи](tutorial-machine-learning-edge-01-intro.md) этой серии.

В рамках этой статьи мы сначала используем Записные книжки Azure для обучения модели машинного обучения с помощью Службы машинного обучения Azure, а затем упакуем эту модель в виде образа контейнера, который можно развернуть как модуль Azure IoT Edge. В Записных книжках Azure используется рабочая область Службы машинного обучения Azure, которая является фундаментальным блоком, который служит для выполнения экспериментов, обучения и развертывания моделей машинного обучения.

Действия в этой части учебника выполняются в двух записных книжках.

* **01-turbofan\_regression.ipynb.** В этой записной книжке выполняются этапы обучения и публикации модели с помощью Машинного обучения Azure. В целом используется следующий порядок действий:

  1. Скачивание, подготовка и изучение обучающих данных.
  2. Создание и запуск эксперимента по машинному обучению с помощью рабочей области службы.
  3. Оценка результатов модели по завершении эксперимента.
  4. Публикация модели в рабочей области службы.

* **02-turbofan\_deploy\_model.ipynb.** Эта записная книжка принимает модель, созданную в предыдущей книжке, и использует ее для создания образа контейнера, готового для развертывания на устройстве Azure IoT Edge.

  1. Создайте скрипт для оценки модели.
  2. Создайте и опубликуйте образ.
  3. Разверните образ в качестве веб-службы в экземпляре контейнера Azure.
  4. С помощью веб-службы проверьте, работают ли модель и образ надлежащим образом

Описанные в этой статье действия обычно выполняются специалистами по обработке и анализу данных.

## <a name="set-up-azure-notebooks"></a>Настройка Записных книжек Azure

Мы используем службу "Записные книжки Azure" для размещения двух записных книжек Jupyter и вспомогательных файлов. В этой статье мы создаем и настраиваем проект Записных книжек Azure. Если вы впервые используете записные книжки Jupyter и (или) Azure, ознакомьтесь с приведенными ниже документами:

* **Краткое руководство.** [Создание и совместное использование записной книжки](../notebooks/quickstart-create-share-jupyter-notebook.md)
* **Руководство.** [Создание и запуск записной книжки Jupyter с помощью Python](../notebooks/tutorial-create-run-jupyter-notebook.md)

Как и виртуальная машина разработки, записные книжки Azure обеспечивают согласованную среду для выполнения упражнения.

> [!NOTE]
> После настройки вы сможете получить доступ к службе "Записные книжки Microsoft Azure" с любого компьютера. Во время установки используйте виртуальную машину разработки, которая содержит все необходимые файлы.

### <a name="create-an-azure-notebooks-account"></a>Создание учетной записи службы "Записные книжки Azure"

Учетные записи службы "Записные книжки Azure" не зависят от подписок Azure. Чтобы использовать Записные книжки Azure, необходимо создать учетную запись.

1. Перейдите к службе [Записные книжки Azure](https://notebooks.azure.com).

2. В правом верхнем углу страницы щелкните **Войти**.

3. Войдите в систему, используя свою рабочую или учебную учетную запись (Azure Active Directory) или личную учетную запись (учетную запись Майкрософт).

4. Если вы ранее не использовали службу "Записные книжки Azure", отобразится запрос на предоставление доступа к приложению службы "Записные книжки Azure".

5. Создайте идентификатор пользователя для приложения Записных книжек Azure.

### <a name="upload-jupyter-notebooks-files"></a>Передача файлов записных книжек Jupyter

На этом шаге мы создадим новый проект Записных книжек Azure и передадим в него файлы. К передаваемым файлам относятся следующие:

* **01-turbofan\_regression.ipynb.** Файл записной книжки Jupyter, в котором выполняется процесс загрузки данных, созданных окружением устройства из учетной записи хранения Azure, изучение и подготовка данных для обучения классификатора, обучение модели, тестирование данных с использованием набора тестовых данных, найденного в файле Test\_FD003.txt, сохранение модели классификатора в рабочей области Службы машинного обучения.

* **02-turbofan\_deploy\_model.ipynb.** Записная книжка Jupyter, в которой выполняется процесс использования модели классификатора, сохраненной в рабочей области Службы машинного обучения, для создания образа контейнера. После создания образа вы сможете развернуть его в записной книжке в качестве веб-службы, чтобы проверить, работает ли он должным образом. Проверенный образ будет развернут на устройстве Edge, как описано в статье [Tutorial: Create and deploy custom IoT Edge modules](tutorial-machine-learning-edge-06-custom-modules.md) (Руководство. Создание и развертывание настраиваемых модулей IoT Edge).

* **Test\_FD003.txt.** Файл, содержащий данные, которые мы будем использовать в качестве тестового набора при проверке обученного классификатора. Для простоты примера мы решили использовать в качестве тестового набора тестовые данные, предоставленные для исходной проверки.

* **RUL\_FD003.txt.** Файл, содержащий RUL для последнего цикла каждого устройства в файле Test\_FD003.txt. Подробное описание данных см. в файлах **readme.txt** и **Damage Propagation Modeling.pdf** в папке C:\\source\\IoTEdgeAndMlSample\\data\\Turbofan.

* **Utils.py.** Файл, содержащий набор функций Python для работы с данными. Первая записная книжка содержит подробное описание функций.

* **README.md.** Файл сведений, содержащий описание использования записных книжек.

Создайте новый проект и передайте файлы в свою записную книжку.

1. В строке меню сверху выберите **Мои проекты**.

1. Выберите **+ Создать проект**. Укажите имя и идентификатор. Нет необходимости делать этот проект общедоступным и загружать в него файл сведений.

1. Щелкните **Загрузить** и выберите **From Computer** (С компьютера).

1. Щелкните **Выбрать файлы**.

1. Перейдите в папку **C:\source\IoTEdgeAndMlSample\AzureNotebooks**. Выберите все файлы и щелкните **Открыть**.

1. Выберите **Отправить**, чтобы начать передачу, а затем щелкните **Готово** после завершения процесса.

## <a name="run-azure-notebooks"></a>Запуск Записных книжек Azure

Теперь, когда проект создан, запустите записную книжку **01-turbofan\_regression.ipynb**.

1. На странице проекта турбореактивного устройства выберите **01-turbofan\_regression.ipynb**.

    ![Выбор первой записной книжки для запуска](media/tutorial-machine-learning-edge-04-train-model/select-turbofan-regression-notebook.png)

2. При появлении запроса выберите из диалогового окна ядро ​​Python 3.6 и щелкните **Set Kernel​** (Настроить ядро).

3. Если для записной книжки указано значение **Not Trusted** (Нет доверия), щелкните виджет **Not Trusted** (Нет доверия) в правом верхнем углу записной книжки. Когда появится диалоговое окно, выберите **Доверие**.

4. Следуйте указаниям в записной книжке.

    * Сочетание клавиш `Ctrl + Enter` запускает ячейку.
    * Сочетание клавиш `Shift + Enter` запускает ячейку и переходит к следующей.
    * Запущенная ячейка содержит звездочку в квадратных скобках **[\*]** . По завершении выполнения ячейки звездочка будет заменена на число и ниже могут отобразиться соответствующие выходные данные. Так как ячейки часто продолжают работу предыдущих ячеек, одновременно можно запустить лишь одну ячейку.

5. По завершении работы с записной книжкой **01-turbofan\_regression.ipynb** вернитесь на страницу проекта.

6. Откройте **02-turbofan\_deploy\_model.ipynb** и повторите шаги, описанные в этом разделе, чтобы запустить вторую записную книжку.

## <a name="next-steps"></a>Дополнительная информация

В рамках этой статьи мы использовали две записные книжки Jupyter, работающие в службе "Записные книжки Azure", для обучения классификатора оставшегося срока полезного использования (RUL) с помощью данных с турбореактивных устройств, чтобы сохранить классификатор в качестве модели, создать образ контейнера, а также развернуть и протестировать образ в качестве веб-службы.

Переходите к следующей статье, где вы создадите устройство IoT Edge.

> [!div class="nextstepaction"]
> [Tutorial: Configure an IoT Edge device](tutorial-machine-learning-edge-05-configure-edge-device.md) (Руководство. Настройка устройства IoT Edge)
