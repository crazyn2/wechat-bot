# Wechat-bot 馈人玫瑰之手，历久犹有余香
<a href="https://github.com/cixingguangming55555/wechat-bot">
  <img align="center" src="https://github-readme-stats.vercel.app/api/pin/?username=cixingguangming55555&repo=wechat-bot" />
</a>
<a href="https://github.com/cixingguangming55555/wechat-bot">
  <img align="center" src="https://github-readme-stats.vercel.app/api/top-langs/?username=cixingguangming55555&layout=compact" />
</a>

<p align="center">
  <img src="https://i.loli.net/2020/05/09/HXClIf5A2EpUG4u.png" width="120">
</p>

<p align="center">
   <a href="https://github.com/cixingguangming55555/wechat-bot/blob/master/LICENSE">
    <img src="https://img.shields.io/github/license/cixingguangming55555/wechat-bot" alt="GitHub">
  </a>
  <a href="https://github.com/cixingguangming55555/wechat-bot/releases">
    <img src="https://img.shields.io/github/v/release/cixingguangming55555/wechat-bot?include_prereleases" alt="license">
  </a>
</p>

# 新坑QQ群: 712370539 因微信号被举报被封，且没有精力管理，暂不建微信群，请直接到QQ群
# 如何使用

## 服务器端
```
第一步 启动PC微信并登陆完成，微信的版本必须是：3.6.0.18

第二步 启动非注入版，funtool.exe

第三步 点启动即可
```
## 客户端
```
本质就是构造一个json，当然，http和websocket的json构造，还有一些区别，具体请去看  这两个文件：
websocket 客户端  ：client-3.2.1.121.js 
http  客户端示例：http-3.2.1.121.js 
```
### 发送文本消息示例--websocket
```javascript
function send_txt_msg()
{
  const j={
    id:getid(),
    type:TXT_MSG,
    wxid:'23023281066@chatroom',//roomid或wxid,必填
    roomid:'null',//此处为空
    content:'hello word',
    nickname:"null",//此处为空
    ext:'null'//此处为空
  };
  const s = JSON.stringify(j);
  return s;
}
```

### 发送文本消息示例--http
```javascript
async function send_txt_msg()
{
  const jpara={
    id:getid(),
    type:TXT_MSG,
    wxid:'23023281066@chatroom',//roomid或wxid,必填
    roomid:'null',//此处为空
    content:'hello word',
    nickname:"null",//此处为空
    ext:'null'//此处为空
  };
  const options =
  {
          url: url+'/api/sendtxtmsg',
          body:{
            para:jpara
        },
        json:true
  };
  let data = await rp(options);
  return data;
}
```
### 注意 
* 所有遇到的异常，都是因为json构造错误造成
* 切记，json里面,是7个配对的key和value，差1个都不行

## 多开
```sh
chmod +x build-injector-box.sh
./build-injector-box.sh
sudo docker run -itd --name wechat-bot --rm  \
    -e HOOK_PROC_NAME=WeChat \
    -e TARGET_AUTO_RESTART="yes" \
    -e INJ_CONDITION="[ \"\`ps -aux | grep funtool | grep -v grep\`\" != '' ] && exit 0" \
    -e TARGET_CMD=wechat-start \
    -p 8080:8080 -p 5555:5555 -p 5900:5900 \
    --add-host=dldir1.qq.com:127.0.0.1 \
    chisbread/wechat-bot:1.0
```
```sh
# 与宿主机共享屏幕, 可以共享剪切板和输入法, 但是图形界面依旧会有些bug, 
# 整体功能不受影响。与宿主机共享屏幕会使vnc功能会失效, 这里就取消了vnc端口映射
xhost +local: # or xhost + # 这个很重要, 不然docker容器正常启动也不会显示微信界面
docker run -d --name wechat-bot1 \
    --device /dev/snd \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v $HOME/WeChatFiles:'/home/app/WeChat Files' \
    -e DISPLAY=unix$DISPLAY \
    -e XMODIFIERS=@im=fcitx \
    -e QT_IM_MODULE=fcitx \
    -e GTK_IM_MODULE=fcitx \
    -e AUDIO_GID=`getent group audio | cut -d: -f3` \
    -e GID=`id -g` \
    -e UID=`id -u` \
    -e HOOK_PROC_NAME=WeChat \
    -e TARGET_AUTO_RESTART="yes" \
    -e INJ_CONDITION="[ \"\`ps -aux | grep funtool | grep -v grep\`\" != '' ] && exit 0" \
    -e TARGET_CMD=wechat-start -p 5555:5555 --add-host=dldir1.qq.com:127.0.0.1 \
    chisbread/wechat-bot:1.0
```
### 参考项目
* https://github.com/ChisBread/wechat-service
* 记得为这位大佬加星
