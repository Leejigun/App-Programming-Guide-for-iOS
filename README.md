# App-Programming-Guide-for-iOS

App Programming Guide for iOS 문서 정리 작업



[IOS의 생명주기](https://soooprmx.com/archives/4454)



### About iOS App Architecture

IOS에서 동작하는 app은 사용자에게 훌륭한 사용자 경험을 제공하는 것을 보장 할 필요가 있다. 앱의 디자인과 UI가 좋은 것을 넘어서, 좋은 사용자 경험은 다른 많은 요소들을 포함하고 있다. 사용자는 IOS app을 사용하면서 가능하면 적은 전원을 사용하면서 빠르고 민감하게 반응해야 한다고 생각한다. (fast and responsive) 앱들은 최신 장치를 모두 지원하며 현재 장치도 적합하도록 할 필요가 있다. 이 모든 것들을 구현하는 것은 쉬운 일이 아니지만 IOS는 당신이 이것들을 해내도록 도와줄 것 이다.



# chapter 1. Expected App Behaviors

이 쳅터에서는 모든 앱들에서 처리되어야 하는 동작과 개발 초기에 고려해야 할 동작을에 대해서 다룹니다.

### 1. Providing the Required Resources

제작하는 모든 앱은 iOS 기기에서 제대로 표시 될 수 있도록 다음과 같은 리소스 및 메타 데이터 집합이 있어야합니다.

* **An information property-list file**(프로퍼티 리스트의 정보) - Info.plist 파일에는 시스템이 앱과 상호 작용할 때 사용하는 앱에 대한 메타 데이터가 들어 있습니다. Xcode는이 파일을 프로젝트의 구성과 설정에 따라 자동으로 생성합니다. 이 파일의 내용을 직접 보거나 수정하려면 프로젝트의 정보 탭에서 할 수 있습니다. 이 파일을 편집하는 방법과 포함 할 키에 대한 권장 사항은 [여기](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/ExpectedAppBehaviors/ExpectedAppBehaviors.html#//apple_ref/doc/uid/TP40007072-CH3-SW5)를 참조하세요. (안드로이드에서 Manifest.xml 파일과 같은 역할은 하는 것 같다.)
* **One or more icons** - 모든 앱은 기기의 홈 화면과 앱 스토어에 표시 할 아이콘을 제공해야합니다. 앱은 실제로 여러 상황에서 사용할 여러 아이콘을 지정할 수 있습니다. 예를 들어 앱은 검색 결과를 표시 할 때 사용할 작은 아이콘을 제공 할 수 있으며 고 해상도 디스플레이가있는 기기에 고해상도 아이콘을 제공 할 수 있습니다.
* **One or more launch images**(스플레시 화면) - 앱을 실행하면 앱에서 사용자 인터페이스를 표시 할 수있을 때까지 임시 이미지가 표시됩니다. 이 임시 이미지는 앱의 시작 이미지이며 앱이 시작되고 즉각적인 준비가 될 것이라는 즉각적인 피드백을 사용자에게 제공합니다. 앱에 하나 이상의 스플레시 이미지를 제공해야하며 특정 시나리오를 해결하기 위해 추가 스플레시 이미지를 제공해야합니다. ( 안드로이드 앱에서 역시 앱이 시작되고 초기 설정을 위해서 스플레시 엑티비티를 사용하곤 했었는데, 이를 IOS에서는 launch image라 하는 것 같다.)

이러한 리소스는 모든 앱에 필요하지만 전부는 아닙니다.Info.plist 파일에 기본적으로 포함하지 않는 많은 속성들이 있습니다. 대부분의 추가적인 속성들은 그 기능을 사용할 때만 포함합니다. 예를 들어 마이크를 사용하는 앱에는NSMicrophoneUsageDescription 키가 포함있어야 합니다.

### 2. The App Bundle

iOS 앱을 만들 때 Xcode는 그것들을 번들로 묶습니다. 번들은 관련 자원들을 한곳으로 묶는 디렉토리입니다.  iOS 앱 번들에는 앱 실행 파일과 앱 아이콘, 이미지 파일 및 로컬 콘텐츠와 같은 지원 리소스 파일이 포함되어 있습니다.

아래 표는 MyApp이라는 앱이 있다고 가정할 때 Bundle에 속하는 file들입니다.

| File                                   | Example                                  | Description                              |
| -------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| App executable                         | `MyApp`                                  | 실행 파일에는 앱의 컴파일 된 코드가 들어 있습니다. 앱의 실행 파일 이름은 앱 이름에서 .app 확장자를 뺀 것과 같습니다.이 파일은 필수 항목입니다. |
| The information property list file     | `Info.plist`                             | Info.plist 파일에는 앱의 구성 데이터가 들어 있습니다. 시스템은이 데이터를 사용하여 앱과 상호 작용하는 방법을 결정합니다.이 파일은 필수이며 Info.plist라고해야합니다. |
| App icons                              | `Icon.png``Icon@2x.png``Icon-Small.png``Icon-Small@2x.png` | 앱 아이콘은 기기의 홈 화면에서 앱을 나타내는 데 사용됩니다. 다른 아이콘은 적절한 위치에서 시스템에 의해 사용됩니다. 파일 이름에 @2x가 표시된 아이콘은 망막 디스플레이가있는 장치 용입니다. 응용 프로그램 아이콘이 필요합니다. |
| Launch images                          | `Default.png``Default-Portrait.png``Default-Landscape.png` | 앱이 실행되는 동안 시스템은이 파일을 임시 배경으로 사용합니다. 앱에서 사용자 인터페이스를 표시 할 준비가되면 즉시 사라집니다.  반드시 하나 이상의 launch image가 필요합니다. |
| Storyboard files (or nib files)        | `MainBoard.storyboard`                   | 기존에 xib 파일을 만들고 뷰와 뷰 컨트롤러를 연결하는 개발에서 스토리 보드기능이 생기면서 각각의 뷰를 쉽게 관리 할 수 있게 되었다. 스토리 보드에는 앱이 화면에 표시하는 뷰 및 뷰 컨트롤러가 포함되어 있습니다. 이 방법에서 뷰 간의 이동은 segue(세그웨이)를 통해서 이루어 진다. 스토리 보드의 사용은 필수는 아니지만 권장됩니다. |
| Ad hoc distribution icon               | `iTunesArtwork`                          | 만약에 개발 중 앱을 임시로 배포해야 하는 경우 512 x 512 픽셀의 앱 아이콘을 포함시켜야 합니다. 일반적인 경우 아이콘은 App store에 저장되는데, 임시로 배포되는 앱은 App Store를 거치지 않기 때문에 앱 번들에 아이콘이 있어야합니다. |
| Settings bundle                        | `Settings.bundle`                        | 사전에 설정을 지정해 맞춤 앱 환경을 제공하려면 이 파일이 번들에 포함되야 합니다. 이 번들에는 property list file의 데이터와 앱 환경 설정을 정의하는 다른 리소스 파일이 들어 있습니다. 앱은이 번들의 정보를 사용하여 앱에 필요한 인터페이스 요소를 조합합니다. |
| Nonlocalized resource files            | `sun.png``mydata.plist`                  | 로컬라이징을 고려하지 않은 리소스에는 앱에서 사용하는 이미지, 사운드 파일, 동영상 및 맞춤 데이터 파일과 같은 항목이 포함됩니다. 이 파일들은 모두 앱 번들의 최상위에 위치해야합니다. |
| Subdirectories for localized resources | `en.lproj``fr.lproj``es.lproj`           | 지역화 된 리소스는 언어 별 프로젝트 디렉토리에 배치해야하며 이름은 ISO 639-1 언어 약어와 .lproj 접미사로 구성됩니다. 예를 들어 en.lproj, fr.lproj 및 es.lproj 디렉토리에는 영어, 프랑스어 및 스페인어로 현지화 된 리소스가 포함되어 있습니다. iOS 앱은 국제화되어야하며 지원하는 각 언어에 대해 language.lproj 디렉토리가 있어야합니다. |

### 3. Supporting User Privacy

* 사용자 개인 정보 보호를위한 설계가 중요합니다. 대부분의 iOS 기기에는 사용자가 앱이나 외부에 공개하고 싶지 않은 개인 데이터가 포함되어 있습니다. 앱이 부적절하게 데이터에 액세스하거나 데이터를 사용하는 경우 사용자는 앱을 삭제 할 수 있습니다.
* 앱이 보호 된 항목을 사용하려고 시도하면 시스템에서 사용자에게 **액세스 권한을 묻는 경고 메시지**를 표시합니다. **iOS 10부터 Info.plist 파일에는 각 항목에 대한 권한 경고에 표시 할 목적 문자열이 포함되어야합니다.** 해당 용도 문자열을 제공하지 않고 앱이 보호 된 항목에 액세스하려고하면 앱이 종료됩니다.

### 4. Internationalizing Your App

 iOS 앱이 많은 국가에 배포되기 때문에 앱의 콘텐츠를 현지화하면 더 많은 고객에게 다가 갈 수 있습니다. 사용자는 모국어로 현지화되어있을 때 앱을 사용할 확률이 훨씬 높습니다. 사용자 지향 콘텐츠를 리소스 파일로 고려할 때 해당 콘텐츠를 현지화하는 것은 상대적으로 간단한 과정입니다.

 콘텐츠를 현지화하기 전에 현지화 작업을 용이하게하기 위해 앱을 **국제화(Internationalizing)**해야합니다. 응용 프로그램을 국제화하면 모든 사용자 지향 콘텐츠를 현지화 할 수있는 리소스 파일로 분해하고 해당 콘텐츠를 저장하기위한 언어 별 프로젝트 (.lproj) 디렉토리를 제공합니다.

* 스토리 보드 -스토리 보드에는 현지화해야하는 텍스트 레이블 및 기타 컨텐츠가 포함될 수 있습니다. 텍스트 길이의 변경 사항을 수용하도록 인터페이스 항목의 위치를 조정할 수도 있습니다. (마찬가지로, nib 파일에는 현지화가 필요한 텍스트 또는 업데이트해야하는 레이아웃이 포함될 수 있습니다.)
* 스트링 파일 - .strings 파일에는 이름을 지정한 정적 문자열들을 기록합니다. 이 때 다른 언어마다 이름이 같은 다른 언어로 문자열을 지정해 놓아야 합니다.
* 이미지 파일 - 이미지에 문화권 관련 콘텐츠가 포함되어 있지 않으면 이미지를 현지화하지 마십시오. 이미지 파일에 텍스트를 직접 저장하지 않아야합니다. 앱에서 로드하여 사용하는 이미지의 경우 문자열 파일에 텍스트를 저장하고 런타임에 해당 텍스트를 이미지와 합치도록 개발해야 합니다.
* 비디오 및 오디오 파일 - 언어 별 또는 문화권 별 콘텐츠가 포함되어 있지 않으면 멀티미디어 파일의 현지화는 피해야합니다. 예를 들어, 음성 해설 트랙이 포함 된 비디오 파일을 현지화하고자 할 수 있습니다.



# chapter 2. The App Life Cycle

**앱은 사용자 정의 코드와 시스템 프레임 워크 간의 정교한 상호 작용입니다.** 시스템 프레임 워크는 모든 앱이 실행해야하는 기본 인프라를 제공하며 해당 인프라를 개발자가 커스텀 하는데 필요한 코드들을 지원합니다. 이를 효과적으로 수행하려면 iOS 인프라와 작동 방식에 대해 약간 이해해야합니다.

### 1. The Main Function

모든 C 기반 앱의 진입 점이 main 함수이며 iOS 앱도 마찬가지입니다. 다른점은 IOS App은 Main 함수를 직접 작성하지 않고 Xcode가 자동으로 생성해 준다는 것 입니다. 몇 가지 예외를 제외하고는 Xcode가 제공하는 주요 기능의 구현을 절대 변경해서는 안됩니다.

```
#import <UIKit/UIKit.h>
#import "AppDelegate.h"
 
int main(int argc, char * argv[])
{
    @autoreleasepool {
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}
```

Main 함수에 대해 언급할 한가지는 UIApplicationMain에 컨트롤을 넘긴다는 점 입니다. UIApplicationMain 함수에서는 스토리보드 파일과 사용자가 입력한 최초 초기화 코드들을 수행해 앱의 핵심 기능들을 작동시킵니다. 따라서 초기화를 위해 사용자는 스토리 보드 파일과 사용자 지정 초기화 코드를 넘겨줘야 합니다.



### 2. The Structure of an App

 앱을 시작하는 동안 UIApplicationMain 함수는 여러 핵심 개체를 설정하고 실행중인 응용 프로그램을 시작합니다. 모든 iOS 응용 프로그램의 핵심은 UIApplication 객체입니다.이 객체는 시스템과 응용 프로그램의 다른 객체 간의 상호 작용을 용이하게합니다.

![mvc](./img/MVC.png)



모든 iOS 응용 프로그램의 핵심은 UIApplication 객체입니다.이 객체는 시스템과 응용 프로그램의 다른 객체 간의 상호 작용을 용이하게합니다.

* iOS앱은 사실 `UIApplication`이라는 클래스의 객체이다. 프로젝트의 `main` 함수는 기본적으로 `UIApplication` 클래스의 인스턴스를 만들어서 GUI를 사용하기 위한 런루프를 돌려주는 작업을 수행한다. 그리고 그 이후에 앱 내에서 일어나는 모든 처리는 `UIApplication` 객체가 관리하게 된다.
  * 1. 아이폰에서 사용자가 어플리케이션을 Tap 해서 실행
     undefined해당 어플리케이션의 main 실행
     undefinedmain 에서 UIApplicationMain() 실행 
     undefinedAppDelegate 의 applicationDidFinishLaunching: 을 호출
     undefinedapplicationDidFinishLaunching이 완료되면  EventLoop로 들어감
     undefined이제부터는 개발자가 코드로 구현한 작업들 수행
     undefined어플리케이션 종료
     undefinedAppDelegate의 applicationWillTerminate: 호출
     undefined어플리케이션 종료

[The role of objects in an iOS app]

| Object                                   | Description                              |
| ---------------------------------------- | ---------------------------------------- |
| `UIApplication`object                    | UIApplication는 이벤트 루프를 포함한 기타 상위 응용 프로그램 동작을 관리합니다. 또한 주요 앱 전환과 일부 특수 이벤트 (예 : 수신 푸시 알림)를 정의한 맞춤 델리게이트로 위임합니다. |
| App delegate object                      | 앱 델리게이트는 커스텀 코드의 핵심입니다. 이 객체는 UIApplication 객체와 함께 작동하여 응용 프로그램 초기화, 상태 전환 및 많은 고급 응용 프로그램 이벤트를 처리합니다. 이 객체는 모든 응용 프로그램에 존재할 수있는 유일한 객체이기 때문에 응용 프로그램의 초기 데이터 구조를 설정하는 데 자주 사용됩니다.  앱 델리게이트는 이름 그대로 앱 객체의 대리인 역할을 한다. UIApplication은 서브 클래싱을 하는 경우도 드물고, 별로 그럴 이유도 없으며 그게 쉽지도 않다. 하지만 분명히 앱의 라이프 사이클의 여러 스테이지에서 수행되어야 하는 일은 있다. 따라서 앱 객체 클래스를 직접 서브 클래싱하지 않고 델리게이트를 통해 처리하게 된다. |
| Documents and data model objects         | 데이터 모델 객체는 앱의 콘텐츠를 저장하며 앱에만 적용됩니다. 애플리케이션은 Documents (UIDocument의 사용자 정의 하위 클래스)를 사용하여 데이터 모델 객체 일부 또는 전체를 관리 할 수도 있습니다. 문서 객체는 필수는 아니지만 단일 파일 또는 파일 패키지에 속한 데이터를 그룹화하는 편리한 방법을 제공합니다. |
| View controller objects                  | View controller는 보여주는 앱의 화면을 관리합니다. UIViewController 클래스는 모든 뷰 컨트롤러 객체의 기본 클래스입니다. 로딩, 화면 회전 등 표준 시스템 동작을위한 기본 기능을 제공합니다. UIKit 및 기타 프레임 워크는 이미지 선택기, 탭 표시 줄 인터페이스 및 탐색 인터페이스와 같은 표준 시스템 인터페이스를 구현하기 위해 추가적인 컨트롤러 클래스를 정의합니다. |
| `UIWindow`object                         | UIWindow 객체는 화면에서 하나 이상의 뷰를 관리합니다. 대부분의 앱에는 내용을 보여주는 Window가 하나만 있지만 외부 화면에 표시될 내용을 보여주는 추가적인 Window가 있는 앱도 있습니다. 앱의 콘텐츠를 변경하려면 뷰 컨트롤러를 사용하여 표시된 뷰를 변경해야 합니다. 절대 Window자체를 바꿀 수 없습니다. Window는 뷰를 표시하는 것 외에도 UIApplication과 함께 뷰의 이벤트 전달합니다. |
| View objects, control objects and layer objects | 뷰 및 컨트롤은 앱 콘텐츠의 시각적 표현을 제공합니다. 뷰는 지정된 직사각형 영역에 내용을 그리고 그 영역 내의 이벤트에 응답하는 객체입니다. 컨트롤은 버튼, 텍스트 필드 및 토글 스위치와 같은 친숙한 인터페이스 개체를 구현하는 특수 유형의 뷰입니다. UIKit 프레임 워크는 다양한 유형의 콘텐츠를 표시하기위한 standard views를 제공합니다. 또한 UIView를 직접 서브 클래 싱하여 자신 만의 커스텀 View를 정의 할 수 있습니다. 또한 뷰 및 컨트롤을 통합하는 것 외에도 Core Animation 레이어를 뷰 및 컨트롤 계층에 통합 할 수 있습니다. 레이어 객체는 실제로 시각적 인 내용을 나타내는 데이터 객체입니다. 뷰는 레이어 객체를 집중적으로 사용하여 내용을 렌더링합니다. 인터페이스에 사용자 정의 레이어 객체를 추가하여 복잡한 애니메이션 및 기타 유형의 정교한 시각 효과를 구현할 수도 있습니다. |

### 3. The Main Run Loop

 앱의 기본 실행 루프는 모든 사용자 관련 이벤트를 처리합니다. UIApplication 객체는 실행시 기본 실행 루프를 설정하고 이를 사용하여 이벤트를 처리하고 뷰 기반 인터페이스에 대한 업데이트를 처리합니다. 이름에서 알 수 있듯이 기본 실행 루프는 앱의 main 스레드에서 실행됩니다. 이 동작은 사용자 관련 이벤트가 수신 된 순서대로 순차적으로 처리되도록합니다.

![event_flow](./img/event_flow.png)

*  사용자가 기기와 상호 작용할 때 시스템에서 이러한 상호 작용과 관련된 이벤트를 생성 하고 UIKit에 의해 설정된 특수 포트를 통해 앱에 전달됩니다.
* 이벤트는 앱에 의해 내부적으로 대기열에 넣어지고 실행을 위해 Main Run Loop에 하나씩 전달됩니다. 
* UIApplication 객체는 이벤트를 수신하여 수행해야 할 작업을 결정하는 첫 번째 객체입니다.
* 터치 이벤트는 대개 메인 윈도우 객체에 전달되고, 터치는 터치가 발생한 뷰에 전달합니다. 다른 이벤트는 다양한 앱 객체를 통해 약간 다른 경로를 취할 수 있습니다.

이러한 이벤트 유형의 대부분은 앱의 Main Run Loop를 사용하여 전달되지만 일부 이벤트 유형은 제공되지 않습니다. 일부 이벤트는 델리게이트 객체로 전송되거나 사용자가 제공 한 블록으로 전달됩니다.

| 이벤트 타입                                   | 전달            | 비고                                       |
| ---------------------------------------- | ------------- | ---------------------------------------- |
| Touch                                    | 이벤트가 발생한 뷰 객체 | 뷰는 응답 객체입니다. 뷰에 의해 처리되지 않은 모든 터치 이벤트는 처리를 위해 응답 체인에 전달됩니다. |
| Remote control,<br />Shake motion events | 첫 번째 응답자 객체   | 원격 제어 이벤트는 미디어 재생을 제어하기위한 것으로, 헤드폰 및 기타 액세서리로 생성됩니다. |
| Accelerometer<br />Magnetometer<br />Gyroscope | 사용자 지정 객체     | 가속도계, 자력계 및 자이로 스코프 하드웨어와 관련된 이벤트는 사용자가 지정하는 객체로 전달됩니다. |
| Location                                 | 사용자 지정 객체     | 핵심 위치 프레임 워크를 사용하여 위치 이벤트를 수신하도록 등록하십시오. |
| Redraw<br />(화면 갱싱)                      | 갱신이 필요한 객체    | 다시 그리기 이벤트는 이벤트 객체를 포함하지 않지만 단순히 그 자체를 그리기 위해 뷰를 호출하는 것입니다. |

### 4. Execution States for Apps

 언제든지 앱은 표에 나열된 상태 중 하나에 있습니다. 시스템은 시스템 전체에서 일어나는 작업에 응답하여 앱을 상태에서 상태로 이동시킵니다.

| State       | Description                              |
| ----------- | ---------------------------------------- |
| Not running | 앱이 시작되지 않았거나 실행 중이었지만 시스템에서 종료되었습니다.     |
| Inactive    | 앱이 포어그라운드에서 실행 중이지만 현재 이벤트를 수신하지 않습니다. (그것은 다른 코드를 실행하고 있을지도 모른다.) 앱은 대개 다른 상태로 이동 할 때 잠깐 동안이 상태에 머문다. |
| Active      | 앱이 포어그라운드에서 실행 중이며 이벤트를 수신하고 있습니다. 이것은 포어그라운드 앱을위한 정상 모드입니다. |
| Background  | 앱이 백그라운드에서 코드를 실행하고 있습니다. 대부분의 앱은 Suspended 상태 이전에 잠시 이 상태가됩니다. 그러나 추가 실행 시간을 요청하는 앱은 잠시동안만 이 상태로 유지 될 수 있습니다. 또한 백그라운드로 직접 실행되는 앱은 비활성 상태 대신이 상태로 전환됩니다. 백그라운드에서 코드를 실행하는 방법에 대한 자세한 내용은 백그라운드 실행을 참조하십시오. |
| Suspended   | 앱이 백그라운드에 있지만 코드를 실행하지 않습니다. 시스템은 앱을 자동으로이 상태로 이동시키고 그렇게하기 전에 앱에 알리지 않습니다. Suspended상태에서 앱은 메모리에 남아 있지만 코드는 실행하지 않습니다. 메모리 부족 상태가 발생하면 시스템이 Suspended된 앱을 예고없이 삭제하여 포 그라운드 앱에 더 많은 공간을 확보 할 수 있습니다. |

* inactive와 active 상태를 합쳐 Foreground라고 합니다.

예를 들어 사용자가 홈 버튼을 누르면 전화가 걸리거나 다른 여러 가지 중단이 발생하면 현재 실행중인 앱이 응답으로 상태가 변경됩니다. 그림은 상태에서 상태로 이동할 때 앱이 수행하는 경로를 보여줍니다.



- Application의 생명주기 사이 사이 호출되는 메소드들로 이 메소드들을 이용해 어플리케이션의 생명주기에 따른 앱의 관리를 할 수 있다.

  ```
  1. (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      => 어플리케이션이 처음 실행될 때. (처음 메모리상에 올라가게 될 때를 말함)
      => 앱의 화면이 사용자에게 보여지기 직전에 최종 초기화 작업을 진행할 수 있습니다.
      
  2. (void)applicationDidBecomeActive:(UIApplication *)application
      => 어플리케이션이 활성화 될 때, 
      => 즉 didFinishLaunchingWithOption 호출 직후, 어플리케이션이 백그라운드로 돌아갔다가 다시 불러질 때 호출
      => 안드로이드의 onResume처럼 일시적으로 저장했던 앱의 상태를 복원할 때 필요한 코드를 기입해두면 될것 같다.
       
  3. (void)applicationWillResignActive:(UIApplication *)application
      => 어플리케이션이 백그라운드로 들어가기 직전(홈버튼을 누른 직후)에 호출
      => 안드로이드의 onPause처럼 백 그라운드로 들어갔다가 언제 종료될지 모르는 앱의 현재 상태를 저장하기 위해서 준비하는 코드를 기입하는게 좋아 보인다.
      
  4. (void)applicationDidEnterBackground:(UIApplication *)application
      => 어플리케이션이 백그라운드로 완전히 들어갔을 때 호출됨
      => 앱의 백그라운드에 실행중이며 언제든지 suspended 상태가 될 수 있음을 알립니다.
      
  5. (void)applicationWillEnterForeground:(UIApplication *)application
      => 어플리케이션이 다시 활성되 되기 직전에 호출됨
      =>앱이 백그라운드에서 벗어나 포 그라운드로 돌아 왔지만 아직 활성화되지 않았 음을 알 수 있습니다. 
      =>백그라운드 상에서 다시 어플리케이션이 활성되 되면 willEnterForeground 호출 후 applicationDidBecomeActive 호출
         
  6. (void)applicationWillTerminate:(UIApplication *)application 
      => 어플리케이션이 완전히 종료되기 직전에 호출 됨
  ```



### 5. App Termination

 앱은 언제든지 종료될 수 있도록 준비되어야하며 사용자 데이터를 저장하거나 다른 중요한 작업을 수행 할 때까지 기다려서는 안됩니다. 시스템에 의한 종료는 앱 수명주기의 정상적인 부분입니다. 시스템에 의한 종료는 일반적으로 메모리가 부족할 때  suspended의 앱을 종료시키지만,  오작동하거나 응답이 없는 앱을 종료 할 수도 있습니다.

* suspended 상태의 앱은 시스템이 메모리를 회수하기 위해서 종료시킬 때 알림을 받지 못합니다.
* 앱이 현재 백그라운드에서 실행 중지만 suspended 상태가 아닌 경우 시스템은 종료하기 전에applicationWillTerminate :를 호출합니다. 
* 하지만 장치가 재부팅될 때에는 이 메소드를 호출하지 않습니다.

 사용자는 multitasking UI를 이용해서 앱을 종료 시킬 수 도 있습니다. 이 경우 suspended 상태와 마찬가지로 사용자는 알림을 받지 못합니다.



### 6. Threads and Concurrency

 IOS는 앱의 메인 스레드에서 동작하고 필요에 따라 추가적인 스레드를 생성 할 수 있습니다. IOS는 이러한 스레드들을 직접 관리하기 보다는 GCD(Grand Central Dispatch)를 이용해서 관리합니다.

* GCD(Grand Central Dispatch)란, C언어로 되어 있는 스레드 관리 기술로 iOS4 부터 지원하고 있다.
* 또한, GCD와 같은 시점에 등장한 블럭 코딩 기반으로 기존에 사용하던 NSThread, NSOperation 보다 쉽게 스레드 응용 기술을 구현할 수 있도록 지원해준다.

 GCD와 같은 기술을 사용하여 수행 할 작업과 수행 할 작업을 정의 할 수 있지만, 사용 가능한 CPU에서 해당 작업을 가장 효과적으로 실행하는 방법을 시스템이 결정하도록 할 수 있습니다. 시스템이 스레드 관리를 처리하도록하면 작성해야하는 코드가 단순 해지고 코드의 정확성을 보장하기가 쉬워지고 전반적인 성능이 향상됩니다.

 스레드와 동시성에 대해 생각할 때 다음을 고려하십시오.

* 뷰, Core Animation 및 다른 많은 UIKit 클래스와 관련된 작업은 일반적으로 메인 스레드에서 발생해야합니다. 이 규칙에는 몇 가지 예외가 있습니다. 예를 들어 이미지 기반 조작은 종종 백그라운드 스레드에서 발생할 수 있지만 의심 스러울 때는 작업이 주 스레드에서 발생해야한다고 가정합니다.
* 긴 작업은 항상 백그라운드 스레드에서 수행해야합니다. 네트워크 액세스, 파일 액세스 또는 대용량 데이터 처리와 관련된 작업은 모두 GCD 또는 작업 객체를 사용하여 비동기 적으로 수행해야합니다.
* 메인 스레드에서 앱 시작시 작업을 최소화 하십시오. 실행시 앱은 가능한 한 빨리 사용자 인터페이스를 설정할 수있는 시간을 사용해야합니다. 앱시 실행되고 최대한 빨리 사용자가 사용 할 수 있는 Active 상태로 만들기 위해서입니다. 주 스레드에서 사용자 인터페이스 설정에 기여하는 작업만 수행해야합니다. 다른 모든 작업은 비동기 적으로 실행해야하며 결과가 준비되는 즉시 사용자에게 표시됩니다.

## chapter 3. Background Execution

