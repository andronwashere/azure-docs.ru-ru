---
title: Настройка промежуточных сред для веб-приложений в службе приложений Azure | Документация Майкрософт
description: Узнайте, как использовать промежуточную публикацию для веб-приложений в службе приложений Azure.
services: app-service
documentationcenter: ''
author: cephalin
writer: cephalin
manager: jpconnoc
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2019
ms.author: cephalin
ms.openlocfilehash: fd488d475e24bc1aeebfa49b9d81b04ebae449ff
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445598"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Настройка промежуточных сред в службе приложений Azure
<a name="Overview"></a>

При развертывании веб-приложения, веб-приложения в Linux, серверные части мобильных или приложение API для [службе приложений Azure](https://go.microsoft.com/fwlink/?LinkId=529714), можно использовать отдельный слот развертывания вместо рабочего слота по умолчанию при запуске **Standard**, **Уровня "премиум"** , или **Isolated** уровень плана службы приложений. Слоты развертывания являются динамические приложения с собственными именами узлов. Содержимое приложений и элементы конфигурации можно переключать между двумя слотами развертывания (включая рабочий слот). 

Развертывание приложения в слоте, не являющимся рабочим, дает следующие преимущества:

* Внеся изменения в приложение, вы можете проверить их в промежуточном слоте развертывания, прежде чем переключать в рабочий слот.
* Развертывание приложения в промежуточном слоте и последующее переключение в рабочий слот гарантирует, что все экземпляры слота будут подготовлены до переключения в рабочую среду. Это позволит вам избежать простоя при развертывании приложения. Перенаправление трафика не вызывает затруднений, а запросы не теряются из-за операций переключения. Весь этот рабочий процесс можно автоматизировать, настроив [автоматического переключения](#Auto-Swap) когда проверка перед переносом не требуется.
* После переключения в слоте, где ранее находилась промежуточная версия приложения, будет размещено приложение, которое до этого было рабочим. Если изменения в рабочем слоте не соответствуют вашим ожиданиям, вы можете мгновенно переключиться назад к последней рабочей версии сайта.

Каждый уровень плана службы приложений поддерживает разное количество слотов развертывания. Нет отсутствие дополнительной платы за использование слотов развертывания. Чтобы узнать число слотов поддерживает уровень вашего приложения, см. в разделе [ограничения службы приложений](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). 

Для масштабирования приложения к другому уровню, убедитесь, что для целевого уровня поддерживает число ячеек, которые ваше приложение уже использует. Например, если приложение имеет более пяти слотов, нельзя масштабировать его вниз до **стандартный** tier, так как **стандартный** уровень поддерживает только пять слотов развертывания. 

<a name="Add"></a>

## <a name="add-a-slot"></a>Добавление слота
Чтобы включить несколько слотов развертывания, приложение должно быть ценовой категории **Стандартный**, **Премиум** или **Изолированный**.

1. На [портале Azure](https://portal.azure.com/) откройте [страницу ресурсов](../azure-resource-manager/manage-resources-portal.md#manage-resources) приложения.

2. В левой области выберите **слоты развертывания** > **добавить слот**.
   
    ![Добавить новый слот развернутого приложения](./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png)
   
   > [!NOTE]
   > Если приложение еще не в **стандартный**, **уровня "премиум"** , или **Isolated** уровня, вы получите сообщение, указывающее, поддерживаемых уровней, позволяющих включить промежуточную публикацию. На этом этапе у вас есть возможность выбрать **обновление** и перейдите к **масштабирования** вкладке вашего приложения, прежде чем продолжить.
   > 

3. В **добавить слот** диалоговое окно, присвойте слоту имя и укажите, следует ли клонировать конфигурацию приложения из другого слота развертывания. Выберите **добавить** для продолжения.
   
    ![Источник конфигурации](./media/web-sites-staged-publishing/ConfigurationSource1.png)
   
    Вы можете клонировать конфигурацию из любого существующего слота. При клонировании копируются такие параметры, как настройки приложения, строки подключения, языковые версии платформы, веб-сокеты, версия HTTP и разрядность платформы.

4. После добавления области, выберите **закрыть** чтобы закрыть диалоговое окно. Новый слот теперь отображается на **слоты развертывания** страницы. По умолчанию **% трафика** имеет значение 0 для новой области, со всех клиентов трафик, направленный на рабочий слот.

5. Выберите новый слот развертывания, чтобы открыть страницу ресурсов этого слота.
   
    ![Название области развертывания](./media/web-sites-staged-publishing/StagingTitle.png)

    Страница управления для промежуточного слота ничем не отличается от страницы для любого приложения Службы приложений. Здесь вы можете изменить конфигурацию слота. В верхней части страницы отображается имя слота, напоминающее о том, что вы просматриваете слот развертывания.

6. Выберите URL-адрес приложения на странице ресурсов для слота. Слот развертывания имеет собственное имя узла и также является динамическим приложением. Чтобы ограничить общий доступ к слот развертывания, см. в разделе [ограничения IP-адресов службы приложений Azure](app-service-ip-restrictions.md).

Новый слот развертывания не имеет содержимого, даже если вы клонировали параметры из другого слота. Например, вы можете [публикацию в эту область с помощью Git](app-service-deploy-local-git.md). Вы можете выполнять развертывание в слот из другой ветви репозитория или даже из другого репозитория. 

<a name="AboutConfiguration"></a>

## <a name="what-happens-during-a-swap"></a>Что происходит во время переключения

### <a name="swap-operation-steps"></a>Шаги операции замены

При переключении два слота (обычно из промежуточного слота в рабочий слот), служба приложений выполняет следующие действия, чтобы убедиться, что целевой слот не будет времени простоя:

1. Применяются следующие параметры из целевого слота (например, на рабочий слот) ко всем экземплярам исходный слот: 
    - [Определяемые слотом](#which-settings-are-swapped) параметры приложения и строки подключения, если применимо.
    - [Непрерывное развертывание](deploy-continuous-deployment.md) параметры, если включен.
    - [Проверка подлинности службы приложений](overview-authentication-authorization.md) параметры, если включен.
    
    Любой из этих случаев триггер все экземпляры в исходный слот для перезапуска. Во время [переключения с предварительным просмотром](#Multi-Phase), это обозначает конец первого этапа. Операция замены будет приостановлен, а вы можете проверить, что исходный слот работает корректно вместе с параметры целевого слота.

1. Ожидания для каждого экземпляра в исходный слот, чтобы завершить его перезапуска. Если не удается перезапустить любой экземпляр, операция переключения отменяет все изменения к исходному слоту и остановить операцию.

1. Если [локальный кэш](overview-local-cache.md) — включена, запустить инициализация локального кэша, выполнив запрос HTTP, к корневому каталогу приложения («/») на каждом экземпляре исходный слот. Подождите, пока каждый экземпляр возвращает любой ответ HTTP. Инициализация локального кэша приводит к перезагрузке на каждом экземпляре.

1. Если [автоматического переключения](#Auto-Swap) включен с [пользовательский прогрев](#Warm-up), триггер [запуска приложения](https://docs.microsoft.com/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) , сделав запрос HTTP к корневому каталогу приложения («/») на каждом экземпляре источника слот.

    Если `applicationInitialization` не указан, триггер HTTP-запроса к корню приложения исходный слот на каждом экземпляре. 
    
    Если экземпляр возвращает любой ответ HTTP, он рассматривается как быть активированию.

1. Если все экземпляры в исходном слоте, будут подготовлены успешно, замените два разъема, переключившись правила маршрутизации для этих двух слотов. После выполнения этого шага целевого слота (например, на рабочий слот) имеет приложение, которое ранее разогреется в исходный слот.

1. Теперь, когда исходный слот у приложение перед переносом ранее в целевом слоте, выполните ту же операцию, применяя все параметры и перезапуска экземпляров.

В любой момент операция переключения всю работу по инициализации замененных приложений происходит в исходном слоте. Целевой слот остается в сети во время исходного слота подготовлен, а специалисты, независимо от того, где успешного или неуспешного выполнения замены. Чтобы переключить промежуточный слот на рабочий слот, убедитесь, что рабочий слот, всегда иметь целевой слот. Таким образом, операция замены не влияет на основным приложением.

### <a name="which-settings-are-swapped"></a>Какие параметры переносятся?
При клонировании конфигурации из другой области развертывания вы можете изменять клонированную конфигурацию. Некоторые элементы конфигурации, выполните содержимое через операцию переключения (не определяются слотом), тогда как другие элементы конфигурации остаются в том же слоте после переключения (слотом). Ниже приведен список параметров, которые изменяются при переключении слотов.

**Переносимые параметры**:

* Общие параметры, например версия платформы, 32/64-разрядная версия, веб-сокеты
* Параметры приложения (их также можно привязать к слоту)
* Строки подключения (их также можно привязать к слоту)
* Сопоставления обработчиков
* Настройки диагностики и мониторинга
* Открытые сертификаты
* Содержимое веб-заданий
* Гибридные подключения *
* Интеграция виртуальной сети *
* Конечные точки службы *
* Сеть доставки содержимого Azure *

Функции, отмеченные звездочкой (*) мы планируем сделать их прикрепленные к слоту. 

**Параметры, которые не переносятся**:

* Конечные точки публикации
* Имена пользовательских доменов
* Закрытые сертификаты и привязки SSL
* Параметры масштабирования
* Планировщики веб-заданий
* Ограничения IP-адресов
* Всегда включено
* Параметры протокола (HTTPS, версию TLS, сертификатов)
* Параметры журнала диагностики
* Кросс-совместного использования ресурсов (CORS)

<!-- VNET and hybrid connections not yet sticky to slot -->

Чтобы настроить приложение параметр или строки подключения фиксируется в конкретный слот (не местами), перейдите к **конфигурации** страницы для этого слота. Добавить или изменить параметр, а затем выберите **параметр слота развертывания**. Установка этого флажка указывает службе приложений, что параметр не является заменой. 

![Параметр слота](./media/web-sites-staged-publishing/SlotSetting.png)

<a name="Swap"></a>

## <a name="swap-two-slots"></a>Переключение двух слотов 
Вы можете переключить слоты развертывания для приложения **слоты развертывания** страницы и **Обзор** страницы. Дополнительные технические сведения о переключения слотов, см. в разделе [что происходит во время замены](#AboutConfiguration).

> [!IMPORTANT]
> Перед переключением приложения из слота развертывания в рабочей среде, убедитесь, что рабочей среде вашей целевой слот и что все параметры в исходный слот настроены именно так, как вы хотите разместить их в рабочей среде.
> 
> 

Чтобы переключить слоты развертывания:

1. Перейдите к своему приложению **слоты развертывания** и укажите **замены**.
   
    ![Кнопка "Переключить"](./media/web-sites-staged-publishing/SwapButtonBar.png)

    **Замены** диалоговом окне отображаются параметры в выбранных исходном и целевом слотах, которые будут изменены.

2. Выберите **исходный** и **целевой** слоты. В качестве целевого чаще всего используется рабочий слот. Кроме того, выберите **изменениях в источнике** и **цели** вкладки и убедитесь, что изменения конфигурации ожидается. Когда вы закончите, вы можете переключить слоты немедленно, выбрав **замены**.

    ![Завершить переключение](./media/web-sites-staged-publishing/SwapImmediately.png)

    Чтобы увидеть, как ваш целевой слот будет запущено с новыми параметрами прежде, чем фактически происходит переключение, не выбирайте **замены**, но инструкции в [переключения с предварительным просмотром](#Multi-Phase).

3. Когда вы закончите, закройте диалоговое окно, выбрав **закрыть**.

Если у вас возникли проблемы, см. в разделе [Устранение переключений](#troubleshoot-swaps).

<a name="Multi-Phase"></a>

### <a name="swap-with-preview-multi-phase-swap"></a>Переключение с предварительным просмотром (многофазное переключение)

> [!NOTE]
> Переключение с предварительным просмотром не поддерживается для веб-приложений в ОС Linux.

До того, как в производственной среде в качестве целевой слот, проверьте, что приложение выполняется с параметрами местами. Исходный слот также инициализируется до их завершения переключения, который является предпочтительным для критически важных приложений.

При выполнении переключения с предварительным просмотром, служба приложений выполняет же [операцию переключения](#AboutConfiguration) но приостанавливается после первого шага. Затем можно проверить результат в промежуточном слоте перед выполнением переключения. 

Если вы отменяете переключения, службы приложений повторно применяет элементы конфигурации к исходному слоту.

Чтобы переключить с предварительным просмотром:

1. выполните действия, описанные в [переключение слотов развертывания](#Swap) но выберите **выполнение переключения с предварительным просмотром**.

    ![Переключение с предварительным просмотром](./media/web-sites-staged-publishing/SwapWithPreview.png)

    Диалоговое окно показывает, как изменения конфигурации в исходный слот на первом этапе, и как в исходной и целевой слот изменить на этапе 2.

2. Когда вы будете готовы приступить к переключения, выберите **запустить замены**.

    После завершения этапа 1, вы получите уведомление об в диалоговом окне. Предварительный просмотр подтверждения переноса исходного слота, перейдя к `https://<app_name>-<source-slot-name>.azurewebsites.net`. 

3. Когда вы будете готовы завершить ожидающие переключение, выберите **завершить переключение** в **действие переключения** и выберите **завершить переключение**.

    Чтобы Отмена промежуточного переключения, выберите **отменить замены** вместо этого.

4. Когда вы закончите, закройте диалоговое окно, выбрав **закрыть**.

Если у вас возникли проблемы, см. в разделе [Устранение переключений](#troubleshoot-swaps).

Чтобы автоматизировать многофазное переключение, см. руководство [Автоматизация с помощью PowerShell](#automate-with-powershell).

<a name="Rollback"></a>

## <a name="roll-back-a-swap"></a>Откатить операцию переключения
Если в целевом слоте (обычно это рабочий слот) будут обнаружены ошибки после переключения слотов, немедленно восстановите прежнее (до переключения) состояние слотов, выполнив обратное переключение.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Настройка автоматического переключения

> [!NOTE]
> Автоматическое переключение не поддерживается в веб-приложениях на платформе Linux.

Функция автоматического переноса упрощает сценарии DevOps в Azure, где требуется развернуть приложение постоянно с нуля холодного запуска и нулевым временем простоя для пользователей этого приложения. Если автоматическое переключение включена из слот в рабочую среду, при каждой отправке изменений кода в этом слоте служба приложений автоматически [меняет местами приложения в рабочей среде](#swap-operation-steps) после его подготовки в исходный слот.

   > [!NOTE]
   > Перед настройкой автоматического переключения для производственного слота, вам следует протестировать автоматическое переключение на нерабочей целевой слот.
   > 

Настройка автоматического переключения.

1. Перейдите на страницу ресурсов приложения. Выберите **слоты развертывания** >  *\<определенным источником разъем >*  > **конфигурации**  >  **Общие параметры**.
   
2. Для **включено автоматическое переключение**выберите **на**. Выберите целевой слот для **слот автоматического переключения развертывания**и выберите **Сохранить** на панели команд. 
   
    ![Выбранные параметры для настройки автоматического переключения](./media/web-sites-staged-publishing/AutoSwap02.png)

3. Передайте код в исходный слот. Автоматическое переключение происходит через некоторое время, а обновление отображается в URL-адрес целевого слота.

Если у вас возникли проблемы, см. в разделе [Устранение переключений](#troubleshoot-swaps).

<a name="Warm-up"></a>

## <a name="specify-custom-warm-up"></a>Укажите пользовательский прогрев
При использовании [автоматического переключения](#Auto-Swap), некоторые приложения могут потребовать действий пользовательский прогрев перед заменой. `applicationInitialization` Элемента конфигурации в файле web.config можно указать пользовательские действия инициализации. [Операцию переключения](#AboutConfiguration) ожидает этот пользовательский прогрев не до конца, прежде чем переключаться с целевого слота. Ниже приведен пример фрагмента файла web.config.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

Дополнительные сведения о настройке `applicationInitialization` элемент, см. в разделе [наиболее распространенных сбоев замены слота развертывания и способы их устранения](https://ruslany.net/2017/11/most-common-deployment-slot-swap-failures-and-how-to-fix-them/).

Также можно настроить поведение прогрева один или оба следующих [параметры приложения](configure-common.md):

- `WEBSITE_SWAP_WARMUP_PING_PATH`: Путь для проверки связи для прогрева веб-узла. Добавьте этот параметр приложения, указав настраиваемый путь, который начинается с косой черты в качестве значения. Например, `/statuscheck`. По умолчанию используется значение `/`. 
- `WEBSITE_SWAP_WARMUP_PING_STATUSES`: Допустимые коды ответов HTTP для операции прогрева. Добавьте этот параметр приложения со списком кодов HTTP, разделенным запятыми. Например, `200,202` . Если возвращаемый код состояния отсутствует в списке, останавливаются операций прогрева и замены. По умолчанию все коды ответов являются допустимыми.

Если у вас возникли проблемы, см. в разделе [Устранение переключений](#troubleshoot-swaps).

## <a name="monitor-a-swap"></a>Отслеживать операцию переключения

Если [операцию переключения](#AboutConfiguration) занимает много времени для завершения, можно получить сведения об операции замены в [журнал действий](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

На странице ресурсов приложения на портале в области слева выберите **журнал действий**.

Операция переключения отображается в запросах журнала в виде `Swap Web App Slots`. Вы можете развернуть этот элемент и выбрать вложенные операции или ошибки, чтобы увидеть подробные сведения о них.

## <a name="route-traffic"></a>Маршрутизация трафика

По умолчанию все клиентские запросы, поступающие на URL-адрес приложения (`http://<app_name>.azurewebsites.net`), направляются в рабочий слот. Вы можете перенаправить часть этого трафика в другой слот. Это очень удобно, когда вам нужно собрать отзывы пользователей о новой версии, но вы пока не готовы публиковать ее в рабочей среде.

### <a name="route-production-traffic-automatically"></a>Автоматическая маршрутизация рабочего трафика

Чтобы автоматически Маршрутизация рабочего трафика:

1. Перейдите на страницу ресурсов приложения и выберите **слоты развертывания**.

2. В столбце **Процент трафика** для слота, в который нужно направить часть трафика, укажите значение в процентах (от 0 до 100) от общего объема распределяемого трафика. Щелкните **Сохранить**.

    ![Параметр процент трафика](./media/web-sites-staged-publishing/RouteTraffic.png)

После сохранения параметра указанный процент клиентов случайным образом перенаправляться в нерабочий слот. 

После клиент автоматически направляется в конкретный слот, его «закрепленные» в этом слоте в течение данного клиентского сеанса. Вы можете проверить, к какому слоту прикреплен клиент, просмотрев значение параметра cookie `x-ms-routing-name` в заголовках HTTP средствами браузера. Если запросы направляются в промежуточный слот, этот параметр имеет значение `x-ms-routing-name=staging`. Если запросы направляются в рабочий слот, этот параметр имеет значение `x-ms-routing-name=self`.

### <a name="route-production-traffic-manually"></a>Маршрутизация рабочего трафика вручную

Кроме автоматической маршрутизации трафика, Служба приложений поддерживает направление запросов в конкретные слоты. Это полезно в том случае, если вы хотите, чтобы пользователи могли согласиться или отказаться от бета-версии приложения. Для маршрутизации рабочего трафика вручную применяется параметр запроса `x-ms-routing-name`.

Чтобы пользователи могли отказаться бета-версии приложения, например, можно поместить эту ссылку на веб-страницу:

```HTML
<a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>
```

Строка `x-ms-routing-name=self` определяет рабочий слот. После браузер клиента получает доступ к связи, он перенаправляется в рабочий слот. Каждый последующий запрос имеет `x-ms-routing-name=self` файл cookie, который связывает данный сеанс в рабочий слот.

Чтобы пользователи могли согласиться на бета-версию приложения, присвойте имя нерабочей области один параметр запроса. Ниже приведен пример:

```
<webappname>.azurewebsites.net/?x-ms-routing-name=staging
```

По умолчанию новый слотов, получают правила маршрутизации из `0%`, которые отображаются серым цветом. Если вы явно присвоить это значение `0%` (показано черным шрифтом), пользователи смогут работать промежуточный слот вручную с помощью `x-ms-routing-name` параметр запроса. Но они не будет перенаправлен в слоте автоматически так, как процент маршрутизации имеет значение 0. Это расширенный сценарий, где можно «скрыть» промежуточного слота из общедоступной предоставляя внутренних групп проверить изменения в слоте.

<a name="Delete"></a>

## <a name="delete-a-slot"></a>Удаление слота

Перейдите на страницу ресурсов приложения. Выберите **слоты развертывания** >  *\<слот, чтобы удалить >*  > **Обзор**. Выберите **удалить** на панели команд.  

![Удаление слота развертывания](./media/web-sites-staged-publishing/DeleteStagingSiteButton.png)

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-powershell"></a>Автоматизация с помощью PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell — это модуль, который предоставляет командлеты для управления Azure с помощью Windows PowerShell, а также поддерживает управление слотами развертывания в службе приложений Azure.

Сведения об установке и настройке Azure PowerShell и об аутентификации Azure PowerShell с использованием подписки Azure см. в разделе [Установка и настройка Microsoft Azure PowerShell](/powershell/azure/overview).  

---
### <a name="create-a-web-app"></a>Создание веб-приложения
```powershell
New-AzWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

---
### <a name="create-a-slot"></a>Создайте слот
```powershell
New-AzWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

---
### <a name="initiate-a-swap-with-a-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-the-source-slot"></a>Запуск переключения с предварительным просмотром (многофазное переключение) и применение конфигурации целевого слота к исходному слоту
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

---
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-the-source-slot-configuration"></a>Отмена промежуточного переключения (переключения с предварительным просмотром) и восстановление конфигурации исходного слота
```powershell
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

---
### <a name="swap-deployment-slots"></a>Переключение слотов развертывания
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Монитор замены события в журнале действий
```powershell
Get-AzLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

---
### <a name="delete-a-slot"></a>Удаление слота
```powershell
Remove-AzResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

---
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-the-cli"></a>Автоматизация с помощью интерфейса командной строки

Чтобы ознакомиться с командами [Azure CLI](https://github.com/Azure/azure-cli) для слотов развертывания, изучите описание команды [az webapp deployment slot](/cli/azure/webapp/deployment/slot).

## <a name="troubleshoot-swaps"></a>Устранение неполадок переключений

При возникновении любой ошибки во время [переключения слотов](#AboutConfiguration), регистрируется в *D:\home\LogFiles\eventlog.xml*. Это действие также регистрируется в журнале ошибок конкретного приложения.

Ниже приведены некоторые распространенные ошибки замены.

- Истекло время ожидания HTTP-запроса к корню приложения. Операция переключения ожидает в течение 90 секунд для каждого HTTP-запроса и повторных попыток до 5 раз. Если истекло всех повторных попыток операция замены будет остановлено.

- Инициализация локального кэша может завершиться ошибкой, если содержимое приложения превышает квоту локальный диск, указанный для локального кэша. Дополнительные сведения см. в разделе [Общие сведения о локальном кэше](overview-local-cache.md).

- Во время [пользовательский прогрев](#Warm-up), HTTP-запросы выполняются внутренним образом (минуя внешний URL-адрес). Могут возникнуть сбои с определенные правила подстановки URL-адрес в *Web.config*. Например правила для перенаправления доменных имен или применение HTTPS можно предотвратить прогрева запросов достижения кода приложения. Чтобы обойти эту проблему, измените правила переопределения, добавив следующие два условия:

    ```xml
    <conditions>
      <add input="{WARMUP_REQUEST}" pattern="1" negate="true" />
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Без пользовательский прогрев правила подстановки URL-адрес по-прежнему могут блокировать запросы HTTP. Чтобы обойти эту проблему, измените правила переопределения, добавив следующее условие:

    ```xml
    <conditions>
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Некоторые [правила ограничения IP-адресов](app-service-ip-restrictions.md) может предотвращается swap с отправки HTTP-запросов к приложению. Диапазоны адресов IPv4, начинающихся с `10.` и `100.` являются внутренними для развертывания. Вы должны разрешить им подключиться к приложению.

## <a name="next-steps"></a>Дальнейшие действия
[Ограничения статических IP-адресов в службе приложений Azure](app-service-ip-restrictions.md)
