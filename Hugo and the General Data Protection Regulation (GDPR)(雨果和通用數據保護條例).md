# Hugo and the General Data Protection Regulation (GDPR)(雨果和通用數據保護條例)
About how to configure your Hugo site to meet the new regulations(關於如何配置您的Hugo網站以符合新規定).

General Data Protection Regulation ([GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation)) is a regulation in EU law on data protection and privacy for all individuals within the European Union and the European Economic Area. It became enforceable on 25 May 2018(通用數據保護法規（[GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation)）是歐盟法律中針對歐盟和歐洲經濟區內所有個人的數據保護和隱私的法規。它於2018年5月25日生效).

**Hugo is a static site generator. By using Hugo you are already standing on very solid ground. Static HTML files on disk are much easier to reason about compared to server and database driven web sites**.(**雨果是一個靜態網站生成器。通過使用Hugo，您已經站在了堅實的基礎上。與服務器和數據庫驅動的網站相比，磁盤上的靜態HTML文件更容易推斷**)

But even static websites can integrate with external services, so from version `0.41`, Hugo provides a **Privacy Config** that covers the relevant built-in templates(但是，即使靜態網站也可以與外部服務集成，因此`0.41`Hugo 從版本開始，提供了一個涵蓋相關內置模板的**Privacy Config**。).

Note that(注意):

- These settings have their defaults setting set to *off*, i.e. how it worked before Hugo `0.41`. You must do your own evaluation of your site and apply the appropriate settings(這些設置的默認設置為*off*，即Hugo之前的工作方式`0.41`。您必須對自己的網站進行評估並應用適當的設置).
- These settings work with the [internal templates](https://gohugo.io/templates/internal/). Some theme may contain custom templates for embedding services like Google Analytics. In that case these options have no effect(這些設置與[內部模板一起使用](https://gohugo.io/templates/internal/)。某些主題可能包含用於嵌入服務（例如Google Analytics（分析））的自定義模板。在這種情況下，這些選項無效).
- We will continue this work and improve this further in future Hugo versions(我們將繼續這項工作，並在將來的Hugo版本中進一步改進).

## All Privacy Settings (所有隱私設置)

Below are all privacy settings and their default value. These settings need to be put in your site config (e.g. `config.toml`)(以下是所有隱私設置及其默認值。這些設置需要放在您的網站配置中（例如`config.toml`）).

config. yaml

```yaml
privacy:
  disqus:
    disable: false
  googleAnalytics:
    anonymizeIP: false
    disable: false
    respectDoNotTrack: false
    useSessionStorage: false
  instagram:
    disable: false
    simple: false
  twitter:
    disable: false
    enableDNT: false
    simple: false
  vimeo:
    disable: false
    simple: false
  youtube:
    disable: false
    privacyEnhanced: false
```

config. toml

```toml
[privacy]
  [privacy.disqus]
    disable = false
  [privacy.googleAnalytics]
    anonymizeIP = false
    disable = false
    respectDoNotTrack = false
    useSessionStorage = false
  [privacy.instagram]
    disable = false
    simple = false
  [privacy.twitter]
    disable = false
    enableDNT = false
    simple = false
  [privacy.vimeo]
    disable = false
    simple = false
  [privacy.youtube]
    disable = false
    privacyEnhanced = false
```

config. json

```json
{
   "privacy": {
      "disqus": {
         "disable": false
      },
      "googleAnalytics": {
         "anonymizeIP": false,
         "disable": false,
         "respectDoNotTrack": false,
         "useSessionStorage": false
      },
      "instagram": {
         "disable": false,
         "simple": false
      },
      "twitter": {
         "disable": false,
         "enableDNT": false,
         "simple": false
      },
      "vimeo": {
         "disable": false,
         "simple": false
      },
      "youtube": {
         "disable": false,
         "privacyEnhanced": false
      }
   }
}
```

## Disable All Services(禁用所有服務)

An example Privacy Config that disables all the relevant services in Hugo. With this configuration, the other settings will not matter(隱私配置示例，它禁用了Hugo中的所有相關服務。使用此配置，其他設置將無關緊要).

config. yaml

```yaml
privacy:
  disqus:
    disable: true
  googleAnalytics:
    disable: true
  instagram:
    disable: true
  twitter:
    disable: true
  vimeo:
    disable: true
  youtube:
    disable: true
```

config. toml

```toml
[privacy]
  [privacy.disqus]
    disable = true
  [privacy.googleAnalytics]
    disable = true
  [privacy.instagram]
    disable = true
  [privacy.twitter]
    disable = true
  [privacy.vimeo]
    disable = true
  [privacy.youtube]
    disable = true
```

config. json

```json
{
   "privacy": {
      "disqus": {
         "disable": true
      },
      "googleAnalytics": {
         "disable": true
      },
      "instagram": {
         "disable": true
      },
      "twitter": {
         "disable": true
      },
      "vimeo": {
         "disable": true
      },
      "youtube": {
         "disable": true
      }
   }
}
```

## The Privacy Settings Explained(隱私設置介紹)

### GoogleAnalytics(谷歌分析)

- anonymizeIP(**匿名IP**)

  Enabling this will make it so the users’ IP addresses are anonymized within Google Analytics(啟用它可以使用戶的IP地址在Google Analytics（分析）中匿名化).

- respectDoNotTrack(**尊重不要追踪**)

  Enabling this will make the GA templates respect the “Do Not Track” HTTP header(啟用此功能將使GA模板遵守"請勿跟踪"HTTP標頭).

- useSessionStorage(**使用會話存儲**)

  Enabling this will disable the use of Cookies and use Session Storage to Store the GA Client ID(啟用此功能將禁用Cookies的使用，並使用會話存儲來存儲GA客戶ID).

### Instagram 

- simple(**簡單**)

  If simple mode is enabled, a static and no-JS version of the Instagram image card will be built. Note that this only supports image cards and the image itself will be fetched from Instagram’s servers(如果啟用了簡單模式，則將構建靜態且非JS版本的Instagram圖像卡。請注意，這僅支持圖像卡，圖像本身將從Instagram的服務器中獲取).

**Note:** If you use the *simple mode* for Instagram and a site styled with Bootstrap 4, you may want to disable the inline styles provided by Hugo(**注意：**如果您對Instagram 使用*簡單模式*以及使用Bootstrap 4樣式的網站，則可能要禁用Hugo提供的內聯樣式):

config. yaml

```yaml
services:
  instagram:
    disableInlineCSS: true
```

config. toml

```toml
[services]
  [services.instagram]
    disableInlineCSS = true
```

config. json

```json
{
   "services": {
      "instagram": {
         "disableInlineCSS": true
      }
   }
}
```



### Twitter(推特)

- enableDNT(啟用DNT)

  Enabling this for the twitter/tweet shortcode, the tweet and its embedded page on your site are not used for purposes that include personalized suggestions and personalized ads(為twitter / tweet短代碼啟用此功能時，您的網站上的tweet及其嵌入頁面不會用於包括個性化建議和個性化廣告的目的).

- simple(**簡單**)

  If simple mode is enabled, a static and no-JS version of a tweet will be built(如果啟用了簡單模式，則將構建靜態且非JS版本的tweet).

**Note:** If you use the *simple mode* for Twitter, you may want to disable the inlines styles provided by Hugo(**注意：**如果對Twitter 使用*簡單模式*，則可能要禁用Hugo提供的內聯樣式):

config. yaml

```yaml
services:
  twitter:
    disableInlineCSS: true
```

config. toml

```toml
[services]
  [services.twitter]
    disableInlineCSS = true
```

config. json

```json
{
   "services": {
      "twitter": {
         "disableInlineCSS": true
      }
   }
}
```



### YouTube

- privacyEnhanced(**隱私增強**)

  When you turn on privacy-enhanced mode, YouTube won’t store information about visitors on your website unless the user plays the embedded video(啟用隱私增強模式後，除非用戶播放嵌入式視頻，否則YouTube不會在您的網站上存儲有關訪問者的信息).

### Vimeo 

- simple(**簡單**)

  If simple mode is enabled, the video thumbnail is fetched from Vimeo’s servers and it is overlayed with a play button. If the user clicks to play the video, it will open in a new tab directly on Vimeo’s website(如果啟用了簡單模式，則將從Vimeo的服務器中提取視頻縮略圖，並在其上覆蓋一個播放按鈕。如果用戶單擊播放視頻，它將直接在Vimeo網站上的新標籤中打開).