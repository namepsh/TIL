## 일렉트론이란?
 node 기반으로 웹 기술을 활용한 테스크탑 앱 개발 도구

## main
- api 통신을 모아 두는 곳
- module.exports → 외부에서도 함수를 호출해서 사용 가능하게 만들어 주는 함수
- mainForm → mian_home.js에서 보내 준 main 고정 레이아웃
- ipcMain: 렌더러에서 보낸 응답에 대한 처리를 함

## renderer
- 프론트 역할을 하며 이벤트 처리를 하는 곳
- IPC.send: 렌더러 쪽에서 main(ipcMain)으로 보내는 것
- IPC.receive: ipcMain에서 반환한 것을 렌더러에서 받는 것

## html
- 뷰 뼈대