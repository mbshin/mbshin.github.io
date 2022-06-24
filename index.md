# Hugo를 활용하여 GitHub Page 블로그 만들기
블로그를 여러번 시도해 보았으나 스타일이 안맞는지 중도 포기를 여러번. 정리의 중요성이 갈수록 높아지는 시기이다.  나를 위한 기록이다.

Golang으로 구현된  HUGO static site generator를 활용하여 Markdown으로 작성된 글을 publishing 한다 . 일주일에 최소 한개의 블로그를 올리는 것이 목표이다. 

## Github Page Setup
Github가 제공하는 static site hosting 서비스이다. Jekyll, Hugo 등 static site generator를 활용하여 블로그에 많이 사용된다. 약간의 개발 지식이 필요하다. 

### Github Page Site 만들기
* mbshin.github.io 레포지토리 생성 
	* username.github.io 형식으로 Repo를 생성한다. 개별 프로젝트 블로그 생성은 공식 문서 참고
	* 무료 계정은 모두 public으로 공개된다. Private로 소스 관리를 위해서는 유료 계정이 필요하다. 

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
		* https://github.com/mbshin/mbshin.github.io/settings/pages
	 * github page 주소
		* 	https://mbshin.github.io/x

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
* TODOS
	* https 적용
	
## Hugo Setup
TBD

## Ref
[Creating a GitHub Pages site - GitHub Docs](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)
[Managing a custom domain for your GitHub Pages site - GitHub Docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain)