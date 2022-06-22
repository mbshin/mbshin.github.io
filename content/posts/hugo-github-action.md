---
title: "Hugo를 활용한 GitHub Page 블로그 만들기"
date: 2022-06-22T10:01:40+09:00
draft: false
---

# Hugo를 활용한 GitHub Page 블로그 만들기
#blog #tech

블로그 중도 포기를 여러번. 정리의 중요성이 갈수록 높아지는 시기이다. 나를 위한 기록이다.
Golang으로 구현된  HUGO static site generator를 활용하여 Markdown으로 작성된 글을 publish 한다.  최소 한개의 블로그를 주마다 꾸준히 올리는 것이 목표이다. 

## Github Page
Github가 제공하는 static site hosting 서비스이다.  Markdown이나 html을 직접 작성하는 것도 가능하다. 하지만 편의를 위해서 Jekyll, Hugo 등 static site generator들이 publishing에 많이 사용된다. 약간의 개발 지식이 필요하다. 

### Github Page Site 만들기
* mbshin.github.io 레포지토리 생성 
	* username.github.io 형식으로 Repo를 생성한다. 개별 프로젝트 블로그 생성은 공식 문서 참고
	* 무료 계정은 모두 public으로 공개된다. Private로 관리를 위해서는 GitHub 유료 계정이 필요하다. 

* local에 git repo 생성 후 index.md를 github로 push 한다.
```bash
mkdir mbshin.github.io && cd mbshin.github.io
git init
echo "# mbshin.github.io" >> README.md
echo "# First" >> index.md
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:mbshin/mbshin.github.io.git
git push -u origin main
```

### Page 주소 확인
* GitHub Repo -> setting -> Page url 주소 확인
	* repo
		* https://github.com/mbshin/mbshin.github.io/settings/pages
	 * github page 주소
		* 	https://mbshin.github.io/

사용할 branch 및 document root 지정이 가능하며 index.html 혹은 index.md를 노출시켜준다. index.html이 우선순위가 높다.

### Custom Domain 설정
* domain 구입
	* mobum.me 구입 - 8,999 KRW  from Godaddy.com
* CNAME record 설정
	* godaddy.com -> domain -> manage dns -> add CNAME record
```
Type  name data             TTL			
CNAME blog mbshin.github.io 1Hr
```
* github page url 설정
	* Github -> Setting -> Pages -> custom domain -> insert url (blog.mobum.me) -> save
	
## Hugo 
지킬(jekyll)도 유명한 플랫폼이나 ruby에 정이 안가는 이유로 패스

### Overview
Go 언어로 구현된 static site generator. Markdown으로 글을 작성하면 테마가 적용된 정적 페이지를 생성해 준다. 

## Setup
* Hugo setup
```bash
brew install hugo # hugo install
cd mbshin.github.io # git repo
hugo new site . --force
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke # add a theme
echo theme = \"ananke\" >> config.toml # set the theme in the config file
hugo new posts/my-first-post.md # create first blog
```

* config 수정
```toml
baseURL = 'http://blog.mobum.me/' # custom dns
languageCode = 'en-us'
title = 'My Tech Blog'
theme = "ananke"
```

* blog 작성
```bash
code content/posts/my-first-post.md 
```
```yaml
---
title: "My First Post"
date: 2022-06-22T10:01:40+09:00
draft: true
---

## Test
- This is test content
```

* 정적 파일 생성 및 로컬 테스트 
```bash
hugo server -D # 로컬 서버 실행
hugo -D --destination docs # 정적 파일 생성, drafe 포함
git add .
git commit -m "setup hugo initilally"
git push
```
	
* document root 변경
Setting -> pages -> source -> docs
단순 테스트 용도로 활용. Github action 적용 후 변경 필요

* GitHub action 적용
```bash
mkdir -p .github/workflows
touch .github/workflows/gh-pages.yml
code .github/workflows/gh-pages.yml
```

```yaml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

* actions-gh-pages action은 static 파일을 ph-pages 브랜치의 최상위 dir에 publish한다. GitHub Pages setting을 변경해야 한다. 
	* Source
		* branch : gh-pages
		* /(root)

Hugo의 테마 변경 및 커스터마이징은 시간날때마다 진행예정

## Ref
* [Creating a GitHub Pages site - GitHub Docs](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)
* [Managing a custom domain for your GitHub Pages site - GitHub Docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain)
* [Quick Start | Hugo](https://gohugo.io/getting-started/quick-start/)
* [Host on GitHub | Hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/)



