# Using Responders and the Responder Chain to Handle Events

Learn how to handle events that propagate through your app.

앱을 통해 전파되는 이벤트를 처리하는 방법에 대해 알아보자.

Framework

- UIKit

On This Page

- [Overview](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events#overview)
- [See Also](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events#see-also)

## Overview

Apps receive and handle events using *responder objects*. A responder object is any instance of the [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder) class, and common subclasses include [`UIView`](https://developer.apple.com/documentation/uikit/uiview), [`UIViewController`](https://developer.apple.com/documentation/uikit/uiviewcontroller), and [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication). 

Responders receive the raw event data and must either handle the event or forward it to another responder object. When your app receives an event, UIKit automatically directs that event to the most appropriate responder object, known as the *first responder*.

Unhandled events are passed from responder to responder in the active *responder chain*, which is the dynamic configuration of your app’s responder objects. [Figure 1](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events#3004381) shows the responders in an app whose interface contains a label, a text field, a button, and two background views. The diagram also shows how events move from one responder to the next, following the responder chain.

앱은 *응답 객체를* 사용하여 이벤트를 수신하고 처리 *합니다* . 응답자 개체는 모든 인스턴스 [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder)클래스, 공통 서브 클래스에는 [`UIView`](https://developer.apple.com/documentation/uikit/uiview), 하고 . 응답자는 원시 이벤트 데이터를 수신하고 이벤트를 처리하거나 다른 응답자 오브젝트로 전달해야합니다. 앱이 이벤트를 받으면 UIKit은 자동으로 해당 이벤트를 *첫 번째 응답자* 라고하는 가장 적합한 응답자 개체로 보냅니다 .[`UIViewController`](https://developer.apple.com/documentation/uikit/uiviewcontroller)[`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)

처리되지 않은 이벤트는 응답자에서 응용 프로그램 응답자 객체의 동적 구성 인 활성 *응답자 체인* 의 응답자로 전달됩니다. [그림 1](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events#3004381) 은 인터페이스의 레이블, 텍스트 필드, 버튼 및 두 개의 배경보기가 포함 된 앱의 응답자를 보여줍니다. 또한 다이어그램은 이벤트가 응답자 체인을 따라 한 응답자에서 다음 응답으로 이동하는 방법을 보여줍니다.

Figure 1

 

Responder chains in an app



If the text field does not handle an event, UIKit sends the event to the text field’s parent `UIView` object, followed by the root view of the window. From the root view, the responder chain diverts to the owning view controller before directing the event to the window. If the window cannot handle the event, UIKit delivers the event to the `UIApplication` object, and possibly to the app delegate if that object is an instance of `UIResponder` and not already part of the responder chain.

텍스트 필드가 이벤트를 처리하지 않으면 UIKit은 이벤트를 텍스트 필드의 부모 `UIView`객체 로 보내고 그 다음에 윈도우의 루트보기를 보냅니다 . 루트보기에서 이벤트를 창에 보내기 전에 응답자 체인이 소유 한보기 컨트롤러로 전환됩니다. 창에서 이벤트를 처리 할 수없는 경우 UIKit `UIApplication`은 해당 객체가 `UIResponder`응답 자 체인 의 인스턴스 이고 아직 응답자 체인의 일부가 아닌 경우 이벤트를 객체 및 응용 프로그램 대리인 에게 전달합니다 

### Determining an Event's First Responder

UIKit designates an object as the first responder to an event based on the type of that event. Event types include:

| Event type            | First responder                           |
| --------------------- | ----------------------------------------- |
| Touch events          | The view in which the touch occurred.     |
| Press events          | The object that has focus.                |
| Shake-motion events   | The object that you (or UIKit) designate. |
| Remote-control events | The object that you (or UIKit) designate. |
| Editing menu messages | The object that you (or UIKit) designate. |



Note

Motion events related to the accelerometers, gyroscopes, and magnetometer do not follow the responder chain. Instead, Core Motion delivers those events directly to the designated object. For more information, see [Core Motion Framework](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/iPhoneOSTechOverview/CoreServicesLayer/CoreServicesLayer.html#//apple_ref/doc/uid/TP40007898-CH10-SW27)

Controls communicate directly with their associated target object using action messages. When the user interacts with a control, the control sends an action message to its target object. Action messages are not events, but they may still take advantage of the responder chain. When the target object of a control is `nil`, UIKit starts from the target object and traverses the responder chain until it finds an object that implements the appropriate action method. For example, the UIKit editing menu uses this behavior to search for responder objects that implement methods with names like [`cut(_:)`](https://developer.apple.com/documentation/uikit/uiresponderstandardeditactions/2354193-cut), [`copy(_:)`](https://developer.apple.com/documentation/uikit/uiresponderstandardeditactions/2354191-copy), or [`paste(_:)`](https://developer.apple.com/documentation/uikit/uiresponderstandardeditactions/2354189-paste).

Gesture recognizers receive touch and press events before their view does. If a view's gesture recognizers fail to recognize a sequence of touches, UIKit sends the touches to the view. If the view does not handle the touches, UIKit passes them up the responder chain. For more information about using gesture recognizer’s to handle events, see [Handling UIKit Gestures](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/handling_uikit_gestures).



### Determining Which Responder Contained a Touch Event

UIKit uses view-based hit-testing to determine where touch events occur. Specifically, UIKit compares the touch location to the bounds of view objects in the view hierarchy. The [`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest) method of [`UIView`](https://developer.apple.com/documentation/uikit/uiview) traverses the view hierarchy, looking for the deepest subview that contains the specified touch, which becomes the first responder for the touch event.

Note

If a touch location is outside of a view’s bounds, the [`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest)method ignores that view and all of its subviews. As a result, when a view’s [`clipsToBounds`](https://developer.apple.com/documentation/uikit/uiview/1622415-clipstobounds) property is `false`, subviews outside of that view’s bounds are not returned even if they happen to contain the touch. For more information about the hit-testing behavior, see the discussion of the [`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest) method in [`UIView`](https://developer.apple.com/documentation/uikit/uiview).

When a touch occurs, UIKit creates a [`UITouch`](https://developer.apple.com/documentation/uikit/uitouch) object and associates it with a view. As the touch location or other parameters change, UIKit updates the same `UITouch` object with the new information. The only property that does not change is the view. (Even when the touch location moves outside the original view, the value in the touch’s [`view`](https://developer.apple.com/documentation/uikit/uitouch/1618109-view) property does not change.) When the touch ends, UIKit releases the `UITouch` object.

### Altering the Responder Chain

You can alter the responder chain by overriding the [`next`](https://developer.apple.com/documentation/uikit/uiresponder/1621099-next) property of your responder objects. When you do this, the next responder is the object that you return.

Many UIKit classes already override this property and return specific objects, including:

- [`UIView`](https://developer.apple.com/documentation/uikit/uiview) objects. If the view is the root view of a view controller, the next responder is the view controller; otherwise, the next responder is the view’s superview.
- [`UIViewController`](https://developer.apple.com/documentation/uikit/uiviewcontroller) objects.
  - If the view controller’s view is the root view of a window, the next responder is the window object.
  - If the view controller was presented by another view controller, the next responder is the presenting view controller.
- [`UIWindow`](https://developer.apple.com/documentation/uikit/uiwindow) objects. The window's next responder is the [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)object.
- [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) object. The next responder is the app delegate, but only if the app delegate is an instance of [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder) and is not a view, view controller, or the app object itself.



## See Also

### First Steps

```
class UIResponder
```

An abstract interface for responding to and handling events.

```
class UIEvent
```

An object that describes a single user interaction with your app.



### 이벤트의 첫 번째 응답자 결정

UIKit은 해당 이벤트의 유형을 기반으로 이벤트에 대한 첫 번째 응답자로 객체를 지정합니다. 이벤트 유형은 다음과 같습니다.

| 이벤트 유형      | 최초 반응자                                |
| ---------------- | ------------------------------------------ |
| 터치 이벤트      | 터치가 발생한 뷰입니다.                    |
| 보도 자료        | 포커스를 가지는 오브젝트입니다.            |
| 흔들기 이벤트    | 사용자 (또는 UIKit)가 지정하는 객체입니다. |
| 원격 제어 이벤트 | 사용자 (또는 UIKit)가 지정하는 객체입니다. |
| 메뉴 메시지 편집 | 사용자 (또는 UIKit)가 지정하는 객체입니다. |



노트

가속도계, 자이로 스코프 및 자력계와 관련된 동작 이벤트는 응답 체인을 따르지 않습니다. 대신 Core Motion은 해당 이벤트를 지정된 객체로 직접 전달합니다. 자세한 내용은 [Core Motion Framework를](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/iPhoneOSTechOverview/CoreServicesLayer/CoreServicesLayer.html#//apple_ref/doc/uid/TP40007898-CH10-SW27) 참조하십시오.

컨트롤은 액션 메시지를 사용하여 연결된 대상 객체와 직접 통신합니다. 사용자가 컨트롤과 상호 작용할 때 컨트롤은 대상 메시지에 작업 메시지를 보냅니다. 액션 메시지는 이벤트는 아니지만 리스폰 더 체인을 여전히 활용할 수 있습니다. 컨트롤의 대상 객체가 `nil`UIKit이면 대상 객체에서 시작하여 적절한 작업 메서드를 구현하는 객체를 찾을 때까지 응답 체인을 탐색합니다. 예를 들어, 메뉴 작성, UIKit은 같은 이름을 가진 방법을 구현 응답자의 개체를 검색하려면이 동작을 사용 [`cut(_:)`](https://developer.apple.com/documentation/uikit/uiresponderstandardeditactions/2354193-cut), [`copy(_:)`](https://developer.apple.com/documentation/uikit/uiresponderstandardeditactions/2354191-copy)또는를 [`paste(_:)`](https://developer.apple.com/documentation/uikit/uiresponderstandardeditactions/2354189-paste).

제스처 인식기는보기 전에 터치 및 프레스 이벤트를 수신합니다. 뷰의 제스처 인식기가 일련의 터치를 인식하지 못하면 UIKit은 뷰에 터치를 전송합니다. 뷰가 터치를 처리하지 않으면 UIKit은이를 응답 체인에 전달합니다. 제스처 인식기를 사용하여 이벤트를 처리하는 방법에 대한 자세한 내용은 [UIKit 동작 처리를](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/handling_uikit_gestures) 참조하십시오 .



### 터치 이벤트를 포함하는 응답자 결정

UIKit은 뷰 기반 히트 테스트를 사용하여 터치 이벤트가 발생하는 위치를 결정합니다. 특히 UIKit은 터치 위치를 뷰 계층 구조의 뷰 객체 범위와 비교합니다. 이 방법은 지정된 터치를 포함하는 가장 깊은 하위 뷰를 찾고 뷰 계층 을 가로 지르며 터치 이벤트의 첫 번째 응답자가됩니다.[`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest)[`UIView`](https://developer.apple.com/documentation/uikit/uiview)

노트

터치 위치가 뷰의 범위 밖에있는 경우이 메서드는 해당 뷰와 모든 하위 뷰를 무시합니다. 결과적으로 뷰 속성이있는 경우 해당 뷰의 경계 외부에있는 하위 뷰는 터치가 포함 된 경우에도 반환되지 않습니다. 히트 테스트 동작에 대한 자세한 내용은 논의 참조 의 방법을 .[`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest)[`clipsToBounds`](https://developer.apple.com/documentation/uikit/uiview/1622415-clipstobounds)`false`[`hitTest(_:with:)`](https://developer.apple.com/documentation/uikit/uiview/1622469-hittest)[`UIView`](https://developer.apple.com/documentation/uikit/uiview)

터치가 발생하면 UIKit은 [`UITouch`](https://developer.apple.com/documentation/uikit/uitouch)객체를 만들고 뷰와 연관시킵니다. 터치 위치 또는 다른 매개 변수가 변경되면 UIKit은 동일한 `UITouch`객체를 새 정보로 업데이트합니다 . 변경되지 않는 속성은보기뿐입니다. 터치 위치가 원래 뷰 외부로 이동하더라도 터치 [`view`](https://developer.apple.com/documentation/uikit/uitouch/1618109-view)속성 의 값은 변경되지 않습니다. 터치가 끝나면 UIKit에서 `UITouch`객체를 해제 합니다.

### Responder Chain 변경

[`next`](https://developer.apple.com/documentation/uikit/uiresponder/1621099-next)리스폰 더 객체 의 속성을 재정 의하여 응답 체인을 변경할 수 있습니다 . 이렇게하면 다음 응답자가 반환하는 개체가됩니다.

많은 UIKit 클래스가 이미이 속성을 재정 의하여 다음과 같은 특정 객체를 반환합니다.

- [`UIView`](https://developer.apple.com/documentation/uikit/uiview)사물. 뷰가 뷰 컨트롤러의 루트 뷰인 경우 다음 응답자는 뷰 컨트롤러입니다. 그렇지 않으면 다음 응답자는보기의 수퍼 뷰입니다.
- [`UIViewController`](https://developer.apple.com/documentation/uikit/uiviewcontroller) 사물.
  - 뷰 컨트롤러의 뷰가 윈도우의 루트 뷰인 경우 다음 응답자는 윈도우 객체입니다.
  - 뷰 컨트롤러가 다른 뷰 컨트롤러에 의해 제공 되었다면, 다음 응답자는 프리젠 테이션 뷰 컨트롤러입니다.
- [`UIWindow`](https://developer.apple.com/documentation/uikit/uiwindow)사물. 윈도우의 다음 응답자가 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)객체입니다.
- [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)목적. 다음 응답자는 앱 대리인이지만, 앱 대리인이 [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder)보기,보기 컨트롤러 또는 앱 객체 자체 의 인스턴스 가 아닌 경우에만 해당 됩니다.



## 참고 사항

### 첫 단계

```
class UIResponder
```

이벤트에 응답하고 처리하기위한 추상 인터페이스.

```
class UIEvent
```

앱과의 단일 사용자 상호 작용을 설명하는 객체입니다.