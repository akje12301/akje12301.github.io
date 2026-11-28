# MkDocs 설치와 프로젝트 구조

Markdown 파일을 사이트로 보려면 MkDocs를 설치하고, 정해진 폴더에 글과 설정을 둔다.

## 1. 설치

Python이 있는 터미널에서 pip로 설치한다. Material 테마를 쓰므로 테마 패키지를 함께 둔다.

```bash
python -m pip install mkdocs mkdocs-material
mkdocs --version
```

`python` 이 없으면 `py -m pip install mkdocs mkdocs-material` 을 쓴다. 버전 번호가 출력되면 설치가 된 것이다.

## 2. 폴더 구조

새 폴더에서 초기화할 수 있다.

```bash
mkdocs new my-site
cd my-site
```

이미 공부 폴더가 있으면 `mkdocs new`로 덮지 않고, 아래만 맞춘다.

```text
website/
  mkdocs.yml
  docs/
    index.md          첫 화면. 이 파일이 사이트의 / 가 된다.
    01_intro.md
    02_starting_github.md
    03_markdown.md
    05_install_mkdocs.md
    06_git_deploy.md
    07_design.md
```

| 경로 | 역할 |
| --- | --- |
| `docs/` | 페이지가 될 Markdown |
| `docs/index.md` | 사이트 첫 화면 |
| `mkdocs.yml` | 사이트 이름, 테마, 목차 |

빌드 결과인 `site/` 는 생성된 파일이므로 저장소에 커밋하지 않아도 된다. GitHub Actions가 배포할 때 다시 만든다.

## 3. mkdocs.yml

들여쓰기는 스페이스로 맞춘다. 탭을 섞으면 설정이 읽히지 않는다.

```yaml
site_name: My blog

theme:
  name: material
  language: ko

nav:
  - 홈: index.md
  - 웹 사이트와 웹 페이지: 01_intro.md
  - GitHub와 공부 웹로그: 02_starting_github.md
  - Markdown과 MkDocs: 03_markdown.md
  - MkDocs 설치: 05_install_mkdocs.md
  - Git으로 배포: 06_git_deploy.md
  - 배포 이후 디자인: 07_design.md
```

`theme` 아래의 `name`, `language` 는 `theme` 보다 한 단계 들여 쓴다. `nav` 항목의 `-` 도 같은 열에 맞춘다.

## 4. 로컬 미리보기

프로젝트 루트( `mkdocs.yml` 이 있는 폴더)에서 실행한다.

```bash
mkdocs serve
```

브라우저에서 `http://127.0.0.1:8000` 을 연다. `127.0.0.1` 은 이 컴퓨터 자신이다. 다른 사람에게 그 주소로는 보이지 않는다. `docs/` 의 Markdown을 저장하면 서버가 다시 읽어 화면이 바뀐다. 끝낼 때는 터미널에서 `Ctrl+C` 를 누른다.

한 번만 HTML로 뽑을 때는 `mkdocs build` 를 쓴다. 결과는 `site/` 에 생긴다.

## 5. 확인할 것

- 첫 화면이 `docs/index.md` 인가
- 왼쪽 목차에 `nav` 순서가 그대로 나오는가
- 제목, 표, 코드 블록이 HTML로 그려지는가
- 주소가 `127.0.0.1:8000` 이고, 파일을 고치면 새로고침 없이 또는 새로고침 뒤에 반영되는가

여기까지가 로컬 사이트다. 인터넷에 올리는 설정은 [Git으로 배포](06_git_deploy.md)로 넘어간다.

## 정리

1. `pip`으로 `mkdocs`와 `mkdocs-material`을 설치한다.
2. 글은 `docs/`, 설정은 `mkdocs.yml`이다.
3. `mkdocs serve`로 `127.0.0.1:8000` 에서 확인한 뒤 배포한다.
