<table style="display:table">
    <tbody>
        <tr>
            <td style="background-color:#CCCCCC;">
                <strong>
                    分类
                </strong>
            </td>
            <td style="background-color:#CCCCCC;">
                <strong>
                    功能
                </strong>
            </td>
            <td style="background-color:#CCCCCC;">
                <strong>
                    接口名称
                </strong>
            </td>
        </tr>
        <!--视频上传-->
        <tr>
            <td rowspan=2>
                服务端视频上传
            </td>
            <td>
                发起上传
            </td>
            <td>
                <a href="/document/product/266/9756">
                    ApplyUpload
                </a>
            </td>
        </tr>
        <tr>
            <td>
                确认上传
            </td>
            <td>
                <a href="/document/product/266/9757">
                    CommitUpload
                </a>
            </td>
        </tr>
        <!--URL拉取视频上传-->
        <tr>
            <td rowspan=1>
                URL拉取视频上传
            </td>
            <td>
                URL拉取视频上传
            </td>
            <td>
                <a href="/document/product/266/7817">
                    MultiPullVodFile
                </a>
            </td>
        </tr>
        <!--视频管理-->
        <tr>
            <td rowspan=9>
                视频管理
            </td>
            <td>
                获取视频信息
            </td>
            <td>
                <a href="/document/product/266/7824">
                    DescribeVodPlayUrls
                </a>
            </td>
        </tr>
        <tr>
            <td>
                获取视频信息V2
            </td>
			<td>
                <a href="/document/product/266/8586">
                    GetVideoInfo
                </a>
            </td>
        </tr>
        <tr>
            <td>
                批量获取视频信息
            </td>
			<td>
                <a href="/document/product/266/7823">
                    DescribeVodInfo
                </a>
            </td>
        </tr>
        <tr>
            <td>
                依照视频名称前缀获取视频信息
            </td>
            <td>
                <a href="/document/product/266/7825">
                    DescribeVodPlayInfo
                </a>
            </td>
        </tr>
        <tr>
            <td>
                增加视频标签
            </td>
            <td>
                <a href="/document/product/266/7826">
                    CreateVodTags
                </a>
            </td>
        </tr>
        <tr>
            <td>
                删除视频标签
            </td>
            <td>
                <a href="/document/product/266/7827">
                    DeleteVodTags
                </a>
            </td>
        </tr>
        <tr>
            <td>
                修改视频属性
            </td>
            <td>
                <a href="/document/product/266/7828">
                    ModifyVodInfo
                </a>
            </td>
        </tr>
        <tr>
            <td>
                删除视频
            </td>
            <td>
                <a href="/document/product/266/7838">
                    DeleteVodFile
                </a>
            </td>
        </tr>
        <tr>
            <td>
                截图地址设为视频封面
            </td>
            <td>
                <a href="/document/product/266/8814">
                    DescribeVodCover
                </a>
            </td>
        </tr>
        <!--视频处理-->
        <tr>
            <td rowspan=7>
                视频处理
            </td>
            <td>
                对视频文件进行处理
            </td>
            <td>
                <a href="/document/product/266/9642">
                    ProcessFile
                </a>
            </td>
        </tr>
        <tr>
            <td>
                视频转码
            </td>
            <td>
                <a href="/document/product/266/7822">
                    ConvertVodFile
                </a>
            </td>
        </tr>
        <tr>
            <td>
                视频拼接
            </td>
            <td>
                <a href="/document/product/266/7821">
                    ConcatVideo
                </a>
            </td>
        </tr>
        <tr>
            <td>
                视频剪辑
            </td>
            <td>
                <a href="/document/product/266/10156">
                    ClipVideo
                </a>
            </td>
        </tr>
        <tr>
            <td>
                截取雪碧图
            </td>
            <td>
                <a href="/document/product/266/8101">
                    CreateImageSprite
                </a>
            </td>
        </tr>
        <tr>
            <td>
                按时间点截图
            </td>
            <td>
                <a href="/document/product/266/8102">
                    CreateSnapshotByTimeOffset
                </a>
            </td>
        </tr>
        <tr>
            <td>
                HLS视频简单剪切
            </td>
            <td>
                <a href="/document/product/266/8859">
                    SimpleClipHls
                </a>
            </td>
        </tr>
        <!--
        <tr>
            <td>
                HLS视频简单拼接
            </td>
            <td>
                <a href="/document/product/266/7820">
                    SimpleConcatHls
                </a>
            </td>
        </tr>
        -->
        <!--视频分类管理-->
        <tr>
            <td rowspan=5>
                视频分类管理
            </td>
            <td>
                创建视频分类
            </td>
            <td>
                <a href="/document/product/266/7812">
                    CreateClass
                </a>
            </td>
        </tr>
        <tr>
            <td>
                获取视频分类层次结构
            </td>
            <td>
                <a href="/document/product/266/7813">
                    DescribeAllClass
                </a>
            </td>
        </tr>
        <tr>
            <td>
                获取视频分类信息
            </td>
            <td>
                <a href="/document/product/266/7814">
                    DescribeClass
                </a>
            </td>
        </tr>
        <tr>
            <td>
                修改视频分类
            </td>
            <td>
                <a href="/document/product/266/7815">
                    ModifyClass
                </a>
            </td>
        </tr>
        <tr>
            <td>
                删除视频分类
            </td>
            <td>
                <a href="/document/product/266/7816">
                    DeleteClass
                </a>
            </td>
        </tr>
        <!--可靠事件通知-->
        <tr>
            <td rowspan=2>
                可靠事件通知
            </td>
            <td>
                拉取事件通知
            </td>
            <td>
                <a href="/document/product/266/7818">
                    PullEvent
                </a>
            </td>
        </tr>
        <tr>
            <td>
                确认事件通知
            </td>
            <td>
                <a href="/document/product/266/7819">
                    ConfirmEvent
                </a>
            </td>
        </tr>
        <!--点播1.0兼容接口-->
        <tr>
            <td>
                点播1.0兼容接口
            </td>
            <td>
                依照VID查询视频信息
            </td>
            <td>
                <a href="/document/product/266/8227">
                    DescribeRecordPlayInfo
                </a>
            </td>
        </tr>
        <!--任务管理-->
        <!-- <tr>
        <td rowspan=1>任务管理</td>
        <td>查询异步任务的状态</td>
        <td><a href="">QueryVodTaskStatus</a></td></tr>
        -->
    </tbody>
</table>