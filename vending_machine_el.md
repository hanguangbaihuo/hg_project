### 合作伙伴关注文档 ###
    #### 工作流程说明 ###

        下单:
        1 合作伙伴按照汉光二维码生成方式,生成一个二维码,展示在屏幕上面
        2 用户使用 微信原生扫一扫扫描二维码,进入到汉光百货小程序
        3 用户支付成功后,航光百货推送一条消息给 合作伙伴,通知支付成功

        # product_id为合作伙伴和汉光百货之间传递商品的标识,需要在两个系统里面保持一致
        # app_id 和 app_secret 从汉光百货这里获取

    #### 1 Note: sign生成方法 ####
        1   参数(只有下列参数参与签名)
            {
                "product_id": "1",                          # 汉光商品ID(闪购ID)
                "nonce": "23455",                           # 一次性字符串
                "app_id": "app_1534851431",                 # 汉光分配的app_id
                "app_secret": "dkdkkkkdkdk",                # 从汉光获取的 密钥
                "order_id": "12345",                        # 自动售货机订单ID,汉光推送成功消息使用该订单ID
                "timestamp": "1534853718",                  # 时间戳
            }
        2 将上述参数按照key值进行排序后，按照key=value的格式拼接成一个字符串
            "app_id=app_1534851431&app_secret=cdke8945lk92dc5b889c40a0519&nonce=23455&product_id=1&timestamp=1534853718&order_id=12345"
        3 对这个字符串取MD5，然后大写
            可获得类似: A1D05398BE89B4906006364DE2725579 的签名

    #### 2 二维码 生产成方法 ####
        参数:
        {
            "product_id": "1",                          # 汉光商品ID(闪购ID)
            "nonce": "23455",                           # 一次性字符串
            "app_id": "app_1534851431",                 # 汉光分配的app_id
            "timestamp": "1534853718",                  # 时间戳
            "sign": "A1D05398BE89B4906006364DE2725579", # 签名
            "jump_type": "vm"                           # 固定参数
        }
        二维码内容拼接(测试系统)
        https://sparrow.dongyouliang.com/wx-app/jumpBridge?
        jump_type=vm&order_id=12345&product_id=1&app_id=app_1534851431&timestamp=1534853718&nonce=23455&sign=A1D05398BE89B4906006364DE2725579

        二维码内容拼接(正式系统)
        https://sparrow.hanguangbaihuo.com/wx-app/jumpBridge?
        jump_type=vm&order_id=12345&product_id=1&app_id=app_1534851431&timestamp=1534853718&nonce=23455&sign=A1D05398BE89B4906006364DE2725579

        将这个url生成二维码供用户扫码使用

            Note:
                测试和正式的区别是: 前缀不同
                测试系统: https://sparrow.dongyouliang.com/wx-app/jumpBridge
                正式系统: https://sparrow.hanguangbaihuo.com/wx-app/jumpBridge


    #### 2 支付成功后，汉光百货支付成功返回的消息 ####

        请求方式：POST
        该接口: 需要合作伙伴提供一个接收汉光百货消息推送的 接口
        参数：

            {
                "order_id": "12345",        # 自动售货机的订单号
                "price": "341.97",          # 订单原始价格
                "app_id": "app_343434",     # 汉光分配的app_id
                "timestamp": "12345657",    # 时间戳
                "nonce": "dafjljadn",       # 随机字符串
                "sign": "LKelkoo34o9ukm"    # 签名
            }
        Note：price一定是两位小数的字符串，比如5元钱应该是"5.00"

        要求返回：
            如果成功：
            http status 是 200
            返回内容

                {
                    "message": "success"
                }

        如果返回http状态不是200，或者虽然是200但是返回内容不是以上规定内容，都认为是返回失败。
        如果返回失败，我们会每一分钟推送一次，直到收到成功返回为止。

    ### 以上为合作伙伴文档 ###
