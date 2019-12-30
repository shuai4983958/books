#### 课程商品流程

> 一些静态的无复杂交互的页面就不写了，重点梳理一下课程以及C端直播的逻辑

#### 1. 课程，商品相关

##### （1）. 创建课程

   1. [路由](http://admin105.aijianzi.com/#/subjectSales/course-manage/detail)
   2. 功能：
      1. 发布大纲以后不可编辑
      2. 课程信息的选择是相互影响的，大致总结了一版本，可能不全，具体还是以代码为准[爱尖子课程创建联动逻辑](http://note.youdao.com/noteshare?id=f572c98bcbaeae8dd5e09f7245bfed18)


##### （2）. 创建课程大纲

   1. [路由](http://admin105.aijianzi.com/#/subjectSales/course-manage/course?courseId=1023&syllabusId=1027&watchType=1)
   2. 在创建完课程以后，在进入课程大纲页面之前会先选择是否创建，[进入课程大纲页面以后，先提交大纲时间才可对大纲进行编辑](http://39.107.98.122:8090/aijianzi/FE/manageside/AiJianZiPlatform/blob/inner_test/src/pages/SubjectSales/CourseManage/Syllabus/components/CourseStartDate.tsx)，在这里只对课程类型分了组件，[统一了有无章节的接口](http://39.107.98.122:8090/aijianzi/FE/manageside/AiJianZiPlatform/blob/master/src/models/SubjectSales/allCourseManage.js)中的`fetchSyllabusInfo`
  ```
  onChangeChapter = (checked: number) => {
    this.setState({
      haveChapter: checked ? 1 : 0,
    });
  };
  import CourseStartDate from './components/CourseStartDate';

  ```
   3. 录播课课程大纲
```
import RecordCourse from '../recordCourse/list';
```
    限制：整体来说录播课的课程大纲是比较清晰跟简单的
    1.唯一的限制，大纲是否发布，跟时间无关
    2.章节内排序，需要至少2个章节才能激活排序按钮，无法跨章节排序

  1. 章节：新建，修改， 删除章节：只需要章节名称
  2. 课次：按类型区分（3种）
      1. 录播讲：
            1. 录播名称，
            2. 视频来源 可修改删除，跟时间无关
               1. 回放(需要关联回放录播资源)
               2. 素材
      2. 考试讲：
           1. 考试名称
           2. 时长

   3. [章节内排序](http://39.107.98.122:8090/aijianzi/FE/manageside/AiJianZiPlatform/blob/inner_test/src/pages/SubjectSales/CourseManage/Syllabus/index.tsx)中的`sortLessonsById`，需要前端做聚合，这里主要用了`findIndex`的功能
```

```

  4. 直播课课程大纲，包含直播，双师直播
```
import CourserLiveVideo from './components/CourserLiveVideo';
```
    限制：整体来说直播课比录播稍微类型多一点，但也不复杂
    1.唯一的限制，大纲是否发布，内部的课节修改，增加，删除权限跟课节的时间无关
    2.章节内排序，需要至少2个章节才能激活排序按钮，无法跨章节排序

  1. 章节：新建，修改， 删除章节：只需要章节名称
  2. 课次：按类型区分（3种）内部的课节修改，增加，删除权限跟课节的时间无关
      1. 双师直播讲：
            1. 课节名称
            2. 开课时间
      2. 校内讲：
            1. 课节名称
            2. 开课时间
      3. 考试讲：
            1. 课节名称
            2. 开考时间
               1. 开考开始日期 === 选择卡片的日期 
               2. 开考的结束日期 <= 大纲的结束日期
            3. 考试时长
 ```
 ```
   5. 发布大纲，由后端控制，需要提交一个课程大纲的时间，就可以发布，可以是个空的模版授课老师可以随后指定

##### （3）. 发布大纲

   1. [路由](http://admin105.aijianzi.com/#/subjectSales/course-manage/list)
   
   2. 逻辑功能：
      1. 无任何限制，可以发布一个空的大纲，也可以发布带任何类型的课次的大纲


##### （4）. 创建商品

   1. [路由](http://admin105.aijianzi.com/#/subjectSales/commodity-course/detail)
   
   2. 逻辑功能：
      1. 商品的名称取决于关联课程的名称。
      2. 商品不可以关联未发布的大纲。

##### （5）. 商品上架，下架

   1. [路由](http://admin105.aijianzi.com/#/subjectSales/commodity-course/list)
   
   2. 逻辑功能：
      1. 是否关联课程
      2. 课程中的直播讲全部关联组件，直播和双师直播商品，上架时要求每个直播/双师直播课次都需关联主讲老师， 否则上架状态一直显示：准备中
      3. 下架需要谨慎，没有逻辑限制


##### （5）. 课程的内容关联
   1. [路由](http://admin105.aijianzi.com/#/teachResearch/lesson-connect/detail?courseId=1024)
   
   2. 逻辑功能：
      1. 渲染有无章节的课程这里没问题
         1. 课节类型(0:直播讲;1:考试;2:录播;3:测评;4:双师;5:校内讲)
         2. 录播要区分'来源（1回放；2素材；）',
      2. 组件类型的操作上有些限制和处理
      3. [不同组件的编辑，禁用，删除逻辑](http://39.107.98.122:8090/aijianzi/FE/manageside/AiJianZiPlatform/blob/inner_test/src/pages/TeachResearch/ContentConnect/lesson-connect-detail.js)：
         1. 启用禁用权限:`handleDisabledFlag`,
         2. 编辑逻辑`onConfirmWhetherAnswerOrnot`
         3. 删除`onClickRelationShips`,`onGetRelationStatus`
         通过课次的类型，来控制不同组件的编辑，禁用，删除逻辑
      4. [增加作业功能](http://39.107.98.122:8090/aijianzi/FE/manageside/AiJianZiPlatform/blob/inner_test/src/pages/TeachResearch/ContentConnect/lesson-connect-detail.js)（0:直播讲;4:双师;5:校内讲以及录播素材2）录播回放和测试是无增加作业功能的
         1. 启用状态：当前的课节的开始时间 > 当前的时区时间（课节未开始）
         2. 课节类型下的组件类型作业（HOMEWORK）增加次数暂设上限为1次,

|组件类型 | 组件名称 | 操作|
|---|---|--
|COURSEWARE：课件|上传过可点击下载，无上传只显示名成|上传、删除|
| HANDOUT：讲义|上传过可点击下载，无上传只显示名成|上传、删除|
|DATAGRAM： 资料包|上传过可点击下载，无上传只显示名成| 上传、删除|
|EXCELLENTVIDEO：精品视频|点击弹窗|编辑，删除|
|DIAGNOSE_REPORT: 测评诊断报告| 只显示名称|设置报告
|其他所有组件类型|只显示名称| 编辑、删除|
   3. 其实总的来说，就是不同的课节类型下的不同组件的处理方式不同，最大程度的体现在不同组件编辑所起的弹窗不同
   (toDO DIAGNOSE_REPORT flag中的字段跟后端结构匹配)

##### （6）. 课程排班管理   
  1. [路由](http://admin105.aijianzi.com/#/teachingBack/schedule-manage/lession?syllabusId=1021)
  2. 功能：
     1. 把课节关联上主讲老师
     2. 关联老师在上架之前操作都可以，发布大纲以后可以
   





// 画一个流程