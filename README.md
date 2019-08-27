### 接入说明
用到我公司热敏打印、扫码、客显屏、二代身份证、PSAM等功能需要用到SDK，其他Android标准功能开发请使用Android通用SDK
我公司功能接入SDK，需要安装SmartDeviceSDK.apk，版本不同后缀略有差别，一般机器出厂都会默认安装，该SDK3506,5501，PC900机型通用
SDK的调用需要用到AIDL方式接入，接入方案请参考下一页文文档说明

###### 扫码功能，建议免开发（按Scan键获取扫码结果到输入框）方式，如果项目特殊需求建议使用自定义广播方式开发



-----------------

#### **各机型SDK下载地址**
> https://share.weiyun.com/55lOLjW

#### **二次开发包以及SDK调用DEMO源码下载**
> http://www.sznewbest.com/down_data_detail_281.html



------------




![](http://doc.szzkc.com/Public/Uploads/2018-09-17/5b9f58d75cdec.png)


> 请严格按照步骤操作，并保证目录与文档一致，不要修改包名以及文件名

> 在当前工程下，选择Android预览模式，选择需要引入AIDL的应用模块或库，
右键依次选择New——》Folder——》AIDL Folder；

![](http://doc.szzkc.com/Public/Uploads/2018-02-25/5a926297422c5.png)


> 经过上述操作之后，会出现下图所示的窗口；

![](http://doc.szzkc.com/Public/Uploads/2018-02-25/5a9262f2072e5.png)

> 确保Target Source Set 选中main目录，点击Finish。这时在应用模块下会出现一个与manifests、java和res同级的文件夹aidl，接下来在该文件夹下新建与外部AIDL文件所在包包名相同的包（该远程服务的包名为 com.smartdevice.aidl）。

![](http://doc.szzkc.com/Public/Uploads/2018-02-25/5a9264b256b95.png)

> 将提供的AIDL文件复制粘贴在该包（ com.smartdevice.aidl）下。


> 绑定服务前，需确保设备已经安装好正确的服务程序，可用提供的测试程序先行测试，在测试的过程中发现任何问题请及时联系服务程序提供者。

#### 定义服务连接监听对象ServiceConnection


//服务接口对象,服务连接成功后会返回该对象。
//Service interface object, the object will be return after the connection is successful


------------

		public static IZKCService mIzkcService;
		private ServiceConnection mServiceConn = new ServiceConnection() {
		@Override
		public void onServiceDisconnected(ComponentName name) {
			Log.e("client", "onServiceDisconnected");
			mIzkcService = null;
		}

		@Override
		public void onServiceConnected(ComponentName name, IBinder service) {
			Log.e("client", "onServiceConnected");
			/**
			服务绑定成功，获取到该服务接口对象，可通过该接口调用相关的接口方法来完成相应的功能
			*success to get the sevice interface object
			*/
			mIzkcService = IZKCService.Stub.asInterface(service);
			}
		}
	};

------------

#### 绑定服务
//com.zkc.aidl.all 为远程服务的名称，**不可更改**
//com.smartdevice.aidl为远程服务声明所在的包名，**不可更改**，
//对应的项目所导入的AIDL文件也应该在该包名下

------------

		Intent intent = new Intent("com.zkc.aidl.all");
		intent.setPackage("com.smartdevice.aidl");
		bindService(intent, mServiceConn, Context.BIND_AUTO_CREATE);
------------
#### 解绑服务
------------
//退出程序之前，需要解除已绑定的服务。
	unbindService(mServiceConn);
