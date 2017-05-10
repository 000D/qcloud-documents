## �Խ�����

- **����Ƶ¼��**��
���ɼ�����ͷ�������˷�����������ͼ�����������󣬽��б���ѹ�������������������ȵ� MP4 �ļ���

- **����Ƶ����**��
���� MP4 �ļ��ϴ�����Ѷ��Ƶ�ƣ���������߹ۿ� URL�� ��Ѷ��Ƶ��֧����Ƶ�ۿ��ľͽ����ȡ��뿪���š���̬���� �Լ���������Ҫ�󣬴Ӷ�ȷ�����ʵĹۿ����顣

![](//mc.qcloudimg.com/static/img/283c8d7fe0a5a316097ae687a2bf6c5a/image.png)

* ��һ����ʹ�� TXUGCRecord �ӿ�¼��һ��С��Ƶ��¼�ƽ����������һ��С��Ƶ�ļ���MP4���ص����ͻ���

* �ڶ��������� APP ������ҵ������������ϴ�ǩ�����ϴ�ǩ���� APP �� MP4 �ļ��ϴ�����Ѷ����Ƶ�ַ�ƽ̨�� �����֤����Ϊ��ȷ����ȫ�ԣ���Щ�ϴ�ǩ����Ҫ��������ҵ�� Server ����ǩ�������������ն� App ���ɡ�

* ��������ʹ�� TXUGCPublish �ӿڷ�����Ƶ�������ɹ��� SDK �Ὣ�ۿ���ַ�� URL �ص�������

## �ر�ע��

- APP ǧ��Ҫ�Ѽ����ϴ�ǩ���� SecretID �� SecretKey д�ڿͻ��˵Ĵ�����������ؼ���Ϣй¶�����°�ȫ������������⹥����һ���ƽ� APP ��ȡ����Ϣ���Ϳ������ʹ�����������ʹ洢����

- ��ȷ�������������ķ���������  SecretID �� SecretKey ����һ���Ե��ϴ�ǩ��Ȼ��ǩ������ APP����Ϊ������һ����ѱ����ݣ����԰�ȫ���ǿ��Ա�֤�ġ�

- ��������Ƶʱ������ر�֤��ȷ���� SecretID �� Signature �ֶΣ�����ᷢ��ʧ�ܣ�

- �Զ���Ƶ¼�ƽӿ� startRecord �� stopRecord �ĵ��ñ��뱣֤��ԣ�

- ��Ƶ¼��ʱ���ɿͻ����Ĵ��������ƣ��������ܿ��ǣ�Ϊ�˱�֤���õ��û����飬����¼��ʱ���������60�롣��


## �ӿڽ��� 
��Ѷ�� UGC SDK �ṩ����ؽӿ�����ʵ�ֶ���Ƶ��¼���뷢��������ϸ�������£�

|  �ӿ��ļ� |  ���� |
| ------| --------|
| `TXUGCRecord.java` |ʵ��С��Ƶ��¼�ƹ���|
| `TXUGCPublish.java` | ʵ��С��Ƶ���ϴ����� |
| `TXRecordCommon.java` | �����������壬������С��Ƶ¼�ƻص��������ص��ӿ� |

## �Խӹ���

![](http://mc.qcloudimg.com/static/img/6b21b033259c1b5124648b73e88fb243/image.png)


### 1. ����Ԥ��
TXUGCRecord��λ�� TXUGCRecord.java�� ����С��Ƶ��¼�ƹ��ܣ����ǵĵ�һ���������Ȱ�Ԥ������ʵ�֡�startCameraSimplePreview������������Ԥ������������Ԥ��Ҫ������ͷ����˷磬����������ܻ���Ȩ���������ʾ����

```java
mTXCameraRecord = TXUGCRecord.getInstance(this.getApplicationContext());
mTXCameraRecord.setVideoRecordListener(this);					//����¼�ƻص�
mVideoView = (TXCloudVideoView) findViewById(R.id.video_view);	//׼��һ��Ԥ������ͷ�����TXCloudVideoView
mVideoView.enableHardwareDecode(true);
TXRecordCommon.TXUGCSimpleConfig param = new TXRecordCommon.TXUGCSimpleConfig();
//param.videoQuality = TXRecordCommon.VIDEO_QUALITY_LOW;		// 360p
param.videoQuality = TXRecordCommon.VIDEO_QUALITY_MEDIUM;		// 540p
//param.videoQuality = TXRecordCommon.VIDEO_QUALITY_HIGH;		// 720p
param.isFront = true;											//�Ƿ�ǰ������ͷ��ʹ�� switchCamera �����л�
mTXCameraRecord.startCameraSimplePreview(param,mVideoView);
```

### 2. ������Ч
������¼��ǰ������¼���У���������ʹ�� TXUGCRecord �����Ч������Ϊ��Ƶ�������һЩ��Ч�������Ƕ�����ͷ������п��ơ�

```java
//////////////////////////////////////////////////////////////////////////
//                      ����Ϊ 1.9.1 �汾���֧�ֵ���Ч
//////////////////////////////////////////////////////////////////////////
//
// �л�ǰ������ͷ ���� mFront �����Ƿ�ǰ������ͷ Ĭ��ǰ��
mTXCameraRecord.switchCamera(mFront);

// �������� �� ���� Ч������
// beautyDepth     : ���ռ���ȡֵ��Χ 0 ~ 9�� 0 ��ʾ�ر� 1 ~ 9ֵԽ�� Ч��Խ���ԡ�
// whiteningDepth  : ���׼���ȡֵ��Χ 0 ~ 9�� 0 ��ʾ�ر� 1 ~ 9ֵԽ�� Ч��Խ���ԡ�
mTXCameraRecord.setBeautyDepth(beautyDepth, whiteningDepth);

// ������ɫ�˾������������¡�Ψ�������ۡ�����...
// Bitmap     : ָ���˾��õ���ɫ���ұ�ע�⣺һ��Ҫ��png��ʽ������
// demo�õ����˾����ұ�ͼƬλ��RTMPAndroidDemo/app/src/main/res/drawable-xxhdpi/Ŀ¼�¡�
// setSpecialRatio : ���������˾���Ч���̶ȣ���0��1��Խ���˾�Ч��Խ���ԣ�Ĭ��ȡֵ0.5
mTXCameraRecord.setFilter(filterBitmap);
mTXCameraRecord.setSpecialRatio(0.5);

// �Ƿ������� ���� mFlashOn �����Ƿ�����ص� Ĭ�Ϲر�
mTXCameraRecord.toggleTorch(mFlashOn);

//////////////////////////////////////////////////////////////////////////
//                       ����Ϊ����Ȩ���֧�ֵ���Ч
// �����ڲ�����ͼ�Ŷӵ�֪ʶ��Ȩ�������޷���������ṩ����Ҫʹ����Ȩ�� SDK ����֧�֣�
//////////////////////////////////////////////////////////////////////////

// ���ö�Ч��ֽ motionTmplPath ��Ч�ļ�·���� ��String "" ��ȡ����Ч
mTXCameraRecord.setMotionTmp(motionTmplPath);
```


### 3. �ļ�¼��
���� TXUGCRecord �� startRecord �������ɿ�ʼ¼�ƣ����� stopRecord �������ɽ���¼�ƣ�startRecord �� stopRecord �ĵ���һ��Ҫ��ԡ�
```java
mTXCameraRecord.startRecord();
mTXCameraRecord.stopRecord();
``` 

¼�ƵĹ��̺ͽ����ͨ�� TXRecordCommon.ITXVideoRecordListener��λ�� TXRecordCommon.java �ж��壩�ӿڷ��������ģ�

- onRecordProgress ���ڷ���¼�ƵĽ��ȣ�����millisecond��ʾ¼��ʱ������λ����:
```java
@optional
void onRecordProgress(long milliSecond);
``` 

- onRecordComplete ����¼�ƵĽ����TXRecordResult �� retCode �� descMsg �ֶηֱ��ʾ������ʹ���������Ϣ��videoPath ��ʾ¼����ɵ�С��Ƶ�ļ�·����coverImage Ϊ�Զ���ȡ��С��Ƶ��һ֡���棬��������Ƶ�����׶�ʹ�á�
```java   
@optional
void onRecordComplete(TXRecordResult result);
```     

### 4. �ļ�Ԥ��
ʹ�� [����SDK](https://www.qcloud.com/document/product/454/7886) ����Ԥ���ղ����ɵ� MP4 �ļ�����Ҫ�ڵ��� startPlay ʱָ����������Ϊ [PLAY_TYPE_LOCAL_VIDEO](https://www.qcloud.com/document/product/454/7886#step-3.3A-.E5.90.AF.E5.8A.A8.E6.92.AD.E6.94.BE.E5.99.A86) ��

### 5. ��ȡǩ��
Ҫ�Ѹղ����ɵ� MP4 ��������Ѷ����Ƶ�ַ� CDN �ϣ�����Ҫ **SecretID** �� **Signature**���������������û���������һ����ȷ�������ƴ洢����ȫ���������������ʹ洢�ռ䱻���������ߵ��á�

- **SecretID ����ԿID��**
������� [�� API ��Կ](https://console.qcloud.com/capi) �������ȡ���ߴ���һ�� SecretID������ͼ����ע���֣�
![](http://mc.qcloudimg.com/static/img/23f95aaa97adf3eeae3bf90470fe5122/image.png)

- **Signature���ϴ�ǩ����**
�ϴ�ǩ�����ǻ��ڴ���Ѷ�ƻ�ȡ�� SecretID �� SecretKey ����һ�ױ�׼��ǩ���㷨�������һ��һ������Ч���ַ�����

 Ϊ��ȷ����ȫ����Ҫ��������ǩ���ĳ���������ĺ�̨�������ϣ������ǰѼ��㺯��д�� APP ���Ϊ�ƽ� APP ����ȡǩ���õ� SecretKey �ǱȽ����׵����飬��Ҫ�������ķ������򲢷���һ�������Ĺ����������õ��ġ�

 ǩ�����㷽���ο���[�������ǩ����](https://www.qcloud.com/document/product/266/7835?!preview&lang=zh#.E8.8E.B7.E5.8F.96.E7.AD.BE.E5.90.8D.E8.AE.A1.E7.AE.97.E6.89.80.E9.9C.80.E4.BF.A1.E6.81.AF) ���ɷ���ǩ��ʱ��<font color='red'>FileName��FileSha �Լ� uid �ֶζ��������ղ���д��</font>

### 6. �ļ�����
TXUGCPublish��λ�� TXUGCPublish.java������ MP4 �ļ���������Ѷ����Ƶ�ַ�ƽ̨�ϣ���ȷ����Ƶ�ۿ��ľͽ����ȡ��뿪���š���̬���� �Լ�������������

```java
TXRecordCommon.TXPublishParam param = new TXRecordCommon.TXPublishParam();
param.secretId = "sdIDeqtlGihED4oqjRP2324seJn1313MLnxx"; // ��Ҫ��д���� SecretId
param.signature = mCosSignature;						// ��Ҫ��д���Ĳ��м�����ϴ�ǩ��
// ¼�����ɵ���Ƶ�ļ�·��, ITXVideoRecordListener �� onRecordComplete �ص��п��Ի�ȡ
param.videoPath = mVideoPath;
// ¼�����ɵ���Ƶ��֡Ԥ��ͼ��ITXVideoRecordListener �� onRecordComplete �ص��п��Ի�ȡ
param.coverPath = mCoverPath;
mVideoPublish.publishVideo(param);
``` 

�����Ĺ��̺ͽ����ͨ�� TXRecordCommon.ITXVideoPublishListener��λ�� TXRecordCommon.java ͷ�ļ��ж��壩�ӿڷ��������ģ�

- onPublishProgress ���ڷ����ļ������Ľ��ȣ����� uploadBytes ��ʾ�Ѿ��ϴ����ֽ��������� totalBytes ��ʾ��Ҫ�ϴ������ֽ�����
```java
void onPublishProgress(long uploadBytes, long totalBytes);
```

- onPublishComplete ���ڷ������������TXPublishResult ���ֶ� errCode �� descMsg �ֱ��ʾ������ʹ���������Ϣ��videoURL��ʾ����Ƶ�ĵ㲥��ַ��coverURL��ʾ��Ƶ������ƴ洢��ַ��videoId��ʾ��Ƶ�ļ��ƴ洢Id��������ͨ�����Id���õ㲥 [�����API�ӿ�](https://www.qcloud.com/document/product/266/1965)��
```java 
void onPublishComplete(TXPublishResult result);
```