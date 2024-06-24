실무에서의 시행착오에 대해 스스로 문제를 해결한 능력과 경험 기록  
사용자들은 내가 개발한 의도대로 행동하지 않기에 의도치 않은 예외처리에 대한 고민   
[스택오버플로우](https://stackoverflow.com/)    
[안드로이드 백과사전](https://www.youtube.com/watch?v=WlJszSmK_es&t=0s)

## 1. 문제상황
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/5e6c28a8-ba00-4e4a-8ff6-d03ae43332b7)
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/fe0e91aa-c8de-4d8d-80b7-87234b469da3)  
```
동일한 Url을 사용했지만 Android WebView와 chrome브라우저에 표시되는 UI가 서로 다르다.
```
## 1. 문제상황의 코드
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/26f6d088-7b8a-4976-af2a-fa74b294511e)
## 1. 해결 방안 제시
```
android view 에 modifier 추가
웹뷰에 대한 사이즈 디버깅 진행
Android view + webview 조합에서 크기 지정 안해주면 프론트쪽에서 높이에 대한 처리가 0으로 잡힌다.
```
## 2. 문제상황 -  포트원을 사용하여 결제 시스템 구현
```
쿠폰을 개수를 조절가능하게 하여 낱개로 구매가능하게 한다.
구매를 하면 관리자한테 로그들이 있는데 취소하면 구매한게 세트로 취소가 된다.
```
## 2. 해결 방안 제시 
```
부분취소에 대해서는 결제대행사 관할이라 포트원에 직접 문의
```
## 3. 문제상황 - 네이버 소셜 로그인 구현 시 토큰 관리
```
구글이나 애플로그인 구현할떄는 토큰 유효한지 확인하려고 백엔드 개발자에게 토큰 확인을 맡김
네이버는 그냥 프론트에서 처리 가능한가?
```
## 3. 해결
```
-> 카카오, 네이버 토큰유효한지 sdk 로 판단가능
```
## 4. 질문
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/3eb02556-36ae-4a00-a0c5-350f26bf3b9f)
```
private val binding get() = _binding!!

!!<- 매번 non-null 처리를 하지 않게 하기 위해
_binding의 값이 들어왔을 때 해당 value 값을 받기 위함이다.

_binding = null

프래그먼트의 상태를 보지않고 외부에서 프래그먼트 함수(ui작업이 포함된) 를 호출하거나
프래그먼트내에 지연된 작업에서 destoryview이후 ui작업 하려고 하는 경우

_binding = null 이후에 정리하지 못한 지연된 작업이( 웹뷰의 콜백 네트웍 콜백등등) 완료되고
작업완료 코드에 ui작업이 필요해 binding에 접근하는 경우가 대부분이다.
```
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/28db7a14-1254-4b56-ae41-fe41f0e975ab)
```
onCreateView에서 뷰 바인딩
onViewCreated 뷰가 생성되고 난 후 UI 요소들을 연결한다.
```
## 4. 질문에 대한 튜터님의 답변
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/82577c3a-896a-4754-8bb4-6824026635a3)
```kotlin
class MainActivity : AppCompatActivity() { 
    private lateinit var binding: ActivityMainBinding 
    override fun onCreate(savedInstanceState: Bundle?) {    
        super.onCreate(savedInstanceState) binding =    
        ActivityMainBinding.inflate(layoutInflater) setContentView(binding.root) }  
    }

```
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/d5ce5c32-38bb-4eda-b1d5-182adbabffe8)
```kotlin
class WeatherHomeFragment : Fragment() { 
     private var _binding: FragmentWeatherHomeBinding? = null 
    // This property is only valid between onCreateView and// onDestroyView. 
private val binding get() = _binding!! 

    // 1. onCreateView에서 뷰를 생성하고 binding을 초기화 함
override fun onCreateView( inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle? ): View? { 
        _binding = FragmentWeatherHomeBinding.inflate(inflater, container, false)         
        return binding.root 
    } 
   // 2. onViewCreated에서 뷰가 생성된 이후 UI 작업을 진행
override fun onViewCreated(view: View, savedInstanceState: Bundle?) { super.onViewCreated(view, savedInstanceState)     
        binding.buttonFirst.setOnClickListener {   
      findNavController().navigate(R.id.action_FirstFragment_to_SecondFragment)    
        } 
    } 

   // 3. onDestroyView에서 뷰가 소멸될 때 binding을 해제함
override fun onDestroyView() { 
         super.onDestroyView() _binding = null } }
```
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/af07fec5-304b-4236-ac06-6b9ec4638ca5)



## 5. 경험 - SharedPreference를 사용하여 앱 내에 데이터를 저장하고자 한다.
1. PreferenceUtil 클래스 파일 생성
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/ea69f208-4ec7-4878-87ef-047ddf96f7cd)
```
데이터베이스에 저장할 데이터의 자료형과 함수를 호출할 때 형식(틀, 매개변수)을 지정해줄 수 있다.
getString, getBoolean 만 정의하였지만 추가로 Float, Int, StringSet 등을 정의하여 사용할 수 있다.
```
2. 데이터를 저장할 클래스에 싱글톤 패턴으로 preference 객체 생성
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/1ad642a7-bec8-48cb-ba4b-472873143695)
```
SharedPreference 데이터베이스를 사용할 클래스에 싱글톤 패턴으로 preferences 객체를 생성해주고 초기화한다.
```
3. 데이터베이스에 데이터 저장하기
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/f866fe5e-409c-45e9-8deb-91142c3861a3)
```
저장할 때는 PreferenceUtil에 정의한 setBoolean 메서드를 사용한다.
예시는 스위치의 체크 유무를 확인하여 체크 시(활성화 시) 데이터베이스의 noti 키값에 true Boolean 값을 넣어주는 예시이다. 

저장되는 위치는 내장 메모리의 앱 폴더에 XML 파일로 저장된다.
```
4. 데이터베이스의 데이터를 가져오기
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/d9b9df8f-d77b-4f1a-8286-429d87a3f5ca)
```
가져올 때는 싱글톤 패턴으로 정의한 클래스.객체를 사용하여 getBoolean 메서드를 호출한다.
이렇게 호출한 값을 저장할 변수를 선언하여 넣어준다.
```
```
ps. 앱의 설정을 구성할 때 AndroidX의 Preference를 이용할 것을 권장하고 있다. AndroidX 프리펀스 사용 선언(exclude lifecycle, lifecycle-viewmodel)
하고 xml 폴더 밑에 settings.xml 파일에 SwitchPreferencCompat 스위치를 사용하는 것이 원칙이다.
```
## 6. RecyclerView를 생성할 때 화면을 가득 채우고 싶은 상황에 대한 질문
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/83052449-ff40-492f-ba97-f63db1ca932d)
```
layout_width와 layout_height가 match_parent일 것이라고 예상했으나 0dp로 줘야 하는 이유 질문
```
## 6. 답변
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/e4f60ae3-8781-4d1b-b934-3d922b726736)     
```
Constraint 레이아웃 안에서 width와 height 속성의 값으로 match_parent를 주는 것을 권장하지 않는다.
parent로 지정하고자 한다면 match_constraint 라는 특수한 행동으로 0dp를 주면 된다.
```
[Constraint Layout 개발 문서](https://developer.android.com/reference/androidx/constraintlayout/widget/ConstraintLayout#dimensions-constraints)     
## 7. 이미지뷰 clipToOutline 속성 경험
```
ImageView의 백그라운드에 모서리를 둥글게하는 xml 레이아웃을 추가한 후 clipToOutline="true"로 지정할 때 오류 발생

API 31이상부터 적용 가능 -> minSdk의 값 수정
```
## 7. 분석 답변
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/057c9985-2d3c-4e26-a85a-aaacda08458c)

## 8. sealed class 란?
[sealed class 알아보기](https://velog.io/@haero_kim/Kotlin-Sealed-Class-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)
```
sealed 클래스는 자기 자신이 추상 클래스이고, 자신을 상속받는 여러 서브 클래스들을 가질 수 있다.
이를 사용하면 enum 클래스와 달리 상속을 지원하기 때문에, 상속을 활용한 풍부한 동작을 구현할 수 있다.

Sealed Class 의 이점
sealed 클래스의 서브 클래스 각각에 대해 여러 개의 인스턴스 생성 가능
때문에 상태값을 유동적으로 변경할 수 있음
sealed 클래스의 계층을 생성할 수 있음
Sealed Class 는 Enum Class 의 확장판과도 같다. 제한적인 계층관계를 효과적으로 표현할 수 있고, 이에 따라 when 문 사용 시 효과적으로 사용할 수 있다.
```

## 9. @Parcelize 란?
[Parcelize 란?](https://onlyfor-me-blog.tistory.com/entry/Android-Parcelize%EB%9E%80)
```
앱을 개발하다 보면 다른 화면으로 넘어갈 때 데이터를 전달해야 할 수 있다.
 앱에서 데이터를 전달해야 한다면 인텐트에 putExtra()로 넣어서 이동할 화면으로 보내고,
 해당 화면에서 받아서 사용하는 게 가장 기초적이면서 일반적인 방법이라고 할 수 있다.

이 때 보내야 하는 데이터가 객체 형태라면 어떻게 해야 할까?
data class를 보내야 하는 경우 그대로 putExtra()를 사용하면 컴파일 에러가 발생한다.
에러 내용은 "None of the following functions can be called with the arguments supplied."다.

Parcelable은 안드로이드의 IPC(Inter-Process Communication)를 위해 만들어진 인터페이스다.
IPC는 같은 시스템에서 실행되는 서로 다른 프로세스 간 데이터를 전송하는 매커니즘이다.
안드로이드에선 이걸로 다른 앱 또는 앱 안의 컴포넌트 간에 데이터를 전달해서 통신할 수 있게 된다.
이 인터페이스를 사용하면 객체를 이동시키기 위한 방법인 마샬링(또는 직렬화)를 정의할 수 있게 된다.
그러나 마샬링 정의 과정이 복잡하기 때문에 에러가 발생할 확률이 올라가고, 코드가 길어지기 때문에 가독성도 떨어질 수 있다.
```
#### Parcelize Plugin
```kotlin
plugins {
    id 'kotlin-android-extensions'
}
```
```
Parcelable과 @Parcelize다. Serializable에 비해 속도는 빨라졌고, 좀 더 코드 가독성을 올릴 수 있게 된 것이다.
객체를 인텐트에 담아서 보내야 할 경우 Parcelable과 @Parcelize를 사용하면
편하고 빠르게 다른 화면으로 이동시킬 수 있으니 필요한 경우 잘 사용하면 좋을 것이다.
```
## 10. R.java의 의미
```
[ R. 의 의미 ]
- R. : R은 Resource고 안드로이드 시스템에서 R.java 파일로 리소스 관리를 해주는 것.
즉, R. 은 R.java를 의미!!

- R.java : res 폴더에 있는 리소스를 보고 자동으로 만듦 (개발자가 만드는 파일 아님)
- 이제 R.java 파일을 보여주지 않음
  => 개발자가 직접 건드리지 않고 내부에서 리소스를 등록하기 위해

- 리소스 파일 규칙
 * res 하위의 폴더명은 지정된 폴더명을 사용해야 함
 * 각 리소스 폴더에 다시 하위 폴더를 정의할 수 없음
 * 리소스 파일명은 자바의 이름 규칙을 위해할 수 없음
 * 리소스 파일명에서 알파벳 대문자를 이용할 수 없음 => 두 단어 연결 시 언더바(_) 사용
```
## 11. Map 사용법
[Map 사용법](https://math-coding.tistory.com/229)
```

```
## 12. 안드로이드 에뮬레이터 Location 흰 화면 문제 상황
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/4a9621bf-7b72-4bc9-b620-3748b13e202d)
## 12 해결 에뮬레이터 버전 업그레이드
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/f9efee77-4735-42f1-a69e-29af5a537536)
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/7931bda4-7322-4b16-898d-41dd16d3e30a)
