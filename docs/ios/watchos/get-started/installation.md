---
title: 설치 및 Xamarin에서 watchOS 사용
description: 이 문서에는 설치 및 Xamarin을 사용한 watchOS를 사용 하는 방법을 설명 합니다. 설치, watchOS 프로젝트에 설명 구조, iOS 디자이너, Xcode 통합을 사용 하는 방법 및 문제 해결 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/05/2017
ms.openlocfilehash: ea0c7b6a68077cde83fa211e4e6f3432b3e39d5c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791240"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>설치 및 Xamarin에서 watchOS 사용

4 watchOS macOS Xcode 9로 시에라 (10.12) 필요합니다.

watchOS 1에는 원래 Xcode 7 OS X Yosemite (10.10) 필요 합니다.

> [!WARNING]
> [2018 년 4 월 1 후 watchOS 1 업데이트를 허용 하지 것입니다](https://developer.apple.com/news/?id=11162017a)합니다. 향후 업데이트 watchOS 2를 사용 해야 SDK 이상; watchOS 사용 구축 4 SDK 것이 좋습니다.

## <a name="project-structure"></a>프로젝트 구조

Watch 앱의 세 프로젝트로 구성 됩니다.

- **Xamarin.iOS iPhone 앱 프로젝트** -정상적인 iPhone 프로젝트인, Xamarin.iOS 템플릿 중 하나일 수 있습니다. 이 주 프로젝트 내 Watch 앱 및 해당 확장 프로그램 번들로 됩니다.

- **조사식 확장 프로젝트** -이 Watch 앱 대 코드 (예: 컨트롤러 클래스)를 포함 합니다.

- **조사식 응용 프로그램 프로젝트** -여기 Watch 앱에 대 한 UI 리소스를 모두 사용 하 여 사용자 인터페이스 스토리 보드 파일에 포함 합니다.

[조사식 키트 카탈로그 샘플](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) Xamarin.Studio에 다음과 같은 솔루션:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](installation-images/catalog-solution.png "Visual Studio에서 솔루션")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/catalog-solution-vs.png "Visual Studio에서 솔루션")

-----

다운로드 및 실행 된 [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) 샘플을 시작 합니다.
이 샘플에서 화면에서 확인할 수 있습니다는 [컨트롤](~/ios/watchos/user-interface/index.md) 페이지.


## <a name="creating-a-new-project"></a>새 프로젝트 만들기

새 "조사식 솔루션"...을 만들 수 없습니다 대신 Watch 앱 기존 iOS 응용 프로그램에 추가할 수 있습니다. Watch 앱을 만드는 다음이 단계를 수행 합니다.

1. 기존 프로젝트를 설정 하지 않은 경우 먼저 선택 **파일 > 새 솔루션** iOS 앱을 만듭니다 (예를 들어 한 **단일 앱 보기**):

    [![](installation-images/cycle8-2-sml.png "파일 > 새 솔루션 및 iOS 앱 만들기")](installation-images/cycle8-2.png#lightbox)

2. IOS 앱이 생성 됩니다 (또는 기존 iOS 앱을 사용 하려는) 되 면 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트 추가...** . 에 **새 프로젝트** 창 선택 **watchOS > 앱 > WatchKit 앱**:

    [![](installation-images/cycle8-6-sml.png "WatchOS 선택 > 앱 > WatchKit 응용 프로그램")](installation-images/cycle8-6.png#lightbox)

3. 다음 화면에는 iOS 앱 프로젝트는 watch 앱을 포함 해야 선택할 수 있습니다.

    [![](installation-images/cycle8-7-sml.png "IOS 앱 프로젝트는 watch 앱을 포함 해야 선택")](installation-images/cycle8-7.png#lightbox)

4. 마지막으로 프로젝트를 저장할 위치를 선택 (및 필요에 따라 소스 제어를 사용 하도록 설정):

    [![](installation-images/cycle8-8-sml.png "프로젝트를 저장할 위치를 선택 합니다.")](installation-images/cycle8-8.png#lightbox)

5. Mac 용 visual Studio에서 자동으로 구성 [프로젝트 참조 및 **Info.plist** 설정을](~/ios/watchos/get-started/project-references.md) 드립니다.

## <a name="creating-the-watch-user-interface"></a>조사식 사용자 인터페이스 만들기

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Xamarin iOS 디자이너를 사용 하 여

Watch 앱을 두 번 클릭 **Interface.storyboard** iOS 디자이너를 사용 하 여 편집 합니다. 스토리 보드도 인터페이스 컨트롤러 및 UI 컨트롤을 끌 수는 **도구 상자** 구성 및 사용 하 여는 **속성** 패드:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](installation-images/iosdesigner-sml.png "디자이너에서 스토리 보드")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](installation-images/iosdesigner-sml-vs.png "디자이너에서 스토리 보드")](installation-images/iosdesigner-vs.png#lightbox)

-----

각각의 새 인터페이스 컨트롤러를 제공 해야는 **클래스** 선택 하 고 다음에 이름을 입력 하 여는 **속성** 패드 (이 필요한 C# 코드 숨김 파일이 자동으로 만듭니다):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](installation-images/iosdesigner-classname.png "각각의 새 인터페이스 컨트롤러 클래스 제공")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/iosdesigner-classname-vs.png "각각의 새 인터페이스 컨트롤러 클래스 제공")

-----

만들기 여 segues **Ctrl + 드래그** 를 다른 인터페이스 컨트롤러에는 단추, 테이블 또는 인터페이스 컨트롤러에서 합니다.


### <a name="using-xcode-on-the-mac"></a>Mac에서 Xcode를 사용 하 여

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xcode Interface.storyboard 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 사용자 인터페이스를 작성 하는 데 계속 수 **프로그램 > Xcode 인터페이스 작성기**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

도 visual Studio 사용자 Xcode를 사용 하 여 Mac 빌드 호스트를 직접 사용 하도록 전환 하 여 사용자 인터페이스를 빌드할 수 있습니다.
Mac 용 Visual Studio에서 솔루션을 열고 다음 Interface.storyboard 파일을 마우스 오른쪽 단추로 클릭 및 선택 **프로그램 > Xcode 인터페이스 작성기**:

-----

![](installation-images/openwith-xcode.png "Interface.storyboard Xcode 인터페이스 작성기에서 열기")

Xcode를 사용 하는 경우 보통의 경우와 watch 앱에 대 한 동일한 단계를 수행 해야 [iOS 앱 스토리 보드](~/ios/user-interface/storyboards/index.md) (콘센트 및 여 동작 만들기와 같은 **Ctrl + 드래그** 에 **.h**헤더 파일).

저장할 때 스토리 보드는 자동으로 추가 하는 Xcode 인터페이스 작성기에서 만들 작업과 콘센트는 C# **. designer.cs** 조사식 확장 프로젝트에서 파일입니다.


### <a name="adding-additional-screens-in-xcode"></a>Xcode에서 추가 화면을 추가합니다.

추가 화면 (기본적으로 서식 파일에 포함 된) 이외의 Xcode 인터페이스 작성기를 사용 하 여 스토리 보드에 추가 하면 **C# 코드 파일을 수동으로 추가 해야** 각각의 새 인터페이스 컨트롤러에 대 한 합니다.

참조는 [스토리 보드에 새로운 인터페이스 컨트롤러를 추가 하는 방법에 지침은 고급](~/ios/watchos/troubleshooting.md#add)합니다.

*Xamarin iOS 디자이너에서는이 자동으로, 어떤 수동 단계도 필요 합니다.*


## <a name="building"></a>빌드

다른 iOS 프로젝트와 같은 watch 앱을 포함 한 프로젝트를 빌드합니다. 빌드 프로세스 코드 없는 조사식 응용 프로그램 (.app)을 포함 한 조사식 확장 (.appex)을 포함 하는 iPhone 응용 프로그램 (.app) 발생 합니다.


## <a name="launching"></a>시작

Studio (Mac 빌드 호스트에서 시작 됨) Visual 또는 Mac 용 Visual Studio를 사용 하 여 시뮬레이터에서 watch 앱을 시작할 수 있습니다.

가지 WatchKit 응용 프로그램을 실행 하기 위한 두 가지 모드가 있습니다.

 - 일반적인 앱 모드 (기본값) 및
- [알림](~/ios/watchos/platform/notifications.md) (JSON 형식에 테스트 알림 페이로드가 필요 입니다).

### <a name="xcode-8-support"></a>Xcode 8 지원

Apple Watch 시뮬레이터는 iOS 시뮬레이터 별개 8 또는 그 이상의 Xcode가 설치 되 면 (달리 [Xcode 6](#xcode6)로 표시 되 던 것 여기서는 *외부 디스플레이*).
시뮬레이터 목록에 표시 됩니다 Watch 앱 프로젝트를 선택 하 고 시작 프로젝트를 만들 때 *iOS 시뮬레이터* 를 선택할 수 (아래 참조).

[![](installation-images/xs-xcode8-watchos3-sml.png "시뮬레이터 유형 선택")](installation-images/xs-xcode8-watchos3.png#lightbox)

디버깅을 시작할 때 *두* 시뮬레이터 시작 해야-iOS 시뮬레이터 *및* Apple Watch 시뮬레이터. 사용 하 여 **명령 + Shift + H** 시계 메뉴 및 시계 화면의;로 이동 하 여 사용할는 **하드웨어** 설정 메뉴는 **Force 터치 압력**합니다. 트랙 패드 또는 마우스에 스크롤 디지털 왕관을 사용 하 여 시뮬레이트합니다.

#### <a name="troubleshooting"></a>문제 해결

에 다음과 같은 오류가 표시 됩니다는 **응용 프로그램 출력** 쌍을 이루는 조사식 없는 시뮬레이터에서 앱을 시작 하려고 하는 경우:

```csharp
error MT0000: Unexpected error - Please file a bug report at http://bugzilla.xamarin.com
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

참조 [Apple의 포럼](https://forums.developer.apple.com/thread/7783) 기본값 작동 하지 않을 경우에 시뮬레이터 구성에 대 한 지침은 합니다.


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 및 watchOS 1

확인 해야는 *조사식 확장 프로젝트* 는 **시작 프로젝트** 실행 또는 응용 프로그램을 디버깅 하기 전에. 있습니다 수 없습니다 "시작" 자체를 watch 앱 및 iOS 앱을 선택 하면 다음 시작 iOS 시뮬레이터에서에서 정상적으로 합니다.

기본적으로 기본에서 조사식 응용 프로그램이 시작 **앱** 모드 (없습니다 보기 또는 알림 모드) Mac의에 대 한 Visual Studio에서 **실행** 또는 **디버그** 명령입니다.

Xcode 6을 사용 하는 경우 iPhone 5, iPhone 5, 6, iPhone 및 iPhone 6 더하기에 대 한 외부 디스플레이 활성화할 수 **Apple Watch-38 mm** 또는 **Apple Watch-42 mm** watch 응용 프로그램 될 위치 표시 합니다.

**참고:** Xcode 6을 사용 하는 경우 조사식 화면이 iOS 시뮬레이터에서에서 자동으로 표시 되지 않으면 기억 합니다.
사용 하 여는 **하드웨어 > 외부 디스플레이** 조사식 화면 표시 될 메뉴입니다.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>알림 모드를 시작합니다.

참조는 [알림 페이지](~/ios/watchos/platform/notifications.md) 정보에 대 한 코드에서 알림을 처리 하는 방법입니다.


Mac 용 visual Studio는 알림과 함께 watch 앱을 시작할 수 _시작 모드_ 알림:



조사식 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **실행으로 > 사용자 지정 구성 중...** :


[![](installation-images/runwith-customparams-sml.png "사용자 지정 구성을 실행합니다.")](installation-images/runwith-customparams.png#lightbox)


열립니다는 **사용자 지정 매개 변수** 선택할 수 있는 창을 **알림** (및 JSON 페이로드 제공) 키를 누릅니다 **실행** 시뮬레이터에서 watch 앱을 시작 하려면:


[![](installation-images/runwith-execargs-sml.png "알림 및 페이로드를 설정합니다.")](installation-images/runwith-execargs.png#lightbox)



## <a name="debugging"></a>디버깅

Mac 용 Visual Studio와 Visual Studio에서 디버깅은 지원 됩니다.
알림 모드에서 디버깅할 때 알림 JSON 파일을 제공 해야 합니다. 이 스크린샷에서 watch 앱에서 적중 되 고 디버그 중단점을 보여 줍니다.

![](installation-images/debug-sml.png "이 스크린샷은 watch 앱에서 적중 되 고 디버그 중단점")

시작 지침을 따랐다면 됩니다 /fd watch 앱에서 실행 되는 **iOS 시뮬레이터 (조사식)** 합니다.
알림 모드 선택 **디버그 > 시스템 로그를 열기** (**CMD + /**) 사용 하 여 `Console.WriteLine` 코드에서.

### <a name="debugging-lifecycle-event-handlers"></a>수명 주기 이벤트 처리기를 디버깅합니다.

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

WatchOS 템플릿 파일 (같은 `InterfaceController`, `ExtensionDelegate`, `NotificationController`, 및 `ComplicationController`) 해당 필요한 수명 주기 메서드를 이미 구현 되어 함께 제공 합니다. 추가 `Console.WriteLine` 호출과 읽기는 **응용 프로그램 출력** 이벤트 수명 주기를 더 잘 이해 하려면.



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [첫 번째 Watch 앱 비디오](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple의 WatchKit 팁](https://developer.apple.com/watchkit/tips/)
