Android Programing
----------------------------------------------------
### 2017.09.20 7일차

#### 공부정리
____________________________________________________

##### Activity LifeCycle

![Activity LifeCycle](https://github.com/Hooooong/DAY13_Activity_etc/blob/master/image/Activity%20LifeCycle.jpg)

- Activity 의 LifeCycle


     메소드 | 설명
     :----: | :----:
     onCreate() | 액티비티가 생성될 때 호출되며 사용자 인터페이스 초기화에 사용된다
     onRestart() | 액티비티가 멈췄다가 다시 시작되기 바로 전에 호출된다
     onStart() | 액티비티가 사용자에게 보여지기 바로 직전에 호출된다
     onResume() | 액티비티가 사용자와 상호작용하기 바로 전에 호출된다
     onPause() | 다른 액티비티가 보여질 때 호출된다.
     onStop() | 액티비티가 더이상 사용자에게 보여지지 않을 때 호출된다
     onDestroy() | 액티비티가 소멸될 때 호출된다.<br> finish() 메소드가 호출되거나 시스템이 메모리 확보를 위해 액티비티를 제거할 때 호출된다

- Activity 변화

    ![B Activity 가 화면에 올라오면](https://github.com/Hooooong/DAY13_Activity_etc/blob/master/image/Activity1.PNG)

    - B Activity 가 A Activity 를 가리기 시작하면 A Activity는 onPause()이 된다.
    - 완전히 가리기 전에 B Activity 가 종료되면 A Activity는 onPause() -> onReasum() 이 되고 다시 RUNNING 상태로 돌아간다.

    ![B Activity 가 화면에 가리게 되면](https://github.com/Hooooong/DAY13_Activity_etc/blob/master/image/Activity2.PNG)

    - B Activity 가 A Activity 를 완전히 가리면 A Activity 는 onPause()->onStop() 이 된다.
    - B Activity 가 종료되면 A Activity 는 onStop() -> onRestart() -> onStart() -> onResume() 이 되고 다시 RUNNING 상태로 돌아간다.


- 참조 : [Activity LifeCycle](https://developer.android.com/guide/components/activities/activity-lifecycle.html)

##### Activity Result

- startActivityForResult ,onActivityResult ,setResult

    > Android startActivity와는 다르게 실행시킨 Activity 의 결과를 받을 수 있다.<br>
    > 결과 값을 통해 Activity 의 초기화 작업이나 다양한 작업을 할 수 있는 이점이 있다.

    ```java
    Intent intent = new Intent(getBaseContext(), 실행시킬Activity.class);

    // 일반적인 intent 실행
    startActivity(intent);

    // 실행 결과를 알 수 있는 intent 실행
    startActivityForResult(intent, 요청식별코드);
    ```

    - 요청식별코드`requestCode`를 통해 실행시킨 intent의 결과를 식별할 수 있다.

    - startActivityForResult를 실행시킨 Activity에서 onActivityResult 를 재정의해야 한다.

    - 호출된 Activity 에 대한 요청결과코드`resultCode`와 `data`를 받아 다양한 결과 처리를 할 수 있다.

    - 호출된 Activity 에서 `data`를 넘겨주기 위해서는 `setResult(resultCode)` 메소드를 사용해야 한다.

    ```java
    /**
     * startActivityForResult 를 통해 호출된 Activity가 종료되는 순간 호출되는 함수
     *
     * @param requestCode : 요청식별코드
     * @param resultCode : 결과 코드 (RESULT_OK, RESULT_CANCEL, ~~)
     * @param data : 호출된 Activity 에서 보낸 data
     */
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        // requestCode 를 통해 실행시킨 Activity의 결과를 알 수 있다.
        switch(requestCode){
          case 요청식별코드1:
            // 요청식별코드1가 끝나면 실행시킬 코드
            if(resultCode == RESULT_OK){
              // 결과 코드가 RESULT_OK 일 떄 실행시킬 코드

              /**
               * 보내준 Data 를 활용하는 코드
               *
               * getStringExtra();
               *  - StringExtra 만 초기값이 null 이기 때문에 설정을 해주지 않아도 되지만
               * getIntExtra(, 초기값);
               *  - Int, Long, Float 등 데이터형은 null 이 없기 때문에 초기값 설정을 해줘야 한다.
               */
              int resultInt = data.getIntExtra("ResultInt", 0);
              String resultString = data.getStringExtra("ResultString");

            }else if(resultCode == RESULT_CANCEL){
              // 결과 코드가 RESULT_CANCEL 일 때 실행시킬 코드
            }
            break;
          case 요청식별코드2:
            // 요청식별코드2가 끝나면 실행시킬 코드
            break;
        }
    }
    ```

- 참조 : [Activity Result](https://developer.android.com/training/basics/intents/result.html?hl=ko#ReceiveResult)

##### Style.xml

- Style.xml 이란?

    > Style.xml 이란 View 또는 창의 모양과 형식을 지정하는 속성 모음이다. <br>
    > 주로 Activity 의 테마 또는 View 들의 모양을 정의할 떄 사용된다.

    - style 은 상속을 받을 수 있으며 각종 속성에 대한 정의를 내려 값을 설정한다.
    - `<style name="스타일 이름" parent="상속받을 상위 스타일"></style>`: 스타일 명과 상위 스타일을 설정할 수 있다.
    - `<item name="style을 고유하게 식별하는 name 사용">설정값</item>` : 요소들을 정의할 수 있다.

- 기본 작성 방법

    ```xml
    <resources>
        <!-- Base application theme. -->
        <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
            <!-- Customize your theme here. -->
            <item name="colorPrimary">@color/colorPrimary</item>
            <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
            <item name="colorAccent">@color/colorAccent</item>
            <item name="android:windowTranslucentNavigation">false</item>
            <item name="android:windowTranslucentStatus">true</item>
            <item name="android:windowContentOverlay">@null</item>
        </style>
    </resources>
    ```

- 상태표시줄에 대한 속성

    ![상태표시줄](https://github.com/Hooooong/DAY13_Activity_etc/blob/master/image/%EC%83%81%ED%83%9C%ED%91%9C%EC%8B%9C%EC%A4%84.png)

    - material Theme 의 상태표시줄 명이다.
    - item name 에 이름을 설정하여 Theme 의 속성을 변경한다.

- 참조 : [스타일 및 테마](https://developer.android.com/guide/topics/ui/themes.html), [머티리얼 테마 사용](https://developer.android.com/training/material/theme.html)
