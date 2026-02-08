#!name=YouTubeNoAds For Loon
#!desc=适用于 YouTube & YouTube Music 去广告。适配自 iab0x00 的脚本。
#!author=iab0x00, Maasea
#!icon=https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/YouTube.png
#!date=2024-05-20

[Argument]
blockUpload = select,"true","false",tag=屏蔽上传按钮,desc=是否屏蔽上传按钮
blockImmersive = select,"true","false",tag=屏蔽选段按钮,desc=是否屏蔽选段按钮
debug = select,"false","true",tag=启用调试模式,desc=非调试用途请保持关闭
captionLang = input,"off",tag=字幕翻译语言,desc=语言代码如 zh-Hans, ja, ko
lyricLang = input,"off",tag=歌词翻译语言,desc=歌词翻译语言代码

[Rule]
# 屏蔽 UDP 流量以强制走 HTTPS 过滤
AND,((DOMAIN-SUFFIX,googlevideo.com), (PROTOCOL,UDP)),REJECT
AND,((DOMAIN,youtubei.googleapis.com), (PROTOCOL,UDP)),REJECT

[Rewrite]
# 拒绝初始化播放的广告请求
^https?:\/\/[\w-]+\.googlevideo\.com\/initplayback.+&oad - reject

[Script]
# 核心处理脚本
http-response ^https:\/\/youtubei\.googleapis\.com\/(youtubei\/v1\/(browse|next|player|search|reel\/reel_watch_sequence|guide|account\/get_setting|get_watch))(\?(.*))?$ script-path=https://raw.githubusercontent.com/Maasea/sgmodule/master/Script/Youtube/youtube.response.js, requires-body=true, binary-body-mode=true, tag=YouTube去广告, argument={ "lyricLang": "{lyricLang}", "captionLang": "{captionLang}", "blockUpload": {blockUpload}, "blockImmersive": {blockImmersive}, "debug": {debug} }

[MITM]
hostname = *.googlevideo.com, youtubei.googleapis.com
