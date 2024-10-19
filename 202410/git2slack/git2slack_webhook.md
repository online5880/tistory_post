
# GitHub Actions를 사용해 Slack으로 알림 보내기

GitHub Actions를 사용해 코드가 푸시될 때마다 Slack에 알림을 보내는 방법을 단계별로 설명합니다. 이 방법을 사용하면 팀원들이 실시간으로 코드 변경 사항을 공유받을 수 있습니다.

## 1. Slack에서 Incoming Webhook 설정하기

### 1.1 Slack에 Incoming Webhook 추가
Slack에서 Incoming Webhook을 설정하려면, 먼저 Slack API를 통해 Webhook을 추가해야 합니다.

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/01-slack.png?raw=true" width="100%" height="100%"/>

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/02-01-webhook.png?raw=true" width="100%" height="100%"/>

Slack에 Webhook을 추가하고, 메시지가 전달될 채널을 선택합니다.

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/02-02-webhook.png?raw=true" width="100%" height="100%"/>


### 1.2 Slack에 Incoming Webhook 설정

1. 앞에서 선택한 채널 또는 다시 선택
2. Webhook  URL `복사해두기` <다시 볼 수 있음>
3. Webhook  설명
4. 슬랙에 보여질 이름
5. 슬랙에 보여질 아이콘 이미지

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/02-03-webhook.png?raw=true" width="100%" height="100%"/>

Webhook URL을 복사한 후, 나중에 GitHub Secrets에 추가해야 합니다.

## 2. GitHub에서 Secrets 설정하기

### 2.1 GitHub Secrets에 Webhook URL 추가
GitHub에서 설정 메뉴로 이동한 후, `Secrets`에서 새로 추가 버튼을 눌러 Slack Webhook URL을 추가합니다.

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/03-01-git-webhook-url.png?raw=true" width="100%" height="100%"/>

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/03-02-git-webhook-url.png?raw=true" width="100%" height="100%"/>

## 3. GitHub Actions 워크플로우 작성

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/04-01-git-actions.png?raw=true" width="100%" height="100%"/>

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/04-02-git-actions.png?raw=true" width="100%" height="100%"/>

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/04-03-git-actions.png?raw=true" width="100%" height="100%"/>

```yaml
name: Notify Slack on Push
on: push
jobs:
  slack_notification:
    runs-on: ubuntu-latest
    steps:
    - name: Check out repository
      uses: actions/checkout@v2
      with:
        fetch-depth: 0  # 전체 Git 히스토리를 가져오도록 설정

    - name: Send Slack notification
      run: |
        COMMIT_MESSAGE=$(git log -1 --pretty=%B)
        AUTHOR_NAME=$(git log -1 --pretty=format:'%an')
        FILES_CHANGED=$(git diff-tree --no-commit-id --name-only -r HEAD)
        BRANCH_NAME=$(echo "${{ github.ref }}" | sed 's|refs/heads/||')
        # Slack 메시지 포맷
        curl -X POST -H 'Content-type: application/json' --data "{
          \"text\":\":rocket: *New Push by [$AUTHOR_NAME]*\n\n*Branch:* $BRANCH_NAME\n\n*Files Changed:*\n\`\`\`$FILES_CHANGED\`\`\`\n\n*Commit Message:*\n\`\`\`$COMMIT_MESSAGE\`\`\`\"
        }" ${{ secrets.SLACK_WEBHOOK_URL }}
```

위와 같이 설정하면, 코드가 푸시될 때마다 Slack에 알림이 전송됩니다.

## 4. GitHub Actions 동작 확인하기

워크플로우가 정상적으로 작동하는지 확인하기 위해 코드를 푸시하면 아래와 같이 실행됩니다.

### 4.1 워크플로우 실행 중

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/05-01-git-actions.png?raw=true" width="100%" height="100%"/>


### 4.2 워크플로우 성공

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/05-02-git-actions.png?raw=true" width="100%" height="100%"/>

Slack 채널에 다음과 같은 메시지가 표시됩니다.

<img src="https://github.com/online5880/tistory_post/blob/main/202410/git2slack/images/06-slack-message.png?raw=true" width="100%" height="100%"/>

```
🚀 New Push by [Author Name]
Branch: main
Files Changed:
file1.txt
file2.txt

Commit Message:
Added new features
```

이를 통해 실시간으로 팀원들에게 코드 변경 사항을 공유할 수 있습니다.
