---
title: "[TIL] September In 2022"
excerpt: "The log that I learned what is related with programming in september 2022"

categories:
  - TIL
tags:
  - [TIL, Java]

toc: true
toc_sticky: false

date: 2022-09-05
last_modified_at: 2022-09-05
---

# On 09/05

## "생활코딩 JAVA 입문 수업"

- Watching video lecture From "자바 설치" to "입력과 출력" in [생활코딩](https://opentutorials.org/course/3930)

- After watching video "입력과 출력", I wonder how to pass arguments in not eclipse but vsCode
  - launch.json
  ```json
  {
    "version": "0.2.0",
    "configurations": [
      {
        "type": "java",
        "name": "Launch Current File",
        "request": "launch",
        "mainClass": "${file}"
      },
      {
        "type": "java",
        "name": "Launch App",
        "request": "launch",
        "mainClass": "App",
        "projectName": "test project_6ad5dc36"
      }
    ]
  }
  ```
  - input "args": "one two" in the first curly brackets, it makes error.
  - I input "args": "one two" in the second curly brackets, it dose work.
  - Reference: [Stack overflow](https://stackoverflow.com/questions/52061958/vscode-java-launch-json-args)
