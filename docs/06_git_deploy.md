# Git으로 배포

로컬의 `mkdocs serve`는 내 컴퓨터에서만 보인다. 인터넷에 올리려면 웹 서버가 필요하다. 이 노트에서는 그 서버를 **GitHub Pages**로 두고, 빌드는 **GitHub Actions**가 맡는다.

## 필요한 것

| 도구 | 하는 일 |
| --- | --- |
| Git | 로컬 폴더의 버전을 커밋으로 남긴다. |
| GitHub 저장소 | 그 커밋의 원격 복사본이다. |
| GitHub Actions | `main`에 푸시되면 MkDocs로 `site/`를 만든다. |
| GitHub Pages | 만들어진 HTML을 URL로 제공한다. |

## 작업 흐름

1. VS Code에서 `docs/` 의 Markdown과 `mkdocs.yml`을 수정한다.
2. `mkdocs serve`로 로컬에서 확인한다.
3. 변경을 커밋하고 GitHub `main` 브랜치에 푸시한다.
4. Actions가 빌드와 배포를 끝낸다.
5. 브라우저에서 Pages 주소로 연다.

브라우저만 쓸 때는 GitHub 저장소의 **Upload files**로 같은 파일을 올리고 **Commit changes**를 누른다. Actions가 그 커밋을 보고 다시 배포한다.

```bash
git add mkdocs.yml docs
git commit -m "Update study notes"
git push
```

## 1. GitHub Actions 설정

저장소에 워크플로 파일이 있어야 한다. 경로는 `.github/workflows/ci.yml` 이다. MkDocs 공식 문서의 GitHub Pages용 예시를 저장소에 두고, 브랜치 이름이 `main` 인지 확인한다.

워크플로는 대략 다음을 한다.

1. `main`에 푸시되면 실행된다.
2. Python을 설치하고 `mkdocs-material`을 설치한다.
3. `mkdocs gh-deploy` 또는 `mkdocs build` 결과물을 Pages에 올린다.

파일을 추가한 커밋을 푸시하면 **Actions** 탭에 실행 기록이 생긴다.

## 2. GitHub Pages 설정

저장소 **Settings → Pages** 에서 소스를 고른다.

- Actions로 배포하는 워크플로라면 Source를 **GitHub Actions**로 둔다.
- 예전 방식(`gh-pages` 브랜치)이라면 Source를 그 브랜치의 루트로 둔다.

한 저장소에서 두 방식을 섞으면 빈 페이지나 오래된 빌드가 남을 수 있다. 워크플로가 안내하는 소스 하나만 남긴다.

## 3. Actions 실행 확인

**Actions** 탭에서 가장 최근 워크플로를 연다. 초록색 체크가 끝나면 배포가 끝난 것이다. 빨간 실패면 로그에서 `pip`, `mkdocs.yml` 들여쓰기, 권한(`permissions: pages`) 오류를 먼저 본다.

## 4. 브라우저에서 확인

Pages 주소는 보통 다음 형태다.

```text
https://<사용자>.github.io/<저장소>/
```

사용자 사이트(`<사용자>.github.io` 저장소)면 저장소 이름이 빠진다. 첫 배포 직후에는 1–2분 늦게 바뀔 수 있다. 목차의 각 글이 로컬 `mkdocs serve`와 같은 제목으로 열리는지 본다.

## 5. 터미널로 배포하는 방법

Actions 없이 내 PC에서 바로 `gh-pages` 브랜치에 올릴 수도 있다.

```bash
mkdocs gh-deploy
```

이 명령은 빌드한 뒤 `gh-pages` 브랜치에 푸시한다. Actions를 이미 쓰는 저장소에서는 둘 중 하나만 남기는 편이 안전하다. 이 노트의 기본 흐름은 **git push → Actions** 이다.

## 로그인 정보를 지우는 방법

다른 계정으로 푸시가 막히면 Windows에 남은 GitHub 로그인 정보를 지운다.

1. 제어판 → **자격 증명 관리자** → **Windows 자격 증명**
2. `git:https://github.com` 또는 `github.com` 항목을 제거한다.
3. 다음 `git push`에서 다시 로그인한다.

터미널에서 같은 항목만 지울 때는 아래를 쓴다. 계정 자체를 지우지는 않는다.

```bash
cmdkey /delete:git:https://github.com
```

항목 이름이 다르면 `cmdkey /list` 로 `github` 가 들어 있는 대상을 확인한 뒤 그 이름을 지운다.

## 정리

1. 글 수정은 로컬 `docs/`에서 하고, 확인은 `mkdocs serve`로 한다.
2. `git push`가 `main`을 갱신하면 Actions가 빌드하고 Pages가 사이트를 제공한다.
3. Pages의 소스 설정은 워크플로와 하나로 맞춘다.
