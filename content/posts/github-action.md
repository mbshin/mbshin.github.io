---
title: "Github Action"
date: 2022-06-22T17:12:25+09:00
draft: true
---

망분리 환경에서 1년 넘게 살다오니 Github Action을  잊고 살았다. 다시한번 컨셉 정리

## Overview
Github 상에서 무료로 제공되는 CI/CD 플랫폼

## Components
* Workflow
	* 하나 이상의 job을 실행하는 workflow.  레포지토리의 이벤트 발생, 스케줄에 따라 워크플로우를 실행하도록 설정
	* 레포지토리의 .github/workflows dir에 yaml 파일로 정의. Parallel하게 실행되는 복수의 workflow 정의 가능
* Event
	* workflow를 실행시키는 push등의 github 이벤트
* Jobs
	* 동일한 runner(리눅스 와 같은 실행환경) 에서 실행되는 steps로 구성
	* 각 setp은 shell script나 action으로 구성
* Action
	* Custom application , 복잡한 작업을 하나의 app으로 생성
	* github marketplace에서 publish된 다양한 action 검색 가능
* Runner
	* workflow를 실행하는 서버

## Example
* github에 신규 repo를 생성하고 local에 환경을 구성
* .github/workflows dir을 생성하고 첫번째 workflow 작성

```bash
mkdir -p .github/workflows
cd .github/workflows
touch first.yaml
code first.yaml
```

  * first.yaml
```yaml
name: learn-github-actions #workflow name
on: [push] # trigger event
jobs:
  check-bats-version: # job name
    runs-on: ubuntu-latest # runner
    steps:
      - uses: actions/checkout@v3 # action
      - uses: actions/setup-node@v3 # action
        with:
          node-version: '14'
      - run: npm install -g bats # run keyworkd - execute command
      - run: bats -v
```

* 두번째 workflow 작성
  * second.yaml
```yaml
name: second-workflow
on: [push]
jobs:
	confirm-checkout:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3 # action
      - run: ls -al
```


* 사용가능한 action list는 GitHub marketplace에서 조회가 가능하며 개발자가 직접 publish 할 수 있다. 

## Refs
* [Understanding GitHub Actions - GitHub Docs](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
* [GitHub Marketplace · Actions to improve your workflow · GitHub](https://github.com/marketplace?type=actions)

## Follow-ups
* Custom Action 퍼블리싱 


