# 배포하기 - Firebase 사용

초보자도 쉽게할 수있는 Firebase를 사용하도록 하자. 방법은 아래와 같다.

1. 구글로그인 후 [Firebase 홈페이지](https://console.firebase.google.com/?hl=ko)에 접속을 한다.
2. 시작하기 버튼을 누른다.
3. 프로젝트 추가 클릭
4. 프로젝트 이름 지정 후 약관동의 계속하기 버튼 클릭
5. 구글애널리틱스 (트래픽측정) 사용여부 체크 후 프로젝트 만들기 클릭
6. 생성 완료 후 계속버튼 클릭
7. 왼쪽메뉴에서 빌드 안에 Hosting버튼 클릭
8. 시작하기 클릭
9. npm install ~~ 부분 복사 후 cmd 관리자 권한 실행
10. 설치 완료 후 다음 클릭
11. firebase login cmd창에서 실행
12. Allow Firebase to collect ... (Y/n) 이라고 뜨면 오류 정보수집동의서 이니 Y누르고 엔터를 치면 크롬 계정선택 창이 뜬다.
13. 크롬로그인을 완료하고 엑세스 허용을 눌러준다.
14. 성공 화면이 나오고 cmd창을 확인하면 + Success! Logged in as 라는 멘트를 볼 수 있다.
15. cmd에서 firebase init 을 실행한다.
16. 레디 명령어가 뜨면 y
17. 두번째는 Hosting: Configure files for ... 스페이스바 선택 후 엔터
18. please select an option 에서는 프로젝트가 이미 만들어져 있으니 Use an existing project 위치하고 엔터
19. 프로젝트 선택 후 엔터
20. What do you want to use as your public directory? 라고 퍼블릭 디렉터리가 뭐야라고 물어보는데, 우리는 build를 배포할 것이기 때문에 build를 입력하고 엔터
21. Configure as a single page app 싱글페이지 앱으로 만들꺼냐 물어보면 y
22. Set up --- Github라고 뜨는데 y
23. File build/index.html already exists. Overwrite? 는 y
24. 깃허브 연결로 뜨는데 동기화 진행 -> 완료 해준다.
25. For which GitHub repository would you like to set up a GitHub workflow? 라고 리포지토리를 물어보는데 MyoungSeob-Pohang/React-Diary-Upgrade를 입력해주었다
26. Set up the workflow to run a build script before every deploy? 는 n
27. Set up automatic deployment to your site's live channel when a PR is merged? 도 n
28. What is the name of the GitHub branch associated with your site's live channel? 은 main
29. - Firebase initialization complete! 완료
30. 프로젝트에 .firebaserc 와 firebase.json 생성된 것을 확인
31. 호스팅설정으로 다시 이동하여 다음 클릭
32. 콘솔로 이동 클릭
33. 고급에 Firebase 호스팅의 여러 사이트 지원에 다른 사이트 추가 클릭
34. 내가 정한 도메인 입력 (moonsbi-diary) 후 사이트 추가 클릭
35. 코드의 firebase.json에 site를 만들어 ㄷ처음 작성한 프로젝트 명 (moonsbi-react-project)을 입력한다.

```
{
  "hosting": {
    "site": "moonsbi-react-project",
    "public": "build",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

36. 다시 빌드 npm run build
37. firebase deploy 터미널에서 실행
38. 완료되면 cmd에 뜨는 호스팅 url에 접속 ( https://moonsbi-react-project.web.app )
39. 프로젝트 배포 완료
