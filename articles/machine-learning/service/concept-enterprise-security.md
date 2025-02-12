---
title: Корпоративная безопасность
titleSuffix: Azure Machine Learning service
description: 'Безопасно использовать службы машинного обучения Azure: проверка подлинности, авторизации, сетевой безопасности, шифрование данных и мониторинга.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/10/2019
ms.openlocfilehash: 8682baa961ca3270e76614702b51ac50f197e847
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795489"
---
# <a name="enterprise-security-for-azure-machine-learning-service"></a>Безопасность корпоративного уровня для службы машинного обучения Azure

В этой статье вы узнаете о функциях безопасности, доступных в Azure служба машинного обучения.

При использовании облачной службы, рекомендуется ограничить доступ только пользователям, которым он нужен. Запустится, общие сведения о модели проверки подлинности и авторизации, используемый службой. Может также потребоваться ограничить доступ к сети или безопасного подсоединения к ресурсам в вашей локальной сети и в облаке. Шифрование данных важно также, при хранении и во время перемещения данных между службами. Наконец необходимо иметь возможность наблюдения за службами и создания журнала аудита всех действий.

## <a name="authentication"></a>Проверка подлинности
Многофакторная проверка подлинности поддерживается в том случае, если Azure Active Directory (Azure AD) настроен для той же цели.
* Клиент выполняет вход в Azure AD и получает маркер Azure Resource Manager.  Пользователи и участники службы, полностью поддерживаются.
* Клиент предоставляет маркер с помощью Azure Resource Manager и все службы машинного обучения Azure
* Служба машинного обучения Azure предоставляет маркер машинного обучения Azure для пользователя вычислений. Например, службы вычислений для машинного обучения. Этот машинного обучения Azure, которые пользователя используется токен вычислений, обратный вызов службы машинного обучения Azure (ограничивает область действия в рабочую область) после запуска завершена.

![Снимок экрана, показывающий, как работает проверка подлинности в службе машинного обучения Azure](./media/enterprise-readiness/authentication.png)

### <a name="authentication-keys-for-web-service-deployment"></a>Ключи аутентификации для развертывания веб-службы

При включении проверки подлинности для развертывания автоматически создаются ключи проверки подлинности.

* Проверка подлинности включена по умолчанию при развертывании в Службе Azure Kubernetes.
* Проверка подлинности отключена по умолчанию при развертывании в Экземплярах контейнеров Azure.

Чтобы управлять проверкой подлинности, используйте параметр `auth_enabled` при создании или обновлении развертывания.

Если включена проверка подлинности, можно использовать метод `get_keys` для извлечения первичного и вторичного ключей проверки подлинности:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Если вам нужно повторно создать ключ, используйте [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)


## <a name="authorization"></a>Authorization

Вы можете создать несколько рабочих областей, и каждая рабочая область может совместно использоваться несколькими пользователями. При совместном использовании рабочей области вы управляете доступом к ней путем назначения пользователям следующих ролей:
* Владелец
* участник;
* Читатель
    
В следующей таблице перечислены некоторые основные операции службы машинного обучения Azure и роли, которые можно выполнить их:

| Служба машинного обучения Azure операции | Владелец | участник; | Читатель |
| ---- |:----:|:----:|:----:|
| Создание рабочей области | ✓ | ✓ | |
| Совместное использование рабочей области | ✓ | |  |
| Создание вычислений | ✓ | ✓ | |
| Присоединение вычислений | ✓ | ✓ | |
| Присоединение хранилища данных | ✓ | ✓ | |
| Запуск эксперимента | ✓ | ✓ | |
| Просмотр выполнений/метрик | ✓ | ✓ | ✓ |
| Регистрация модели | ✓ | ✓ | |
| Создание образа | ✓ | ✓ | |
| Развертывание веб-службы | ✓ | ✓ | |
| Просмотр моделей/images | ✓ | ✓ | ✓ |
| Вызов веб-службы | ✓ | ✓ | ✓ |

Если встроенные роли не соответствует требованиям, можно также создать пользовательские роли. Только пользовательские роли, которые мы поддерживаем предназначены для операций в рабочей области и вычислений для машинного обучения. Пользовательские роли могут быть чтение, запись или удаление разрешений на рабочей области и вычислительные ресурсы в этой рабочей области. Вы можете предоставить роли на уровне определенную рабочую область, конкретный ресурс группы или уровнем конкретной подписки. Дополнительные сведения см. в разделе [управлять пользователями и ролями в рабочей области машинного обучения Azure](how-to-assign-roles.md)

### <a name="securing-compute-and-data"></a>Защита вычислений и данных
Владельцы и участники могут использовать все вычислительные целевых объектов и хранилища данных, подключенных к рабочей области.  
Каждая рабочая область также имеет связанный назначенный системой управляемого удостоверения (с тем же именем, что и рабочая область) со следующими разрешениями на подключенные ресурсы, используемые в рабочей области:

Дополнительные сведения об управляемых удостоверений, см. в разделе [управляемые удостоверения для ресурсов Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

| Resource | Разрешения |
| ----- | ----- |
| Рабочая область | Участник | 
| Учетная запись хранения | Участник для данных больших двоичных объектов хранилища | 
| Key Vault | Доступ ко всем сертификатам ключи, секреты, | 
| Реестр контейнеров Azure | Участник | 
| Группа ресурсов, которая содержит рабочую область | Участник | 
| Группы ресурсов, содержащей Key Vault (если он отличается от той, содержащую рабочую область) | Участник | 

Рекомендуется, что администраторы не отзывают доступ управляемое удостоверение для указанных выше ресурсов. Доступ может быть восстановлена с помощью операции повторной синхронизации ключей.

Служба машинного обучения Azure создает дополнительное приложение (имя начинается с `aml-`) с участник уровня доступа в вашей подписке для каждого региона, в рабочей области. Для ex. Если у вас есть рабочая область в восточной части США и другую рабочую область в Северной Европе в той же подписке, вы увидите два таких приложений. Это необходимо, чтобы машинного обучения Azure, служба может помочь в управлении вычислительными ресурсами.


## <a name="network-security"></a>Безопасность сети

Служба машинного обучения Azure использует вычислительные ресурсы других служб Azure. Вычислительные ресурсы (целевые объекты вычислений) используются для обучения и развертывания моделей. Эти целевые объекты вычислений могут создаваться в виртуальной сети. Например, вы можете обучить модель с помощью Виртуальной машины обработки и анализа данных корпорации Майкрософт, а затем развернуть модель в Службе Azure Kubernetes (AKS).  

Дополнительные сведения см. в разделе [выполнение экспериментов и вывод в виртуальной сети](how-to-enable-virtual-network.md).

## <a name="data-encryption"></a>Шифрование данных

### <a name="encryption-at-rest"></a>Шифрование при хранении
#### <a name="azure-blob-storage"></a>Хранилище BLOB-объектов Azure
Служба машинного обучения Azure хранятся моментальные снимки, выходные данные и журналы в учетной записи хранилища BLOB-объектов Azure, привязывается к рабочей области службы машинного обучения Azure и находится в подписке пользователя. Все данные, хранящиеся в хранилище BLOB-объектов шифруются при хранении с помощью Microsoft-Managed ключей.

Дополнительные сведения о способах использования собственных ключей для данных, хранящихся в хранилище BLOB-объектов см. в разделе [шифрование службы хранилища Azure с помощью управляемых пользователем ключей в Azure Key Vault](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Обучающие данные обычно также хранятся в хранилище BLOB-объектов Azure, чтобы был доступен для обучения вычислений. Это хранилище не управляется машинного обучения Azure, но подключены к расчет в качестве удаленной файловой системы.

Сведения о повторном создании ключей доступа для учетных записей хранения Azure, используемых с рабочей областью, см. в разделе [повторное создание ключей доступа к хранилищу](how-to-change-storage-access-key.md) статьи.

#### <a name="cosmos-db"></a>Cosmos DB
Служба машинного обучения Azure хранит метрики и метаданных в Cosmos DB, которая существует в подписке Майкрософт, управляемых службой машинного обучения Azure. Все данные, хранящиеся в Cosmos DB, шифруются при хранении с помощью ключей, управляемых Майкрософт.

#### <a name="azure-container-registry-acr"></a>Реестр контейнеров Azure (ACR)
Все образы контейнеров в реестре (ACR) шифруются в неактивном состоянии. Azure автоматически шифрует образ перед сохранением и расшифровывает его в режиме реального времени, когда службы машинного обучения Azure получает изображение.

#### <a name="machine-learning-compute"></a>Вычислений для машинного обучения
Диск операционной системы для каждого вычислительного узла, хранящихся в службе хранилища Azure шифруются с помощью ключей, управляемых Майкрософт в учетных записях хранения службы машинного обучения Azure. Используется временный ключ этого вычислений и кластеры уменьшается обычно, когда нет запусков в очереди. Отмена подготовки базовой виртуальной машины и удалить диск операционной системы. Шифрование дисков Azure не поддерживается для диска операционной системы.
Каждая виртуальная машина также имеет локальный временный диск для операционной системы операций. Этот диск при необходимости можно подготовить данные обучения. Этот диск не зашифрован. Дополнительные сведения о том, как работает шифрование при хранении в Azure, см. в разделе [Azure данных шифрования неактивных](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest). 

### <a name="encryption-in-transit"></a>Шифрование при передаче
Оба внутренней связи между различными micro служб машинного обучения Azure и внешнего взаимодействия используют вызова конечной точки оценки поддерживаются с помощью протокола SSL. Любой доступ службы хранилища Azure, также по защищенному каналу. Дополнительные сведения см. в разделе [Secure машинного обучения Azure веб-служб с помощью протокола SSL](https://docs.microsoft.com/azure/machine-learning/service/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Использование Azure Key Vault
Экземпляр хранилища ключей, связанный с рабочей областью используется службой машинного обучения Azure для хранения учетных данных разного типа:
* Строка подключения учетной записи хранилища.
* Пароли к экземплярам репозиторий контейнеров Azure
* Строки подключения к данным хранилища. 

SSH пароли и ключи для вычисления целевых объектов, таких как HDInsight HDI и виртуальной Машины хранятся в отдельном хранилище ключ, связанный с подпиской Microsoft. Служба машинного обучения Azure хранить все пароли или ключи указанное пользователем вместо этого он создает, авторизует и хранит свои собственные ключи SSH для подключения к виртуальной Машине/HDInsight для выполнения экспериментов. Каждая рабочая область имеет связанный назначенный системой управляемого удостоверения (с тем же именем, что и рабочая область), имеющий доступ ко всем ключи, секреты и сертификаты в хранилище ключей.

 
## <a name="monitoring"></a>Мониторинг

Пользователи могут просматривать журнал действий в рабочей области, чтобы просмотреть различные операции, выполняемые в рабочей области и получить основную информацию, как имя операции, Кем инициировано событие, отметка времени и т.д.

На следующем рисунке показан журнал действий для рабочей области:

![Отображение журнала действий на снимке экрана в рабочей области](./media/enterprise-readiness/workspace-activity-log.png)


Оценки сведений о запросе, хранятся в AppInsights, который создается в подписке пользователя при создании рабочей области. Сюда входят такие поля, как HTTPMethod, UserAgent, ComputeType, RequestUrl, StatusCode, RequestId, длительность и т.д.


## <a name="data-flow-diagram"></a>Схема потока данных

### <a name="create-workspace"></a>Создание рабочей области
В примере ниже показан рабочий процесс создания рабочей области.
Пользователь выполняет вход в Azure AD из любого из поддерживаемых клиентов службы машинного обучения Azure (портал Azure CLI, пакет SDK для Python) и запрашивает соответствующий маркер Azure Resource Manager.  Затем пользователь вызывает Azure Resource Manager создать рабочую область.  Azure Resource Manager контакты машинного обучения Azure службы поставщика ресурсов для подготовки рабочей области.  Дополнительные ресурсы создаются в подписке клиента во время создания рабочей области:
* Хранилище ключей (для хранения секретов)
* Учетная запись хранилища Azure (включая BLOB-объектов и общая папка)
* Реестр контейнеров Azure (для хранения образов docker для вывода и оценка и службы "Экспериментирование")
* Application Insights (для хранения данных телеметрии)

Другие вычислительные ресурсы, подключенные к рабочей области (службы Azure Kubernetes, виртуальной Машины и т.д.) также можно подготовить клиенты при необходимости. 

![Снимок экрана: Создание рабочей области рабочего процесса](./media/enterprise-readiness/create-workspace.png)

### <a name="save-source-code-training-scripts"></a>Сохраните исходный код (скриптов обучения)
В примере ниже показан снимок кода рабочего процесса.
Связанные с машинного обучения Azure рабочей области службы — это каталоги (эксперименты), который содержит исходный код (скриптов обучения).  Они хранятся на локальном компьютере клиента, так и в облаке (в хранилище BLOB-объектов Azure в рамках подписки клиента). Эти моментальные снимки кода используются для выполнения или проверки для журнала аудита.

![Снимок экрана: Создание рабочей области рабочего процесса](./media/enterprise-readiness/code-snapshot.png)

### <a name="training"></a>Обучение
В примере ниже показан рабочий процесс обучения.
* Служба машинного обучения Azure вызывается с Идентификатором моментальных снимков для сохранения выше моментального снимка кода
* Служба машинного обучения Azure, служба создает запуска (необязательно) идентификатор & маркер службы машинного обучения Azure, который далее используются методом целевые объекты вычисления как машины обучения вычислений и виртуальных Машин мог подключиться обратно к службе машинного обучения Azure
* Вы можете либо управляемого вычислений (например) Машинное обучение вычислений) или неуправляемых вычислений (например) Виртуальная машина) для выполнения заданий обучения. Поток данных приведено объяснение для обоих приведенных ниже сценариев.
* (ВМ/HDInsight — получить доступ с помощью учетных данных SSH в Key Vault в подписке Microsoft) Служба машинного обучения Azure выполняет код управления в целевой объект вычислений:
    1.  Подготавливает среду (Примечание: Docker — это вариант для виртуальной Машины или локальной также. См. в разделе действия вычислений для машинного обучения ниже понять, как выполнение эксперимента на works контейнер docker)
    2.  Загрузка кода
    3.  Настраивает переменные среды и конфигурации
    4.  Запускает сценарий пользователя (моментальный снимок кода упомянутых выше)
* (Вычислений для машинного обучения — получить доступ с помощью рабочей области управляемых удостоверений) Обратите внимание, что поскольку вычислений для машинного обучения — это управляемая вычислительная т.е управляется корпорацией Майкрософт в результате выполняется в рамках подписки Microsoft.
    1.  Построение удаленного Docker запускается процесс, при необходимости
    2.  Записывает код управления пользователя общей папки Azure
    3.  Контейнер начинается с начальной то есть команды управления код на предыдущем шаге


#### <a name="querying-runs--metrics"></a>Запрос выполняется и метрики
Этот шаг показан в потоке, где обучения вычислительных операций записи *запуска метрики* в службе машинного обучения Azure, из которой оно будет сохранено в Cosmos DB. Клиенты могут вызывать службы машинного обучения Azure, которая в свою очередь будет извлечь метрики из Cosmos DB и вернуть его обратно клиенту.

![Снимок экрана: Создание рабочей области рабочего процесса](./media/enterprise-readiness/training-and-metrics.png)

### <a name="creating-web-services"></a>Создание веб-служб
В примере ниже показан вывод рабочего процесса. Определение или модель оценки, — это этап, где используется развернутой модели для прогнозирования, чаще всего для производственных данных.
Подробности см. ниже:
* Пользователь проходит регистрацию модели с помощью клиента, например пакета SDK для Azure ML
* Пользователь создает образ с помощью модели, файл оценки и другие зависимости модели
* Создается и сохраняется в ACR образа Docker
* Веб-служба развертывается в целевой объект вычислений (ACI и AKS) с помощью ранее созданного образа
* Оценки сведений о запросе, хранятся в AppInsights, которая находится на подписку пользователя
* Данные телеметрии также отправляются в Майкрософт или подписку Azure

![Снимок экрана: Создание рабочей области рабочего процесса](./media/enterprise-readiness/inferencing.png)

## <a name="next-steps"></a>Следующие шаги

* [Защита веб-служб Машинного обучения Azure с помощью SSL](how-to-secure-web-service.md)
* [Использование модели Машинного обучения Azure, развернутой в качестве веб-службы](how-to-consume-web-service.md)
* [Run batch predictions on large data sets with Azure Machine Learning service](how-to-run-batch-predictions.md) (Составление пакетных прогнозов для больших наборов данных с помощью Службы машинного обучения Azure)
* [Мониторинг моделей машинного обучения в Azure с помощью Application Insights](how-to-enable-app-insights.md)
* [Сбор данных для моделей в рабочей среде](how-to-enable-data-collection.md)
* [What is the Azure Machine Learning SDK for Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) (Что такое пакет SDK Службы машинного обучения Azure для Python?)
* [Securely run experiments and inferencing inside an Azure Virtual Network](how-to-enable-virtual-network.md) (Безопасное выполнение экспериментов и формирование выводов внутри виртуальной сети Azure)
* [Recommenders](https://github.com/Microsoft/Recommenders) (Системы рекомендаций)
* [Создание API рекомендаций в режиме реального времени в Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
