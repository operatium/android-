# Activity
Activity的四种状态 
running / paused / stopped / killed
### running:   
表明Activity处于活动（完全可见）状态，用户可以与屏幕进行交互，此时，Activity处于栈顶。   
### paused:   
表明Activity处于失去焦点的状态（例如：被非全屏的的Activity覆盖），此时用户无法与该Activity进行交互。   
### stopped：   
表明Activity处于不可见的状态（例如：被另一个Activity完全覆盖）。   
### killed：  
表明Activity被系统回收了.  

## Activity生命周期
![image](https://github.com/operatium/androidLesson/blob/master/res/activityLift.jpg)

## Activity跳转生命周期
![image](https://github.com/operatium/androidLesson/blob/master/res/startActivity.jpg)

## scheme协议

#### Uri.parse("qh://test:8080/goods?goodsId=8897&name=fuck")
- qh代表Scheme协议名称
- test代表Scheme作用的地址域
- 8080代表改路径的端口号
- /goods代表的是指定页面(路径)
- goodsId和name代表传递的两个参数

### Scheme使用
    <!-- 给页面添加指定的过滤器-->
        <activity android:name=".SchemeActivity">
            <intent-filter>
                 <!--该页面的路径配置-->
                <data
                    android:host="test"
                    android:path="/goods"
                    android:port="8080"
                    android:scheme="qh"/>
                <!--下面这几行也必须得设置-->
                <category android:name="android.intent.category.DEFAULT"/>
                <action android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.BROWSABLE"/>
            </intent-filter>
        </activity>
        
    Uri uri = getIntent().getData();
        if (uri != null) {
            // 完整的url信息
            String s = uri.toString();
            sb.append(s + "\n");
            // scheme部分
            String scheme = uri.getScheme();
            sb.append("scheme=" + scheme + "\n");
            // host部分
            String host = uri.getHost();
            sb.append("host=" + host + "\n");
            // 访问路劲
            String path = uri.getPath();
            sb.append("path=" + path + "\n");
            //port部分
            int port = uri.getPort();
            sb.append("port=" + port + "\n");
            // Query部分
            String query = uri.getQuery();
            sb.append("query=" + query + "\n");
            //获取指定参数值
            String goodsId = uri.getQueryParameter("goodsId");
            sb.append("goodsId=" + goodsId + "\n");
            //列举所以参数名
            Set<String> queryParameterNames = uri.getQueryParameterNames();
            tv_scheme.setText(sb.toString());
        }
        
### 原生调用
    Intent intent1 = new Intent(Intent.ACTION_VIEW,
    Uri.parse("qh://test:8080/goods?goodsId=8897&name=fuck"));
    startActivity(intent1);

### html调用
    <a href="qh://test:8080/goods?goodsId=8897&name=fuck">打开商品详情</a>

### 判断某个Scheme是否有效
    Intent intent = new Intent(Intent.ACTION_VIEW,
    Uri.parse("qh://test:8080/goods?goodsId=8897&name=fuck"));
    List<ResolveInfo> activities = getPackageManager().queryIntentActivities(intent, 0);
    boolean isValid = activities.isEmpty();
    if (isValid == false) {
        startActivity(intent);
    }