# 배포 이후 디자인

글이 Pages에 올라간 뒤에는 HTML을 다시 쓰지 않고, `mkdocs.yml`의 Material 테마로 보이는 방식을 다듬는다. 본문은 Markdown에 남긴다.

## 지금 설정의 의미

```yaml
site_name: My blog

theme:
  name: material
  language: ko
```

| 키 | 효과 |
| --- | --- |
| `site_name` | 브라우저 탭과 헤더에 나오는 사이트 이름 |
| `theme.name: material` | 목차, 검색, 헤더가 있는 Material 테마 |
| `theme.language: ko` | 검색 창, 메뉴 등 테마 문구를 한국어로 |

`site_name`만 바꿔도 다음 배포부터 헤더가 바뀐다. 로컬에서는 `mkdocs serve`를 다시 열거나 저장 후 새로고침으로 확인한다.

## 색

Material은 `palette`로 기본 색을 받는다. 들여쓰기는 `theme` 아래 두 단계다.

```yaml
theme:
  name: material
  language: ko
  palette:
    primary: indigo
    accent: indigo
```

`primary`는 헤더, `accent`는 링크와 버튼에 가깝다. 이름표는 `red`, `pink`, `purple`, `indigo`, `blue`, `teal`, `green` 처럼 Material이 정해 둔 단어를 쓴다. 임의의 `#112233` 을 여기 문자열로 넣으면 테마가 그대로 받지 않는다.

밝은 화면과 어두운 화면을 같이 두려면 `palette`를 목록으로 둔다.

```yaml
theme:
  name: material
  language: ko
  palette:
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: indigo
      accent: indigo
      toggle:
        icon: material/weather-night
        name: 어둡게
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: indigo
      accent: indigo
      toggle:
        icon: material/weather-sunny
        name: 밝게
```

## 글 안에서 맞추는 것

테마가 간격을 맡으므로, Markdown에서는 구조를 우선한다.

- 한 페이지에 `#` 제목은 하나. 그 아래는 `##`, `###`.
- 비교는 표, 순서는 번호 목록, 명령은 코드 블록.
- 긴 문단보다 소제목으로 나눈다. Material 오른쪽 목차가 그 제목을 집어 준다.

```yaml
theme:
  name: material
  language: ko
  features:
    - navigation.sections
    - content.code.copy
    - toc.follow
```

`content.code.copy`는 코드 블록에 복사 버튼을 붙인다. `toc.follow`는 스크롤에 따라 현재 소제목을 표시한다.

## 고친 뒤 확인하는 순서

1. `mkdocs.yml` 저장. YAML 들여쓰기가 스페이스인지 본다.
2. `mkdocs serve`로 `http://127.0.0.1:8000` 에서 헤더, 목차, 표, 코드 블록을 본다.
3. 문제 없으면 커밋하고 푸시한다.
4. Actions가 끝난 뒤 Pages 주소에서 같은 화면인지 확인한다.

로컬과 Pages가 다르면 배포된 커밋에 `mkdocs.yml`이 포함됐는지, 브라우저가 이전 CSS를 갖고 있는지를 본다. 새로고침으로 바뀌지 않으면 주소 뒤에 쿼리를 붙여 한 번 더 연다.

## 정리

1. 배포 이후의 모양은 `mkdocs.yml`의 `theme`이 맡는다.
2. 사이트 이름, 언어, `palette`, `features`를 바꾸고 로컬에서 먼저 본다.
3. 글의 구조는 Markdown 제목과 목록으로 유지한다.
