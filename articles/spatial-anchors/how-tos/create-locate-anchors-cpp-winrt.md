---
title: Создание и поиск привязок с помощью Пространственных привязок Azure в C++ (WinRT) | Документация Майкрософт
description: В этой статье подробно рассказывается о том, как создавать и находить привязки с помощью Пространственных привязок Azure в C++ (WinRT).
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: e8cacb443a9fc13d742f84b92136be7428375fc6
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67669227"
---
# <a name="how-to-create-and-locate-anchors-using-azure-spatial-anchors-in-cwinrt"></a>Создание и поиск привязок с помощью Пространственных привязок Azure в C++ (WinRT)

> [!div  class="op_single_selector"]
> * [Unity](create-locate-anchors-unity.md)
> * [Objective-C](create-locate-anchors-objc.md)
> * [Swift](create-locate-anchors-swift.md)
> * [Android (Java)](create-locate-anchors-java.md)
> * [C++ (NDK)](create-locate-anchors-cpp-ndk.md)
> * [C++ (WinRT)](create-locate-anchors-cpp-winrt.md)

Пространственные привязки Azure позволяют вам совместно использовать привязки на различных устройствах. Служба поддерживает несколько сред разработки. Из этой статьи вы узнаете, как использовать пакет SDK Пространственных привязок Azure в C++/WinRT, чтобы выполнять следующие действия:

- правильно настраивать сеанс Пространственных привязок Azure и управлять им;
- создавать и настраивать свойства в локальных привязках;
- передавать их в облако;
- обнаруживать и удалять пространственные привязки в облаке.

## <a name="prerequisites"></a>Предварительные требования

Для работы с руководством вам потребуется следующее:

- Прочесть статью [Описание службы "Пространственные привязки Azure"](../overview.md).
- Выполнить одно из [5-минутных руководств](../index.yml).
- Базовые знания о C++ и <a href="https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt" target="_blank">API среды выполнения Windows</a>.

[!INCLUDE [Start](../../../includes/spatial-anchors-create-locate-anchors-start.md)]

Дополнительные сведения см. в статье о классе [CloudSpatialAnchorSession](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession).

```cpp
    SpatialAnchorsFactory m_asafactory{ nullptr };
    CloudSpatialAnchorSession m_cloudSession{ nullptr };
    winrt::com_ptr<::IUnknown> unk;
    winrt::check_hresult(ASACreateFactory(unk.put()));
    m_asafactory = unk.as<SpatialAnchorsFactory>();
    m_cloudSession = m_asafactory.CreateCloudSpatialAnchorSession();
```

[!INCLUDE [Account Keys](../../../includes/spatial-anchors-create-locate-anchors-account-keys.md)]

Дополнительные сведения см. в статье о классе [SessionConfiguration](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/sessionconfiguration).

```cpp
    auto configuration = m_cloudSession.Configuration();
    configuration.AccountKey(LR"(MyAccountKey)");
```

[!INCLUDE [Access Tokens](../../../includes/spatial-anchors-create-locate-anchors-access-tokens.md)]

```cpp
    auto configuration = m_cloudSession.Configuration();
    configuration.AccessToken(LR"(MyAccessToken)");
```

[!INCLUDE [Access Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-access-tokens-event.md)]

Дополнительные сведения см. в статье о делегате [TokenRequiredDelegate](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/tokenrequireddelegate).

```cpp
    m_accessTokenRequiredToken = m_cloudSession.TokenRequired(winrt::auto_revoke, [](auto&&, auto&& args) {
        args.AccessToken(LR"(MyAccessToken)");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```cpp
    m_accessTokenRequiredToken = m_cloudSession.TokenRequired(winrt::auto_revoke, [this](auto&&, auto&& args) {
        auto deferral = args.GetDeferral();
        MyGetTokenAsync([&deferral, &args](winrt::hstring const& myToken) {
            if (!myToken.empty()) args.AccessToken(myToken);
            deferral.Complete();
        });
    });
```

[!INCLUDE [Azure AD Tokens](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens.md)]

```cpp
    auto configuration = m_cloudSession.Configuration();
    configuration.AuthenticationToken(LR"(MyAuthenticationToken)");
```

[!INCLUDE [Azure AD Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens-event.md)]

```cpp
    m_accessTokenRequiredToken = m_cloudSession.TokenRequired(winrt::auto_revoke, [](auto&&, auto&& args) {
        args.AuthenticationToken(LR"(MyAuthenticationToken)");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```cpp
    m_accessTokenRequiredToken = m_cloudSession.TokenRequired(winrt::auto_revoke, [this](auto&&, auto&& args) {
        auto deferral = args.GetDeferral();
        MyGetTokenAsync([&deferral, &args](winrt::hstring const& myToken) {
            if (!myToken.empty()) args.AuthenticationToken(myToken);
            deferral.Complete();
        });
    });
```

[!INCLUDE [Setup](../../../includes/spatial-anchors-create-locate-anchors-setup-non-ios.md)]

Дополнительные сведения см. в статье о методе [Start](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#start).

```cpp
    m_cloudSession.Start();
```

[!INCLUDE [Frames](../../../includes/spatial-anchors-create-locate-anchors-frames.md)]

Дополнительные сведения см. в статье о методе [ProcessFrame](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession).

```cpp
    m_cloudSession->ProcessFrame(ar_frame_);
```

[!INCLUDE [Feedback](../../../includes/spatial-anchors-create-locate-anchors-feedback.md)]

Дополнительные сведения см. в статье о делегате [SessionUpdatedDelegate](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/sessionupdateddelegate).

```cpp
    m_sessionUpdatedToken = m_cloudSession.SessionUpdated(winrt::auto_revoke, [this](auto&&, auto&& args)
    {
        auto status = args.Status();
        if (status.UserFeedback() == SessionUserFeedback::None) return;
        m_feedback = LR"(Feedback: )" + FeedbackToString(status.UserFeedback()) + LR"( -)" +
            LR"( Recommend Create=)" + FormatPercent(status.RecommendedForCreateProgress() * 100.f);
    });
```

[!INCLUDE [Creating](../../../includes/spatial-anchors-create-locate-anchors-creating.md)]

Дополнительные сведения см. в статье о классе [CloudSpatialAnchor](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchor).

```cpp
    // Initialization
    SpatialStationaryFrameOfReference m_stationaryReferenceFrame = nullptr;
    HolographicDisplay defaultHolographicDisplay = HolographicDisplay::GetDefault();
    SpatialLocator spatialLocator = defaultHolographicDisplay.SpatialLocator();
    m_stationaryReferenceFrame = spatialLocator.CreateStationaryFrameOfReferenceAtCurrentLocation();

    // Create a local anchor, perhaps by positioning a SpatialAnchor a few meters in front of the user
    SpatialAnchor localAnchor{ nullptr };
    PerceptionTimestamp timestamp = PerceptionTimestampHelper::FromHistoricalTargetTime(DateTime::clock::now());
    SpatialCoordinateSystem currentCoordinateSystem = m_attachedReferenceFrame.GetStationaryCoordinateSystemAtTimestamp(timestamp);
    SpatialPointerPose pose = SpatialPointerPose::TryGetAtTimestamp(currentCoordinateSystem, timestamp);

    // Get the gaze direction relative to the given coordinate system.
    const float3 headPosition = pose.Head().Position();
    const float3 headDirection = pose.Head().ForwardDirection();

    // The anchor is positioned two meter(s) along the user's gaze direction.
    constexpr float distanceFromUser = 2.0f; // meters
    const float3 gazeAtTwoMeters = headPosition + (distanceFromUser * headDirection);

    localAnchor = SpatialAnchor::TryCreateRelativeTo(currentCoordinateSystem, gazeAtTwoMeters);

    // If the user is placing some application content in their environment,
    // you might show content at this anchor for a while, then save when
    // the user confirms placement.
    CloudSpatialAnchor cloudAnchor = m_asafactory.CreateCloudSpatialAnchor();
    cloudAnchor.LocalAnchor(localAnchor);
    co_await m_cloudSession.CreateAnchorAsync(cloudAnchor);
    m_feedback = LR"(Created a cloud anchor with ID=)" + cloudAnchor.Identifier();
```

[!INCLUDE [Session Status](../../../includes/spatial-anchors-create-locate-anchors-session-status.md)]

Дополнительные сведения см. в статье о методе [GetSessionStatusAsync](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#getsessionstatusasync).

```cpp
    SessionStatus status = co_await m_cloudSession.GetSessionStatusAsync();
    if (value.RecommendedForCreateProgress() < 1.0f) return;
    // Issue the creation request ...
```

[!INCLUDE [Setting Properties](../../../includes/spatial-anchors-create-locate-anchors-setting-properties.md)]

Дополнительные сведения см. в статье о методе [AppProperties](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchor#appproperties).

```cpp
    CloudSpatialAnchor cloudAnchor = m_asafactory.CreateCloudSpatialAnchor();
    cloudAnchor.LocalAnchor(localAnchor);
    auto properties = m_cloudAnchor.AppProperties();
    properties.Insert(LR"(model-type)", LR"(frame)");
    properties.Insert(LR"(label)", LR"(my latest picture)");
    co_await m_cloudSession.CreateAnchorAsync(cloudAnchor);
```

[!INCLUDE [Update Anchor Properties](../../../includes/spatial-anchors-create-locate-anchors-updating-properties.md)]

Дополнительные сведения см. в статье о методе [UpdateAnchorPropertiesAsync](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#updateanchorpropertiesasync).

```cpp
    CloudSpatialAnchor anchor = /* locate your anchor */;
    anchor.AppProperties().Insert(LR"(last-user-access)", LR"(just now)");
    co_await m_cloudSession.UpdateAnchorPropertiesAsync(anchor);
```

[!INCLUDE [Getting Properties](../../../includes/spatial-anchors-create-locate-anchors-getting-properties.md)]

Дополнительные сведения см. в статье о методе [GetAnchorPropertiesAsync](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#getanchorpropertiesasync).

```cpp
    CloudSpatialAnchor anchor = co_await m_cloudSession.GetAnchorPropertiesAsync(LR"(anchorId)");
    if (anchor != nullptr)
    {
        anchor.AppProperties().Insert(LR"(last-user-access)", LR"(just now)");
        co_await m_cloudSession.UpdateAnchorPropertiesAsync(anchor);
    }
```

[!INCLUDE [Expiration](../../../includes/spatial-anchors-create-locate-anchors-expiration.md)]

Дополнительные сведения см. в статье о методе [Expiration](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchor#expiration).

```cpp
    const int64_t oneWeekFromNowInHours = 7 * 24;
    const DateTime oneWeekFromNow = DateTime::clock::now() + std::chrono::hours(oneWeekFromNowInHours);
    cloudAnchor.Expiration(oneWeekFromNow);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating.md)]

Дополнительные сведения см. в статье о методе [CreateWatcher](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#createwatcher).

```cpp
    AnchorLocateCriteria criteria = m_asafactory.CreateAnchorLocateCriteria();
    criteria.Identifiers({ LR"(id1)", LR"(id2)", LR"(id3)" });
    auto cloudSpatialAnchorWatcher = m_cloudSession.CreateWatcher(criteria);
```

[!INCLUDE [Locate Events](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

Дополнительные сведения см. в статье о делегате [AnchorLocatedDelegate](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/anchorlocateddelegate).

```cpp
    m_anchorLocatedToken = m_cloudSession.AnchorLocated(winrt::auto_revoke, [this](auto&&, auto&& args)
    {
        switch (args.Status())
        {
            case LocateAnchorStatus::Located:
            {
                CloudSpatialAnchor foundAnchor = args.Anchor();
            }
                break;
            case LocateAnchorStatus::AlreadyTracked:
                // This anchor has already been reported and is being tracked
                break;
            case LocateAnchorStatus::NotLocatedAnchorDoesNotExist:
                // The anchor was deleted or never existed in the first place
                // Drop it, or show UI to ask user to anchor the content anew
                break;
            case LocateAnchorStatus::NotLocated:
                // The anchor hasn't been found given the location data
                // The user might in the wrong location, or maybe more data will help
                // Show UI to tell user to keep looking around
                break;
        }
    });
```

[!INCLUDE [Deleting](../../../includes/spatial-anchors-create-locate-anchors-deleting.md)]

Дополнительные сведения см. в статье о методе [DeleteAnchorAsync](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#deleteanchorasync).

```cpp
    co_await m_cloudSession.DeleteAnchorAsync(cloudAnchor);
    // Perform any processing you may want when delete finishes
```

[!INCLUDE [Stopping](../../../includes/spatial-anchors-create-locate-anchors-stopping.md)]

Дополнительные сведения см. в статье о методе [Stop](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#stop).

```cpp
    m_cloudSession.Stop();
```

[!INCLUDE [Resetting](../../../includes/spatial-anchors-create-locate-anchors-resetting.md)]

Дополнительные сведения см. в статье о методе [Reset](https://docs.microsoft.com/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#reset).

```cpp
    m_cloudSession.Reset();
```

[!INCLUDE [Cleanup](../../../includes/spatial-anchors-create-locate-anchors-cleanup-others.md)]

```cpp
    m_cloudSession = nullptr;
```

[!INCLUDE [Next Steps](../../../includes/spatial-anchors-create-locate-anchors-next-steps.md)]
