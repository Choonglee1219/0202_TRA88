## 도움말
<br><br>
- **역할:** 애플리케이션의 **"Queries" 패널 UI 템플릿**을 정의합니다. 이 패널은 사용자가 모델에서 특정 아이템 그룹을 찾는 쿼리(예: 모든 Wall 찾기)를 관리하고, 관리자(admin) 권한이 있는 경우 이 쿼리들을 파일로 내보내는(export) 기능을 제공합니다.
- **주요 기능:**
	1. 인터페이스 `QueriesPanelState`: 이 UI 컴포넌트가 동작하는 데 필요한 데이터의 타입을 정의. (ref: [[Interface]])
	    - `components`: OBC의 핵심 컴포넌트 관리자 객체입니다.
	    - `isAdmin`: 사용자가 관리자인지 여부를 나타내는 `boolean` 값으로, UI의 특정 기능(여기서는 'Export' 버튼)을 조건부로 렌더링하는 데 사용됩니다.
	2. **`queriesPanelTemplate` 함수**:
		- `@thatopen/ui`의 상태 기반 컴포넌트(StatefullComponent)를 생성하는 템플릿 함수
	3. 컴포넌트 가져오기: `OBC.ItemsFinder` 컴포넌트는 모델 내에서 아이템을 찾는 쿼리를 생성하고 관리하는 역할.
	4. `queriesList` 컴포넌트 생성: `../../ui-components` 폴더에 정의된 `queriesList`라는 커스텀 UI 컴포넌트를 호출하여 쿼리 목록을 표시하는 UI 요소를 생성. 실제 쿼리 정의는 `src > setup > finders.ts`에서 이루어진다.
	5. **조건부 렌더링 (Export 버튼)**: `isAdmin` 상태가 `true`일 때만 "Export" 버튼을 생성합니다.
	    - **`onExport` 함수**: 이 버튼을 클릭하면 `ItemsFinder` 컴포넌트의 `export()` 메서드를 호출하여 현재 정의된 모든 쿼리 데이터를 가져옵니다.
	    - 가져온 데이터를 JSON 형식으로 변환하고, `Blob` 객체를 생성한 뒤, 동적으로 `<a>` 태그를 만들어 파일 다운로드를 실행합니다. 
	    - 이 때 사용되는 `URL` 객체는 브라우저 메모리에 있는 데이터에 대한 임시 주소를 생성하고 해제하는 데 사용됩니다. (ref: [[Web API]])
		    - `URL.createObjectURL(blob)`: blob 객체를 인자로 받아 해당 객체를 가리키는 임시 URL을 생성
		    - `URL.revokeObjectURL(link.href)`: `createObjectURL`로 생성했던 임시 URL을 메모리에서 제거하여 리소스를 해제
	6. **UI 구조 반환:**
		- 최종적으로 `<bim-panel-section>` 안에 exportBtn(isAdmin = true일 경우에만 존재)과 element = queriesList({ components })를 배치하여 패널의 전체 UI를 구성합니다.
<br><br>
