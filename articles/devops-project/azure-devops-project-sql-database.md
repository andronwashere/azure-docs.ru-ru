---
title: Руководство. Развертывание кода базы данных SQL Azure и приложения ASP.NET с помощью Azure DevOps Projects
description: Служба DevOps Projects позволяет быстро приступить к работе с Azure. С помощью DevOps Projects можно развернуть код приложения ASP.NET, использующего Базу данных SQL Azure, выполнив несколько простых действий.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 0d05a2f3de92791572f0a5e6313777b5388af3df
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "57845228"
---
# <a name="tutorial-deploy-your-aspnet-app-and-azure-sql-database-code-by-using-azure-devops-projects"></a>Руководство. Развертывание кода базы данных SQL Azure и приложения ASP.NET с помощью Azure DevOps Projects

Azure DevOps Projects — это упрощенный интерфейс, который позволяет добавить существующий код и репозиторий Git либо выбрать пример приложения, чтобы создать конвейер непрерывной интеграции (CI) и непрерывной поставки (CD) в Azure. 

DevOps Projects также позволяет:
* автоматически создавать ресурсы Azure, например базу данных SQL Azure;
* создавать и настраивать конвейер выпуска в Azure Pipelines, который включает конвейер сборки для непрерывной интеграции;
* настраивать конвейер выпуска для непрерывного развертывания; 
* создавать ресурс Azure Application Insights для мониторинга.

Изучив данный учебник, вы научитесь:

> [!div class="checklist"]
> * применять службу Azure DevOps Projects для развертывания кода приложения ASP.NET, использующего Базу данных SQL Azure;
> * настройка Azure DevOps и подписки Azure; 
> * Просмотр конвейера CI
> * Просмотр конвейера CD
> * Фиксация изменений в Azure Repos и их автоматическое развертывание в Azure
> * подключаться к базе данных SQL Azure; 
> * Очистка ресурсов

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Вы можете получить бесплатную подписку с помощью [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="create-a-project-in-devops-projects-for-an-aspnet-app-and-an-azure-sql-database"></a>Создание проекта в DevOps Projects для приложения ASP.NET и базы данных SQL Azure

DevOps Projects создает конвейер CI/CD в Azure Pipelines. Вы можете создать новую организацию Azure DevOps или использовать существующую. Служба DevOps Projects также создает ресурсы Azure, например базу данных SQL, в подписке Azure по вашему выбору.

1. Войдите на [портале Azure](https://portal.azure.com).

1. В области слева выберите **Создать ресурс**.

1. В поле поиска введите **DevOps Projects**, а затем выберите **Создать**.

    ![Панель мониторинга DevOps Projects](_img/azure-devops-project-github/fullbrowser.png)

1. Выберите **.NET**, а затем **Далее**.

1. Для параметра **Выберите платформу приложений** выберите **ASP.NET**.

1. Выберите **Добавление базы данных** и нажмите кнопку **Далее**.  
    Платформа приложений, выбранная ранее, определяет доступный тип целевого объекта развертывания службы Azure. 
    
1. Щелкните **Далее**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>настройка Azure DevOps и подписки Azure;

1. Создайте новую организацию Azure DevOps или выберите существующую. 

1. Введите имя проекта Azure DevOps. 

1. Выберите службы подписки Azure.  
    При необходимости можно нажать кнопку **Изменить**, чтобы просмотреть дополнительные параметры конфигурации Azure и найти имя пользователя в разделе **Учетные данные для сервера базы данных**. Сохраните имя пользователя для следующих шагов этого руководства. Если вы выполняете этот дополнительных шаг, прежде чем нажать кнопку **Готово**, закройте область конфигурации Azure.
 
1. Нажмите кнопку **Готово**.  
    Через несколько минут процесс будет завершен и на портале Azure откроется панель мониторинга DevOps Projects. К панели мониторинга также можно перейти непосредственно из представления **Все ресурсы** на портале Azure. В области справа щелкните **Просмотр**, чтобы просмотреть запущенное приложение.
    
## <a name="examine-the-ci-pipeline"></a>Просмотр конвейера CI

Служба DevOps Projects автоматически настраивает весь конвейер CI/CD в Azure Repos. Вы можете изучить и настроить конвейер. Чтобы изучить конвейер сборки Azure DevOps, выполните следующие действия:

1. В верхней части панели мониторинга DevOps Projects выберите **Конвейеры сборки**.  
    На вкладке браузера отобразится конвейер сборки нового проекта.

1. Выберите поле **Состояние** и щелкните значок многоточия (…).  
    В появившемся меню содержится несколько пунктов, таких как постановка новой сборки в очередь, приостановка сборки и изменение конвейера сборки.

1. Выберите **Изменить**

1. В этой области можно просмотреть различные задачи для конвейера сборки.  
    В процессе сборки выполняются различные задачи, такие как получение исходного кода из репозитория Git, восстановление зависимостей и публикация полученных файлов, используемых для развертывания.

1. Выберите имя конвейера сборки в верхней части соответствующей области.

1. Замените имя конвейера сборки более понятным для себя, выберите **Сохранить и поместить в очередь** и нажмите кнопку **Сохранить**.

1. Под именем конвейера сборки щелкните **Журнал**.  
    В этой области отображается журнал аудита последних изменений сборки. Azure Pipelines отслеживает любые изменения, внесенные в конвейер сборки, и позволяет сравнивать версии.

1. Выберите **Триггеры**.  
    DevOps Projects автоматически создает триггер CI, а при каждой фиксации в репозитории запускается новая сборка. При необходимости выберите для процесса непрерывной интеграции включение или исключение ветвей.

1. Щелкните **Период удержания**.  
    В зависимости от сценария можно указать политики для хранения или удаления определенного количества сборок.

## <a name="examine-the-cd-pipeline"></a>Просмотр конвейера CD

DevOps Projects автоматически создает и настраивает необходимые шаги для развертывания из вашей организации Azure DevOps в подписке Azure. Эти шаги включают настройку подключения к службе Azure для аутентификации Azure DevOps в подписке Azure. Средства автоматизации также создают конвейер CD, который обеспечивает непрерывное развертывание на виртуальной машине Azure. Чтобы получить дополнительные сведения о конвейере непрерывного развертывания в Azure DevOps, выполните следующие действия:

1. Последовательно выберите **Сборка и выпуск** и **Выпуски**.  
    DevOps Projects создаст конвейер выпуска для управления развертываниями в Azure.

1. Щелкните многоточие (…) рядом с конвейером выпуска и выберите **Изменить**.  
    Конвейер выпуска содержит *конвейер*, в котором определен процесс выпуска.

1. В разделе **Артефакты** выберите **Удалить**.  
    Конвейер сборки, который вы изучали на предыдущих этапах, создаст выходные данные для артефакта. 

1. Справа от значка **Удалить** выберите **Триггер непрерывного развертывания**.  
    В этом конвейере выпуска активирован триггер непрерывного развертывания, выполняющий развертывание каждый раз, когда становится доступным новый артефакт сборки. При необходимости можно отключить триггер, чтобы выполнять развертывание вручную. 

    Служба DevOps Projects настраивает случайный пароль SQL и использует его в конвейере выпуска.
    
1. В области слева выберите **Переменные**. 

   > [!NOTE]
   > Указанное ниже действие нужно выполнить, только если вы изменили пароль SQL Server. Для пароля используется одна переменная.
  
1. Рядом с полем **Значение** нажмите значок с замком, введите новый пароль и щелкните **Сохранить**.

1. В области слева выберите **Задачи**, а затем выберите свою среду.  
    Задачи — это действия, которые выполняются в процессе развертывания. Эти действия объединены в этапы. Для конвейера выпуска предусмотрен один этап, который состоит из задач *развертывания Службы приложений Azure* и *развертывания Базы данных SQL Azure*.

1. Выберите задачу *Execute Azure SQL* (Выполнить SQL Azure) и изучите различные свойства, используемые для развертывания SQL.  
    В разделе **Пакет развертывания** задача использует файл *SQL DACPAC*.

1. В области справа выберите **Просмотреть выпуски**, чтобы отобразился журнал выпусков.

1. Щелкните многоточие (…) рядом с одним из выпусков и выберите **Открыть**.  
     Отобразится меню с несколькими элементами, такими как сводка выпуска, связанные рабочие элементы и тесты.

1. Щелкните **Фиксации**.  
     В этом представлении отображаются фиксации кода, связанные с текущим развертыванием. Сравните выпуски, чтобы просмотреть различия между развертываниями.

1. Выберите **Журналы**.  
     Журналы содержат полезную информацию о процессе развертывания. Их можно просматривать во время и после развертывания.

## <a name="commit-changes-to-azure-repos-and-automatically-deploy-them-to-azure"></a>Фиксация изменений в Azure Repos и их автоматическое развертывание в Azure 

 > [!NOTE]
 > Ниже описан процесс проверки конвейера CI/CD путем простого изменения текста. Чтобы проверить процесс развертывания SQL, дополнительно можно изменить схему SQL Server в таблице.

Теперь вы готовы к совместной работе над приложением с использованием процесса CI/CD, который позволяет автоматически развернуть последние результаты работы на веб-сайте. При каждом изменении в репозитории Git запускается сборка в Azure DevOps и конвейер CD выполняет развертывание в Azure. Используйте описанную в этом разделе процедуру или другой метод, чтобы зафиксировать изменения в репозитории. Изменение кода запускает процесс CI/CD, который автоматически развертывает изменения в Azure.

1. В области слева щелкните **Код** и откройте репозиторий.

1. Откройте каталог *SampleWebApplication\Views\Home*, нажмите значок многоточия (…) рядом с файлом *Index.cshtml*, а затем нажмите кнопку **Правка**. 

1. Внесите изменения в файл, например, измените текст внутри одного из тегов div. 

1. В правом верхнем углу выберите **Зафиксировать**, после чего снова нажмите **Зафиксировать**, чтобы отправить изменения.  
    Через несколько секунд начнется процесс сборки в Azure DevOps, после чего будет выполнена операция выпуска для развертывания изменений. Вы можете отслеживать состояние сборки на панели мониторинга DevOps Projects или в браузере в своей организации Azure DevOps.

1. По завершении выпуска обновите приложение, чтобы проверить изменения.

## <a name="connect-to-the-azure-sql-database"></a>Подключение к базе данных SQL Azure

Для подключения к базе данных SQL Azure вам нужны соответствующие разрешения.

1. На панели мониторинга DevOps Projects выберите **База данных SQL**, чтобы открыть панель управления базой данных SQL.
   
1. Выберите **Настройка брандмауэра для сервера**, а затем нажмите кнопку **Добавить IP-адрес клиента**. 

1. Щелкните **Сохранить**.  
    Вашему IP-адресу клиента теперь разрешен доступ к ресурсу SQL Server в Azure.

1. Вернитесь к панели **База данных SQL**. 

1. В области справа выберите имя сервера, чтобы открыть страницу конфигурации **SQL Server**.

1. Выберите **Сброс пароля**, введите пароль для входа администратора SQL Server, а затем щелкните **Сохранить**.  
    Запишите этот пароль, чтобы использовать его позднее в этом руководстве.

    Теперь при необходимости можно использовать клиентские средства, например SQL Server Management Studio или Visual Studio для подключения к SQL Server и базе данных Azure. Используйте свойство **Имя сервера** для подключения.

    Если при начальной настройке проекта в DevOps Projects вы не меняли имя пользователя для базы данных, в качестве имени пользователя будет использоваться префикс имени в вашем адресе электронной почты. Например, если ваш адрес электронной почты — *johndoe\@microsoft.com*, ваше имя пользователя — *johndoe*.

   > [!NOTE]
   > Если вы изменили пароль для входа на сервер SQL, необходимо изменить этот пароль в переменной конвейера выпуска, как описано в разделе "Просмотр конвейера CD".

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы выполняете тестирование, можно очистить ресурсы, чтобы избежать дополнительных расходов. Можно удалить базу данных SQL Azure и связанные с ней ресурсы, созданные в рамках этого руководства, когда в них отпадет необходимость. Для этого воспользуйтесь функцией **Удалить** на панели мониторинга DevOps Projects.

> [!IMPORTANT]
> Следующая процедура безвозвратно удаляет ресурсы. Функция *Удалить* безвозвратно удаляет в Azure и в Azure DevOps данные, созданные в проекте DevOps Projects. Выполняйте эту процедуру только после того, как прочли предупреждение.

1. На портале Azure перейдите к панели мониторинга DevOps Projects.
2. В правом верхнем углу выберите **Удалить**. 
3. В окне предупреждения выберите **Да**, чтобы *удалить ресурсы без возможности восстановления*.

## <a name="next-steps"></a>Дополнительная информация

Вы можете по желанию изменить эти конвейеры сборки и выпуска в соответствии с потребностями вашей команды. Вы также можете использовать этот шаблон CI/CD в качестве шаблона для других конвейеров. Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * применять службу Azure DevOps Projects для развертывания кода приложения ASP.NET, использующего Базу данных SQL Azure;
> * настройка Azure DevOps и подписки Azure; 
> * Просмотр конвейера CI
> * Просмотр конвейера CD
> * Фиксация изменений в Azure Repos и их автоматическое развертывание в Azure
> * подключаться к базе данных SQL Azure; 
> * Очистка ресурсов

См. дополнительные сведения о конвейере CI/CD:

> [!div class="nextstepaction"]
> [Define your multi-stage continuous deployment (CD) pipeline](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts) (Настройка многоэтапного конвейера для непрерывного развертывания)

## <a name="videos"></a>Видеоролики

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3308/player]
