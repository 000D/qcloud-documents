## Product Architecture

### Overall Architecture
This section describes the design and implementation of CCS system, and its product architecture is as follows:
![Alt text](https://mc.qcloudimg.com/static/img/8381e7cdb015bf0f8d01437287779f92/%7B6271F726-FE22-4222-B487-81ACEA386EED%7D.png)

CCS consists of the following modules:

- **CCS console and APIs**: Users work with clusters and services through the console and cloud APIs.
- **User terminal**: Users perform operations such as upload/download and automatic building of images on their terminals or UI provided by Docker.
- **Image service modules:** Users can upload or download images locally using the image service modules provided by Tencent Cloud.
- **Container service modules**: The core CCS modules, including addition, deletion, modification and query of clusters and services.


### Additional Information
Tencent Cloud's CCS is built on open-source Kubernetes orchestration framework, allowing you to manage clusters using native Kubernetes API. The corresponding relationship between Tencent Cloud's CCS and Kubernetes resources is shown below:

![Alt text](https://mc.qcloudimg.com/static/img/6aa5ec231df20984395de29641477eb4/Image+056.png)

