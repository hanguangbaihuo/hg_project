### 合作伙伴关注文档-赫莲娜 ###
    #### 工作流程说明 ###

        下单:
        1 合作伙伴按照汉光二维码生成方式,生成二维码,展示在屏幕上面
        2 用户使用 微信原生扫一扫 扫描二维码,进入到汉光百货小程序
        3 用户支付成功后,汉光百货推送一条消息给 合作伙伴,通知支付成功

        # 商品ID为合作伙伴和汉光百货之间传递商品的标识,需要在两个系统里面保持一致
        # app_id 和 app_secret 从汉光百货这里获取

    #### 1 Note: 签名sign生成方法 ####
        参数(只有下列参数参与签名)
            {
                "product": "23432",             # 汉光商品ID
                "nonce": "23455",               # 一次性字符串,长度不超过8位
                "app_id": "app_1534851431",     # 汉光分配的app_id
                "app_secret": "dkdkkkkdkdk",    # 从汉光获取的 密钥
                "vmhr_order_id": "12345",         # 自动售货机订单ID
                "timestamp": "1534853718",      # 时间戳
                "source": "vm_hr",              # 来源: 固定为 vm_hr
            }
        1 将上述参数按照key值进行排序(参数名ASCII码从小到大排序,字典序)，按照key=value的格式拼接成一个字符串
            "app_id=app_1534851431&app_secret=dkdkkkkdkdk&nonce=23455&product=23432&timestamp=1534853718&vmhr_order_id=12345&source=vm_hr"
        2 对这个字符串取MD5，然后大写
            可获得类似: A1D05398BE89B4906006364DE2725579 的签名

    #### 2 vm_code 生成方法 ####
        参数:
        {
            "product": "23432",                         # 汉光商品ID和
            "nonce": "23455",                           # 一次性字符串
            "app_id": "app_1534851431",                 # 汉光分配的app_id
            "vmhr_order_id": "12345",                   # 自动售货机订单ID
            "timestamp": "1534853718",                  # 时间戳
            "sign": "A1D05398BE89B4906006364DE2725579", # 步骤1里面的签名
            "source": "vm_hr"
        }
        1 将上述参数按照key=value拼接成一个字符串(排序无所谓)
            "app_id=app_1534851431&product=23432&nonce=23455&timestamp=1534853718&vmhr_order_id=12345&source=vm_hr&sign=A1D05398BE89B4906006364DE2725579"
        2 把 1 里面的字符串进行base64, 所获取的值为 vm_code
            YXBwX2lkPWFwcF8xNTM0ODUxNDMxJnByb2R1Y3Q9MjM0MzImbm9uY2U9MjM0NTUmdGltZXN0YW1wPTE1MzQ4NTM3MTgmdm1fb3JkZXJfaWQ9MTIzNDUmc291cmNlPXZtX2hyJnNpZ249QTFEMDUzOThCRTg5QjQ5MDYwMDYzNjRERTI3MjU1Nzk=

    #### 3 二维码 生产成方法 ####
        参数:
            jump_type=vm_v1  : 固定
            vm_code          : 上面生成的 vm_code 值

        二维码内容拼接(测试系统)
        https://sparrow.hanguangbaihuo.com/wx-app/jumpBridge?jump_type=vm_v1&vm_code=YXBwX2lkPWFwcF8xNTM0ODUxNDMxJnByb2R1Y3Q9MjM0MzImbm9uY2U9MjM0NTUmdGltZXN0YW1wPTE1MzQ4NTM3MTgmdm1fb3JkZXJfaWQ9MTIzNDUmc291cmNlPXZtX2hyJnNpZ249QTFEMDUzOThCRTg5QjQ5MDYwMDYzNjRERTI3MjU1Nzk=

        二维码内容拼接(正式系统)
        https://sparrow.hanguangbaihuo.com/wx-app/jumpBridge?jump_type=vm_v1&vm_code=YXBwX2lkPWFwcF8xNTM0ODUxNDMxJnByb2R1Y3Q9MjM0MzImbm9uY2U9MjM0NTUmdGltZXN0YW1wPTE1MzQ4NTM3MTgmdm1fb3JkZXJfaWQ9MTIzNDUmc291cmNlPXZtX2hyJnNpZ249QTFEMDUzOThCRTg5QjQ5MDYwMDYzNjRERTI3MjU1Nzk=

        将这个url生成二维码供用户扫码使用
            Note:
                测试和正式的区别是: 前缀不同
                测试系统: https://sparrow.dongyouliang.com/wx-app/jumpBridge
                正式系统: https://sparrow.hanguangbaihuo.com/wx-app/jumpBridge


    #### 4 支付成功后，汉光百货支付后推送消息 ####

        请求方式：POST
        该接口: 需要合作伙伴提供一个接收汉光百货消息推送的 接口
        参数：
            {
                "vmhr_order_id": "12345",   # 自动售货机的订单号
                "price": "341.97",          # 订单原始价格
                "app_id": "app_343434",     # 汉光分配的app_id
                "timestamp": "12345657",    # 时间戳
                "nonce": "dafjljadn",       # 随机字符串
                "sign": "LKelkoo34o9ukm"    # 签名
            }
        Note：
            price一定是两位小数的字符串，比如5元钱应该是"5.00"

        要求返回：
            如果成功：
            http status 是 200
            返回内容

                {
                    "message": "ok"
                }

        如果返回http状态不是200，或者虽然是200但是返回内容不是以上规定内容，都认为是返回失败。
        如果返回失败，我们会每一分钟推送一次，直到收到成功返回为止。

    ### 以上为合作伙伴文档 ###
