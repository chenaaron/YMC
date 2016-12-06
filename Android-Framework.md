framework接入说明：
    maven 地址：
    maven { url "http://100.160.136.14:8081/nexus/content/repositories/snapshots" }
    maven { url "http://100.160.136.14:8081/nexus/content/repositories/releases" }

    a、接入单独的framework
    在你的app gralde加入： compile 'com.XXX.android:yuntu_framework:${version}'

    b、接入带im模块的framework
    在你的app gradle加入： compile 'com.XXX.android:rongim:${version}'
    因为rongim模块里已经加入framewrok基础功能，不用另外接入framework.

    Application需继承BaseApplication，并在OnCreate中加入 im初始化代码：
    ImClient.initIm(Context context);


1、更新接口对接说明
	framework对接1.0.5版本及以后的版本。

	UpdateUtils.init(Context context);前提是manifest配置了CUR_APP_NAME，UMENG_CHANNEL。
	否则调用
	/**
     *
     * @param context
     * @param appKey  APP名称简写,如csjl, cm,可在manifest配置CUR_APP_NAME
     * @param channel UMENG APP CHANNEL
     * @param version
     * @param buildNo
     * @param showTip 是否显示无更新的提示
     */
    public static void init(final Context context, String appKey, String channel, String version, String buildNo,
                            boolean showTip);

	如果需要在gqc测试阶段，并且不更改版本号的情况下，更新可修改build号来进行更新。



2、im对接说明
	framework对接1.0.5版本及以后的版本。
	对话界面及对话列表界面使用fragment（ImFragment）实现。

	a、AndroidManifest配置app key.
		<meta-data
		    android:name="RONG_CLOUD_APP_KEY"
		    android:value="z3v5yqkbv8v30" /> <！--您自己的App Key-- >

	b、app的Application类继承framework的BaseApplication; BaseApplication里对im做了初始化。

	c、app登录成功后设置 token（各app服务自己的token）
		APPEnvironment.setUserToken(userAuth.getToken());

	c、AndroidManifest配置你的对话Activity。建议将这个activity的lunchmode设置为singleTask。

	<!--聚合会话列表-->
	 <activity
	     android:name="com.xxx.activity.ConversationListActivtiy"
	     android:screenOrientation="portrait"
	     android:windowSoftInputMode="stateHidden|adjustResize">

	     <intent-filter>
	         <action android:name="android.intent.action.VIEW" />

	         <category android:name="android.intent.category.DEFAULT" />

	         <data
	             android:host="${package name}"		// 你的app包名
	             android:pathPrefix="/conversation/"
	             android:scheme="rong" />
	     </intent-filter>
	 </activity>

	 d、在你的对话activity或者fragment中，加入ImFragment

	 	ImFragment fragment = new ImFragment();
        Bundle bundle = new Bundle();
        bundle.putString(ImFragment.TARGET_USER_ID_KEY, targetId);		// 接受方的userId（比如客服的userId）
        bundle.putString(ImFragment.USER_ID_KEY, "562557");				// 用户的userId，
        fragment.setArguments(bundle);

        FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();
        transaction.add(R.id.fragment, fragment);
        transaction.commitAllowingStateLoss();

    e、拷贝framework下的Rong_Cloud
    	libagora-rtc-sdk-jni.so
		libHDACEngine.so
		libRongCallLib.so
		libRongIMLib.so到app工程的jniLib（或lib,具体看app的配置）目录。

	f、要实现im消息push，需要在manifest配置；并且设置用户信息（见g）
		<receiver
            android:name="com.xxx.android.framework.im.NotificationReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="io.rong.push.intent.MESSAGE_ARRIVED" />
                <action android:name="io.rong.push.intent.MI_MESSAGE_ARRIVED" />
                <action android:name="io.rong.push.intent.MESSAGE_CLICKED" />
                <action android:name="io.rong.push.intent.MI_MESSAGE_CLICKED" />
            </intent-filter>
        </receiver>

        <!--必选： SDK 核心功能-->
        <!--第三方相关,向第三方推送服务请求 token 的服务 -->
        <service
            android:name="io.rong.push.core.PushRegistrationService"
            android:exported="false">
        </service>


        <!-- 处理 push 消息相关的服务 -->
        <service
            android:name="io.rong.push.core.MessageHandleService"
            android:exported="true">
        </service>


        <!-- push服务 -->
        <service
            android:name="io.rong.push.PushService"
            android:exported="false"
            android:process="io.rong.push">  <!-- push进程，可以改名 -->
        </service>


        <!-- push 相关事件接收器 -->
        <receiver
            android:name="io.rong.push.PushReceiver"
            android:process="io.rong.push">
            <!-- 此处进程可以改名，名称需要和PushService所在进程统一 -->

            <!-- 心跳事件 -->
            <intent-filter>
                <action android:name="io.rong.push.intent.action.HEART_BEAT" />
            </intent-filter>
            <!-- 网络变动事件 -->
            <intent-filter>
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
            </intent-filter>
            <!-- 部分用户事件 -->
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.USER_PRESENT" />
                <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
                <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
            </intent-filter>
        </receiver>

        <!--必选： SDK 核心功能-->

    g、设置用户im信息
    	/**
	     * 设置IM用户信息
	     * @param id
	     * @param userName
	     * @param portraitUrl 头像URL
	     */
	    public static void ImClient.setImUserInfo(String id, String userName, String portraitUrl)


	有任何问题可和我沟通(erik7@126.com), 或参考http://www.rongcloud.cn/docs/android.html

******************************************************************
    push 对接
******************************************************************
    1、配置manifest
        a、permission配置
        <!--mi push permission-->
        ${PACEAGE_NAME} // 你的app包名
        <permission android:name="PACEAGE_NAME.permission.MIPUSH_RECEIVE" android:protectionLevel="signature" />
        <uses-permission android:name="PACEAGE_NAME.permission.MIPUSH_RECEIVE" />
        <!--mi push permission end-->

        <!-- jpush permisson -->
        <permission
            android:name="PACEAGE_NAME.permission.JPUSH_MESSAGE"
            android:protectionLevel="signature" />

        <uses-permission android:name="PACEAGE_NAME.permission.JPUSH_MESSAGE" />
        <uses-permission android:name="android.permission.RECEIVE_USER_PRESENT" />
        <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
        <!-- jpush permisson end -->

        系统permission要求：
        <uses-permission android:name="android.permission.RECEIVE_USER_PRESENT" />
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" /> 
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.GET_TASKS" />

    2、service及receiver配置
         <!--hw push-->
        <!-- PushSDK:PushSDK接收外部请求事件入口 -->
        <receiver
            android:name="com.huawei.android.pushagent.PushEventReceiver"
            android:process=":pushservice" >
            <intent-filter>
                <action android:name="com.huawei.android.push.intent.REFRESH_PUSH_CHANNEL" />
                <action android:name="com.huawei.intent.action.PUSH" />
                <action android:name="com.huawei.intent.action.PUSH_ON" />
                <action android:name="com.huawei.android.push.PLUGIN" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_ADDED" />
                <action android:name="android.intent.action.PACKAGE_REMOVED" />

                <data android:scheme="package" />
            </intent-filter>
        </receiver>
        <receiver
            android:name="com.huawei.android.pushagent.PushBootReceiver"
            android:process=":pushservice" >
            <intent-filter>
                <action android:name="com.huawei.android.push.intent.REGISTER" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
            </intent-filter>
            <meta-data
                android:name="CS_cloud_version"
                android:value="\u0032\u0037\u0030\u0035" />
        </receiver>

        <!-- PushSDK:Push服务 -->
        <service
            android:name="com.huawei.android.pushagent.PushService"
            android:process=":pushservice" >
        </service>

        <!-- 第三方相关 :接收Push消息（注册、Push消息、Push连接状态、标签，LBS上报结果）广播 -->
        <receiver android:name="com.xxx.android.framework.utils.receiver.HwPushReceiver" >
            <intent-filter>
                <!-- 必须,用于接收token-->
                <action android:name="com.huawei.android.push.intent.REGISTRATION" />
                <!-- 必须，用于接收消息-->
                <action android:name="com.huawei.android.push.intent.RECEIVE" />
                <!-- 可选，用于点击通知栏或通知栏上的按钮后触发onEvent回调-->
                <action android:name="com.huawei.android.push.intent.CLICK" />
                <!-- 可选，查看push通道是否连接，不查看则不需要-->
                <action android:name="com.huawei.intent.action.PUSH_STATE" />
                <!-- 可选，标签、地理位置上报回应，不上报则不需要 -->
                <action android:name="com.huawei.android.push.plugin.RESPONSE" />
            </intent-filter>
            <meta-data android:name="CS_cloud_ablitity" android:value="@string/hwpush_ability_value"/>
        </receiver>
        <!--hw push end-->

        <!--mi push-->
        <service
            android:enabled="true"
            android:process=":pushservice"
            android:name="com.xiaomi.push.service.XMPushService"/>
        <service
            android:name="com.xiaomi.push.service.XMJobService"
            android:enabled="true"
            android:exported="false"
            android:permission="android.permission.BIND_JOB_SERVICE"
            android:process=":pushservice" />
        <!--注：此service必须在3.0.1版本以后（包括3.0.1版本）加入-->
        <service
            android:enabled="true"
            android:exported="true"
            android:name="com.xiaomi.mipush.sdk.PushMessageHandler" />
        <service android:enabled="true"
            android:name="com.xiaomi.mipush.sdk.MessageHandleService" />
        <!--注：此service必须在2.2.5版本以后（包括2.2.5版本）加入-->
        <receiver
            android:exported="true"
            android:name="com.xiaomi.push.service.receivers.NetworkStatusReceiver" >
            <intent-filter>
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </receiver>
        <receiver
            android:exported="false"
            android:process=":pushservice"
            android:name="com.xiaomi.push.service.receivers.PingReceiver" >
            <intent-filter>
                <action android:name="com.xiaomi.push.PING_TIMER" />
            </intent-filter>
        </receiver>

        <receiver
            android:exported="true"
            android:name="com.yuntu.android.framework.utils.receiver.MiMessageReceiver">
            <intent-filter>
                <action android:name="com.xiaomi.mipush.RECEIVE_MESSAGE" />
            </intent-filter>
            <intent-filter>
                <action android:name="com.xiaomi.mipush.MESSAGE_ARRIVED" />
            </intent-filter>
            <intent-filter>
                <action android:name="com.xiaomi.mipush.ERROR" />
            </intent-filter>
        </receiver>
        <!--mi push end-->

        <!-- jpush -->
        <!-- Required SDK 核心功能-->
        <!-- option since 2.0.5 可配置PushService，DaemonService,PushReceiver,AlarmReceiver的android:process参数 将JPush相关组件设置为一个独立进程 -->
        <!-- 如：android:process=":remote" -->
        <service
            android:name="cn.jpush.android.service.PushService"
            android:enabled="true"
            android:exported="false" >
            <intent-filter>
                <action android:name="cn.jpush.android.intent.REGISTER" />
                <action android:name="cn.jpush.android.intent.REPORT" />
                <action android:name="cn.jpush.android.intent.PushService" />
                <action android:name="cn.jpush.android.intent.PUSH_TIME" />
            </intent-filter>
        </service>

        <!-- since 1.8.0 option 可选项。用于同一设备中不同应用的JPush服务相互拉起的功能。 -->
        <!-- 若不启用该功能可删除该组件，将不拉起其他应用也不能被其他应用拉起 -->
        <service
            android:name="cn.jpush.android.service.DaemonService"
            android:enabled="true"
            android:exported="true">
            <intent-filter >
                <action android:name="cn.jpush.android.intent.DaemonService" />
                <category android:name="com.xxx.android.example"/>
            </intent-filter>
        </service>

        <!-- Required -->
        <receiver
            android:name="cn.jpush.android.service.PushReceiver"
            android:enabled="true" >
            <intent-filter android:priority="1000">
                <action android:name="cn.jpush.android.intent.NOTIFICATION_RECEIVED_PROXY" />
                <category android:name="com.xxx.android.example"/>
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.USER_PRESENT" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
            </intent-filter>
            <!-- Optional -->
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_ADDED" />
                <action android:name="android.intent.action.PACKAGE_REMOVED" />
                <data android:scheme="package" />
            </intent-filter>
        </receiver>
        <!-- Required SDK核心功能-->
        <activity
            android:name="cn.jpush.android.ui.PushActivity"
            android:configChanges="orientation|keyboardHidden"
            android:exported="false" >
            <intent-filter>
                <action android:name="cn.jpush.android.ui.PushActivity" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="com.xxx.android.example" />
            </intent-filter>
        </activity>
        <!-- Required SDK核心功能-->
        <service
            android:name="cn.jpush.android.service.DownloadService"
            android:enabled="true"
            android:exported="false" >
        </service>
        <!-- Required SDK核心功能-->
        <receiver android:name="cn.jpush.android.service.AlarmReceiver" />

        <!-- User defined. 用户自定义的广播接收器-->
        <receiver
            android:name="com.xxx.android.framework.utils.receiver.JPushReceiver"
            android:enabled="true">
            <intent-filter>
                <!--Required 用户注册SDK的intent-->
                <action android:name="cn.jpush.android.intent.REGISTRATION" />
                <!--Required 用户接收SDK消息的intent-->
                <action android:name="cn.jpush.android.intent.MESSAGE_RECEIVED" />
                <!--Required 用户接收SDK通知栏信息的intent-->
                <action android:name="cn.jpush.android.intent.NOTIFICATION_RECEIVED" />
                <!--Required 用户打开自定义通知栏的intent-->
                <action android:name="cn.jpush.android.intent.NOTIFICATION_OPENED" />
                <!--Optional 用户接受Rich Push Javascript 回调函数的intent-->
                <action android:name="cn.jpush.android.intent.ACTION_RICHPUSH_CALLBACK" />
                <!-- 接收网络变化 连接/断开 since 1.6.3 -->
                <action android:name="cn.jpush.android.intent.CONNECTION" />
                <category android:name="com.xxx.android.example" />
            </intent-filter>
        </receiver>

        <!-- Required. For publish channel feature -->
        <!-- JPUSH_CHANNEL 是为了方便开发者统计APK分发渠道。-->
        <!-- 例如: -->
        <!-- 发到 Google Play 的APK可以设置为 google-play; -->
        <!-- 发到其他市场的 APK 可以设置为 xxx-market。 -->
        <!-- 目前这个渠道统计功能的报表还未开放。-->
        <!-- Required. AppKey copied from Portal -->
        <!-- 你的 jpush appkey -->
        <meta-data android:name="JPUSH_APPKEY" android:value=""/>
        <!-- jpush end -->

        <!-- app自定义receiver -->    
        <receiver android:name="MyReceiver" >
            <intent-filter>
                <action android:name="com.xxx.push.NOTIFICATION_OPENED" />
            </intent-filter>
        </receiver>

        在app module gralde加入jpush配置代码：
            manifestPlaceholders = [   
                JPUSH_PKGNAME : "", // app包名
                JPUSH_APPKEY : "", //JPush上注册的包名对应的appkey.
                JPUSH_CHANNEL : "developer-default", //暂时填写默认值即可.
            ]


    3、push初始化, app的application需继承framework的BaseApplication

        // 在application的initAppEnvironment函数里，
        // 小米设置 appid和appkey
        PushConfig pushConfig = new PushConfig();
        pushConfig.setMiAppId("");
        pushConfig.setMiAppKey("");
        APPEnvironment.setPushConfig(pushConfig);
        // 华为push和包名相关，不用设置appid等

    4、自定义receiver,继承YTPushReceiver
        父类默认实现打开app。
        app可自定义打开通知栏动作。
        public void onReceive(Context context, Intent intent) {
        //super.onReceive(context, intent);
        if (intent.getAction().equals(YTPushConstant.ACTION_NOTIFICATION_OPENED)) {
            context.startActivity(new Intent(context, MainActivity.class)
                    .setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP | Intent.FLAG_ACTIVITY_NEW_TASK)
                    .putExtras(intent));
        }
    }

    5、设置别名和标签,以及清除别名和标签
        import com.yuntu.android.framework.utils.PushAgent;

        PushAgent.setAlias(Context context, String alias);

        PushAgent.setTags(Context context, JSONObject jsonObject);
        PushAgent.setTags(Context context, Map<String, String> tags);

        PushAgent.clearAlias(Context context);
        PushAgent.clearTags(Context context);


＊   bugly配置
    框架接入了bugly,app在manifest配置BuglyId即可。
    <meta-data
            android:name="BUGLY_APPID"
            android:value="" />
