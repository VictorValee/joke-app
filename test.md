## Push
1. *Create and publish 3 branches, "dev", "staging", "feat/feature-1"*
```sh
git switch -c dev && git push --set-upstream origin dev
git switch -c staging && git push --set-upstream origin staging
git switch -c feat/feature-1 && git push --set-upstream origin feat/feature-1
```

2. When you have a push on the branch "dev" only
```yml
on:
  push:
    branches:
      - dev
```

3. When you have a push on any branch except "staging"
```yml
on:
  push:
    branches-ignore:
      - staging
```

4. When you have a push on any branch where the name starts with "feat"
```yml
on:
  push:
    branches:
      - feat/*
```

5. When only the files with a ts extension have been modified
```yml
on:
  push:
    paths:
      - '**.ts'
```

6. When a specific folder have been modified
```yml
on:
  push:
    paths:
      - 'src/**
```

## Pull request
1. When someone creates a PR
```yml
on:
  pull_request:
    types: [opened]
```

2. When someone closed a PR
```yml
on:
  pull_request:
    types: [closed]
```

## Scheduled
1. Every 5 minutes
```yml
on:
  schedule:
    - cron: "* 0/5 * * * ? *"
```

2. Every 1h 30m
```yml
on:
  schedule:
    - cron: "* 30 1 * *  ? "

3. Every Tuesday at 3am
```yml
on:
  schedule:
    - cron: '0 3 * * 2'
```

## Push & PR
1. Create two jobs inside a new workflow
```yml
jobs:
  build-test:
  #  defenition plateform
    runs-on: ubuntu-latest

    steps:
      - run: echo one

  deploy:
    runs-on: ubuntu-latest

    needs: build-test
```

2. When you have a push, execute both jobs
```yml
on: [push]
```

3. When you have a PR, only execute the first job.
```yml
on: [pull_request]

jobs:
  one:
    runs-on: ubuntu-latest

    steps:
      - run: echo one

  two:
    if: github.event_name != 'pull_request'
    runs-on: ubuntu-latest

    steps:
      - run: echo two
```

4. Create a dev branch, and inside it create a dummy change.txt file. Put your name in it. Publish this branch
```sh
git switch -c dev && echo "MONDESIR" > change.txt
git add . && git commit -m "Add change.txt"
git push --set-upstream origin dev
```

5. Create a pull request from the dev branch to main. You should see the first job executing directly inside the PR (from the github UI)

![Pull Request](image/events/pull-request.png)

Manual & Custom
1. When you click on run workflow button in the UI
```yml
on: [workflow_dispatch]
```

2. TODO Create two workflows
```yml
name: Target

on:
  repository_dispatch:
    types: [customEvent]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - run: Print client payload
        run: |
          echo ${{ github.event.client_payload.message }}
```
```yml
name: Client

on:
  repository_dispatch:
    types: [customEvent]

jobs:
  two:
    runs-on: ubuntu-latest
```

3. TODO Trigger the second workflow from the first one, and pass the github event name from the first one to the second with a variable named "firstJobEvent"
```yml

```