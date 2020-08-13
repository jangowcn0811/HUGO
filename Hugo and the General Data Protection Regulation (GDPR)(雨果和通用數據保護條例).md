# Hugo's Security Model(雨果的安全模式)
A summary of Hugo’s security model(Hugo的安全模型摘要).

## Runtime Security (運行時安全)

Hugo produces static output, so once built, the runtime is the browser (assuming the output is HTML) and any server (API) that you integrate with(Hugo會生成靜態輸出，因此一旦構建，運行時就是瀏覽器（假設輸出為HTML）以及與之集成的任何服務器(API).

But when developing and building your site, the runtime is the `hugo` executable. Securing a runtime can be [a real challenge](https://blog.logrocket.com/how-to-protect-your-node-js-applications-from-malicious-dependencies-5f2e60ea08f9/).(但是，在開發和構建站點時，運行時是`hugo`可執行文件。確保運行時安全是[一個真正的挑戰](https://blog.logrocket.com/how-to-protect-your-node-js-applications-from-malicious-dependencies-5f2e60ea08f9/)).

**Hugo’s main approach is that of sandboxing(雨果的主要方法是沙盒):**

- Hugo has a virtual file system and only the main project (not third-party components) is allowed to mount directories or files outside the project root(Hugo有一個虛擬文件系統，並且只允許主項目（而非第三方組件）在項目根目錄之外掛載目錄或文件).
- Only the main project can walk symbolic links(只有主項目可以走符號鏈接).
- User-defined components have only read-access to the filesystem(用戶定義的組件僅具有對文件系統的讀取權限).
- We shell out to some external binaries to support [Asciidoctor](https://gohugo.io/content-management/formats/#list-of-content-formats) and similar, but those binaries and their flags are predefined. General functions to run arbitrary external OS commands have been [discussed](https://github.com/gohugoio/hugo/issues/796), but not implemented because of security concerns(我們使用一些外部二進製文件來支持Asciidoctor和類似文件，但是這些二進製文件及其標誌是預定義的。已經討論了運行任意外部OS命令的一般功能，但是由於安全方面的考慮未實現).

Hugo will soon introduce a concept of *Content Source Plugins* (AKA *Pages from Data*), but the above will still hold true(Hugo很快會引入內容源插件（Data的 AKA Pages）概念，但是以上內容仍然適用).

## Dependency Security (依賴安全)

Hugo builds as a static binary using [Go Modules](https://github.com/golang/go/wiki/Modules) to manage its dependencies. Go Modules have several safeguards, one of them being the `go.sum` file. This is a database of the expected cryptographic checksums of all of your dependencies, including any transitive(Hugo使用[Go Modules](https://github.com/golang/go/wiki/Modules)構建為靜態二進製文件來管理其依賴項。Go Module具有多種保護措施，其中之一就是go.sum文件保護。這是所有依賴項（包括所有傳遞項）的預期密碼校驗和的數據庫).

[Hugo Modules](https://gohugo.io/hugo-modules/) is built on top of Go Modules functionality, and a Hugo project using Hugo Modules will have a `go.sum` file. We recommend that you commit this file to your version control system. The Hugo build will fail if there is a checksum mismatch, which would be an indication of [dependency tampering](https://julienrenaux.fr/2019/12/20/github-actions-security-risk/). ([Hugo Modules](https://gohugo.io/hugo-modules/)建立在Go Modules功能的基礎上，使用Hugo Modules的Hugo項目將有一個go.sum文件。我們建議您將此文件提交到版本控制系統。如果校驗和不匹配，Hugo構建將失敗，這可能表示[依賴項被篡改](https://julienrenaux.fr/2019/12/20/github-actions-security-risk/)).

## Web Application Security (Web應用安全)

These are the security threats as defined by [OWASP](https://en.wikipedia.org/wiki/OWASP). (這些是[OWASP](https://en.wikipedia.org/wiki/OWASP)定義的安全威脅)

For HTML output, this is the core security model(對於HTML輸出，這是核心安全模型):

https://golang.org/pkg/html/template/#hdr-Security_Model

In short(簡而言之):

Templates authors (you) are trusted, but the data you send in is not. This is why you sometimes need to use the *safe* functions, such as `safeHTML`, to avoid escaping of data you know is safe. There is one exception to the above, as noted in the documentation: If you enable inline shortcodes, you also say that the shortcodes and data handling in content files are trusted, as those macros are treated as pure text. It may be worth adding that Hugo is a static site generator with no concept of dynamic user input(模板作者（您）是受信任的，但您發送的數據不受信任。這就是為什麼有時需要使用*安全*功能（例如`safeHTML`）來避免轉義已知安全數據的原因。如文檔中所述，上述內容有一個例外：如果啟用內聯短代碼，您還說內容文件中的短代碼和數據處理是受信任的，因為這些宏被視為純文本。可能值得補充的是，Hugo是靜態站點生成器，沒有動態用戶輸入的概念).

For content, the default Markdown renderer is [configured](https://gohugo.io/getting-started/configuration-markup) to remove or escape potentially unsafe content. This behavior can be reconfigured if you trust your content(對於內容，默認的Markdown渲染器[配置](https://gohugo.io/getting-started/configuration-markup)為刪除或轉義潛在的不安全內容。如果您信任自己的內容，則可以重新配置此行為).