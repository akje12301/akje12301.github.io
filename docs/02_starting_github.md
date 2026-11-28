# GitHub와 공부 웹로그

HTML로 페이지를 만들 수 있게 되면, 다음 문제는 그 글을 **어디에 모아 두고 인터넷에 올릴지**다. 이 노트는 GitHub 저장소에 공부 글을 쌓고, 그 저장소를 웹로그(공부 사이트)의 원본으로 쓰는 출발점이다.

## GitHub가 하는 일

GitHub는 Git 저장소를 인터넷에 두는 서비스다. 내 컴퓨터의 폴더가 원본의 하나이고, GitHub에 올린 저장소가 또 하나의 원본이 된다. 둘은 커밋이라는 기록으로 맞춰 간다.

공부 사이트에서는 역할을 이렇게 나눈다.

| 장소 | 하는 일 |
| --- | --- |
| 내 PC의 폴더 | Markdown을 쓰고, `mkdocs serve`로 미리 본다. |
| GitHub 저장소 | 글과 설정 파일의 원격 복사본을 보관한다. |
| GitHub Pages | 빌드된 HTML을 브라우저에 보내 주는 서버 역할을 한다. |
| GitHub Actions | `main`에 올린 뒤 MkDocs로 사이트를 만들고 Pages에 배포한다. |

브라우저 주소창의 URL이 서버의 파일을 가리킨다는 앞 장의 그림에서, 배포가 끝나면 그 서버가 GitHub Pages가 된다.

## 웹로그로 볼 때의 저장소

웹로그는 날짜가 있는 짧은 글을 계속 쌓는 사이트다. 여기서는 블로그 엔진을 따로 두지 않고, **공부 노트 한 편을 Markdown 파일 하나**로 둔다. 파일 이름이 목차 순서가 된다.

```text
website/
  mkdocs.yml          사이트 이름, 테마, 목차
  docs/
    index.md          첫 화면
    01_intro.md
    02_starting_github.md
    ...
```

`docs/` 안의 `.md`만 페이지가 된다. 루트의 `01_intro.txt` 같은 초안은 원본으로 남겨 두고, 사이트에 올릴 글은 `docs/`에 다시 쓴다.

## 저장소를 만드는 순서

1. [GitHub](https://github.com)에 로그인하고 **New repository**를 누른다.
2. 저장소 이름은 사이트 폴더와 맞춰 둔다. 예: `website`. Public이면 Pages로 공개할 수 있다.
3. 빈 저장소로 만든 뒤, 이미 있는 로컬 폴더와 연결한다.

```bash
git init
git add mkdocs.yml docs
git commit -m "Add study notes"
git branch -M main
git remote add origin https://github.com/<사용자>/<저장소>.git
git push -u origin main
```

이미 `git init`이 되어 있으면 `git init`은 다시 하지 않는다. `git status`로 추적 중인 파일을 먼저 본다.

브라우저만 쓸 때는 저장소 페이지의 **Add file → Upload files**로 `docs/`와 `mkdocs.yml`을 올리고 커밋 메시지를 적는다. 같은 결과를 나중에 터미널의 `git push`로도 만들 수 있어야 한다. 배포 자동화는 [Git으로 배포](06_git_deploy.md)에서 잇는다.

## 첫 커밋에 넣을 것

- `mkdocs.yml` — 사이트 이름과 Material 테마
- `docs/index.md` — 목차
- 이 장의 Markdown

시크릿, 비밀번호, 토큰이 들어 있는 파일은 커밋하지 않는다. GitHub에 한 번 올라간 비밀은 지워도 기록에 남을 수 있다.

## 정리

1. GitHub 저장소는 공부 글의 원격 원본이다.
2. 웹로그의 한 편은 `docs/` 아래 Markdown 한 파일이다.
3. 로컬에서 고친 뒤 커밋하고 `main`에 푸시하면, 이후 Actions가 사이트를 다시 만든다.
