# Hugo를 활용하여 GitHub Page 블로그 만들기
블로그를 여러번 시도해 보았으나 스타일이 안맞는지 중도 포기 속출. 하지만 다시 시도중이다. 정리의 중요성이 갈수록 높아지는 시기이다.  나를 위한 기록이다.

Golang으로 구현된  Hogo static site generator를 활용하여 Markdown으로 작성된 올해 첫번째 글을 publishing 한다  일주일에 최소 한개의 블로그를 올리는 것이 목표이다. 

## Github Page Setup
Github가 제공하는 Static site hosting 서비스이다. Jekyll, Hogo 등 static site generator를 활용하여 개인 블로그에 많이 사용된다. 약간의 개발 지식이 필요해서 대부분 개발블로그에 활용되고 있다. 

### Github Page Site 만들기

* mbshin.github.io 레포지토리 생성 
	* <username>.github.io 형식으로 Repo를 생성해야 한다. 개별 프로젝트 블로그는 공식 문서 참고

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

### page 주소 확인
* GitHub Repo -> setting -> Page url 주소 확인
	* repo
	https://github.com/mbshin/mbshin.github.io/settings/pages
	 * github page 주소
	https://mbshin.github.io/

사용할 브랜치 및 root 지정이 가능하며 index.html 혹은 index.md를 노출시켜준다. index.html이 우선순위가 높다.

### Ref
[Creating a GitHub Pages site - GitHub Docs](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)

## Hugo Setup
TBD



