**����ǰ�ر�**��

ʹ�ÿͻ���Jedis�����غͲο���ַ��https://github.com/xetorthio/jedis/wiki/Getting-started

**ʾ������**��

```
import redis.clients.jedis.Jedis;

public class HelloRedis {

  public static void main(String[] args) {
        try {
            /**���²����ֱ���д����redisʵ������IP���˿ںţ�ʵ��id������*/
            String host = "192.168.0.195";
            int port = 6379;
            String instanceid = "84ffd722-b506-4934-9025-645bb2a0997b";
            String password = "1234567q";
            //����redis
            Jedis jedis = new Jedis(host, port);
            //��Ȩ
            jedis.auth(instanceid + ":" + password);

            /**�������������Ŀ�ʼ����redisʵ�������Բο���https://github.com/xetorthio/jedis */
            //����key
            jedis.set("redis", "tencent");
            System.out.println("set key redis suc, value is: tencent");
            //��ȡkey
            String value = jedis.get("redis");
            System.out.println("get key redis is: " + value);

            //�ر��˳�
            jedis.quit();
            jedis.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**���н��**��
![](//qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/JAVA-1.jpg)