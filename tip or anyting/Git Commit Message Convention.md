

협업과 기록 가독성을 높이기 위한 규칙

### 기본 구조
```bash
<type> (<scope>): <subject>
```
- **type**: 변경의 성격
- **scope**: 선택 사항. Flutter 앱 구조 기준 (예: `widget`, `screen`, `state`, `service`, `model`, `api`)
- **subject** : 간견하게 변경 내용 요약

### type 종류

| type     | 설명                      | ex                                        |
| -------- | ----------------------- | ----------------------------------------- |
| feat     | 새로운 기능 추가               | feat(widget): 커스텀 버튼에 리플 효과 추가            |
| fix      | 버그 수정                   | fix(screen): 로그인 화면에서 null 오류 처리          |
| docs     | 문서 수정                   | docs(readme): Firebase 설치 가이드 업데이트        |
| style    | 코드 포맷/스타일만 변경, 로직 변화 없음 | style(widget): 카드 위젯 패딩 통일                |
| refactor | 기능 변화 없이 코드 구조 개선       | refactor(state): 로그인 상태 관리 로직 단순화         |
| test     | 테스트 코드 추가/수정            | test(service): 유저 저장소 단위 테스트 추가           |
| chore    | 빌드, 패키지, 설정, 의존성 업데이트   | chore: Flutter SDK 3.13으로 업그레이드           |
| perf     | 성능 개선                   | perf(api): 유저 API 호출 지연 시간 감소             |
| ci       | CI/CD 관련 변경             | ci: 깃허브 액션으로 린트 및 테스트 자동화                 |
| HOTFIX   | **급하게** 치명적인 에러를 고침     | hotfix(widget): 툴팁이 화면 밖으로 벗어나는 문제 해결<br> |

### **Scope 예시 (Flutter 기준)**

- `widget` : UI 위젯
- `screen` : 페이지/화면 단위
- `state` : 상태 관리 관련 (Provider, Bloc 등)
- `service` : API 호출, DB, 로컬 저장소
- `model` : 데이터 모델
- `api` : 외부 API

