시행착오에 대해 스스로 문제를 해결한 능력과 경험 = 실력
[스택오버플로우](https://stackoverflow.com/)

## 문제상황
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/5e6c28a8-ba00-4e4a-8ff6-d03ae43332b7)
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/fe0e91aa-c8de-4d8d-80b7-87234b469da3)  
```
동일한 Url을 사용했지만 Android WebView와 chrome브라우저에 표시되는 UI가 서로 다르다.
```
## 문제상황의 코드
![image](https://github.com/chihyeonwon/Trouble_Shooting/assets/58906858/26f6d088-7b8a-4976-af2a-fa74b294511e)
## 해결 방안 제시
```
android view 에 modifier 추가
웹뷰에 대한 사이즈 디버깅 진행
Android view + webview 조합에서 크기 지정 안해주면 프론트쪽에서 높이에 대한 처리가 0으로 잡힌다.
```
