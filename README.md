
数据库详细设计说明书
1.1	商品类
1.1.1	商品表
逻辑表名	商品表	物理表名	product
主键	product_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
商品ID	product_id	int	not		自增
名称	name	varchar(50)			
款号	no	varchar(20)	not		
年份	year	varchar(10)		当前年份	2012
季节	season	varchar(4)		四季	以四位二进制码进行标识
性别	sex	varchar(4)		中性	男、女、中性
关键词	keywords	varchar(50)			搜索用
商品类型	product_type_id	tinyint			商品类型表主键
销售类型	sell_type_id	tinyint		无	销售类型表主键
上下架状态	is_alive	tinyint		2	
品牌	brand_id	tinyint		童壹库	品牌表主键
品类	category_id	smallint			品类表主键
商店	shop_id	tinyint		童壹库	商店表主键
仓库	warehouse_id	tinyint		北京仓	仓库表主键
供货商	supplier_id	tinyint		派克兰帝	供货商表主键
商品风格	product_style_id	tinyint			商品风格表主键
主题故事	product_story_id	tinyint			主题故事表主键
添加时间	add_time	datetime			商品第一次录入的时间
修改人	modify_admin_id	int			最后一次修改人
修改时间	modify_time	datetime			最后一次修改商品的时间
1.1.2	图片表
存储所有的图片的路径字符串（URL），如果存储量过大，则需要根据主键值做表分区
图片命名规则：款号_颜色_角度_是否默认图_宽_高.jpg
例如：LPZD115602_5rex5YWwLealvOWFsOe0qw@@_1_1_626_800.jpg
图片文件夹路径为：/images/product/款号/图片.jpg
逻辑表名	图片表	物理表名	picture
主键	picture_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
图片ID	picture_id	int			自增
商品ID	product_id	int			商品表主键
颜色ID	color_id	int			颜色表主键
图片角度ID	picture_angle_id	tinyint			图片角度表主键
图片宽高ID	picture_size_id	tinyint			图片宽高表主键
图片路径	picture_url	varchar(100)			
是否默认图	is_default	tinyint		0	0:非默认值  1：默认值
1.1.3	商品类型表
代码表
存放商品类型模板信息：童装、童鞋、配饰、玩具…
逻辑表名	商品类型表	物理表名	product_type
主键	product_type_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	product_type_id	tinyint			自增
名称	name	varchar(50)			
1.2	用户帐户类
概念：一个用户可以包含多个账户，各个账户之间是可以切换的
1.2.1	用户基础信息表
逻辑表名	用户基础信息表	物理表名	member
主键	member_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	member_id	int			自增
姓名	name	varchar(10)			
工作单位雇主	employer	varchar(30)			
手机	mobile	varchar(20)			11位
住址	address	varchar(50)			
邮箱	email	varchar(30)			
身份证	idcard	varchar(20)			18位
性别	sex	varchar(10)			男、女、中性
生日	birthday	datetime			
省份	region_province_id	int			
城市	region_city_id	int			
区县	region_country_id	int			
固定电话	telephone	varchar(20)			
邮编	post_code	varchar(10)			6位
月收入	income_month	varchar(10)			
职业	job	varchar(20)			
喜好	hobby	varchar(30)			
1.2.2	账户表
帐号来源标识出 是哪个平台过来的用户，比如QQ，新浪微博等。
可用蜜豆数：客户可以用这些蜜豆进行交易，换购。
升级蜜豆数：此蜜豆数会一直增加，不会减少，是作为蜜豆等级的升级使用。
蜜豆级数：200蜜豆为1级，4倍关系后可升级。  比如用户有2000蜜豆，2000/200=10级
10级/4=2 余数为2 那么就是2个黄蜜蜂+2个红蜜蜂

逻辑表名	账户表	物理表名	account
主键	account_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	account_id	int			自增
用户ID	member_id	int			
账户等级ID	account_level_id	tinyint			账户等级表ID
登录名	username	varchar(30)			
密码	password	varchar(40)			md5加密
可用蜜豆数	beans_usable	int			用户蜜豆消费
升级蜜豆数	beans_upgrade	int			用户等级升级
蜜豆级数	levels	int			200蜜豆为一级，4倍升级
现金账户余额	balance	double(10)		0.00	现金账户余额
信用等级	credit_level_id	tinyint			
是否内部员工	is_employee	tinyint		0	0:非内部员工  1：是
IP地址	ip	varchar(30)			
注册时间	add_time	datetime			
最后登录时间	last_time	datetime			
是否启用	is_enable	tinyint		1	0:无效  1:有效
帐号来源	source_from	varchar(10)			
1.2.3	宝贝信息表
宝贝信息与用户多对一关联，一个用户可以拥有多条宝贝信息
身高体重之类随年龄变大肯定不一样，所以根据添加时间来推算。
所以此表中没有年龄之类。
逻辑表名	宝贝信息表	物理表名	children
主键	children_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	children_id	int			自增
用户ID	member_id	int			
名字	name	varchar(10)			
身高	height	varchar(10)			
爱好	hobby	varchar(30)			
生日	birthday	datetime			
性别	sex	varchar(10)			男、女、中性
个性	personality	varchar(20)			
体重	weight	varchar(20)			
是否启用	is_enable	tinyint		1	0:不启用 1:启用
添加时间	add_time	datetime			
最后修改时间	modify_time	datetime			
1.2.4	配送地址表
下单时候这个配送地址就会用上，可以设置默认的配送地址
逻辑表名	配送地址表	物理表名	delivery_address
主键	delivery_address_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	delivery_address_id	int			自增
账户ID	account_id	int			与账户表关联
省份	region_province_id	int			
城市	region_city_id	int			
区县	region_country_id	int			
收货人	consignee	varchar(10)			
详细地址	address	varchar(50)			
手机	mobile	varchar(20)			11位
固定电话	telephone	varchar(20)			
邮箱	email	varchar(30)			
邮编	post_code	varchar(10)			
添加时间	add_time	datetime			
默认使用	is_default	tinyint		0	0:非  1:是
1.2.5	通知类型表
代码表（缺货登记，降价通知）
逻辑表名	通知类型表	物理表名	notice_type
主键	notice_type_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	notice_type_id	tinyint			自增
名称	name	varchar(20)			
1.2.6	通知类型-模板关联表
逻辑表名	通知类型-模板关联表	物理表名	notice_type_join_template
主键	notice_type_join_template_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	notice_type_join_template_id	int			自增
类型ID	notice_type_id	tinyint			通知类型ID
短信模板	sms_template_id	smallint			短信模板表ID
邮件模板	email_template_id	smallint			邮件模板表ID
1.2.7	通知表
逻辑表名	通知表	物理表名	notice
主键	notice_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	notice_id	int			自增
类型ID	notice_type_id	tinyint			
账户ID	account_id	int			
商品ID	product_id	int			
颜色ID	color_id	int			
通知内容	content	varchar(50)			50个字以内
添加时间	add_time	datetime			
回复	parent_id	int			
是否启用	is_enable	tinyint		1	0:不启用 1:启用
回复人	admin_id	smallint			回复客服的账户
1.2.8	关注商品表
与收藏夹功能类似
逻辑表名	关注商品表	物理表名	product_focus
主键	product_focus _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	product_focus_id	int			自增
账户ID	account_id	int			
商品ID	product_id	int			
颜色ID	color_id	int			
添加时间	add_time	datetime			
1.2.9	信用等级表
代码表
先划分为五个等级：良好、较好、一般、较差、差
信用等级差的用户，就是黑名单的效果，该用户将不允许登录。
逻辑表名	信用等级表	物理表名	credit_level
主键	credit_level _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	credit_level _id	tinyint			自增
名称	name	varchar(10)			
1.2.10	动作类型表
代码表
该表主要为：蜜豆记录、现金账户记录、券记录中的收入和支出服务。
比如：原因录入：下订单    其附属信息为该动作产生的结果为：单号 200898983094
逻辑表名	原因类型表	物理表名	behavior_type
主键	behavior_type_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	behavior_type_id	tintint			自增
名称	name	varchar(10)			
1.2.11	蜜豆记录表
记录蜜豆的收支情况
逻辑表名	蜜豆记录表	物理表名	bean_record
主键	bean_record _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	bean_record_id	int			自增
账户ID	account_id	int			
之前蜜豆	before_beans	int			
本次蜜豆	tx_beans	int		0	哪个订单或者获得多少蜜豆
冻结蜜豆	frozen_beans	int			如果订单属于途中，未完成则为冻结状态。
之后蜜豆	after_beans	int			
收入/支出	tx_type	tinyint			0:收入 1:支出
动作类型ID	behavior_type_id	tinyint			标识收入或支出的动作
附加信息	tx_result	varchar(30)			收入或支出动作导致的结果
添加时间	add_time	datetime			
辅助说明	aux_info	varchar(30)			描述说明
1.2.12	券记录表
记录券的收支情况
逻辑表名	券记录表	物理表名	coupon_record
主键	coupon_record _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	coupon_record _id	int			自增
账户ID	account_id	int			
券ID	coupon_code_id	int			优惠券充券表
收入/支出	tx_type	tinyint			0:收入 1:支出
原因类型ID	behavior_type_id	smallint			标识收入或支出的动作
附加信息	tx_result	varchar(30)			收入或支出动作导致的结果
添加时间	add_time	datetime			
辅助信息	aux_info	varcahr(30)			描述说明
1.2.13	现金账户记录表
记录现金账户的收支情况
逻辑表名	现金账户记录表	物理表名	cash_record
主键	cash_record _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	cash_record_id	int			自增
账户ID	account_id	int			
之前金额	before_money	double(10)		0.00	上次账户余额
之后金额	after_money	double(10)		0.00	本次账户余额
本次金额	tx_money	double(10)		0.00	充值或使用的金额
冻结金额	frozen_money	double(10)		0.00	已发货后解除冻结
收入/支出	tx_type	tinyint			0:收入 1:支出
原因类型ID	behavior_type_id	smallint			标识收入或支出的动作
附加信息	tx_result	varchar(30)			收入或支出动作导致的结果
添加时间	add_time	datetime			
辅助信息	aux_info	varchar(30)			描述说明
1.2.14	账户等级表
记录蜜豆等级的规则
蜜豆等级与QQ上的星星月亮太阳升级规则一样，四个红蜜蜂可升级成一个黄蜜蜂，四个黄蜜蜂可升级成一个蓝蜜蜂。
逻辑表名	蜜豆等级规则表	物理表名	account_level
主键	account_level _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	account_level_id	tinyint			自增
名称	name	varchar(10)			红黄蓝蜜蜂
该级别所需蜜豆	need_beans	int			200/800/3200
每级蜜豆数	beans_every_level	int			200
各级别减免优惠百分比	reduct_percent	double(10)			红:2%  黄:3%  蓝:6%

消费蜜豆比例	consume_scale	varchar(10)		10:1	10蜜豆=1元钱
每年可免运费次数	free_frequency	tinyint			红：0  黄：5  蓝：10
获得蜜豆时和价钱的比值	obtain_scale	varchar(10)		1:1	送蜜豆时1元钱=1蜜豆
不足的按向上取整算
图标ID	picture_resource_id	int			图片资源表ID
添加时间	add_time	datetime			
最后修改时间	modify_time	datetime			
添加人	add_admin_id	int			
最后修改人	modify_admin_id	int			
是否启用	is_enable	tinyint		1	0：不启用 1：启用

1.2.15	账户升级历史表
逻辑表名	账户升级历史表	物理表名	account_upgrade_history
主键	account_upgrade_history _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	account_upgrade_history_id	int			自增
账户ID	account_id	int			
变动前级数	previous_levels	int		0	账户中蜜豆级数
变动后级数	current_levels	int		0	账户中蜜豆级数
升级前蜜豆	previous_bean	int		0	
升级后蜜豆	current_bean	int		0	
添加时间	add_time	datetime			
1.3	订单及处理类
此处描述各类基础信息表（主数据），例如单位、客户、设备等。
1.3.1	购物车主表
购物车的定时删除时间和登录前后cookie是否合并做配置项
逻辑表名	购物车主表	物理表名	shopping_cart
主键	shopping_cart _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	shopping_cart_id	bigint			自增
账户ID	account_id	int			
总件数	total_quantity	int		0	
添加时间	add_time	datetime			
1.3.2	购物车子表
逻辑表名	购物车子表	物理表名	cart_info
主键	cart_info_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	cart_info_id	bigint			自增
主表ID	shopping_cart_id	bigint			
商品ID	product_id	int			
颜色ID	color_id	int			
尺码ID	size_id	int			
件数	quantity	smallint		0	
1.3.3	订单主表
内容解释
配送时间：指定快递公司在指定的时间段内，以客户的意愿来送货。
支付方式：采用何种支付的平台来付款
订单来源：目前只是扩展用
支付状态：下完订单后客户对订单的处理结果，【到付、已付款、未付款】
订单状态-客户：客户能够看到的该订单的处理流程，【提交订单、付款成功、配货中、商品已出库/等待收货、完成、取消、退换货】
订单状态-客服：客服和库房在审核订单和配送的时候看到的订单状态，【待完善/未确认、已确认（通知配货）、配货中（到配送状态继续处理）、已发货、已到货/完成（换货完成）、中止（作废）、拒收。】
正常/退/退换货：标识该订单是否发生了退换货，正常/换货在同两张表中处理，而退货在另外的两张表中处理。
原单：可退、可换、可复制
退单：无
换单：可退
券/现金/蜜豆退回情况：券|现金|蜜豆三种 以二进制数字形式保存
					如：001 券和现金未退回，蜜豆已退回  默认为000	
退单单号和换单单号 如果为多次则用逗号（英文）隔开
逻辑表名	订单主表	物理表名	order_info
主键	order_info _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	order_info_id	int			自增
订单号	oid	varchar(30)			
关联单号	relate_oid	varchar(30)			
应付金额	amount_payable	double(10)		0.00	订单应该支付的金额
已付金额	amount_paid	double(10)		0.00	已经支付金额-现金账户
销售价总额	sale_price_total	double(10)			
账户ID	account_id	int			
收货人	consignee	varchar(10)			
手机号	mobile	varchar(20)			11位
地址	address	varchar(50)			
配送时间ID	delivery_time_id	tinyint			
支付方式ID	pay_id	tinyint			
配送方式ID	delivery_type_id	tinyint			
省份	province	varchar(20)			
城市	city	varchar(20)			
县区	country	varchar(20)			
订单来源ID	order_source_id	tinyint			
支付状态	pay_status	tinyint			0:到付 1:未付款 2:已付款
订单状态-客户	order_status_customer_id	tinyint			
订单状态-客服	order_status_system_id	tinyint			
正常/退/退换货	order_type	tinyint		0	1:正常  2:退  3:退换
发票类型ID	invoice_type_id	tinyint			
发票抬头	invoice_head	varchar(30)			
运费优惠	freight_reduce	double(10)		0.00	
应付运费	freight_payable	double(10)		0.00	
商品总金额	product_total_price	double(10)		0.00	
优惠金额	discount	double(10)		0.00	
备注-客户	remark_customer	varchar(50)			客户的备注
备注-客服	remark_system	varchar(50)			客服沟通的临时记录
IP地址	ip	varchar(30)			
邮编	post_code	varchar(10)			
固定电话	telephone	varchar(20)			
邮箱	email	varchar(30)			
下单时间	add_time	datetime			
付款时间	pay_time	datetime			
换单单号	exchange_oid	varchar(250)			发生换货时记录换货单号
退单单号	return_oid	varchar(250)			发生退货时记录退单号
自定义金额	custom_price	double(10)		0.00	用于客服差额找齐
券号	coupon_code	varchar(30)			优惠券号
券优惠	coupon_reduce_price	double(10)		0.00	优惠券的优惠金额
现金优惠	cash_reduce_price	double(10)		0.00	现金账户的优惠金额
蜜豆优惠	bean_reduce_price	double(10)		0.00	使用蜜豆后的优惠金额
是否蜜豆等级免的运费	is_free_account_level	tinyint		0	0:否 1:是
1.3.4	订单子项表
逻辑表名	订单子项表-客服	物理表名	order_detail
主键	order_detail_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	order_detail_id	int			自增
订单主表ID	order_info _id	int			
库存表ID	stock_id	int			
订单号	oid	varchar(30)			
款号	no	varchar(20)			
套装款号	suite_no	varchar(20)			
套装名称	suite_name	varchar(50)			
商品名称	name	varcahr(50)			
颜色	color	varchar(20)			
尺码	size	varchar(20)			
数量	quantity	smallint		1	
小计	subtotal	double(10)		0.00	
市场价格	market_price	double(10)		0.00	
销售价	sale_price	double(10)		0.00	
成交价	deal_price	double(10)		0.00	
套装价	suite_price	double(10)		0.00	
区分套装	suite_random	varchar(30)			为了区别同一套装
是否套装	is_suite	tinyint		0	0:非 1:是
折扣比例	discount_rate	double(10)		0.00	
仓库ID	warehouse_id	tinyint			
店铺ID	shop_id	tinyint			
是否已晒单	is_posted	tinyint		0	0:未晒单  1:已晒单
是否已评论	is_comment	tinyint		0	0:未评论 1:已评论
是否为赠品	is_gift	tinyint		0	0:非 1:是
1.3.5	退单主表
退单状态：未收货、确认/已收货、质检通过（质检不通过/退回完成）、入库/退货完成（拒收完成）。
逻辑表名	退单主表	物理表名	order_return
主键	order_return _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	order_return_id	int			自增
订单号	return_oid	varchar(30)			
关联原单号	order_info_oid	varchar(30)		0.00	
原单ID	order_info_id	int		0.00	
应退金额	refund_payable	double(10)		0.00	
已退金额	refund_paid	double(10)		0.00	
销售价总额	sale_price_total	double(10)		0.00	
账户ID	account_id	int			
收货人	consignee	varchar(10)			
手机号	mobile	varchar(20)			11位
地址	address	varchar(50)			
省份	province	varchar(20)			
城市	city	varchar(20)			
县区	country	varchar(20)			
退单状态	return_status_id	tinyint			return_status表主键
运费	freight	double(10)		0.00	
商品总金额	product_total_price	double(10)		0.00	
备注-客户	remark_customer	varchar(30)			客户的备注
备注-客服	remark_system	varchar(30)			客服沟通的临时记录
IP地址	ip	varchar(30)			
邮编	post_code	varchar(10)			
固定电话	telephone	varchar(20)			
邮箱	email	varchar(30)			
退单时间	add_time	datetime			
退货原因	return_reason_id	tinyint			
客户自定义退货原因	custom_return_reason	varchar(50)			用户可以自己填写退货原因
退款方式	refund_type_id	tinyint			
自定义金额	custom_price	double(10)		0.00	用于客服差额找齐
1.3.6	退单子表
逻辑表名	退单子项表	物理表名	order_return_detail
主键	order_return_detail_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	order_return_detail_id	int			自增
退单主表ID	order_return_id	int			
库存表ID	stock_id	int			
退单单号	return_oid	varcahr(30)			
款号	no	varchar(20)			
款号	no	varchar(20)			
套装款号	suite_no	varchar(20)			
商品名称	name	varcahr(50)			
颜色	color	varchar(20)			
尺码	size	varchar(20)			
数量	quantity	smallint		1	
小计	subtotal	double(10)		0.00	
市场价格	market_price	double(10)		0.00	
销售价	sale_price	double(10)		0.00	
成交价	deal_price	double(10)		0.00	
套装价	suite_price	double(10)		0.00	
区分套装	suite_random	varchar(30)			为了区别同一套装
是否套装	is_suite	tinyint		0	0:非 1:是
折扣比例	discount_rate	double(10)		0.00	
仓库ID	warehouse_id	tinyint			
店铺ID	shop_id	tinyint			
是否为赠品	is_gift	tinyint		0	0:非 0:是
1.3.7	退单状态表
代码表
未收货、质检通过、质检未通过、入库、退回
逻辑表名	退单状态表	物理表名	return_status
主键	return_status_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	return_status_id	tinyint			自增
名称	name	varchar(20)			
1.3.8	退款方式表
代码表
逻辑表名	退款方式表	物理表名	refund_type
主键	refund_type_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	refund_type_id	tinyint			自增
名称	name	varchar(20)			
1.3.9	退货原因表
代码表
逻辑表名	退货原因表	物理表名	return_reason
主键	return_reason _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	return_reason _id	tinyint			自增
名称	name	varchar(50)			
1.3.10	订单配送信息表
逻辑表名	订单配送信息表	物理表名	order_delivery
主键	order_delivery _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	order_delivery_id	int			自增
订单主表ID	order_info_id	int			
订单号	oid	varchar(30)			
快递公司	express_company	varchar(10)			
快递单号	tracking_number	varchar(30)			
发货时间	send_time	datetime			
首重	initial_weight	double(10)		0.00	单位：kg
续重	additional_weight	double(10)		0.00	单位：kg
备注	remark	varchar(30)			
1.3.11	运费模板表
注意：这里如果只设置父节点的ID的话，默认该父节点下的所有的值都一样。
如果既有父节点又有子节点，则取子节点的值
这里注意默认运费的配置。
按层级获取运费的值
1：如果省市县、配送方式、支付方式 三者中都存在  则取其值
2：如果省市县、配送方式存、支付方式 任何两者存在而第三者不存在，则取其值
3：如果省市县、配送方式、支付方式 三者中如果任何两者不存在，只有一个存在， 则取其值
4：如果三个值都不存在，则取第一条记录的运费值（默认运费）
逻辑表名	运费模板表	物理表名	freight_template
主键	freight_template _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	freight_template _id	int			自增
名称	name	varchar(20)			
省市县ID	region_id	int			
配送方式ID	delivery_type_id	tinyint			
支付方式ID	pay_id	tinyint			
备注	remark	varchar(30)			
首重运费	initial_weight_freight	double(10)			默认值
续重运费	additional_weight_freight	double(10)			
是否支持到付	is_cod	tinyint		0	0:支持 1:不支持
最短到货时间	min_eta	tinyint			1（天）
最长到货时间	max_eta	tinyint			3（天）
是否启用	is_enable	tinyint		1	0:不启用 1:启用
1.3.12	地区表
逻辑表名	地区表	物理表名	region
主键	region _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	region_id	int			自增
名称	name	varchar(30)			
父ID	parent_id	smallint			
邮编	post_code	varchar(10)			
是否启用	is_enable	tinyint		1	1:启用 0:不启用
1.3.13	用户下单偏好表
根据用户的配送地址ID+账户ID动态读取用户的下单偏好,每个用户可以有多条，以优先级来区隔出来。
逻辑表名	用户下单偏好表	物理表名	order_preferences
主键	order_preferences _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	order_preferences_id	int			自增
账户ID	account_id	int			
配送方式ID	delivery_type_id	tinyint			
配送时间ID	delivery_time_id	tinyint			
支付方式ID	pay_id	tinyint		1	
运费模板ID	freight_template _id	int			
优先级	priority	int	0		按最高优先级显示
添加时间	add_time	datetime			
1.3.14	配送方式表
代码表
逻辑表名	配送方式表	物理表名	delivery_type
主键	delivery_type _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	delivery_type _id	tinyint			自增
名称	name	varchar(20)			
1.3.15	配送时间表
代码表
逻辑表名	配送时间表	物理表名	delivery_time
主键	delivery_time _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	delivery_time _id	tinyint			自增
名称	name	varchar(30)			
1.3.16	支付方式表
逻辑表名	支付方式表	物理表名	pay
主键	pay _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	pay _id	tinyint			自增
支付类型ID	pay_type_id	tinyint			
名称	name	varchar(20)			
是否启用	is_enable	tinyint		1	0:不启用 1:启用
排序	order_by	int		0	
1.3.17	支付类型表
代码表
逻辑表名	支付类型表	物理表名	pay_type
主键	pay_type _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	pay_type _id	tinyint			自增
名称	name	varchar(20)			
1.3.18	发票类型表
代码表
逻辑表名	发票类型表	物理表名	invoice_type
主键	invoice_type _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	invoice_type _id	tinyint			自增
名称	name	varchar(20)			
1.3.19	订单状态表-客户
代码表
逻辑表名	订单状态表-客户	物理表名	order_status_customer
主键	order_status_customer _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	order_status_customer _id	tinyint			自增
名称	name	varchar(20)			
1.3.20	订单状态表-客服
代码表
逻辑表名	订单状态表-客服	物理表名	order_status_system
主键	order_status_system_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	order_status_system _id	tinyint			自增
名称	name	varchar(20)			
1.3.21	订单来源表
代码表
平台级别：淘宝、京东…
店面级别：实体店..
逻辑表名	订单来源表	物理表名	order_source
主键	order_source_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	order_source _id	tinyint			自增
名称	name	varchar(20)			
1.4	销售活动类
1.4.1	群组表
如果群组中含价格，就直接读取价格子表
查询条件，这里根据查询条件来定义商品的范围。
比如：brand=1&sex=1|stock<7
这里的条件内容实现的时候，有固定的语法和语义，只允许查看，修改的话在弹出框中修改
逻辑表名	群组表	物理表名	product_group
主键	product_group_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	product_group_id	int			自增
编号	code	varchar(20)			
名称	name	varchar(20)			
条件内容	condition_find	varchar(100)			查询条件
条件组/款号组/两者并存	is_type	tinyint		0	0:条件群组 1:款号群组 2:条件群组+款号群组
是否货架组	is_shelf	tinyint		0	0:非 1：是
备注	remark	varchar(20)			
是否启用	is_enable	tinyint		1	0:不启用 1:启用
1.4.2	群组-商品条件表
定位群组中的商品条件的内容，主要体现这些条件中的key值是隶属于哪些表的哪些字段，然后可以方便的查询其值的范围。
逻辑表名	群组商品条件表	物理表名	group_condition
主键	group_condition_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	group_condition_id	int			自增
表名	table_name	varchar(30)			
字段逻辑名	show_name	varchar(30)			
字段物理名	column_name	varchar(30)			
1.4.3	群组子表
逻辑表名	群组子表	物理表名	group_sub
主键	group_sub_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	group_sub _id	int			自增
群组ID	product_group_id	int			
商品ID	product_id	int			
颜色ID	color_id	int			如果为空以商品ID为准
价格	custom_price	double(10)		0.00	自定义价格
1.4.4	促销信息表
优先级：如果有多个活动同时进行时，根据优先级来判断先执行哪个活动。
简名、是否在单品页显示，这两个字段为了使促销活动能在单品页提示出来而设计。
要在单品页面显示图标
排斥关系：如果几个活动之间不能允许同时存在，则按优先级顺序执行活动，有排斥关系的先执行第一个，第二个有排斥关系的则不允许执行。
所以，具有排斥关系的促销活动，请注意设置排斥关系及优先级大小
图标ID：如果促销分类中的图标ID为空，则按此图标显示。
逻辑表名	促销信息表	物理表名	sale
主键	sale_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	sale_id	int			自增
名称	name	varchar(20)			
促销类型	sale_type_id	smallint			促销类型表ID
优先级	priority	int		0	
活动简名	sample_name	varchar(30)			
是否单品页显示	is_item	tinyint		0	0:默认不显示
描述	description	varchar(50)			
图标ID	picture_resource_id	int			图片资源表ID
排斥关系	is_exclude	tinyint		0	0:非排斥  1:排斥
是否启用	is_enable	tinyint		1	0:不启用 1:启用
添加时间	add_time	datetime			
1.4.5	促销类型表
对促销活动进行分门别类，每个类别下属不同的规则。预留一个自定义的规则，把所有的规则全部罗列。
代码表
逻辑表名	促销类型表	物理表名	sale_type
主键	sale_type_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	sale_type_id	tinyint			自增
名称	name	varchar(20)			
1.4.6	促销类型-促销规则关联表
关联该促销类型下都包含哪些促销规则
如：满减，立减 之类
逻辑表名	促销类型-促销规则关联表	物理表名	sale_type_join_rule
主键	sale_type_join_rule_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	sale_type_join_rule_id	int			自增
促销类型ID	sale_type_id	tinyint			
促销规则ID	sale_rule_id	int			该促销活动显示图标
1.4.7	促销规则表
此表是所有促销活动的肢解，分解成一条条的规则，然后按规则的顺序来组合成活动，这里只是定义规则的名字，而不会定义规则值，规则的赋值在一个关联表中执行。
所有促销活动罗列如下：
减免运费：订单满多少钱或使用了某种类型的券
券：只是参加活动的一种方式
套装：固定实现
搭配：固定实现
满减：满多少钱后立减多少
买几赠几：买几赠几
满换购：满多少钱可以选择一个价值多少钱的商品。
买换购：买指定商品可以选择一个价值多少钱的商品。
满赠：满多少钱可以选择一个赠品
买赠：买指定商品可以选择一个赠品
折上折：折扣上再折扣
限时抢购：时间相关
单品、订单满多少钱 送券
单品 送几倍蜜豆

转发微博、登录、注册 送券
登录、注册、评论、晒单 送几倍蜜豆

条件：行为+时间范围+用户（个人账号ID或等级、用户组、购物历史）+商品（个体、品类、季节、某种形式的组）+（订单）件数+（订单）金额+支付方式
优惠手段：直接金额、（减）金额/比例折扣、劵、蜜豆（倍数）、赠品（特定组、件数）

触发规则引擎执行的动作可以使用监听事件模式。一个监听器一直监听该活动的启动开关，然后等执行到某一个动作（比如登录）时，启动事件，打开监听器即可。
规则抽取如下：
编码	描述	执行动作
A	登录	验证是否登录
B	用户级别、用户组	验证用户级别
C	时间限制	验证时间
D	指定品类 	验证商品品类
E	指定货架	验证商品货架
F	指定商品组（含价格和不含价格） 	验证商品是否在此群组内
G	排除商品组（含价格和不含价格）	验证商品是否不再此群组内
H	赠品组	验证赠品是否满足
I	多少件商品	验证商品总件数是否满足
K	满多少钱	验证购买总金额是否满足
L	几件多少折 2:9,3:8,4:7	执行折上折活动
M	减多少钱	执行立即优惠的金额
N	打多少折扣	执行打多少折扣
O	直接定最终售价	执行组的商品都为这一个价格
P	减运费	执行减少的运费
Q	免运费	执行免运费
R	返什么卷	执行返券
S	几倍蜜豆	执行送蜜豆=成交价×几倍
T	多少个蜜豆	执行赠送多少个蜜豆
U	买几赠几	执行买几赠几动作

代码表
逻辑表名	促销规则表	物理表名	sale_rule
主键	sale_rule_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	sale _rule_id	int			自增
规则名称	name	varchar(20)			
1.4.8	促销信息-规则关联表
此表组合成了各个的活动，并且根据顺序可调整每条规则的执行先后。
逻辑表名	促销信息-规则关联表	物理表名	sale_join_rule
主键	sale_join_rule_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	sale _join_rule_id	smallint			自增
促销信息ID	sale_id	int			哪个促销活动
促销规则ID	sale_rule_id	int			执行什么规则
规则值	rule_value	varchar(30)			规则的赋值
顺序	order_by	int		0	由大到小排序
是否启用	is_enable	tinyint		1	0:不启用 1:启用
1.4.9	券主表
如果折扣字段字段值不等于1，则会覆盖面值，然后折扣起作用
能否重复使用：标识该券是否可以重复使用，不受使用次数的限制
逻辑表名	券主表	物理表名	coupon
主键	coupon_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	coupon_id	int			自增
名称	name	varchar(20)			
券类型	coupon_type_id	tinyint			
面值	face_value	double(10)		0.00	
描述	description	varchar(30)			
延迟天数	delay_day	tinyint		0	从现在开始几天后执行
是否免运费	is_free	tinyint		0	0:否 1:是
能否重复使用	is_reuse	tinyint		0	0:不可 1:可以
满多少钱	enough_money	double(10)		0.00	
打多少折	discount	double(10)		1	1:不打折
包含商品群组	include_group	varchar(30)			群组号，逗号隔开
排除商品群组	exclude_group	varchar(30)			群组号，逗号隔开
开始时间	begin_time	datetime			
结束时间	end_time	datetime			
添加时间	add_time	datetime			
修改时间	modify_time	datetime			
修改人	admin_id	int			后台账户ID
是否启用	is_enable	tinyint		1	0:不启用 1:启用
1.4.10	券类型表
代码表
红包、现金券、免运费券、折扣券、减运费卷
逻辑表名	券类型表	物理表名	coupon_type
主键	coupon_type _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	coupon_type _id	tinyint			自增
名称	name	varchar(20)			
1.4.11	券明细表
券定义：16位的数字和大写字母组成。排除0 O两个字符
逻辑表名	券明细表	物理表名	coupon_code
主键	coupon_code _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	coupon_code_id	bigint			自增
券主表ID	coupon_id	int			
券编号	code	varchar(20)			
券密码	password	varchar(20)			
账户ID	account_id	int			
是否使用过	is_used	tinyint		0	0: 已使用1:未使用
添加时间	add_time	datetime			
是否启用	is_enable	tinyint		1	0:不启用 1:启用
1.4.12	券记录表
含发券和用券两种记录
行为或结果：存储发券时候的渠道和用券时候的单号
逻辑表名	用券记录表	物理表名	coupon_history
主键	coupon_history_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	coupon_history _id	int			自增
券号	code	varchar(20)			
行为或结果	tx_behavior	varchar(30)			
账户ID	account_id	int			
发券/用券	is_send_used	tinyint			1：发券  2：用券
添加时间	add_time	datetime			
1.5	交互类
1.5.1	互动分类表
代码表
互动分类有：咨询、问答等
逻辑表名	互动分类表	物理表名	interaction_type
主键	interaction_type_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	interaction_type_id	tinyint			自增
名称	name	varchar(20)			
1.5.2	互动详细表
逻辑表名	互动详细表	物理表名	interaction
主键	interaction_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	interaction_id	int			自增
账户ID	account_id	int			
分类ID	interaction_type_id	tinyint			
内容	content	varchar(30)			
父ID	parent_id	int			有父ID说明有回复
有用数	useful	int			
没用数	useless	int			
添加时间	add_time	datetime			
审核人	admin_id	smallint			后台账户ID
审核时间	review_time	datetime			
审核状态	review_status	tinyint		0	0未审核 1已通过 2未通过
审核结果说明	review_result	varchar(30)			
1.5.3	晒单表
库存ID能唯一标识出一个SKU（款色码）。
逻辑表名	晒单表	物理表名	product_post
主键	product_post_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	product_post_id	int			自增
账户ID	account_id	int			
订单子表ID	order_detail_id	int			
库存ID	stock_id	int			stock表的ID
标题	title	varchar(30)			
添加时间	add_time	datetime			
内容	content	varchar(100)			
父ID	parent_id	int			
浏览次数	read_count	int			
回复次数	reply_count	int			
可回复帖子的账户级别	account_level_id	tinyint			蜜蜂规则表ID
审核人	admin_id	int			后台账户ID
审核时间	review_time	datetime			
审核状态	review_status	tinyint		0	0未审核 1已通过 2未通过
审核结果说明	review_result	varchar(30)			
1.5.4	晒单图片说明表
每个晒单贴对应多张图片，每张图片可以允许有不同的图片说明。
逻辑表名	晒单图片说明表	物理表名	product_post_picture
主键	product_post_picture_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	product_post_picture_id	bigint			自增
晒单ID	product_post_id	bigint			
图片	picture_url	varchar(100)			
备注	remark	varchar(20)			
是否默认图	is_default	tinyint		0	0: 非默认图1:默认图
1.5.5	评论表
平均评分有小数的概念，平均评分计算时候，以0.1-0.5之间按0.5算  0.5-1之间 按1算
如果作假则库存ID 即：product_stock表的ID会起作用。能唯一标识一个款色码
逻辑表名	评论表	物理表名	comment
主键	comment_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	comment_id	bigint			自增
订单子项ID	order_detail_id	int			好评、中评、差评
库存ID	stock_id	int			stock表的ID
账户ID	account_id	int			
标题	title	varchar(20)			
内容	content	varchar(50)			
有用数	useful	int			
没用数	useless	int			
父ID	parent_id	int			回复评论时使用
平均评分	score_avg	double(10)		0.00	1-5分，有小数概念
添加时间	add_time	datetime			
审核人	admin_id	int			后台账户ID
审核时间	review_time	datetime			
审核状态	review_status	tinyint		0	0未审核 1已通过 2未通过
审核结果说明	review_result	varchar(30)			
1.5.6	评论打分项表
记录每个商品类别（童鞋、童装、玩具…）的不同的打分类项和打分子项
商品属性ID：查询商品属性表的顶层项
逻辑表名	评论打分类别表	物理表名	comment_item
主键	comment_item_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	comment_item_id	int			自增
商品类型ID	product_type_id	tinyint			product_type表ID
大项/打分项	item_name	varchar(30)		1	
父ID	parent_id	int			
分数值	score	double			
是否启用	is_enable	tinyint		1	0:禁用 1：使用
1.5.7	评论打分结果详细表
逻辑表名	评论打分结果详细表	物理表名	comment_detail
主键	comment_detail_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	comment_detail_id	bigint			自增
评论ID	comment_id	bigint			
打分项	item_name_id	int			comment_item表ID
答案	item_value_id	int			comment_item表ID
评分	score	double			1-5分
添加时间	add_time	datetime			
1.5.8	评论结果统计表
如果打分项ID为Null时，则为大类别统计项，统计该大类别的总评论数和总平均评分
平均评分有小数的概念，平均评分计算时候，以0.1-0.5之间按0.5算  0.5-1之间 按1算
百分比：各项占该层的百分比为多少，只计算同层
数据参考录入：
ID		商品ID		打分项ID	答案ID	打分次数 平均分  总分     百分比
 
逻辑表名	评论结果统计表	物理表名	comment_statistics
主键	comment_statistics_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	comment_statistics_id	int			自增
商品ID	product_id	int			
打分项	item_name_id	int			comment_item表ID
答案	item_value_id	int			comment_item表ID
打分次数	times	int		0	
平均评分	score_avg	double(10)		0.00	1-5分间，有小数的概念
总评分	total_score	double(10)		0.00	总评分
百分比	rate	double(10)		0.00	各项占该层的百分比
1.5.9	评论级别表
逻辑表名	评论级别表	物理表名	comment_level
主键	comment_level_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	comment_level_id	tinyint			自增
级别名	name	varchar(10)			好评、中评、差评
从多少分	star_from	double		0	
到多少分	star_to	double		0	
1.5.10	评论汇总表-单款商品
评论总数计算出来
逻辑表名	评论级别表	物理表名	comment_product
主键	comment_product_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	comment_product_id	int			自增
商品ID	product_id	int			product表ID
评论级别ID	comment_level_id	tinyint			
评论数	times	int			
占比	rate	double(10)			好评中评和差评率
1.6	系统内容类
1.6.1	广告位置表
代码表
标识出页面中可能出现的广告的位置，如：上部Banner条、左侧…
逻辑表名	广告位置表	物理表名	ad_position
主键	ad_position_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	ad_position _id	int			自增
名称	name	varchar(20)			
1.6.2	广告具体位置表
对广告的具体位置信息进行界定。
在哪个页面（按货架打开？ 按品牌打开..）的哪个具体的位置。这样能确定唯一一个广告位置。注意：这里可以定义群组，如果条件名为群组，则可以定义该广告可同时放置在这些群组内的商品上（比如单品页面）。
逻辑表名	广告具体位置表	物理表名	ad_page_position
主键	ad_page_position_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	ad_page_position_id	int			自增
具体名称	name	varchar(20)			
页面ID	page_id	int			列表页
广告位置ID	ad_position_id	int			Banner条
条件名	ad_key	varchar(20)			货架
条件名代码	ad_key_code	varchar(20)			catalog
条件值	ad_value	varchar(20)			T恤
条件值代码	ad_value_code	varchar(20)			t_shirt
1.6.3	广告与位置关联表
关联具体的位置放置什么具体的广告，两列为联合主键
如果某个位置可能出现多个广告的情况，要根据广告的优先级字段进行排序，取出优先级最高的一个即可。
逻辑表名	广告与位置关联表	物理表名	ad_join_position
主键	ad_join_position_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键	ad_join_position_id	int			主键
广告ID	ad_id	int			具体广告内容
广告位置ID	ad_page_position_id	int			具体广告所放置的位置
1.6.4	广告资源表
这里的描述主要是，如果图片alt事件时候的提示内容。
逻辑表名	广告资源表	物理表名	ad
主键	ad_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	ad_id	int			自增
名称	name	varchar(20)			
图片地址	picture_url	varchar(100)			
链接地址	link_url	varchar(100)			
宽	width	int			
高	height	int			
倒计时	countdown_time	datetime			
描述	description	varchar(30)			
优先级	priority	int		0	
是否启用	is_enable	tinyint		1	0:禁用 1：使用
1.6.5	内容类型表
代码表
公告、通知
逻辑表名	内容类型表	物理表名	content_type
主键	content_type_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	content_type_id	tinyint			自增
名称	name	varchar(20)			
1.6.6	内容表
公告和通知的内容
逻辑表名	内容表	物理表名	content
主键	content_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	content_id	int			自增
内容类型ID	content_type_id	tinyint			
主标题	first_title	varchar(30)			
副标题	second_title	varchar(30)			
内容	content	text			支持富文本框编辑
排序	order_by	int		0	值越大越靠前显示
撰写人	added_by	varchar(20)			可自己定义撰写人
撰写人职位	position	varchar(30)			
撰写人单位	employer	varchar(30)			
添加时间	add_time	datetime			
是否启用	is_enable	tinyint		1	0:禁用 1：使用
1.6.7	问卷主表
逻辑表名	问卷主表	物理表名	questionnaire
主键	questionnaire_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	questionnaire_id	int			自增
问卷名称	name	varchar(20)			
问卷标题	title	varchar(30)			
问卷说明	description	varchar(50)			
页面ID	page_id	int			该问卷所在页面
是否启用	is_enable	tinyint		1	0:禁用 1：使用
1.6.8	问卷内容表
如果父ID为null 则为标题，每道题目的父ID为该标题的行的ID
逻辑表名	问卷内容表	物理表名	questionnaire_item
主键	questionnaire_item_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	questionnaire_item_id	int			自增
问卷主表ID	questionnaire_id	int			
题目or选项	qa	varchar(50)			
父ID	parent_id	int			
多选/单选/其他	answer_type	tinyint			0:多选 1:单选 2:其他
是否有输入框	is_text_field	tinyint		0	0: 没有 1:有
1.6.9	问卷结果记录表
逻辑表名	问卷结果记录表	物理表名	questionnaire_result
主键	questionnaire_result_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	questionnaire_result_id	int			自增
账户ID	account_id	int			
问卷主表ID	questionnaire_id	int			
题目	item_name_id	int			questionnaire_item表ID
选项	item_value_id	int			questionnaire_item表ID
文本输入内容	text_field	varchar(20)			
添加时间	add_time	datetime			
1.6.10	问卷结果统计表
逻辑表名	问卷结果记录表	物理表名	questionnaire_statistics
主键	questionnaire_ statistics_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	questionnaire_statistics_id	int			自增
问卷主表ID	questionnaire_id	int			questionnaire表ID
题目	item_name_id	int			questionnaire_item表ID
选项	item_value_id	int			questionnaire_item表ID
次数	frequency	int			
占比	percent	double(10)		0.00	
1.6.11	菜单导航表
逻辑表名	菜单导航表	物理表名	menu_item
主键	menu_item_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	menu_item_id	int			自增
菜单名	name	varchar(10)			
父ID	parent_id	int			
排序	order_by	int			
链接URL	link_url	varchar(100)			
是否启用	is_enable	tinyint		1	0:禁用 1：使用
1.6.12	提示关键词表
注意这里对搜索云的支持。
逻辑表名	搜索关键词表	物理表名	keyword_prompt
主键	keyword_prompt_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	keyword_prompt_id	int			自增
名称	name	varchar(20)			
排序	order_by	int			
搜索次数	times	int		0	搜索次数，为支持搜索云
是否启用	is_enable	tinyint		1	0:禁用 1：使用
1.6.13	浮动图标表
列表页面或者搜索页面的悬浮图标、或单品页面促销活动标识
左上角：促销活动
右上角：新旧品、热销、预售
右下角：特价
位置：1：左上、2：右上、3：左下、4：右下
群组ID：不同群组中的商品使用不同类型的图标，如有雷同，以优先级高的计算。
注意：这里的图标不只是在列表页面显示，在单品页面也是可以定义并显示。
促销活动信息表中有关于和图标绑定的信息。
逻辑表名	浮动图标表	物理表名	float_icon
主键	float_icon_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	float_icon_id	int			自增
群组ID	product_group_id	int			群组ID，可以为空
图片四角	picture_angle	tinyint		0	无
图标	picture_resource_id	int			图片资源表ID
图标上的文字	icon_text	varchar(10)			赠、售、降、hot
优先级	priority	int		0	
描述	description	varchar(30)			
是否启用	is_enable	tinyint		1	0:禁用 1：使用
1.6.14	--商品排序表
这里的英文可以标识该排序的友好描述，可用于url
逻辑表名	商品排序表	物理表名	product_order_by
主键	product_order_by_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	product_order_by_id	int			自增
排序名称	name	varchar(20)			
英文	en_name	varchar(20)			
描述	description	varchar(30)			
是否启用	is_enable	tinyint		1	0:禁用 1：使用
1.6.15	--商品筛选表
商品列表性页面的条件筛选：好评、最新、价格、销量..
逻辑表名	商品筛选表	物理表名	product_filter
主键	product_filter_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	product_filter_id	smallint			自增
名称	name	varchar(20)			
父ID	parent_id	smallint			
英文	en_name	varchar(20)			
描述	description	varchar(30)			
是否启用	is_enable	tinyint		1	0:禁用 1：使用
1.7	系统配置控制类
1.7.1	部门表
代码表
逻辑表名	部门表	物理表名	department
主键	department _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	department_id	tinyint			自增
名称	name	varchar(20)			
1.7.2	后台账户表
逻辑表名	后台账户表	物理表名	admin
主键	admin_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	admin_id	int			自增
登录名	username	varchar(20)			
密码	password	varchar(40)			
真实姓名	name	varchar(10)			
是否超级用户	is_sys	tinyint		0	0:否  1:是
固定电话	telephone	varchar(20)			
手机	mobile	varchar(20)			
描述	description	varchar(50)			
邮件	email	varchar(30)			
所属部门	department_id	tinyint			部门ID
职位	position	varchar(20)			
最后登录时间	login_time	datetime			
添加时间	add_time	datetime			
是否启用	is_enable	tinyint		1	1:启用 0：禁用
1.7.3	角色表
代码表
角色代码：ROLE_开头
逻辑表名	角色表	物理表名	admin_role
主键	admin_role_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	admin_role_id	int			自增
名称	name	varchar(20)			
角色代码	role_code	varchar(20)			
1.7.4	权限表
逻辑表名	权限表	物理表名	admin_action
主键	admin_action_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	admin_action_id	int			自增
权限名	name	varchar(20)			
描述	description	varchar(20)			
权限组ID	admin_action_group_id	tinyint			
权限类型	action_type	tinyint		0	0:url 1:method
链接	resource_url	varchar(100)			
优先权排序	priority	int		0	
是否启用	is_enable	tinyint		1	0:禁用 1：启用
添加时间	add_time	datetime			
修改时间	modify_time	datetime			
修改人	modify_admin_id	int			
1.7.5	用户角色关联表
逻辑表名	用户角色关联表	物理表名	admin_join_role
主键	admin_join_role_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	admin_join_role_id	int			自增
后台账户ID	admin_id	int			
角色ID	admin_role_id	int		1	
1.7.6	角色权限关联表
逻辑表名	角色权限关联表	物理表名	action_join_role
主键	action_join_role _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	action_join_role_id	int			自增
角色ID	admin_role_id	int			
权限ID	admin_action_id	int		1	
1.7.7	权限组表
代码表
标识哪些权限同属于一个组下面的。比如组可以分模块来划分，便于前台显示。
如可划分为，商品类，用户账户类，订单处理类，销售活动类。。。
逻辑表名	权限组表	物理表名	admin_action_group
主键	admin_action_group_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	admin_action_group_id	tinyint			自增
名称	name	varchar(20)			

1.7.8	--商品日志表
日志范围：涉及商品范围的访问关键日志保存在这里。如：列表，搜索，单品，首页，促销页等
日志格式：who#when#where from#what access#what do
内容格式：商品的英文URL+中文说明
例子：12685745#2012-05-04 12:25:12#117.79.239.65
#/product/tshirt/summer/12584_58754.htm#男童小短T小袖T恤@单品页面
逻辑表名	商品日志表	物理表名	log_product
主键	log_product_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	log_product_id	int			自增
账户ID	account_id	int			未登录则为null
来路IP地址	from_ip	varchar(30)			
来路URL	from_url	varchar(100)			
到达页面名称	to_page_name	varchar(50)			
到达页面内容	to_page_content	varchar(100)			
添加时间	add_time	datetime			
1.7.9	--账户日志表
日志范围：用户的个人账户中所有日志
逻辑表名	账户日志表	物理表名	log_account
主键	log_account_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	log_account_id	int			自增
账户ID	account_id	int			
来路IP地址	from_ip	varchar(30)			
来路URL	from_url	varchar(100)			
到达页面名称	to_page_name	varchar(50)			
到达页面内容	to_page_content	varchar(100)			
添加时间	add_time	datetime			
1.7.10	--订单日志表
日志范围：购物车及下单操作日志
逻辑表名	订单日志表	物理表名	log_cart_order
主键	log_cart_order_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	log_cart_order_id	int			自增
账户ID	account_id	int			
来路IP地址	from_ip	varchar(30)			
来路URL	from_url	varchar(100)			
到达页面名称	to_page_name	varchar(50)			
到达页面内容	to_page_content	varchar(100)			
添加时间	add_time	datetime			
1.7.11	--支付结算日志表
日志范围：下单后支付结算操作日志
逻辑表名	支付结算日志表	物理表名	log_payment
主键	log_payment_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	log_payment_id	int			自增
账户ID	account_id	int			
来路IP地址	from_ip	varchar(30)			
来路URL	from_url	varchar(100)			
到达页面名称	to_page_name	varchar(50)			
到达页面内容	to_page_content	varchar(100)			
添加时间	add_time	datetime			
1.7.12	后台系统操作日志表
日志范围：后台账户操作日志
日志格式：
逻辑表名	后台系统操作日志表	物理表名	log_admin
主键	log_admin_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	log_admin_id	int			自增
账户ID	admin_id	int			后台账户ID
内容	content	text			
添加时间	add_time	datetime			
1.7.13	页面表
代码表
逻辑表名	页面表	物理表名	page
主键	page_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	page_id	int			自增
名称	name	varchar(20)			
1.7.14	模板布局表
布局类型：左，右，左中，中右，左右，左中右，中
排序类型：横排/竖排
逻辑表名	模板布局表	物理表名	layout
主键	layout _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	layout_id	int			自增
布局名称	name	varchar(20)			
布局类型	layout_type	tinyint			1234567
排序类型	order_type	tinyint			1：横排2：竖排
页面ID	page_id	int			哪个页面的布局
添加时间	add_time	datetime			
是否启用	is_enable	tinyint		0	0:不启用 1:启用
1.7.15	盒子类别表
逻辑表名	盒子类别表	物理表名	box_type
主键	box_type _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	box_type_id	int			自增
名称	name	varchar(20)			
宽	width	int		1	
高	height	int		1	
每行显示个数	number_of_row	tinyint		1	
最大显示行数	max_row	tinyint		1	
添加时间	add_time	datetime			
1.7.16	盒子表
文件名：表示该盒子对应了哪个的ftl模板文件。
逻辑表名	盒子表	物理表名	box
主键	box _id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	box _id	int			自增
名称	name	varchar(20)			
显示文字	show_text	varchar(30)			盒子的空白栏上显示的文字
盒子类别ID	box_type_id	int			
文件名	file_name	varchar(30)			
排序	order_by	int		0	
是否启用	is_enable	tinyint		1	0:不启用 1:启用
添加时间	add_time	datetime			
无数据是否自动隐藏	is_hidden_no_data	tinyint			0:隐藏 1:显示
1.7.17	模板布局-盒子关联表
联合主键
逻辑表名	模板布局-盒子关联表	物理表名	layout_join_box
主键	layout_join_box_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	layout_join_box_id	int			主键
模板布局ID	layout_id	int			
盒子ID	box_id	int		1	
1.7.18	系统参数表
对系统中的所有参数做配置，包括一些定时数据处理参数
逻辑表名	系统参数表	物理表名	system_param
主键	system_param_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	system_param _id	smallint			自增
参数名	name	varchar(20)			
参数键	param_key	varchar(20)			
参数值	param_value	varchar(20)		1	
是否启用	is_enable	tinyint		1	0:不启用 1:启用
添加时间	add_time	datetime			
1.7.19	META模板表
这里的位置URL，会采用自动匹配的方式，来描述页面的位置。
比如单品页面可能是 /product/item/*
这里的标题，关键字和描述可以采用占位符的形式。
比如标题：${sex} 短袖T恤，${season}春秋两季可穿。
${sex}和${season}必须在占位符表中定义好。
模板数据取出后，放入memCache中即可。
该模板提供各个页面的meta选择。
逻辑表名	META类型表	物理表名	meta_template
主键	meta_template_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	meta_template _id	int			自增
名称	name	varchar(20)			
标题	title	varchar(50)			
关键字	keywords	varcha(100)			
描述	description	varchar(200)			
是否启用	is_enable	tinyint		1	0:不启用 1:启用
添加时间	add_time	datetime			
1.7.20	--占位符表
比如：季节:${season}
值由程序员一期全部填入。后期如果需要修改请程序员协助修改。
要确保占位符名字的唯一性。
逻辑表名	占位符表	物理表名	placeholder
主键	placeholder_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	placeholder_id	int			自增
名称	placeholder_name	varchar(20)			
占位符	param_name	varchar(20)			
是否固定	is_fixed	tinyint		0	0:非固定 1:固定
1.7.21	短信模板表
存储平台所需要发短信的各种模板，模板内容支持通配符${oid},${name}等
逻辑表名	短信模板表	物理表名	sms_template
主键	sms_template_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	sms_template_id	smallint			自增
名称	name	varchar(20)			
模板内容	content	text			HTML文本
是否启用	is_enable	tinyint		1	0:不启用 1:启用
添加时间	add_time	datetime			
1.7.22	邮件模板表
模板内容支持占位符${oid},${name}等
逻辑表名	邮件模板表	物理表名	email_template
主键	email_template_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
主键ID	email_template_id	smallint			自增
名称	name	varchar(20)			
标题	title	varchar(50)			
模板内容	content	text			HTML文本
是否启用	is_enable	tinyint		1	0:不启用 1:启用
添加时间	add_time	datetime			
1.7.23	图片资源类型
逻辑表名	图片资源类型表	物理表名	picture_resource_type
主键	picture_resource_type_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	picture_resource_type_id	tinyint			自增
名称	name	varchar(20)			
1.7.24	图片资源表
逻辑表名	图片资源表	物理表名	picture_resource
主键	picture_resource_id	索引	
逻辑字段名	物理字段名	数据类型	空值	默认值	备注
ID	picture_resource_id	int			自增
资源类型	picture_resource_type_id	tinyint			图片资源类型表ID
名称	name	varchar(20)			
图片URL	picture_url	varchar(100)			
描述	description	varchar(30)			
添加人	add_admin_id	int			添加人帐号
添加时间	add_time	datetime		当前	时间
2	 其他数据库对象
此处描述其他相关的数据库对象，例如视图、序列、触发器、存储过程、同义词等。如果对象过多，可按对象类别分章节。
3	物理结构
3.1	库存储
对各个表的存储量及增长趋势，估算数据量，规划存储结构及其分布，例如Oracle的表空间、数据文件，SQLServer的库及文件。
3.2	表存储
根据各个表的增删改查操作频度、数据量、惯用操作等特点，确定表的存储方式和参数，例如Oracle中的表空间、表分区、PCTFREE、PCTUSED、缓存。
3.3	索引
为调整性能而追加的索引说明。
3.4	其他
关于数据库用户及权限、配置参数等其他说明。
4	 数据字典
各类代码表（包括取值）。 
5	 注意事项
关于数据库管理维护需要注意的事项。例如：
1、	存储空间的扩展。
2、	索引的重建。
3、	数据备份。
4、	……
