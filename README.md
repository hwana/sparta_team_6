## 프로젝트 개요

- 팀
    - Team명 “봄”
- 프로젝트
    - 프로젝트명 : OurTeamMate
    - 프로젝트 소개 : 팀 멤버들의 자기소개를 작성하여 소개하고 팀원의 소개 글 내용 변경을 페이지에 반영할 수 있는 작업과 팀원이 사라졌을 때(?)를 위한 삭제 기능을 구현하였습니다.



### 디자인

#### 메인 상단
팀원들과 함께 즐겁게 코딩하는 모습을 **Gif**로 노출시켜 페이지를 꾸몄습니다<br><br>
![팀6 Spring](https://github.com/JungHyunMoon/sparta_team_6/assets/120004247/872d6375-4f3a-45c5-a33c-49ef1ed42264)

#### Bootstrap
메인 테마는 Bootstrap을 이용하여 css파일을 받아 Link태그로 빌드 하였습니다.

## 핵심 기능 소개

### Create
<NEW MEMBER> 버튼을 클릭할 시에 Modal을 노출시켜 멤버 카드에 들어갈 내용을 작성할 수 있게 구현하였습니다. 이 때 비밀번호를 함께 작성하여 추후 수정/삭제에 필요한 유효성 검사 또한 고려하였습니다.<br>
![ezgif com-video-to-gif](https://github.com/JungHyunMoon/sparta_team_6/assets/120004247/d64160a7-a720-4849-bcd8-5e518b38bfe0)


### Read
DB는 Firebase를 채택하여 getDocs 메소드를 통해 JSON형태로 가져와 forEach로 돌며 client에 뿌려주었습니다.  
Edit 버튼을 눌러 Modal창을 띄웠을때의 데이터 접근 방식은 다양한 방법을 시도해보고자 두가지 방식으로 data를 가져왔습니다. 첫 번째로는 **$(this).attr("id");**와 같은 노드 직접 접근 방식을 통해 script에 이미 담겨있는 데이터를 넘겨주었습니다. 두 번째로는 **getDoc** 메소드로 **docId**파라미터를 이용해 간단히 구현하였습니다<br>
![Read](https://github.com/JungHyunMoon/sparta_team_6/assets/120004247/3647e425-8f31-43ea-8b88-beefe2b1ff39)

### Update / Delete
수정 및 삭제의 기능의 Modal창을 Edit 버튼을 클릭하여 노출시켜 유저가 멤버 카드를 접근하여 편리하게 편집할 수 있게 구현하였습니다. 수정과 삭제를 시도할 때 data의 고유 ID를 통해 직접 getDoc을 통한 데이터 조회로 DB에 있는 비밀번호와 유저가 입력한 비밀번호가 일치 했을 시에 프로세스가 작동할 수 있게 구현하였습니다.<br>
![Update](https://github.com/JungHyunMoon/sparta_team_6/assets/120004247/e6b921ad-fe83-4851-b564-ea8f4f9cb90d)

## TroubleShooting
- 형상관리
- 개발 초기 코드 양이 많지 않을 때 충돌이 일어나지 않았지만 점점 코드가 길어지고 팀원 각자의 기능 개발에 있어 연관성이 많아짐에 따라 merge시에 충돌이 자주 일어나는 현상이 있었다. 심지어 깃허브 repository 소유자(쓰고 있는 본인..)의 깃 설정이 꼬여서 눈물을 머금고 초기화(init)해서 commit 내역을 날려버린 참사가 있었다. 이후 branch를 꼭 따로 분리해서 main은 매우 완벽한 상태의 코드만 merge해야 함을 깨달을 수 있었다.
- 보안 ISSUE
- XSS ATTACK
  본 프로젝트에서는 멤버 카드를 Firebase에서 가져아 EL문법으로 변수를 담고, 자바스크립트 **백틱**에 감싸서 card div에 append하는 방법을 사용하고 있었다. 때문에 유저가 고의 또는 의도하지 않은 악성 스크립트 (ex <br>, \n 등등) 으로 인해 css가 깨지는 상황 발생 했기에 **XSS(Cross Site Scripting) Filter** 를 작업이 필요했다. 이는 Jquery함수 **escapeHTML**으로 간단히 해결하였다.
- Firebase cofiguration
파이어 베이스 API키는 개인정보라 gitHub에 올리면 해킹의 문제가 있음을 인지하였다. vscode 설정으로 환경변수를 등록하는 방법을 최초로 생각해 보았으나 이후 팀원들과 논의한 결과 gitignore로 깃에 올리지 않고 팀원들과 API KEY를 공유하는 방향으로 결정했다.
- CORS
 킥오프 미팅에서 역할 분담을 위해 작업 파일을 팀원 각각의 js파일로 분리하여 작업하고자 했으나 교차 출처 리소스 공유(Cross-Origin Resource Sharing) 일명 CORS에러가 발생 했다. type을 module로 설정한 <script> 태그가 포함된 HTML 파일을 로컬에서 불러올 경우 javascript 모듈 보안 요구사항으로 인해 CORS 오류가 발생한다는 것을 팀원모두 알게되었다. 브라우저가 웹에서 로컬 파일에 접근하지 못하게 했기 때문에 아쉽지만 코드를 병합하여 한 HTML파일에 넣어서 진행 했다.(깃 충돌의 시작..)

  
