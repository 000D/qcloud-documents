﻿在Devops流程中，往往包含多个环境，例如包含：开发(Dev)，测试(Test/beta)，预发布（pre-product）， 生产环境(product)。在不同环境中都需要对应用进行部署。

WordPress是一个内容管理平台，是世界上建立博客和网站最流行的网络发布平台之一，可以让用户轻松地发布，管理和组织各种内容到目标的网站上。在本示例中我们将通过wordpress部署的例子，介绍如何通过`Namespace`命名空间对不同环境进行隔离，通过应用模板+配置实现不同环境下应用的部署。

## 步骤一： 创建wordpress应用模板

在`Wordpress`中包含一个前端的wordpress服务和一个后端的mariadb服务(mariadb是一个和mysql类似的数据库服务，可以参考[mariadb介绍][1])。wordpress应用模板创建过程包括以下几个步骤：

 1. 新建wordpress应用模板
 2. 导入mariadb服务
 3. 导入wordpress服务
 4. 参数转换为配置项

### 新建wordpress应用模板

在[应用模板列表][2]页面，点击`新建`按钮，新建一个应用模板。

![应用管理wordpress-01.png-38.2kB][3]

在应用模板中，填写应用模板的名称`wordpress`。

![应用管理wordpress-02.png-29.3kB][4]

### 导入mariadb服务

**导入方法1： 通过控制台之间导入**

点击`导入服务`按钮，在控制台填写服务对应参数

![应用管理wordpress-03.png-70.1kB][5]

设置服务的基本信息：
1. 填写服务名称`mariadb`
2. 填写服务描述`mariadb服务`

设置服务的数据卷信息：
3. 数据卷选择使用云硬盘，云盘名称填写`vol`

设置镜像参数：
4. 在设置容器运行参数中的镜像参数：
容器名称设置为`mariadb`
镜像名称设置为`mariadb`
版本号选择为`latest`

![应用管理wordpress-04.png-71kB][6]

设置容器其他运行参数：
5. 设置容器环境变量：
MYSQL_ROOT_PASSWORD： root

设置数据卷的挂载点：
6. vol数据卷挂载点设置为：/var/lib/mysql 
(更多关于数据挂载的说明，可以参考[数据卷概述][7])

![应用管理wordpress-05.png-49.1kB][8]

设置服务的实例数：
7. 服务的实例数设置为1

设置服务的访问方式：
8. 服务的访问方式设置为集群内访问
9. 服务的访问端口：容器端口和服务端口都设置成3306
(更多关于服务访问方式的说明，可以参考[服务访问方式设置][9]）

点击`确认`按钮，自动生成YAML形式的模板内容。


**导入方法2： 通过YAML文件导入**

在应用模板页面，点击`+`按钮，新增一个服务。服务名称设置为`mariadb`

![应用管理wordpress-06.png-39.1kB][10]

在模板内容编辑区域，将下面的YAML文本内容直接导入：

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
    description: mariadb服务
  creationTimestamp: null
  name: mariadb
  namespace: '{{.NAMESPACE}}'
spec:
  replicas: 1
  revisionHistoryLimit: 5
  selector: {}
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
    spec:
      containers:
      - env:
        - name: MYSQL_ROOT_PASSWORD
          value: {{.ROOT_PASSWORD_VALUE}}
        image: mariadb:latest
        imagePullPolicy: Always
        name: mariadb
        resources:
          requests:
            cpu: 200m
        securityContext:
          privileged: false
        volumeMounts:
        - mountPath: /var/lib/mysql
          name: vol
      serviceAccountName: ""
      volumes:
      - name: vol
        qcloudCbs:
          cbsDiskId: '{{.ReleaseCBS_mariadb_vol}}'
          fsType: ext4
status: {}
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  name: mariadb
  namespace: '{{.NAMESPACE}}'
spec:
  ports:
  - name: tcp-3306-3306-nh6kj
    nodePort: 0
    port: 3306
    protocol: TCP
    targetPort: 3306
  selector: {}
  type: ClusterIP
status:
  loadBalancer: {}
```
点击`从模板内容导入`提取模板中的变量作为配置项。这里自动提取了`NAMESPACE`和`ReleaseCBS_mariadb_vol`作为配置项。并填写`NAMESPACE`配置项的值为`default`。`NAMESPACE`用来表示服务部署到集群的哪个命名空间，更多关于命名空间的说明可以参数[Namespace使用指引][11]。`ReleaseCBS_XXXX`为容器服务为使用Cbs云盘定义的变量，更多关于ReleaseCBS自定义变量的说明可以参考[自定义变量--ReleaseCBS][12]

![应用管理wordpress-07.png-50.7kB][13]

### 导入wordpress服务

**导入方法1： 通过控制台之间导入**

点击`导入服务`按钮，在控制台填写服务对应参数

![应用管理wordpress-08.png-101.7kB][14]

设置服务的基本信息：
1. 填写服务名称`wordpress`
2. 填写服务描述`wordpress服务`

设置服务的数据卷信息：
3. 数据卷选择使用云硬盘，云盘名称填写`wordpress-persistent-storage`

设置镜像参数：
4. 在设置容器运行参数中的镜像参数：
容器名称设置为`wordpress`
镜像名称设置为`wordpress`
版本号选择为`latest`

设置容器资源限制：
5. 设置容器的CPU分配资源为0.1核，限制最大的使用资源为0.2核

![应用管理wordpress-09.png-62.1kB][15]

设置容器其他运行参数：
6. 设置容器环境变量：
WORDPRESS_DB_HOST： mariadb
WORDPRESS_DB_PASSWORD： root

设置数据卷的挂载点：
7. vol数据卷挂载点设置为：/var/www/html 
(更多关于数据挂载的说明，可以参考[数据卷概述][16])

![应用管理wordpress-10.png-110kB][17]

设置服务的实例数：
8. 服务的实例数设置为1

设置服务的访问方式：
9. 服务的访问方式设置为外网访问
10. 服务的访问端口：容器端口和服务端口都设置成3306
(更多关于服务访问方式的说明，可以参考[服务访问方式设置][18]）

点击`确认`按钮，自动生成YAML形式的模板内容。

**导入方法2： 通过YAML文件导入**

在应用模板页面，点击`+`按钮，新增一个服务。服务名称设置为`wordpress`。

![应用管理wordpress-11.png-54.1kB][19]

在模板内容编辑区域，将下面的YAML文本内容直接导入：

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
    description: wordpress服务
  creationTimestamp: null
  name: wordpress
  namespace: '{{.NAMESPACE}}'
spec:
  replicas: 1
  revisionHistoryLimit: 5
  selector: {}
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
    spec:
      containers:
      - env:
        - name: WORDPRESS_DB_HOST
          value: mariadb
        - name: WORDPRESS_DB_PASSWORD
          value: root
        image: wordpress:latest
        imagePullPolicy: Always
        name: wordpress
        resources:
          limits:
            cpu: 200m
          requests:
            cpu: 100m
        securityContext:
          privileged: false
        volumeMounts:
        - mountPath: /var/www/html
          name: wordpress-persistent-storage
      serviceAccountName: ""
      volumes:
      - name: wordpress-persistent-storage
        qcloudCbs:
          cbsDiskId: '{{.ReleaseCBS_wordpress_wordpress_persistent_storage}}'
          fsType: ext4
status: {}
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  name: wordpress
  namespace: '{{.NAMESPACE}}'
spec:
  ports:
  - name: tcp-80-80-snfig
    nodePort: 0
    port: 80
    protocol: TCP
    targetPort: 80
  selector: {}
  type: LoadBalancer
status:
  loadBalancer: {}
```
点击`从模板内容导入`提取模板中的变量作为配置项。这里自动提取ReleaseCBS_wordpress_wordpress_persistent_storage作为配置项。`ReleaseCBS_XXXX`为容器服务为使用Cbs云盘定义的变量，更多关于ReleaseCBS自定义变量的说明可以参考[自定义变量--ReleaseCBS][20]

![应用管理wordpress-12.png-16.6kB][21]

### 参数转换为配置项

在不同环境中部署，可能会存在不同环境下参数一致的情况。这里在模板内容区域将CPU使用资源设置为变量，在不同环境下设置成不同的值。

![应用管理wordpress-13.png-56.5kB][22]

在这个示例中我们将CPU分配资源量和最大限制使用资源量设置为变量。分布用变量`CPU_LIMITS`和`CPU_REQUEST`表示。变量的形式符合`{{.}}`这样的形式。更多关于模板中变量的使用可以参考[变量设置][23]。

点击`从模板内容导入`提取模板中的变量作为配置项。这里自动提取变量`CPU_LIMITS`和`CPU_REQUEST`，设置`CPU_LIMITS`的默认值为200m,`CPU_REQUEST`为100m。(1m=0.001核)。

点击`完成`后，保存模板信息。在[模板列表页][24]可以看到新创建的模板。

![应用管理wordpress-14.png-27.4kB][25]

## 步骤二： 创建不同环境的命名空间

命名空间 ( Namespace ) 是对一组资源和对象的抽象集合，可以将不同环境的服务实例部署在不同命名空间中。

如果我们已经有了一个集群，可以直接在集群上开始创建命名空间。如果我们还没有创建好的集群，可以参考[集群基本操作][26]，创建一个集群。

**创建集群 Namespace**

1. 在集群列表页中选择某集群的 ID/名称。
2. 单击 Namespace 列表 ，单击【新建 Namespace 】。

更多关于命名空间的操作，可以参考[Namespace使用指引][27]。

![应用管理wordpress-23.png-31.1kB][28]

在本示例中，我们依次创建devnamespace,testnamespace,prenamespace,prodnamespace。

![应用管理wordpress-24.png-33kB][29]

## 步骤三： 创建不同环境的CBS盘
由于[云硬盘][30]页面，选择对应的区域点击新建按钮，创建新的云硬盘。

![应用管理wordpress-16.png-35.2kB][31]

填写对应的参数：

![应用管理wordpress-17.png-93.9kB][32]

设置的主要参数包括：
1. 磁盘名称，例如： wordpress-dev
2. 选择所属项目和所在地域。
3. 选择计费类型和磁盘类型，这里选择包年包月和普通云硬盘
4. 选择容量大小，对于不同环境使用的磁盘，选择不同大小。在dev，test，pre-product使用10GB，product环境使用50GB。
5. 选择磁盘数量，由于应用中两个服务各使用了一块磁盘，所以每个环境需要购买两块磁盘。
6. 点击`完成`按钮，确认购买

购买完成后，等待2~3分钟，可以看cbs盘的页面查看到对应的磁盘。

![应用管理wordpress-18.png-76.3kB][33]

## 步骤四： 创建不同环境的配置项

在不同环境中，可以将不同环境的差异化信息通过配置项保存。在[配置项][34]页面，点击新建按钮，可以创建对应的配置文件。

![应用管理wordpress-15.png-35.2kB][35]

**dev环境配置项：**

![应用管理wordpress-19.png-31.5kB][36]

```
NAMESPACE: devnamespace
ReleaseCBS_mariadb_vol: disk-peivemz1
ReleaseCBS_wordpress_wordpress_persistent_storage: disk-aq0280h5
CPU_LIMITS: 200m
CPU_REQUEST: 100m
```

**test环境配置项：**

![应用管理wordpress-22.png-31.8kB][37]

```
NAMESPACE: testnamespace
ReleaseCBS_mariadb_vol: disk-fifiv4ht
ReleaseCBS_wordpress_wordpress_persistent_storage: disk-1ywc14gb
CPU_LIMITS: 200m
CPU_REQUEST: 100m
```

**pre-product环境配置项：**

![应用管理wordpress-20.png-31.8kB][38]

```
NAMESPACE: prenamespace
ReleaseCBS_mariadb_vol: disk-fyzh7vgp
ReleaseCBS_wordpress_wordpress_persistent_storage: disk-pxtmsa7b
CPU_LIMITS: 200m
CPU_REQUEST: 100m
```
**product环境配置项：**

![应用管理wordpress-22.png-31.8kB][39]

```
NAMESPACE: prodnamespace
ReleaseCBS_mariadb_vol: disk-r68yb55t
ReleaseCBS_wordpress_wordpress_persistent_storage: disk-frs991e5
CPU_LIMITS: 800m
CPU_REQUEST: 400m
```

在product环境中，设置更大的可使用资源。将CPU_LIMITS和CPU_REQUEST分别设置成800m(0.8核)和400m(0.4核)。

## 步骤五： 创建不同环境的应用

### 新建应用
在[应用列表][40]选择创建了命名空间的集群。点击`新建`按钮。

![应用管理wordpress-25.png-13.9kB][41]

### 选择应用对应的模板和配置
在新建应用页面，选择对应的应用模板和配置项。

![应用管理wordpress-26.png-41.7kB][42]

应用名称： 设置为wordpress-dev
应用描述:  开发环境应用
地域选择:  选择`步骤二`中的集群所在的地域
集群选择： 选择`步骤二`中的集群
模板选择:  选择`步骤一`中创建的应用模板
配置项选择: 选择`步骤四`中创建的开发环境的配置项

点击`下一步`，对应用中的模板内容进行再次编辑。由于我们已经在应用模板和配置项中完成了对应的设置，所以这里直接点击`完成`按钮，完成应用内容的编辑。

![应用管理wordpress-27.png-39.5kB][43]

### 查看应用状态

在应用列表页面，可以查看到新创建的应用，只是此时应用还处于未部署状态。

![应用管理wordpress-28.png-17.7kB][44]

### 应用详情页面部署应用中的服务

点击应用的名称，可以查看应用的详情。在应用的详情页面，可以对应用进行部署操作。

![应用管理wordpress-29.png-30.5kB][45]

点击`部署`按钮完成应用中服务的部署。

### 查看服务的信息

在完成部署后，应用中服务的状态变为`已部署`，服务的运行状态变为`运行中`。

![应用管理wordpress-30.png-37.4kB][46]

点击服务的名称，可以跳转到服务详细页面，可以查看更多服务的信息。

![应用管理wordpress-31.png-48.7kB][47]

### 访问服务测试

![应用管理wordpress-32.png-62kB][48]

这样就完成了dev(开发)环境的wordpress应用的部署。

### 应用部署到不同环境

执行同样的步骤，只是在应用配置选择时，选择不同环境下对应的配置。可以将应用部署到不同的环境。部署完成后，可以在应用类别查看应用的信息。

![应用管理wordpress-33.png-28.9kB][49]

这样基于同一个应用模板和不同环境下的配置信息，就可将应用部署到不同的环境。

  [1]: https://baike.baidu.com/item/mariaDB/6466119?fr=aladdin
  [2]: https://console.cloud.tencent.com/ccs/template
  [3]: https://mc.qcloudimg.com/static/img/fec0b45e9d0115ad394bfc9723a57d7e/image.png
  [4]: https://mc.qcloudimg.com/static/img/9b87ed2eb0244880c292c914a69e4942/image.png
  [5]: https://mc.qcloudimg.com/static/img/64a77ad8bb358d1893994089f5099fc6/image.png
  [6]: https://mc.qcloudimg.com/static/img/dea0f7bc36614c816f887b2bfa6dd751/image.png
  [7]: https://cloud.tencent.com/document/product/457/9112
  [8]: https://mc.qcloudimg.com/static/img/08da68619a4d81d717e5bf03016f9f53/image.png
  [9]: https://cloud.tencent.com/document/product/457/9098
  [10]: https://mc.qcloudimg.com/static/img/63de85568b06169699dd49015c0d5963/image.png
  [11]: https://cloud.tencent.com/document/product/457/10177
  [12]: https://cloud.tencent.com/document/product/457/11956#.E8.87.AA.E5.AE.9A.E4.B9.89.E5.8F.98.E9.87.8F--releasecbs
  [13]: https://mc.qcloudimg.com/static/img/9a1ad6f176e8b46896f890704c38d641/image.png
  [14]: https://mc.qcloudimg.com/static/img/eeb7dbe0bca6552d5457461c2965b06d/image.png
  [15]: https://mc.qcloudimg.com/static/img/bd3caf16213e08c34b5fd93ae45f9434/image.png
  [16]: https://cloud.tencent.com/document/product/457/9112
  [17]: https://mc.qcloudimg.com/static/img/b561424ae42e4f7c97b2ee39af67af13/image.png
  [18]: https://cloud.tencent.com/document/product/457/9098
  [19]: https://mc.qcloudimg.com/static/img/16172ae9d16ea7e3e4254c911fab5363/image.png
  [20]: https://cloud.tencent.com/document/product/457/11956#.E8.87.AA.E5.AE.9A.E4.B9.89.E5.8F.98.E9.87.8F--releasecbs
  [21]: https://mc.qcloudimg.com/static/img/73ae4ebec9f7a14fe327745285afd4a1/image.png
  [22]: https://mc.qcloudimg.com/static/img/287bcbc8199b0c493620b80e43a92c75/image.png
  [23]: https://cloud.tencent.com/document/product/457/11956
  [24]: https://console.cloud.tencent.com/ccs/template
  [25]: https://mc.qcloudimg.com/static/img/356329c0bc9dd37f44534cead3a6f438/image.png
  [26]: https://cloud.tencent.com/document/product/457/9091
  [27]: https://cloud.tencent.com/document/product/457/10177
  [28]: https://mc.qcloudimg.com/static/img/9c1f92253cdf0533edc335320c8ad5ec/image.png
  [29]: https://mc.qcloudimg.com/static/img/124c953135374f32b98b7ee41c2babce/image.png
  [30]: https://console.cloud.tencent.com/cvm/cbs
  [31]: https://mc.qcloudimg.com/static/img/a474822226c01989519b851fdefcacf5/image.png
  [32]: https://mc.qcloudimg.com/static/img/b6be1779f4361ab70c5b89b05e25e245/image.png
  [33]: https://mc.qcloudimg.com/static/img/f02938505cd23263f99e269cc0c8f756/image.png
  [34]: https://console.cloud.tencent.com/ccs/config
  [35]: https://mc.qcloudimg.com/static/img/674255f2011d8c4117ada5bd7f6c6359/image.png
  [36]: https://mc.qcloudimg.com/static/img/0078f2c3547177b55af85cfcd4407592/image.png
  [37]: https://mc.qcloudimg.com/static/img/edde901ee9415a431aa4aa7592053fde/image.png
  [38]: https://mc.qcloudimg.com/static/img/c47b6166f9ac694d7007ba0022aae9d1/image.png
  [39]: https://mc.qcloudimg.com/static/img/bffc9672fffd153b3dad8a27d52c5b24/image.png
  [40]: https://console.cloud.tencent.com/ccs/application
  [41]: https://mc.qcloudimg.com/static/img/58c321ec3ce6c9aad5fc56d4f2ba7cc4/image.png
  [42]: https://mc.qcloudimg.com/static/img/1f607149a780cab88223c70cc97fd3d1/image.png
  [43]: https://mc.qcloudimg.com/static/img/55c56855603f94e761d090ac054e99a7/image.png
  [44]: https://mc.qcloudimg.com/static/img/0bb385567036bbd8292a2483e873dfd9/image.png
  [45]: https://mc.qcloudimg.com/static/img/494789266f4a4bf401c9ef245b0d7760/image.png
  [46]: https://mc.qcloudimg.com/static/img/9db4a548647d0460f5208d54eba555e4/image.png
  [47]: https://mc.qcloudimg.com/static/img/0733772a5772f4672b07960d3e46be5b/image.png
  [48]: https://mc.qcloudimg.com/static/img/ec558f6e7736caa987d11b4fb0164de9/image.png
  [49]: https://mc.qcloudimg.com/static/img/f19d6d08555805408fc3f4f2abd570cb/image.png