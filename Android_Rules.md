书写规范
编码方式统一用UTF-8. Android Studio默认已是UTF-8，只要不去改动它就可以了。
缩进统一为4个空格，将Tab size设置为4则可以保证tab键按4个空格缩进。
空格的使用 if、else、for、switch、while等逻辑关键字与后面的语句留一个空格隔开。
运算符两边各用一个空格隔开。
int result = a + b; //Good, = 和 + 两边各用一个空格隔开int result=a+b; //Bad,=和+两边没用空格隔开 方法的每个参数之间用一个空格隔开。
public void method(String param1, String param2); // Good，param1后面的逗号与String之间隔了一个空格method(param1, param2); // Good，方法调用时，param1后面的逗号与param2之间隔了一个空格method(param1,param2); // Bad，没有用一个空格隔开
当一个表达式无法容纳在一行内时，可换行显示，另起的新行用8个空格缩进。
someMethod(longExpression1, longExpression2, longExpression3, longExpression4, longExpression5);
一行声明一个变量，不要一行声明多个变量，这样有利于写注释。
private String param1; // 参数1private String param2; // 参数2
行宽设置为100，设置格式化时自动断行到行宽位置。
使用快捷键进行代码自动格式化。
Windows：CTRL+ALT+L Windows：CTRL+SHIFT+F(eclipse) Mac：OPTION+COMMAND+L
一个方法最多不要超过40行代码。
文字大小的单位统一用sp，元素大小的单位统一用dp。
应用中的字符串统一在strings.xml中定义，然后在代码和布局文件中引用。
颜色值统一在colors.xml中定义，然后在代码和布局文件中引用。另外，不要在代码和布局文件中引用系统的颜色，除了透明。
命名规范
类和接口命名
使用大驼峰规则，用名词或名词词组命名，每个单词的首字母大写。 以下为几种常用类的命名：
activity类，命名以Activity为后缀，如：LoginActivity fragment类，命名以Fragment为后缀，如：ShareDialogFragment service类，命名以Service为后缀，如：DownloadService adapter类，命名以Adapter为后缀，如：CouponListAdapter 工具类，命名以Util为后缀，如：EncryptUtil 模型类，命名以BO为后缀，如：CouponBO 接口实现类，命名以Impl为后缀，如：ApiImpl
控件id命名 控件缩写{范围}意义，范围可选，只在有明确定义的范围内才需要加上。
变量命名 控件：使用ButterKnife自动导出规范 @Bind(R.id.tv_header_title) private TextView tvHeaderTitle; @Bind(R.id.btn_login) private Button btnLogin;
其他：见下 public class MyClass { public static final int SOME_CONSTANT = 42; public int publicField; private static MyClass sSingleton; int mPackagePrivate; private int mPrivate; protected int mProtected; }
layout命名
组件类型{范围}功能，范围可选，只在有明确定义的范围内才需要加上。 以下为几种常用的组件类型命名：
activity{范围}功能，为Activity的命名格式 fragment{范围}功能，为Fragment的命名格式 dialog{范围}功能，为Dialog的命名格式 item_list{范围}功能，为ListView的item命名格式 item_grid{范围}功能，为GridView的item命名格式 headerlist{范围}功能，为ListView的HeaderView命名格式 footerlist{范围}功能，为ListView的FooterView命名格式
strings的命名
类型{范围}功能，范围可选。 以下为几种常用的命名：
页面标题，命名格式为：title_页面 按钮文字，命名格式为：btn_按钮事件 标签文字，命名格式为：label_标签文字 选项卡文字，命名格式为：tab_选项卡文字 消息框文字，命名格式为：toast_消息 编辑框的提示文字，命名格式为：hint_提示信息 图片的描述文字，命名格式为：desc_图片文字 对话框的文字，命名格式为：dialog_文字 menu的item文字，命名格式为：action_文字
colors的命名
前缀{控件}{范围}{_后缀}，控件、范围、后缀可选，但控件和范围至少要有一个。
背景颜色，添加bg前缀 文本颜色，添加text前缀 分割线颜色，添加div前缀 区分状态时，默认状态的颜色，添加normal后缀 区分状态时，按下时的颜色，添加pressed后缀 区分状态时，选中时的颜色，添加selected后缀 区分状态时，不可用时的颜色，添加disable后缀
drawable的命名
前缀{控件}{范围}{_后缀}，控件、范围、后缀可选，但控件和范围至少要有一个。
图标类，添加ic前缀 背景类，添加bg前缀 分隔类，添加div前缀 默认类，添加def前缀 区分状态时，默认状态，添加normal后缀 区分状态时，按下时的状态，添加pressed后缀 区分状态时，选中时的状态，添加selected后缀 区分状态时，不可用时的状态，添加disable后缀 多种状态的，添加selector后缀（一般为ListView的selector或按钮的selector）
动画文件命名
动画类型_动画方向。
fade_in，淡入 fade_out，淡出 push_down_in，从下方推入 push_down_out，从下方推出 slide_in_from_top，从头部滑动进入 zoom_enter，变形进入 shrink_to_middle，中间缩小
