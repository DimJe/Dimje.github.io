---
layout: post
title: 안드로이드 | [안드로이드] LiveData의 setValue와 postValue
subtitle: kotlin
categories: 안드로이드
tags: [Android,안드로이드,kotlin]
---

# LiveData의 setValue와 postValue

LiveData의 setValue와 postValue에 대해서 알아보겠습니다.

## LiveData?
LiveData는 AAC에서 제공하는 라이브러리 중 하나로 관찰 가능한 데이터 홀더 클래스입니다.<br>
주로 mvvm 패턴에서 ViewModel과 같이 쓰입니다.<br>
LiveData에 값을 설정하는 함수는 setValue와 postValue 두가지가 있습니다.

## setValue

setValue는 mainThread에서 실행되는 함수입니다. 그렇기 때문에 곧 바로 getValue를 사용해서 값을 얻어오면 변경된 값을 얻어올 수 있습니다.

## postValue

postValue는 background에서 실행되는 함수입니다.<br>
background에서 실행되다가 mainThread에서 값을 post하는 방식으로 진행됩니다.
```kotlin
new Handler(Looper.mainLooper()).post(() -> setValue())
```
내부적으론 위의 코드가 실행됩니다.

따라서 곧 바로 getValue를 사용해도 변경된 값을 얻어오지 못할수도 있습니다.

```java
    liveData.postValue("a");
    liveData.setValue("b");
```
위의 코드를 보면 liveData의 값은 'a'가 됩니다. 'b'가 먼저 설정되고, postValue로 부터 'a'로 변경됩니다.

## 정리

둘의 차이점은 어떤 Thread에서 실행되는지에 대한 차이라고 생각합니다.<br>
값을 변경하는 Thread에 따라서 올바른 함수를 사용하면 좋을 것 같습니다.

