---
title: iOS Xamarin.iOS의 확장
description: 이 문서에서는 위젯 알림 센터 내에서 같은 표준 컨텍스트에서 iOS에서 제공 되는 확장을 설명 합니다. 확장을 생성 하 고 부모 응용 프로그램에서와 통신 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c26f951ddaff34cf23662f701395e636e1258b7d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786732"
---
# <a name="ios-extensions-in-xamarinios"></a>iOS Xamarin.iOS의 확장

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**IOS에 의해 확장을 만들기 [Xamarin 대학](https://university.xamarin.com/)**

확장명, 8, iOS에서 도입 된 특수화 된 `UIViewControllers` 하 여 표시 되 표준 컨텍스트 내 iOS 등 안에 **알림 센터**수행 하려면 사용자가 요청 하는 사용자 지정 키보드 종류 특수화 된 하나로 서, 입력 또는 다른 컨텍스트에서 확장 필터 특수 효과 제공할 수 있는 사진 편집 같은입니다.

모든 확장 (64 비트 통합 Api를 사용 하 여 작성 된 두 요소)와 컨테이너 앱과 함께에서 설치 되 고 호스트 응용 프로그램에서 특정 확장 지점에서 활성화 됩니다. 하며 고성능, 간결 하 고 강력한 기존 시스템 함수에 대 한 조항은로 사용 될, 이후 여야 합니다. 

이 문서에서는 다음 항목을 다룹니다.

- [확장점](#Extension-Points) -사용할 수 있는 확장 지점 유형과 각 지점에 대해 만들 수 있는 확장의 유형을 나열 합니다.
- [제한 사항](#Limitations) -확장에는 여러 다른 유형의 확장의 사용에 특정 제한 될 수 하는 동안 일부는 모든 형식에 유니버설 제한 합니다.
- [배포, 설치 및 실행 하는 확장](#Distributing-Installing-and-Running-Extensions) -확장 제출 및 앱 스토어를 통해 배포에 컨테이너 응용 프로그램 내에서 배포 됩니다. 앱와 함께 배포 하는 확장은 해당 시점에 설치 되어 있지만 사용자는 각 확장 프로그램을 명시적으로 사용 해야 합니다. 다른 형식의 확장은 다양 한 방법에서 활성화 됩니다.
- [확장 프로그램 수명 주기](#Extension-Lifecycle) -확장의 `UIViewController` 인스턴스화되 하 고 기본 뷰-컨트롤러 수명 주기를 시작 합니다. 그러나 일반적인 앱와 달리 확장은 로드, 실행 및 다음 반복을 종료 합니다.
- [된 확장을 만드는](#Creating-an-Extension) -확장을 개발 하 여 솔루션 적어도 두 개의 프로젝트가 포함 됩니다: 컨테이너 응용 프로그램 및 하나 제공 하는 각 확장의 컨테이너에 대 한 프로젝트입니다.
- [연습](#Walkthrough) -간단한 만드는 표지 **오늘** 위젯 스토리 보드를 사용 하거나 사용자 인터페이스를 제공 하는 확장 하거나 코드에서 확장을 설치 하 고 iOS 시뮬레이터에서에서 테스트 합니다.
- [응용 프로그램 호스트와의 통신](#Communicating-with-the-Host-App) -확장 응용 프로그램 호스트와의 통신에 대해를 간략하게 설명 합니다.
- [부모 앱 통신할](#Communicating-with-the-Parent-App) -확장 프로그램의 부모 앱와의 통신에 대해를 간략하게 설명 합니다.
- [예방 조치 및 고려 사항](#Precautions-and-Considerations) -주의 사항 및 고려 사항을 디자인 하 고 확장을 구현할 때 고려해 야 합니다 알고 일부 목록입니다.
 

<a name="Extension-Points" />

## <a name="extension-points"></a>확장점

|형식|설명|확장 지점|호스트 응용 프로그램|
|--- |--- |--- |--- |
|작업|특수 편집기 또는 특정 미디어 유형에 대 한 뷰어|`com.apple.ui-services`|임의의 값|
|문서 공급자|앱을 원격 문서 저장소를 사용 하도록 할 수 있습니다.|`com.apple.fileprovider-ui`|사용 하 여 앱을 [UIDocumentPickerViewController](https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/)|
|키보드|대체 키보드|`com.apple.keyboard-service`|임의의 값|
|사진 편집|사진 조작 및 편집|`com.apple.photo-editing`|Photos.app 편집기|
|공유|메시징 서비스 등, 소셜 네트워크와 데이터를 공유 합니다.|`com.apple.share-services`|임의의 값|
|오늘|오늘 화면 또는 알림 센터에 표시 되는 "위젯"|`com.apple.widget-extensions`|오늘과 알림 센터|

[추가 확장점](~/ios/platform/introduction-to-ios10/index.md#app-extensions) iOS 10에에서 추가 되었습니다.

<a name="Limitations" />

## <a name="limitations"></a>제한 사항

확장 여러 (없습니다 유형의 확장 인스턴스를 액세스할 수에 대해 카메라 또는 마이크) 일부는 모든 형식에 유니버설 제한에는 다른 형식의 확장명 (예를 들어, 사용자 지정 키보드의 사용에 특정 제한 될 수 하는 동안 사용할 수 없음에 대 한 암호에 대 한 보안 데이터 입력 필드와 같은). 

범용 제한은 다음과 같습니다.

- [상태 키트](~/ios/platform/healthkit.md) 및 [이벤트 키트 UI](~/ios/platform/eventkit.md) 프레임 워크를 사용할 수 없는 경우
- 확장을 사용할 수 없습니다 [백그라운드 모드를 확장 합니다.](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- (하지만 기존 미디어 파일에 액세스할 수 있습니다) 확장 장치의 카메라 또는 마이크를 액세스할 수 없습니다.
- (하지만 공기 삭제를 통해 데이터를 전송할 수 있습니다) 확장 공기 Drop 데이터를 받을 수 없습니다.
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/) 및 [UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/) 수 없는; 확장을 사용 하 여 해야 [UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- 여러 멤버 [UIApplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/) 사용할 수 없는: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/), `UIApplication.OpenURL`, `UIApplication.BeginIgnoringInteractionEvents` 및 `UIApplication.EndIgnoringInteractionEvents`
- 오늘날의 확장 프로그램에 16MB 메모리 사용 제한을 적용 하는 iOS입니다.
- 기본적으로 키보드 확장에는 네트워크에 액세스할이 필요가 없습니다. (제한 시뮬레이터에는 적용 되지 않습니다) 장치에서 디버깅을 미치면 Xamarin.iOS 하려면 디버깅을 수행 하려면 네트워크 액세스가 필요 합니다. 설정 하 여 네트워크 액세스 요청 수는 `Requests Open Access` 값에는 프로젝트의 Info.plist에서 `Yes`합니다. Apple의를 참조 하십시오 [사용자 지정 키보드 가이드](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) 키보드 확장 제한에 대 한 자세한 내용은 합니다.

개별 제한 사항에 대 한 Apple의를 참조 하십시오 [App 확장 프로그래밍 가이드](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/)합니다.

<a name="Distributing-Installing-and-Running-Extensions" />

## <a name="distributing-installing-and-running-extensions"></a>배포, 설치 및 확장을 실행 합니다.

확장은 제출 및 앱 스토어를 통해 배포에 컨테이너 응용 프로그램 내에서 배포 됩니다. 앱와 함께 배포 하는 확장은 해당 시점에 설치 되어 있지만 사용자는 각 확장 프로그램을 명시적으로 사용 해야 합니다. 다른 형식의 확장은 다양 한 방법;에 설정 된 이동할 사용자를 해야 하는 몇 가지는 **설정을** 앱 여기에서 사용 하도록 설정 합니다. 반면 사진을 보낼 때 공유 확장 프로그램을 사용 하도록 설정 하는 등의 사용 시점에서 다른 항목은 활성화 합니다. 

확장이 사용 되는 (사용자 경험 확장 지점) 앱 라고는 **호스트 응용 프로그램**실행 될 때 확장을 호스팅하는 응용 프로그램 이므로, 합니다. 확장을 설치 하는 앱은는 **컨테이너 응용 프로그램**(가) 응용 프로그램 설치 시 확장을 포함 합니다.  

일반적으로 컨테이너 응용 프로그램 확장에 설명 하 고 사용자 설정 되어 있으므로 과정을 단계별로 안내 합니다.

<a name="Extension-Lifecycle" />

## <a name="extension-lifecycle"></a>확장 프로그램 수명 주기

확장은 단일 처럼 간단 해질 수 [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/) 또는 여러 UI 화면을 제공 하는 보다 복잡 한 확장입니다. 사용자가 발생 했을 때는 _확장점_ (예를 들어 이미지를 공유)를 해당 확장 지점에 대해 등록 된 확장 프로그램에서 선택할 수 있는 기회를 갖게 됩니다. 

확장을 이러한 앱 중 하나를 선택 하는 경우 해당 `UIViewController` 인스턴스화되 하 고 기본 뷰-컨트롤러 수명 주기를 시작 합니다. 그러나 달리 일시 중단 있지만 일반적으로 사용자 상호 작용 하 완료 되 면 종료, 일반 응용 프로그램 확장은 로드, 실행 및 다음 반복을 종료 합니다.

확장 통해 응용 프로그램 호스트와 통신할 수는 [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) 개체입니다. 일부 확장 프로그램에는 결과와 비동기 콜백을 수신 하는 작업이 있습니다. 이러한 콜백은 백그라운드 스레드에서 실행 되 고 확장 고려해 야이; 예를 들어, 사용 하 여 [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/) 사용자 인터페이스를 업데이트 하는 경우. 참조는 [응용 프로그램 호스트와의 통신](#Communicating-with-the-Host-App) 자세한 내용을 보려면 아래 섹션.

기본적으로 확장 및 해당 컨테이너 앱 수 통신 하지, 함께 설치 되 고 불구 하 고. 경우에 따라 컨테이너 앱은 기본적으로 빈 "shipping" 컨테이너 목적인 확장이 설치 되 면 제공 됩니다. 그러나 상황으로 인해, 컨테이너 응용 프로그램 및 확장 공유할 수 있습니다 공통 영역에서 리소스. 또한 한 **오늘 확장** 해당 컨테이너를 열도록 앱을 URL을 요청할 수 있습니다. 이 동작에 표시 되는 [카운트다운 위젯 발전](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo)합니다.

<a name="Creating-an-Extension" />

## <a name="creating-an-extension"></a>확장 만들기

확장의 컨테이너 앱 64 비트 이진 파일 이어야 합니다 및는 Xamarin.iOS를 사용 하 여 빌드된 [통합 Api](http://developer.xamarin.com/guides/cross-platform/macios/unified)합니다. 확장을 개발 하 여 솔루션 적어도 두 개의 프로젝트가 포함 됩니다: 각 확장의 컨테이너를 제공에 대 한 개와 컨테이너 응용 프로그램 프로젝트입니다. 

<a name="Container-App-Project-Requirements" />

### <a name="container-app-project-requirements"></a>컨테이너 응용 프로그램 프로젝트 요구 사항

확장을 설치 하는 데 사용 하는 컨테이너 응용 프로그램에 다음과 같은 필요 합니다.

- 이 확장 프로젝트에 대 한 참조를 유지 해야 합니다.   
- (시작 하 고 성공적으로 실행 하는 작업을 할 수 있어야)는 전체 응용 프로그램 해야 하는 아무 작업도 수행 둘 이상의 확장을 설치 하는 방법을 제공 하는 경우에 합니다. 
- 확장의 번들 Id의 기준이 되는 번들 식별자 있어야 합니다 (자세한 내용은 아래 섹션 참조) 프로젝트.

<a name="Extension-Project-Requirements" />

### <a name="extension-project-requirements"></a>확장 프로젝트 요구 사항

또한 확장의 프로젝트에는 다음 요구 사항을:

- 해당 컨테이너 응용 프로그램의 번들 식별자로 시작 하는 번들 식별자 있어야 합니다. 예를 들어 컨테이너 응용 프로그램의 번들 식별자 `com.myCompany.ContainerApp`, 확장의 식별자로 사용할 수 있습니다 `com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- 키를 정의 해야 합니다 `NSExtensionPointIdentifier`, 적절 한 값으로 (같은 `com.apple.widget-extension` 에 대 한는 **오늘** 알림 센터 위젯)에서 해당 `Info.plist` 파일입니다.
- 도 정의 해야 *어느* 는 `NSExtensionMainStoryboard` 키 또는 `NSPrincipalClass` 키에 해당 `Info.plist` 적절 한 값을 갖는 파일:
    - 사용 하 여는 `NSExtensionMainStoryboard` 확장에 대 한 기본 UI를 표시 하는 스토리 보드의 이름을 지정 하는 키 (빼기 `.storyboard`). 예를 들어 `Main` 에 대 한는 `Main.storyboard` 파일입니다.
    - 사용 하 여는 `NSPrincipalClass` 확장 시작 될 때 초기화 될 클래스를 지정 하는 키입니다. 값이 일치 해야는 **등록** 값 프로그램 `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

특정 유형의 확장 추가 요구 사항이 있을 수 있습니다. 예를 들어, 한 **오늘** 또는 **알림 센터** 확장의 기본 클래스를 구현 해야 [INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/)합니다.

> [!IMPORTANT]
> Mac 용 Visual Studio에서 제공 되는 확장 템플릿을 하나를 사용 하 여 프로젝트를 시작 하기 (모두는 아님) 경우에 대부분의 이러한 요구 사항은 제공 되 고 템플릿에 의해 자동으로 사용자에 대해 충족 됩니다.

<a name="Walkthrough" />

## <a name="walkthrough"></a>연습 

다음 연습에서는 예 만들어집니다 **오늘** 일 및 연도에 남은 일 수를 계산 하는 위젯:

[![](extensions-images/carpediemscreenshot-sm.png "일 및 연도에 남은 일 수를 계산 하는 예에서는 오늘 위젯")](extensions-images/carpediemscreenshot.png#lightbox)

<a name="Creating-the-Solution" />

### <a name="creating-the-solution"></a>솔루션 만들기

필요한 솔루션을 만들려면 다음을 수행 합니다.

1. 먼저 새 iOS 만듭니다 **단일 앱 보기** 프로젝트는 **다음** 단추: 

    [![](extensions-images/today01.png "먼저 새 iOS, 단일 보기 응용 프로그램 프로젝트를 만들고 단추를 클릭 합니다.")](extensions-images/today01.png#lightbox)
2. 프로젝트를 호출 `TodayContainer` 클릭는 **다음** 단추: 

    [![](extensions-images/today02.png "다음 단추를 클릭 하 고 프로젝트 TodayContainer call")](extensions-images/today02.png#lightbox)
3. 확인 된 **프로젝트 이름** 및 **SolutionName** 클릭는 **만들기** 솔루션을 만드는 단추: 

    [![](extensions-images/today03.png "프로젝트 이름 및 SolutionName 확인 하 고 솔루션을 만들기 위해 만들기 단추를 클릭 합니다.")](extensions-images/today03.png#lightbox)
4. 다음의 **솔루션 탐색기**솔루션을 마우스 오른쪽 단추로 클릭 하 고 새 추가 **iOS 확장** 에서 프로젝트는 **오늘 확장** 템플릿: 

    [![](extensions-images/today04.png "다음으로, 솔루션 탐색기에서 솔루션을 마우스 오른쪽 단추로 클릭 하 고 오늘 확장 템플릿에서 새 iOS 확장 프로젝트를 추가 합니다.")](extensions-images/today04.png#lightbox)
5. 프로젝트를 호출 `DaysRemaining` 클릭는 **다음** 단추: 

    [![](extensions-images/today05.png "다음 단추를 클릭 하 고 프로젝트 DaysRemaining call")](extensions-images/today05.png#lightbox)
6. 프로젝트를 검토 하 고 클릭는 **만들기** 단추를 만듭니다. 

    [![](extensions-images/today06.png "프로젝트를 검토 하 고 만들 만들기 단추를 클릭 합니다.")](extensions-images/today06.png#lightbox)

생성 되는 솔루션을 두 개의 프로젝트가 만들었습니다 다음과 같이 합니다.

[![](extensions-images/today07.png "생성 되는 솔루션을 만들었습니다 두 개의 프로젝트를 다음과 같이")](extensions-images/today07.png#lightbox)

<a name="Creating-the-Extension-User-Interface" />

### <a name="creating-the-extension-user-interface"></a>확장 사용자 인터페이스 만들기

그런 다음에 대 한 인터페이스를 디자인 해야 합니다 프로그램 **오늘** 위젯입니다. 하거나 이렇게 스토리 보드를 사용 하 여 하거나 UI 코드에서 만들어 합니다. 두 방법 모두 아래 자세히 설명 합니다.

<a name="Using-Storyboards" />

#### <a name="using-storyboards"></a>스토리 보드를 사용 하 여

스토리 보드와 UI를 빌드하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 확장 프로젝트를 두 번 클릭 `Main.storyboard` 편집을 위해 열 파일입니다. 

    [![](extensions-images/today08.png "확장 프로젝트 Main.storyboard 파일을 편집 하기 위해 열을 두 번 클릭")](extensions-images/today08.png#lightbox)
2. 템플릿에서 UI에 자동으로 추가 된 레이블을 선택 하 고는 **이름** `TodayMessage` 에 **위젯** 탭은 **속성 탐색기**: 

    [![](extensions-images/today09.png "템플릿에서 UI에 자동으로 추가 된 레이블을 선택 하 고 이름을 TodayMessage 속성 탐색기의 위젯 탭에서")](extensions-images/today09.png#lightbox)
3. 스토리 보드의 변경 내용을 저장 합니다.

<a name="Using-Code" />

#### <a name="using-code"></a>코드 사용

코드에서 UI를 빌드하려면 다음을 수행 합니다. 

1. 에 **솔루션 탐색기**, 선택는 **DaysRemaining** 프로젝트에서 새 클래스를 추가 하 고 호출할 `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect DaysRemaining 프로젝트는 새 클래스를 추가 하 고 CodeBasedViewController 호출")](extensions-images/code01.png#lightbox)
2. 다시는 **솔루션 탐색기**, 확장의 두 번 클릭 `Info.plist` 편집을 위해 열 파일입니다. 

    [![](extensions-images/code02.png "확장의 Info.plist 파일을 편집 하기 위해 열을 두 번 클릭")](extensions-images/code02.png#lightbox)
3. 선택 된 **소스 뷰** (화면 맨 아래)에서 엽니다는 `NSExtension` 노드: 

    [![](extensions-images/code03.png "화면 아래쪽에서 소스 뷰를 선택 하 고 NSExtension 노드를 열고")](extensions-images/code03.png#lightbox)
4. 제거는 `NSExtensionMainStoryboard` 키와 추가 한 `NSPrincipalClass` 값과 `CodeBasedViewController`: 

    [![](extensions-images/code04.png "NSExtensionMainStoryboard 키를 제거 하 고 값이 CodeBasedViewController NSPrincipalClass 추가")](extensions-images/code04.png#lightbox)
5. 변경 내용을 저장합니다.

다음에 편집는 `CodeBasedViewController.cs` 파일을 다음과 같이 되도록 합니다.

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewContoller")]
    public class CodeBasedViewController : UIViewController, INCWidgetProviding
    {
        public CodeBasedViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Add label to view
            var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
                TextAlignment = UITextAlignment.Center
            };

            View.AddSubview (TodayMessage);
            
            // Insert code to power extension here...

        }
    }
}
```

`[Register("CodeBasedViewContoller")]` 에 대해 지정한 값과 일치 하는 `NSPrincipalClass` 위에 있습니다.

<a name="Coding-the-Extension" />

### <a name="coding-the-extension"></a>확장을 코딩

만든 사용자 인터페이스를 사용 하 여 열 중 하나는 `TodayViewController.cs` 또는 `CodeBasedViewController.cs` 파일 (위의 사용자 인터페이스를 만드는 데 메서드 기반)을 변경은 **ViewDidLoad** 메서드 다음과 비슷하게 표시 및:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Calculate the values
    var dayOfYear = DateTime.Now.DayOfYear;
    var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
    var daysRemaining = 365 + leapYearExtra - dayOfYear;

    // Display the message
    if (daysRemaining == 1) {
        TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
    } else {
        TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
    }
}
```

코드를 사용 하 여 사용자 인터페이스 메서드를 기반으로 하는 경우 대체는 `// Insert code to power extension here...` 위쪽에서 새 코드를 사용 하 여 주석을 합니다. 후 기본 구현을 호출 (및 삽입 된 코드 버전에 대 한 레이블을)이이 코드는 연도 및 남은 일 수를 가져오려는 간단한 계산을 수행 합니다. 레이블에 메시지를 표시 한 다음 (`TodayMessage`) UI 디자인에서 만든 합니다.

이 프로세스는 응용 프로그램을 작성 하는 일반적인 프로세스에 얼마나 비슷한지 note 합니다. 확장의 `UIViewController` 백그라운드 모드를 갖지 않는 확장과 사용자 완료 되 면 일시 중단 되지 않습니다 점을 제외 하 고 보기 컨트롤러와 같은 수명 주기는 응용 프로그램에 사용 합니다. 대신 확장 반복적으로 초기화 되 고 필요에 따라 할당을 취소 합니다.

<a name="Creating-the-Container-App-User-Interface" />

### <a name="creating-the-container-app-user-interface"></a>컨테이너 응용 프로그램 사용자 인터페이스 만들기

이 연습에서는 컨테이너 앱 단순히 제공 및 확장을 설치 하는 방법으로 사용 됩니다 하 고 자체의 기능을 제공 합니다. TodayContainer의 편집 `Main.storyboard` 파일을 설치 하는 방법 및 확장의 함수를 정의 하는 일부 텍스트를 추가 합니다.

[![](extensions-images/today10.png "TodayContainers Main.storyboard 파일을 편집 하는 확장명 함수 및 설치 하는 방법 정의 텍스트 추가")](extensions-images/today10.png#lightbox)

스토리 보드의 변경 내용을 저장 합니다.

<a name="Testing-the-Extension" />

### <a name="testing-the-extension"></a>확장 테스트

IOS 시뮬레이터에서에서 확장 프로그램을 테스트 하려면 실행는 **TodayContainer** 응용 프로그램입니다. 컨테이너의 기본 보기 표시 됩니다.

[![](extensions-images/run01.png "컨테이너의 기본 보기 표시 됩니다.")](extensions-images/run01.png#lightbox)

다음, 결과 **홈** 를 열려면 화면 맨 위에서 아래로 살짝 시뮬레이터에 있는 단추는 **알림 센터**, 선택는 **오늘** 탭을 클릭는 **편집** 단추:

[![](extensions-images/run02.png "알림 센터를 열고 오늘 탭을 선택 하 고 편집 단추를 클릭 하 고 화면 맨 위에서 아래로 살짝 시뮬레이터에서 홈 단추")](extensions-images/run02.png#lightbox)

추가 **DaysRemaining** 확장을는 **오늘** 클릭는 **수행** 단추:

[![](extensions-images/run03.png "오늘 보기에 DaysRemaining 확장을 추가 하 고 완료 단추를 클릭 합니다.")](extensions-images/run03.png#lightbox)

에 추가할 새 위젯에 **오늘** 보기 및 결과가 표시 됩니다.

[![](extensions-images/run04.png "새 위젯에 오늘 보기에 추가 되 고 결과 나타납니다.")](extensions-images/run04.png#lightbox)

<a name="Communicating-with-the-Host-App" />

## <a name="communicating-with-the-host-app"></a>응용 프로그램 호스트와 통신

이 예제에서는 위에서 만든 확장 오늘 한다면 응용 프로그램 호스트와 통신 하지 않습니다 (의 **오늘** 화면). 사용 하는 경우는 [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) 속성은 `TodayViewController` 또는 `CodeBasedViewController` 클래스입니다. 

호스트 응용 프로그램에서 데이터를 받을 확장을 위한 데이터의 배열 형식에는 [NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/) 에 저장 된 개체는 [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/) 의 속성은 [ExtensionContext ](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) 확장의의 `UIViewController`합니다.

사진 편집 확장과 같은 다른 확장명 완료 또는 취소 사용 사용자 구분할 수 있습니다. 이 신호를 받는 통해 호스트 앱으로 다시는 [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/) 및 [CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/) 방식의 [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/) 속성입니다.

자세한 내용은 Apple의를 참조 하십시오 [App 확장 프로그래밍 가이드](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)합니다.

<a name="Communicating-with-the-Parent-App" />

## <a name="communicating-with-the-parent-app"></a>부모 응용 프로그램와 통신

앱 그룹을 사용하면 서로 다른 응용 프로그램(또는 응용 프로그램과 해당 확장 프로그램)이 공유 파일 저장소 위치에 액세스할 수 있습니다. 앱 그룹은 다음과 같은 데이터에 사용할 수 있습니다.

- [Apple Watch 설정](~/ios/watchos/app-fundamentals/settings.md)합니다.
- [공유 NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)합니다.
- [공유 파일](~/ios/watchos/app-fundamentals/parent-app.md#files)합니다.

자세한 내용은 참조 하십시오는 [앱 그룹](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 의 섹션 우리의 **기능 작업** 설명서입니다.

<a name="MobileCoreServices" />

## <a name="mobilecoreservices"></a>MobileCoreServices

Extensions를 사용할 때는 균일 한 형식 식별자 (유틸리티)를 사용 하 여 만들고 응용 프로그램, 다른 응용 프로그램 및 서비스 간에 교환 되는 데이터를 조작 합니다.

`MobileCoreServices.UTType` Apple의 관련 된 다음 도우미 속성을 정의 하는 정적 클래스 `kUTType...` 정의:

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

다음 예제를 참조 하십시오.

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

자세한 내용은 참조 하십시오는 [앱 그룹](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 의 섹션 우리의 **기능 작업** 설명서입니다.


<a name="Precautions-and-Considerations" />

## <a name="precautions-and-considerations"></a>예방 조치 및 고려 사항

확장에는 훨씬 더 적은 메모리에 비해 응용 프로그램을 사용할 수 있습니다. 신속 하 고 사용자와 응용 프로그램에서 호스팅되는 최소한의 침입을 수행 하는 않을 것입니다. 그러나 확장 구별 되 고 유용한 함수 확장의 개발자 식별 하는 데 사용할 수 있는 브랜드 UI 사용 하 여 사용 중인 앱 또는 자신이 속한 컨테이너 응용 프로그램에 제공 해야 합니다.

긴밀 하 게 이러한 요구 사항에 지정 된 경우 배포 해야 확장을 철저히 테스트 되 고 성능 및 메모리 사용에 대해 최적화 합니다. 

<a name="Summary" />

## <a name="summary"></a>요약

이 문서 무엇 인지, 확장 지점 유형과 ios 확장에 적용 된 알려진된 제한 사항 확장 검사가 수행 됩니다. 만들기, 배포, 설치 및 확장 및 확장 수명 주기를 실행를 설명 합니다. 간단한을 만드는 연습 과정을 제공 **오늘** 코드 또는 스토리 보드를 사용 하 여 위젯의 UI를 만드는 두 가지 방법을 보여 주는 위젯입니다. IOS 시뮬레이터에서에서 확장을 테스트 하는 방법에 알아보았습니다. 마지막으로,이 간단 하 게 설명 호스트 응용 프로그램 및 몇 가지 예방 조치 및 확장을 개발 하는 경우 수행 해야 하는 고려 사항와 통신 합니다. 


## <a name="related-links"></a>관련 링크

- [ContainerApp (샘플)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Xamarin.iOS에서 확장 (비디오) 만들기](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
