ACL (Access Control List, 접근 조절 목록)

파일, 스레드, 이벤트 와 같은 커널 객체(kernel Object) 들은 만들어 질 때 'Security descriptor(보안 기술자)'를 할당 받는다.
이 Security descripto에는 ACL 정보가 포함되어 있다.
이때 <span style="color:orange">ACL 은 해당 객체에 적용되는 보호 정책의 목록 </span>이다.
ACL 은 여러 개의 ACE ( Access Control Entry, 접근 조절 항목)로 구성되어 있으며, ACE은 <span style="color:orange">해당 보안 ID 와 접근 권한에 대한 정보</span> 로 이루어져있다.

따라서 Kernel Object 에 접근하기 위해서는 ACL에 포함되어 있는 보안 ID와 접근권한이 있어야 한다.
