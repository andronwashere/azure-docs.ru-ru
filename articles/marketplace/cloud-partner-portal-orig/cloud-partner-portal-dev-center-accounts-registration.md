---
title: Создание учетной записи разработчика Майкрософт | Azure Marketplace
description: Требования и шаги по созданию учетной записи разработчика Майкрософт.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 4fde5d81fb97bec23fdb46ff53b05874c88d9d67
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935867"
---
<a name="create-a-microsoft-developer-account"></a>Создание учетной записи разработчика Майкрософт
====================================

В этой статье описывается, как стать утвержденным разработчиком Майкрософт для публикации в Azure Marketplace.

## <a name="create-a-microsoft-account"></a>Создание учетной записи Майкрософт

Чтобы начать процесс публикации, нужно выполнить регистрацию в **центре разработчиков Microsoft**. Эта же учетная запись используется на **[Портале Cloud Partner](https://cloudpartner.azure.com/)** для запуска процесса публикации.

### <a name="general-account-guidelines"></a>Общие рекомендации по учетным записям

Мы рекомендуем использовать только одну учетную запись Майкрософт для всех предложений в Azure Marketplace. Эта учетная запись не должна быть привязана к конкретным службам или предложениям.

Адрес, который образует имя пользователя, должен находиться в вашем домене и им должны управлять ваши ИТ-специалисты. Все действия, связанные с публикацией, должны выполняться через эту учетную запись.

>[!WARNING]
>При регистрации учетной записи Майкрософт нельзя использовать слова Azure, Microsoft и т. п. Не используйте эти слова при создании и регистрации учетной записи.

### <a name="company-account-guidelines"></a>Рекомендации по корпоративным учетным записям

Соблюдайте эти рекомендации, если доступ к учетной записи будут осуществлять несколько человек, используя создавшую ее учетную запись Майкрософт.

>[!IMPORTANT]
>Чтобы вашу учетную запись Центра разработчиков могли использовать несколько пользователей, назначьте роли отдельным пользователям с помощью Azure Active Directory. Тогда они смогут получать доступ к этой учетной записи, используя для входа собственные учетные данные Azure AD. Дополнительные сведения см. в статье [об управлении пользователями учетных записей](https://docs.microsoft.com/windows/uwp/publish/manage-account-users).

-   При создании учетной записи Майкрософт используйте адрес электронной почты с доменом вашей компании, а не личный адрес. Например, windowsapps\@fabrikam.com.
-   Доступ к этой учетной записи Майкрософт следует предоставлять как можно меньшему количеству разработчиков.
-   Создайте корпоративный список рассылки, включив в него адреса электронной почты всех пользователей, которым требуется доступ к учетной записи разработчика. Добавьте адрес этого списка рассылки в раздел сведений о безопасности. Тогда все сотрудники, включенные в список, смогут по мере необходимости получать коды безопасности и управлять сведениями о безопасности для учетной записи Майкрософт. Если список рассылки создать невозможно, владелец персонального адреса электронной почты должен быть всегда доступен для связи и готов предоставить полученный код безопасности (например, при добавлении новых сведений о безопасности для учетной записи или при входе в учетную запись с нового устройства).
-   Предоставьте рабочий номер телефона (без добавочных номеров), который доступен для ключевых членов вашей команды.
-   Обычно следует разрешать доступ разработчиков к учетной записи разработчика только с доверенных устройств. Доступ к этим доверенным устройствам должен быть у всех ключевых членов команды. Это снизит потребность в получении кодов безопасности для доступа к учетной записи.
-   Если вам необходимо разрешить доступ к учетной записи с компьютера, не входящего в число доверенных, такой доступ следует предоставлять не более чем пяти разработчикам. Желательно, чтобы все компьютеры, с которых разработчики обращаются к учетной записи, находились в одном географическом и сетевом расположении.
-   Регулярно проверяйте [сведения о безопасности для вашей компании](https://account.live.com/proofs/Manage) и поддерживайте их актуальность.

>[!IMPORTANT]
>Доступ к учетной записи разработчика должен осуществляться в основном с доверенных компьютеров. Это критически важно, поскольку существует ограничение на число кодов для одной учетной записи, создаваемых в течение недели. Кроме того, это повышает удобство входа.
>
>Дополнительные рекомендации и информацию о мерах безопасности для учетной записи разработчика вы найдете [здесь](https://msdn.microsoft.com/windows/uwp/publish/opening-a-developer-account#additional-guidelines-for-company-accounts).

### <a name="to-create-a-microsoft-account"></a>Создание учетной записи Майкрософт

1.  Откройте новое окно в режиме инкогнито в браузере Chrome или запустите режим InPrivate в браузере Internet Explorer, чтобы не войти в существующую учетную запись.
2.  Зарегистрируйте адрес электронной почты (с учетом рекомендаций выше) как учетную запись Майкрософт, перейдя по [этой ссылке](https://signup.live.com/signup.aspx). Выполните следующие инструкции по регистрации.

    - При регистрации своей учетной записи в качестве учетной записи Майкрософт укажите действующий номер телефона, чтобы система могла отправить вам код проверки учетной записи в форме текстового сообщения или автоматизированного звонка.
    - При регистрации своей учетной записи в качестве учетной записи Майкрософт укажите действительный идентификатор электронной почты, чтобы получить автоматическое сообщение для проверки этой учетной записи.
    - Проверьте адрес электронной почты списка рассылки.

    Теперь вы готовы использовать новую учетную запись Майкрософт в Центре разработчиков.

## <a name="register-your-account-in-microsoft-developer-center"></a>Регистрация учетной записи в Центре разработчиков Майкрософт

Центр разработчиков Майкрософт используется для однократной регистрации сведений о компании. Пользователь, выполняющий регистрацию, должен быть действующим представителем компании и должен предоставить личные сведения для проверки своей личности. Пользователь, выполняющий регистрацию, должен использовать учетную запись Майкрософт, которая является общей для организации. **Эту же учетную запись нужно использовать и на облачном портале для партнеров.** Перед созданием учетной записи в Центра разработчиков Майкрософт вам нужно проверить, нет ли в компании существующей учетной записи. В ходе создания учетной записи мы соберем сведения об адресе компании, банковском счете и налогах. Обычно эти сведения собираются у контактных лиц по финансам и бизнесу.

>[!IMPORTANT]
>Для прохождения различных этапов создания и развертывания предложения необходимы следующие компоненты профиля разработчика.

| Профиль разработчика     | Запуск черновой версии    | Промежуточная       | Бесплатная публикация и шаблон решения   | Коммерческая публикация   |
|---------------------- |----------------   |-----------    |-------------------------------------  |---------------------  |
| Регистрация компании  | Необходимо         | Необходимо     | Необходимо                             | Необходимо             |
| Идентификатор налогового профиля        | Необязательно          | Необязательно      | Необязательно                              | Необходимо             |
| Банковский счет          | Необязательно          | Необязательно      | Необязательно                              | Необходимо             |

>[!NOTE]
>Использование собственной лицензии (BYOL) поддерживается только для виртуальных машин. Это предложение считается бесплатным.

### <a name="register-your-company-account"></a>Регистрация учетной записи организации

1. Откройте новое окно в режиме инкогнито в браузере Chrome или запустите режим InPrivate в браузере Internet Explorer, чтобы не входить в свою учетную запись.

2. Чтобы зарегистрироваться в качестве продавца, перейдите в [Центр разработчиков для Windows](https://dev.windows.com/registration?accountprogram=azure). Прежде чем продолжить, прочитайте это важное примечание.

   ![Проверка учетной записи Microsoft](./media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-verify.jpg)

    >[!IMPORTANT]
    >Убедитесь, что идентификатор электронной почты или список рассылки (рекомендуется во избежание зависимости от одного пользователя), который будет использоваться для регистрации в центре разработки, предварительно зарегистрирован как учетная запись Майкрософт. Если он еще не зарегистрирован, то воспользуйтесь этой ссылкой. Учитывайте также, что адреса электронной почты из корпоративного домена Майкрософт нельзя использовать для регистрации в центре разработчиков.

   ![Вход в Центр разработки](./media/cloud-partner-portal-create-dev-center-registration/seller-dashboard-login.jpg)

3. Запустите мастер "Help us protect your account" (Помогите нам защитить вашу учетную запись), который подтвердит вашу личность с помощью номера телефона или адреса электронной почты.

4. В разделе "Информация о регистрации учетной записи" в раскрывающемся меню выберите **страну или регион учетной записи**, а затем щелкните **Далее**.

   ![Выбор страны или региона](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_04.png)

    >[!WARNING]
    >«Продажа — от» странах и регионах: Для продажи своих услуг в Azure Marketplace, ваше зарегистрированное юридическое должно быть в одной из утвержденных «Продажа — от» страны (регионы) в раскрывающемся списке. Это ограничение связано с выплатами и налогообложением. Дополнительные сведения см. в описании политик участия в Marketplace.

5. Для параметра "Тип учетной записи" выберите значение **Компания** и щелкните **Далее**.

    >[!IMPORTANT]
    >Чтобы лучше разобраться в типах учетных записей и выбрать из них наиболее подходящий, изучите страницу "Типы, доступность и стоимость учетных записей", представленную на следующем снимке экрана.

    ![Типы учетных записей для продавцов](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_05.png)

6. Введите **Отображаемое имя издателя**. Обычно это название вашей компании.

    >[!NOTE]
    >Отображаемое имя издателя, указанное в Центре разработчиков, не будет отображаться в Azure Marketplace после публикации предложения. Но эти сведения являются обязательными для завершения процесса регистрации.

7. Введите **контактные данные** для проверки учетной записи.

    >[!IMPORTANT]
    >Указывайте точные контактные сведения, так как они будут использоваться в процессе проверки и утверждения вашей компании в Центре разработчика.

8. Введите контактные данные для **утверждающего лица в компании**. Утверждающее лицо в компании — это пользователь, который может проверить, имеете ли вы право создавать учетные записи в центре разработки от имени своей организации. Завершив ввод данных, щелкните **Далее** для перехода к **Разделу оплаты**.

    ![Определение утверждающего лица в компании](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_08.png)

9. Введите платежные данные для своей учетной записи. Если у вас есть код акции, который влияет на стоимость регистрации, вы можете указать его здесь. В противном случае укажите сведения о вашей кредитной карте (или адрес PayPal на поддерживаемых рынках). Щелкните **Далее**, чтобы перейти к последнему шагу **Проверка**.

   ![Регистрация платежных данных](./media/cloud-partner-portal-create-dev-center-registration/imgRegisterCo_09.png)

10. Проверьте данные учетной записи и убедитесь, что все правильно. Прочитайте и примите условия [соглашения с издателем Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/?LinkID=699560). Установите флажок, который подтверждает, что вы ознакомились с этими условиями и принимаете их.

11. Нажмите кнопку **Готово**, чтобы подтвердить регистрацию. На ваш адрес электронной почты будет отправлено подтверждение.

12. Если вы планируете публиковать только бесплатные предложения, выберите действие [Перейти на Портал Cloud Partner](https://cloudpartner.azure.com/) и пропустите раздел "Регистрация учетной записи на Портале Cloud Partner" в этой статье.

### <a name="commercial-offers"></a>Коммерческие предложения

Если вы планируете публиковать коммерческие предложения, например виртуальные машины с почасовой моделью выставления счетов, необходимо предоставить налоговую и банковскую информацию. Для этого войдите в учетную запись центра разработчиков и выберите действие **Обновите сведения об учетной записи**. Следуйте инструкциям в следующем разделе "Добавление банковских и налоговых сведений".

>[!IMPORTANT]
>Вы не сможете опубликовать коммерческое предложение без предоставления банковских и налоговых сведений.

Если вы предпочитаете ввести банковские и налоговые сведения позднее, можно пока пропустить раздел "Регистрация учетной записи на Портале Cloud Partner" в этой статье.

>[!NOTE]
>Мы рекомендуем предоставить сведения о банковском счете и налоговом учете как можно скорее, поскольку их проверка может занять некоторое время.

### <a name="add-banking-and-tax-information"></a>Добавление банковских и налоговых сведений

Чтобы публиковать коммерческие предложения для продажи, вам нужно добавить сведения о выплатах и налогах в Центре разработчиков и передать их на проверку.

**Предоставление банковских сведений**

1.  Войдите в [Центр разработчиков Майкрософт](https://dev.windows.com/registration?accountprogram=azure) , используя свою учетную запись Майкрософт.
2.  Выберите **Payout account** (Счет для выплаты) в меню слева, а затем в разделе **Choose payment method** (Выбрать метод оплаты) щелкните **Bank account** (Банковский счет) или **PayPal**.

    >[!NOTE]
    >Если у вас есть коммерческие предложения, которые клиенты приобретают в Marketplace, на этот счет вы будете получать выплаты за эти покупки.
3.  Введите сведения об оплате и нажмите кнопку **Сохранить**.

    >[!IMPORTANT]
    >Если вам потребуется изменить счет для выплат, выполните описанные выше действия и обновите сведения.
    >
    >Изменение счета для выплат может привести к задержке выплат на один цикл. Эта задержка связана с необходимостью проверки измененного счета, которая обычно выполняется при добавлении первого счета для выплат. После проверки вам будет выплачена полная сумма. Все выплаты, не отправленные в текущем периоде, переносятся на следующий.

4.  Щелкните **Далее**.

**Предоставление налоговых сведений**

1.  Войдите в [Центр разработчиков Майкрософт](https://dev.windows.com/registration?accountprogram=azure) , используя свою учетную запись Майкрософт (если требуется).
2.  В меню слева щелкните **Tax profile** (Налоговый профиль).
3.  На странице **Сведения о налогах** выполните следующее.
    - Выберите страну или регион постоянного проживания.
    - Выберите страну или регион вашего основного гражданства.
    - Щелкните **Далее**.
4.  Введите сведения о налогах и нажмите кнопку **Далее**.

>[!WARNING]
>Вы не сможете размещать коммерческие предложения, пока не предоставите банковские и налоговые сведения в учетной записи Центра разработчиков Майкрософт.

### <a name="developer-center-registration-issues"></a>Проблемы с регистрацией в Центре разработчиков

При возникновении проблем с регистрацией в Центре разработчиков отправьте запрос в службу поддержки, выполнив следующую процедуру.

1.  Перейдите по [ссылке на службу поддержки](https://developer.microsoft.com/windows/support).
2.  В разделе **Обратная связь** нажмите кнопку **Submit an incident** (Отправить сведения об инциденте).
    ![Запрос в службу поддержки](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_02.png)
3.  В поле **Тип проблемы** выберите Help with Dev Center (Помощь с центром разработчиков), а в поле **Категория** — Publish and manage apps (Публикация приложений и управление ими). Выберите **Start email** (Создать сообщение).

    ![Выбор типа проблемы](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_03.png)

4.  Откроется страница для входа. Выполните вход с любой учетной записью Майкрософт. Если у вас нет учетной записи Майкрософт, [создайте ее](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).

5.  Предоставьте подробные сведения о проблеме и щелкните **Отправить**, чтобы создать запрос.

    ![Отправка запроса в службу поддержки](./media/cloud-partner-portal-create-dev-center-registration/imgAddTax_05.png)

## <a name="register-your-account-in-the-cloud-partner-portal"></a>Регистрация учетной записи на облачном портале для партнеров

[Портал Cloud Partner](https://cloudpartner.azure.com/) используется для публикации предложений и управления ими.

1.  Откройте новое окно в режиме инкогнито в браузере Chrome или запустите режим InPrivate в браузере Internet Explorer, чтобы не использовать личную учетную запись.
2.  Перейдите на [облачный портал для партнеров](https://cloudpartner.azure.com/).
3.  Если вы являетесь новым пользователем и впервые входите на [Портал Cloud Partner](https://cloudpartner.azure.com/), используйте для входа тот же адрес электронной почты, что и для регистрации учетной записи Центра разработки. Это позволит связать между собой учетные записи Центра разработки и Портала Cloud Partner.

Позже вы сможете добавить других сотрудников компании, работающих над этим приложением. Выполнив действия, описанные в следующем разделе, вы предоставите им права участников и (или) владельцев.

Если вы добавлены в качестве участника или владельца на облачный портал для партнеров, то можете войти, используя свою учетную запись.

>[!TIP]
>Политики участия описаны на веб-сайте Azure.

## <a name="manage-users-as-owners-or-contributors-in-the-cloud-partner-portal"></a>Управление владельцами и участниками на Портале Cloud Partner

[Меры по управлению пользователями на облачном портале для партнеров](./cloud-partner-portal-manage-users.md)


## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда ваша учетная запись создана и зарегистрирована, можно приступить к публикации в Azure Marketplace.
