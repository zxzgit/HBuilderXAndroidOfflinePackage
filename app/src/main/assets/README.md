#本目录操作记录

（1）在 `apps` 下放置 从 HBuildX 中 "`发行` -> `原生App-本地打包` -> `生成本地打包App资源包`" 的结果文件，如：__UNI__2***0 放置到该目录下。


 (2) 修改 data 目录下 `dcloud_control.xml` 中 `apps -> app` 的 `appid` 属性设置为 `apps/__UNI__2***0/www/manifest.json`中 id 属性（`一般为DCloud 的 AppId`）