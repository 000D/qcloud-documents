## ��Ƶ�༭

### ���ܸ���

��Ƶ�༭�ǵ�ǰ���е�С��Ƶ�����һ������ȱ�ٵĹ��ܡ�RTMP SDK 2.0.2�ṩ��������Ĳü��ͺϲ����ܣ��˾�����������Ļ�ȸ߼��������ڿ����У���

### �ӿڽ���

| ����                   | ����       |
| -------------------- | -------- |
| TXUGCVideoInfoReader | ��Ƶ�ļ���Ϣ��ȡ |
| TXUGCEditer          | ��Ƶ�༭     |
| TXUGCJoiner          | ��Ƶ�ϲ�     |
| TXOperationParam     | �������Ͷ���   |

### ���벽��

### 1. ��Ƶ��Ϣ��ȡ
#### 1.1 ��ȡ��Ƶ������Ϣ
Ϊ�˷���ʹ�ã�TXUGCVideoInfoReader�ṩ�˷�����һ���Զ������������Ϣ����Щ��Ϣ���ں�������Ƶ�༭������ʹ�á�

```objective-c
/*
 * ��Ƶ��Ϣ
 */
@interface TXVideoInfo : NSObject
@property (nonatomic, strong) UIImage*              coverImage;     //��Ƶ��֡ͼƬ
@property (nonatomic, assign) CGFloat               duration;       //��Ƶʱ��(s)
@property (nonatomic, assign) unsigned long long    fileSize;       //��Ƶ��С(byte)
@property (nonatomic, assign) int                   fps;            //��Ƶfps
@property (nonatomic, assign) int                   bitrate;        //��Ƶ���� (kbps)
@property (nonatomic, assign) int                   audioSampleRate;//��Ƶ������
@property (nonatomic, assign) int                   width;          //��Ƶ���
@property (nonatomic, assign) int                   height;         //��Ƶ�߶�
@end

+ (TXVideoInfo *)getVideoInfo:(NSString *)videoPath;
```

#### 1.2 ����Ԥ������ͼ
��Ƶ�༭ͨ��Ҫ�ṩһ������ͼ�б��Ա��û�����Ԥ���Ͷ�λ��Ƶ�ļ�����ȡ��Ƶ����ͼ�ķ������£�

```objective-c
[TXUGCVideoInfoReader getSampleImages:10 videoPath:_videoPath progress:^(int number, UIImage *image) {
        // UI��ʾ iamge
    }];
```

����������Ƶ�ļ�·��������������ͼ������˴���10��ʹ���߿��Ը���App������Ƶ�Ԥ�������С���������Ҫ������ͼ������SDK������Ƶ�ļ���ʱ�������ȶ�ȡָ����������Ƶ��ͼ��

### 2. ��Ƶ�༭
#### 2.1 ��ƵԤ��
��Ƶ�༭�ṩ�˼�ʱԤ���벥��Ԥ�����ַ�ʽ��Ԥ����Ҫ�ϲ��ṩһ��UIView������ʾ��Ƶ���档

����`- (instancetype)initWithPreview:(TXPreviewParam *)param`����TXUGCEditer���󣬱༭�����еļ�ʱԤ���Ͳ��Ŷ�����Ⱦ�����UIView�ϡ�

Ԥ����֡���棬������ͼ���ƶ����Ը��ݵ�ǰʱ���Ԥ��ĳһ֡���棬ֱ�ӵ���`- (void)previewAtTime:(CGFloat)time;`�˺������ɣ���ǰ��Ƶ֡���Զ���ʾ�ڴ����Ԥ��view�ϣ�ͨ��ָ��Ԥ��view����Ⱦģʽ������������Ӧ���������ģʽ��

*  `PREVIEW_RENDER_MODE_FILL_SCREEN`��ʾ���ģʽ
*  `PREVIEW_RENDER_MODE_FILL_EDGE`��ʾ����Ӧģʽ

Ԥ��������Ƶ��ֱ�ӵ���`- (void)startPlayFromTime:(CGFloat)startTime toTime:(CGFloat)endTime`ʵ�֣���ʾԤ������ĳ��ʱ���������Ƶ����ƵԤ���Ľ��ȿ���ͨ��`previewDelegate`��ȡ��


#### 2.2 �ü�������Ƶ 

��һ����Ƶ�༭�кܶ��ֲ������ü�ֻ������һ�֡���ˣ����ǽ���Щ��������Ϊ`TXOperationParam`����

```objective-c
/*
 * ��Ƶ����
 */
@interface TXOperationParam : NSObject
@property (nonatomic, assign) TXOperationType       type;           //��������
@property (nonatomic, assign) CGFloat               startTime;      //������ʼʱ��(s)
@property (nonatomic, assign) CGFloat               endTime;        //��������ʱ��(s)
@end
```

TXOperationTypeĿǰֻ֧��OPERATION_TYPE_CUT��������Ƶ����startTime��ʾ�ü��Ŀ�ʼʱ�䣬endTime��ʾ�ü��Ľ���ʱ�䡣

������Ƶ���������ͨ������`- (int)setOperationList:(NSArray<TXOperationParam *> *)operationList`���ñ༭���������������ǰ�ǰ�����Ӧ����ÿһ֡�ϣ��������Ϊһ���ļ���

```objective-c
TXUGCEditer* _ugcEdit = [[TXUGCEditer alloc] initWithPreview:param];
TXOperationParam *param = [[TXOperationParam alloc] init];
param.type      = OPERATION_TYPE_CUT;
param.startTime = _videoRangeSlider.leftPos;
param.endTime   = _videoRangeSlider.rightPos;
[_ugcEdit setOperationList:@[param]];
[_ugcEdit generateVideo:VIDEO_COMPRESSED_540P videoOutputPath:_videoOutputPath];
```
���ʱָ���ļ�ѹ�����������·��������Ľ��Ⱥͽ����ͨ��`generateDelegate`�Իص�����ʽ֪ͨ�û���

### 3. ��Ƶ�ϳ�

��Ƶ�ϳ���Ҫ����TXUGCJoiner����ͬTXUGCEditer���ƣ��ϳ�Ҳ��Ҫ�ϲ��ṩԤ��UIView��eg��
```objective-c
TXPreviewParam *param = [[TXPreviewParam alloc] init];
param.videoView = _videoPreview.renderView;
TXUGCJoiner* _ugcJoin = [[TXUGCJoiner alloc] initWithPreview:param];
[_ugcJoin setVideoPathList:_composeArray];//��Ҫ�ϲ����ļ�����
_ugcJoin.previewDelegate = _videoPreview;
```
���ú�Ԥ��viewͬʱ������ϳɵ���Ƶ�ļ�����󣬿��Կ�ʼ����Ԥ�����ϳ�ģ���ṩ��һ��ӿ�������Ƶ�Ĳ���Ԥ����eg��

*  `startPlay`��ʾ��Ƶ���ſ�ʼ
*  `pausePlay`��ʾ��Ƶ������ͣ
*  `resumePlay`��ʾ��Ƶ���Żָ�

Ԥ��Ч�������������ɽӿڼ������ɺϳɺ���ļ���eg��
```objective-c
_ugcJoin.composeDelegate = self;
[_ugcJoin composeVideo:VIDEO_COMPRESSED_540P videoOutputPath:_outFilePath];
```

�ϳ�ʱָ���ļ�ѹ�����������·��������Ľ��Ⱥͽ����ͨ��`composeDelegate`�Իص�����ʽ֪ͨ�û���

### 