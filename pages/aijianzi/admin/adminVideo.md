#### 管理端C端直播交接文档



##### （1）. 课程入口限制

   1. [路由](http://admin105.aijianzi.com/#/teachingBack/myCourse/list)
   2. [功能](http://39.107.98.122:8090/aijianzi/FE/manageside/AiJianZiPlatform/blob/inner_test/src/pages/TeachingBack/myCourse/myCourse-list.js)：`renderCardFooter`

      1. 上课之前，C端直播提前半个小时打开，双师直播提前 1个小时
      2. 下课之后，C端直播延迟半个小时关闭，双师直播延迟 半小时

##### （2）. [小助手安装及注意事项](https://note.youdao.com/ynoteshare1/index.html?id=67f87cff086cea61c8210706fc46c414&type=note)：

   1. 协议写入windows注册表，伪协议启动程序
   2. 参数说明 比如：小助手配置表config.ni文件参数具体的合图步骤
      1. 打开合图开关
      ```
      publishStream\merge=false 改为  publishStream\merge=true
      ```
      2. 合图位置
      ```
      publishStream\width=960         // 合图容器的宽度，也是最终合图的宽度
      publishStream\height=720        // 合图容器的高度，也是最终合图的高度
      publishStream\subX=752          // 小流，即视频头像流，相对于合图容器的的左上角x轴坐标点
      publishStream\subY=0            // 小流，即视频头像流，相对于合图容器的的左上角y轴坐标点
      publishStream\mainX=0           // 大流，即屏幕录制流，相对于合图容器的的左上角x轴坐标点
      publishStream\mainY=0           // 大流，即屏幕录制流，相对于合图容器的的左上角y轴坐标点
      publishStream\subWidth=208      // 小流，即视频头像流的宽度
      publishStream\subHeight=117     // 小流，即视频头像流的高度
      publishStream\mainWidth=960     // 大流，即屏幕录制流的宽度，
      publishStream\mainHeight=720    // 大流，即屏幕录制流的高度
      ```
  
      3. 上述的参数明确：
         1. 输出宽为960，高为720的合图视频.
         2. 默认如果位置有重叠小流会层级会高于大流其中小流的左上角位置为（752，0）
         3. 宽为960-752=208，高为（宽高比16:9算出117），位置为合图容器的右上角大流的左上角位置为（0，0）
         4. 宽为960，高尾720，盛满了整个容器。




##### （3）. [C端直播信令原始设计](http://note.youdao.com/noteshare?id=c50902717442cbe0c4b094e73f30830e)

   1. [路由](http://admin105.aijianzi.com/#/teachingBack/myCourse/video?lessonId=11749)
   2. 课程状态
      1. 老师点击上课以后，学生才可以加入直播频道。
      2. 老师未点击上课，学生可以提前加入信令频道，课前聊天。
      3. 老师点击暂停，只是mute掉老师的声音。
      4. 老师点击继续上课，录制老师的声音
      5. 老师点击下课，老师退出直播，信令房间，学生接受到下课指令以后同步退出直播，信令房间

   3. 题目相关的接口整理：
      1. 老师推题

      2. 学生根据信令内容去生成对应的答题记录id

      3. 学生拉题目，

      4. 提交

      5. 拉结果

   4. 页面初始化，以及调用顺序
     
      1. 通过课程系统lessonId去直播系统查询是否存在roomId
      2. 存在即获取，不存在通过lessonId创建roomId
      3. 获取roomId以后，获取房间的状态，
      4. 开始鉴权，异步获取web端以及windows端直播，信令鉴权token，
      5. 获取widows端鉴权信息通过websocket通信初始化小助手。
   5. [唯一的n判断小助手启动的时候是否在同一个频道里](http://39.107.98.122:8090/aijianzi/FE/manageside/AiJianZiPlatform/blob/inner_test/src/pages/TeachingBack/myCourse/myCourse-video.js)的`rtcHelperIsRoom`

      1. 严格比较roomId以及主流，小流的uid和token是否严格严格相等
      2. 如果相等，获取windwos端是否参入直播间，如果没有加入，重新加入
      3. 如果不严格相等，进入新的房间


// todo稳定版本小助手

