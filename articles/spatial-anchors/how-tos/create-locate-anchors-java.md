---
title: Создание и поиск привязок с помощью Пространственных привязок Azure в Java | Документация Майкрософт
description: В этой статье подробно рассказывается о том, как создавать и находить привязки с помощью Пространственных привязок Azure в Java.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7c8c5de469439f7ada294215682a1919022bcc1d
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672057"
---
# <a name="how-to-create-and-locate-anchors-using-azure-spatial-anchors-in-java"></a>Как создавать и искать привязки с помощью Пространственных привязок Azure в Java

> [!div  class="op_single_selector"]
> * [Unity](create-locate-anchors-unity.md)
> * [Objective-C](create-locate-anchors-objc.md)
> * [Swift](create-locate-anchors-swift.md)
> * [Android (Java)](create-locate-anchors-java.md)
> * [C++ (NDK)](create-locate-anchors-cpp-ndk.md)
> * [C++ (WinRT)](create-locate-anchors-cpp-winrt.md)

Пространственные привязки Azure позволяют вам совместно использовать привязки на различных устройствах. Служба поддерживает несколько сред разработки. Из этой статьи вы узнаете, как использовать пакет SDK Пространственных привязок Azure в Java, чтобы выполнять следующие действия:

- правильно настраивать сеанс Пространственных привязок Azure и управлять им;
- создавать и настраивать свойства в локальных привязках;
- передавать их в облако;
- обнаруживать и удалять пространственные привязки в облаке.

## <a name="prerequisites"></a>Предварительные требования

Для работы с руководством вам потребуется следующее:

- Прочесть статью [Описание службы "Пространственные привязки Azure"](../overview.md).
- Выполнить одно из [5-минутных руководств](../index.yml).
- Базовые знания Java.
- Базовые знания <a href="https://developers.google.com/ar/discover/" target="_blank">ARCore</a> 1.7.

[!INCLUDE [Start](../../../includes/spatial-anchors-create-locate-anchors-start.md)]

Дополнительные сведения см. в статье о классе [CloudSpatialAnchorSession](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession).

```java
    private CloudSpatialAnchorSession mCloudSession;
    // In your view handler
    mCloudSession = new CloudSpatialAnchorSession();
```

[!INCLUDE [Account Keys](../../../includes/spatial-anchors-create-locate-anchors-account-keys.md)]

Дополнительные сведения см. в статье о классе [SessionConfiguration](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.sessionconfiguration).

```java
    mCloudSession.getConfiguration().setAccountKey("MyAccountKey");
```

[!INCLUDE [Access Tokens](../../../includes/spatial-anchors-create-locate-anchors-access-tokens.md)]

```java
    mCloudSession.getConfiguration().setAccessToken("MyAccessToken");
```

[!INCLUDE [Access Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-access-tokens-event.md)]

Дополнительные сведения см. в статье об интерфейсе [TokenRequiredListener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.tokenrequiredlistener).

```java
    mCloudSession.addTokenRequiredListener(args -> {
        args.setAccessToken("MyAccessToken");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```java
    mCloudSession.addTokenRequiredListener(args -> {
        CloudSpatialAnchorSessionDeferral deferral = args.getDeferral();
        MyGetTokenAsync(myToken -> {
            if (myToken != null) args.setAccessToken(myToken);
            deferral.complete();
        });
    });
```

[!INCLUDE [Azure AD Tokens](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens.md)]

```java
    mCloudSession.getConfiguration().setAuthenticationToken("MyAuthenticationToken");
```

[!INCLUDE [Azure AD Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens-event.md)]

```java
    mCloudSession.addTokenRequiredListener(args -> {
        args.setAuthenticationToken("MyAuthenticationToken");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```java
    mCloudSession.addTokenRequiredListener(args -> {
        CloudSpatialAnchorSessionDeferral deferral = args.getDeferral();
        MyGetTokenAsync(myToken -> {
            if (myToken != null) args.setAuthenticationToken(myToken);
            deferral.complete();
        });
    });
```

[!INCLUDE [Setup](../../../includes/spatial-anchors-create-locate-anchors-setup-non-ios.md)]

Дополнительные сведения см. в статье о методе [start](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.start).

```java
    mCloudSession.setSession(mSession);
    mCloudSession.start();
```

[!INCLUDE [Frames](../../../includes/spatial-anchors-create-locate-anchors-frames.md)]

Дополнительные сведения см. в статье о методе [processFrame](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.processframe).

```java
    mCloudSession.processFrame(mSession.update());
```

[!INCLUDE [Feedback](../../../includes/spatial-anchors-create-locate-anchors-feedback.md)]

Дополнительные сведения см. в статье об интерфейсе [SessionUpdatedListener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.sessionupdatedlistener).

```java
    mCloudSession.addSessionUpdatedListener(args -> {
        auto status = args->Status();
        if (status->UserFeedback() == SessionUserFeedback::None) return;
        NumberFormat percentFormat = NumberFormat.getPercentInstance();
        percentFormat.setMaximumFractionDigits(1);
        mFeedback = String.format("Feedback: %s - Recommend Create=%s",
            FeedbackToString(status.getUserFeedback()),
            percentFormat.format(status.getRecommendedForCreateProgress()));
    });
```

[!INCLUDE [Creating](../../../includes/spatial-anchors-create-locate-anchors-creating.md)]

Дополнительные сведения см. в статье о классе [CloudSpatialAnchor](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchor).

```java
    // Create a local anchor, perhaps by hit-testing and creating an ARAnchor
    Anchor localAnchor = null;
    List<HitResult> hitResults = mSession.update().hitTest(0.5f, 0.5f);
    for (HitResult hit : hitResults) {
        Trackable trackable = hit.getTrackable();
        if (trackable instanceof Plane) {
            if (((Plane) trackable).isPoseInPolygon(hit.getHitPose())) {
                localAnchor = hit.createAnchor();
                break;
            }
        }
    }

    // If the user is placing some application content in their environment,
    // you might show content at this anchor for a while, then save when
    // the user confirms placement.
    CloudSpatialAnchor cloudAnchor = new CloudSpatialAnchor();
    cloudAnchor.setLocalAnchor(localAnchor);
    Future createAnchorFuture = mCloudSession.createAnchorAsync(cloudAnchor);
    CheckForCompletion(createAnchorFuture, cloudAnchor);

    // ...

    private void CheckForCompletion(Future createAnchorFuture, CloudSpatialAnchor cloudAnchor) {
        new android.os.Handler().postDelayed(() -> {
            if (createAnchorFuture.isDone()) {
                try {
                    createAnchorFuture.get();
                    mFeedback = String.format("Created a cloud anchor with ID=%s", cloudAnchor.getIdentifier());
                }
                catch(InterruptedException e) {
                    mFeedback = String.format("Save Failed:%s", e.getMessage());
                }
                catch(ExecutionException e) {
                    mFeedback = String.format("Save Failed:%s", e.getMessage());
                }
            }
            else {
                CheckForCompletion(createAnchorFuture, cloudAnchor);
            }
        }, 500);
    }
```

[!INCLUDE [Session Status](../../../includes/spatial-anchors-create-locate-anchors-session-status.md)]

Дополнительные сведения см. в статье о методе [getSessionStatusAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.getsessionstatusasync).

```java
    Future<SessionStatus> sessionStatusFuture = mCloudSession.getSessionStatusAsync();
    CheckForCompletion(sessionStatusFuture);

    // ...

    private void CheckForCompletion(Future<SessionStatus> sessionStatusFuture) {
        new android.os.Handler().postDelayed(() -> {
            if (sessionStatusFuture.isDone()) {
                try {
                    SessionStatus value = sessionStatusFuture.get();
                    if (value.getRecommendedForCreateProgress() < 1.0f) return;
                    // Issue the creation request...
                }
                catch(InterruptedException e) {
                    mFeedback = String.format("Session status error:%s", e.getMessage());
                }
                catch(ExecutionException e) {
                    mFeedback = String.format("Session status error:%s", e.getMessage());
                }
            }
            else {
                CheckForCompletion(sessionStatusFuture);
            }
        }, 500);
    }
```

[!INCLUDE [Setting Properties](../../../includes/spatial-anchors-create-locate-anchors-setting-properties.md)]

Дополнительные сведения см. в статье о методе [getAppProperties](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchor.getappproperties).

```java
    CloudSpatialAnchor cloudAnchor = new CloudSpatialAnchor();
    cloudAnchor.setLocalAnchor(localAnchor);
    Map<String,String> properties = cloudAnchor.getAppProperties();
    properties.put("model-type", "frame");
    properties.put("label", "my latest picture");
    Future createAnchorFuture = mCloudSession.createAnchorAsync(cloudAnchor);
    // ...
```

[!INCLUDE [Update Anchor Properties](../../../includes/spatial-anchors-create-locate-anchors-updating-properties.md)]

Дополнительные сведения см. в статье о методе [updateAnchorPropertiesAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.updateanchorpropertiesasync).

```java
    CloudSpatialAnchor anchor = /* locate your anchor */;
    anchor.getAppProperties().put("last-user-access", "just now");
    Future updateAnchorPropertiesFuture = mCloudSession.updateAnchorPropertiesAsync(anchor);
    CheckForCompletion(updateAnchorPropertiesFuture);

    // ...

    private void CheckForCompletion(Future updateAnchorPropertiesFuture) {
        new android.os.Handler().postDelayed(() -> {
            if (updateAnchorPropertiesFuture.isDone()) {
                try {
                    updateAnchorPropertiesFuture.get();
                }
                catch(InterruptedException e) {
                    mFeedback = String.format("Updating Properties Failed:%s", e.getMessage());
                }
                catch(ExecutionException e) {
                    mFeedback = String.format("Updating Properties Failed:%s", e.getMessage());
                }
            }
            else {
                CheckForCompletion1(updateAnchorPropertiesFuture);
            }
        }, 500);
    }
```

[!INCLUDE [Getting Properties](../../../includes/spatial-anchors-create-locate-anchors-getting-properties.md)]

Дополнительные сведения см. в статье о методе [getAnchorPropertiesAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.getanchorpropertiesasync).

```java
    Future<CloudSpatialAnchor> getAnchorPropertiesFuture = mCloudSession.getAnchorPropertiesAsync("anchorId");
    CheckForCompletion(getAnchorPropertiesFuture);

    // ...

    private void CheckForCompletion(Future<CloudSpatialAnchor> getAnchorPropertiesFuture) {
        new android.os.Handler().postDelayed(() -> {
            if (getAnchorPropertiesFuture.isDone()) {
                try {
                    CloudSpatialAnchor anchor = getAnchorPropertiesFuture.get();
                    if (anchor != null) {
                        anchor.getAppProperties().put("last-user-access", "just now");
                        Future updateAnchorPropertiesFuture = mCloudSession.updateAnchorPropertiesAsync(anchor);
                        // ...
                    }
                } catch (InterruptedException e) {
                    mFeedback = String.format("Getting Properties Failed:%s", e.getMessage());
                } catch (ExecutionException e) {
                    mFeedback = String.format("Getting Properties Failed:%s", e.getMessage());
                }
            } else {
                CheckForCompletion(getAnchorPropertiesFuture);
            }
        }, 500);
    }
```

[!INCLUDE [Expiration](../../../includes/spatial-anchors-create-locate-anchors-expiration.md)]

Дополнительные сведения см. в статье о методе [setExpiration](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchor.setexpiration).

```java
    Date now = new Date();
    Calendar cal = Calendar.getInstance();
    cal.setTime(now);
    cal.add(Calendar.DATE, 7);
    Date oneWeekFromNow = cal.getTime();
    cloudAnchor.setExpiration(oneWeekFromNow);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating.md)]

Дополнительные сведения см. в статье о методе [createWatcher](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.createwatcher).

```java
    AnchorLocateCriteria criteria = new AnchorLocateCriteria();
    criteria.setIdentifiers(new String[] { "id1", "id2", "id3" });
    mCloudSession.createWatcher(criteria);
```

[!INCLUDE [Locate Events](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

Дополнительные сведения см. в статье об интерфейсе [AnchorLocatedListener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.anchorlocatedlistener).

```java
    mCloudSession.addAnchorLocatedListener(args -> {
        switch (args.getStatus()) {
            case Located:
                CloudSpatialAnchor foundAnchor = args.getAnchor();
                // Go add your anchor to the scene...
                break;
            case AlreadyTracked:
                // This anchor has already been reported and is being tracked
                break;
            case NotLocatedAnchorDoesNotExist:
                // The anchor was deleted or never existed in the first place
                // Drop it, or show UI to ask user to anchor the content anew
                break;
            case NotLocated:
                // The anchor hasn't been found given the location data
                // The user might in the wrong location, or maybe more data will help
                // Show UI to tell user to keep looking around
                break;
        }
    });
```

[!INCLUDE [Deleting](../../../includes/spatial-anchors-create-locate-anchors-deleting.md)]

Дополнительные сведения см. в статье о методе [deleteAnchorAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.deleteanchorasync).

```java
    Future deleteAnchorFuture = mCloudSession.deleteAnchorAsync(cloudAnchor);
    // Perform any processing you may want when delete finishes (deleteAnchorFuture is done)
```

[!INCLUDE [Stopping](../../../includes/spatial-anchors-create-locate-anchors-stopping.md)]

Дополнительные сведения см. в статье о методе [stop](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.stop).

```java
    mCloudSession.stop();
```

[!INCLUDE [Resetting](../../../includes/spatial-anchors-create-locate-anchors-resetting.md)]

Дополнительные сведения см. в статье о методе [reset](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.reset).

```java
    mCloudSession.reset();
```

[!INCLUDE [Cleanup](../../../includes/spatial-anchors-create-locate-anchors-cleanup-java.md)]

Дополнительные сведения см. в статье о методе [close](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.close).

```java
    mCloudSession.close();
```

[!INCLUDE [Next Steps](../../../includes/spatial-anchors-create-locate-anchors-next-steps.md)]
