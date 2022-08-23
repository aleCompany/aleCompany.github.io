---
layout: single
title:  "fs.readFileSync, readFile 사용 (electron , Vue)"
categories: electron
tag: [ApexCharts, electron, Vue]
toc: true
author_profile: false
---
#### Electron Vue 환경에서 fs.readFileSync, readFile 사용하는 경우 보안문제로 인하여 다음과 같은 에러가 발생한다.

* [Vue warn]: Error in mounted hook: "TypeError: fs.readFileSync is not a function"
  vue.runtime.esm.js?2b0e:3006 TypeError: fs.readFileSync is not a function

* 해결방법
    * vue.config.js 파일에서 아래를 추가한다.
    <pre>
        module.exports = {
            pluginOptions: {
                electronBuilder: {
                    nodeIntegration: true,
                }
            }
        }
    </pre>