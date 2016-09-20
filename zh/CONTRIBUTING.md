# 为 Microsoft REST API 指南做贡献
Microsoft REST API 指南是整个 Microsoft 为设计一致风格的 REST API 而提出的倡议。这个倡议需要大量来自 Microsoft 内部或者外部的个人开发者的参与和反馈。

根据此文档的指导进行反馈。请注意，这些仅仅是指南，而不是规定。请保持自己主见积极地提议来改进这个仓库的任何东西，包括此文档本身。

请注意此工程是和 [Contributor Code of Conduct][code-of-conduct] 一起发布的。参与此工程你需要遵守以下条款。
- [创建 issues](#创建 issues)
- [文档格式指南](#文档格式指南)
- [Commit 信息](#Commit 信息)
- [Pull requests](#pull-requests)

## 创建 issues
- 你可以 [创建 issues][new-issue], 在这之前你最好仔细阅读下面的一些细节。
- 请[搜索一下][issue-search]，确认是否有相似的issue之前被提出过。
- 指定你在使用的Microsoft REST API指南的版本号。
- 包含你希望或者在其他地方见过的指南，例如 [White House Web API Standards][white-house-api-guidelines]。
- 尽可能的包含请求和响应的例子。

### 相关的 repositories
这个 repository 为 Microsoft REST API 指南文档独有。确保你在正确的 repository 下提 issue。


## 推荐的开发环境配置
- 安装 [Atom][atom], [VS Code][vscode], 或者其他你喜欢的编辑器
- 安装 [markdown-toc package][markdown-toc]

## 文档格式指南
- 使用 [GitHub-flavored markdown][gfm]
- 使用 syntax-highlighted 例子
- 去掉 HTTP requests 尾部的空行
- 为更好理解例子，只保留必要的 headers
- 使用合法的，格式化的 JSON，并且使用2个 space 的缩进
- 使用省略号减小 JSON 大小
- 一行只写一个句子

### 例子
#### 请求

```http
GET http://services.odata.org/V4/TripPinServiceRW/People HTTP/1.1
Accept: application/json
```

#### 响应

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "@nextLink":"http://services.odata.org/V4/TripPinServiceRW/People?$skiptoken=8",
  "value":[
    {
      "userName":"russellwhyte",
      "firstName":"Russell",
      "lastName":"Whyte",
      "emails":[
        "Russell@example.com",
        "Russell@contoso.com"
      ],
      "addressInfo":[
        {
          "address":"187 Suffolk Ln.",
          "city":{
            "countryRegion":"United States",
            "name":"Boise",
            "region":"ID"
          }
        }
      ],
      "gender":"Male",
    },
    ...
  ]
}
```

## Commit 信息
- 使用现在式 "Change ...", 而不是过去式 "Changed ..."
- 使用祈使语气，例如 "Change ...", 不是 "Changes ..."
- 首行使用72个字符或者更少
- 引用 issue 和 pull request

## Pull Requests
Pull Request 作为重要的开源机制被开源贡献者们广泛的推广和接受。我们建议创建一个 [topic branch][topic-branch]，然后发起一个针对 `master` 分支的 pull request。获取更多指南，请参考 [GitHub Flow Guide][github-flow-guide]。

如果有必要，准备好响应你的 pull request 的反馈或者迭代。

[code-of-conduct]: https://opensource.microsoft.com/codeofconduct/
[new-issue]: https://github.com/Microsoft/api-guidelines/issues/new
[issue-search]: https://github.com/Microsoft/api-guidelines/issues
[white-house-api-guidelines]: https://github.com/WhiteHouse/api-standards/blob/master/README.md
[topic-branch]: http://www.git-scm.com/book/en/v2/Git-Branching-Branching-Workflows#Topic-Branches
[gfm]: https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown
[github-flow-guide]: https://guides.github.com/introduction/flow/
[atom-beautify]: https://atom.io/packages/atom-beautify
[atom]: http://atom.io
[markdown-toc]: https://atom.io/packages/markdown-toc
[vscode]: https://code.visualstudio.com/
