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
