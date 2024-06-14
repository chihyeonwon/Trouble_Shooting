실무에서의 시행착오에 대해 스스로 문제를 해결한 능력과 경험 기
[스택오버플로우](https://stackoverflow.com/)

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
5. RecyclerView를 생성할 때 화면을 가득 채우고 싶은 상황에 대한 질문
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/83052449-ff40-492f-ba97-f63db1ca932d)
```
layout_width와 layout_height가 match_parent일 것이라고 예상했으나 0dp로 줘야 하는 이유 질문
```
5. 답변

