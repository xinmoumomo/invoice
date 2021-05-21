# invoice

## desc

上海航天电子发票对接平台，PHP版本的SDK

## 安装

```
composer require xinmoumomo/invoice
```

#### 发布配置

如果您希望覆盖存储库和条件所在的路径,请发布配置文件:

```shell script
php artisan vendor:publish
```

然后只需打开 `config/invoice.php` 并编辑即可！

#### 配置文件

```
return [
    // 电商平台编码
    'DSPTBM' => 'P1000001',
    // 纳税人识别码
    'NSRSBH' => '913101010000000090',
    // 纳税人名称
    'NSRMC' => 'XXX官方旗舰店',
    // 销货方名称
    'XHFMC' => 'XXX官方旗舰店',
    // 销货方地址
    'XHF_DZ' => '上海市徐汇区XXX号',
    // 销货方电话
    'XHF_DH' => '17621251***',
    // 销货方银行账号
    'XHF_YHZH' => 'XX支行    87878787878788787878787',
    // 开票员
    'KPY' => '开票员',
    // 收款员（可选）
    'SKY' => '收款员',
    //'复核员'
    'FHR' => '复核员',
    // 含税标志
    'HSBZ' => '0',
    // 终端类型标识
    'TERMINALCODE' => '0',
    // APPID
    'APPID' => 'ZZS_PT_DZFP',
    // 税号
    'TAXPAYWERID' => '913101010000000090',
    // 认证码
    'AUTHORIZATIONCODE' => 'NH873FG4KW',
    // 加密码
    'ENCRYPTCODE' => '2',
    // 发票开具 ECXML.FPKJ.BC.E_INV
    'INTERFACE_FPKJ' => 'ECXML.FPKJ.BC.E_INV',
    // 发票信息下载 ECXML.FPXZ.CX.E_INV
    'INTERFACE_FPXZ' => 'ECXML.FPXZ.CX.E_INV',
    // 邮箱发票推送 ECXML.EMAILPHONEFPTS.TS.E.INV
    'INTERFACE_FPYX' => 'ECXML.EMAILPHONEFPTS.TS.E.INV',
    // 请求码
    'REQUESTCODE' => 'sdf11dfd1MsfdFWegesdfIK',
    // 响应码
    'RESPONSECODE' => '121',
    // 密码
    'PASSWORD' => '',
    // 交互码
    'DATAEXCHANGEID' => '交互码',
    // 发票明细信息下载 ECXML.FPKJ.BC.E_INV
    'KJFP' => 'ECXML.FPKJ.BC.E_INV',
    // 发票信息推送 ECXML.FPXZ.CX.E_INV
    'DOWNLOAD' => 'ECXML.FPXZ.CX.E_INV',
    // 获取企业可用发票数量API ECXML.EMAILPHONEFPTS.TS.E.INV
    'EMAIL' => 'ECXML.EMAILPHONEFPTS.TS.E.INV',
    // 注册码
    'REGISTERCODE' => '922588450019',
    // 电子发票网址
    'HOST' => 'http://xxxxx',
    // 3DES密码
    'KEY' => '*************',
];
```

## 用法

```
          public static function info($order_ids, &$post_data, $no)
          {
              return [
                  'invoice_type'  => '01',
                  'invoice_title' => '测试发票单',
                  'items' => [
                      [
                          'name' => '治疗',  //项目名称
                          'quantity' => '1',
                          'price' => '100', //项目单价
                          'spbm' => '0000101000000000000', //商品编码 填商品名称对应的商品税收分类编码，19位不足补0
                          'zxbm' => '',    //自行编码
                          'id' => '',      //有折扣时自行编码取值
                          'sl' => '0.06',      //税率
                          'hsbz' => '0',      //含税标志
                          //是否享受优惠政策
                          'yhzcbs' => $yhzcbs ?? "",
                          //优惠政策类型
                          'zzstsgl' => $zzstsgl ?? '',
                          //零税率标识
                          'lslbs' => $lslab ?? '',
                      ],
                  ],
                  'discount' => '',
                  'mobile'   => config('invoice')['XHF_DH'],
                  // 价税合计金额
                  'sum' => '',
                  //订单号
                  'order_bn' => $no,
                  //请求流水号
                  'FPQQLSH' => $no,
                  //商品信息中第一条
                  'KPXM' => 'sfd',
                  //购货方名称
                  'GHFMC' => $company,
                  // 购货方手机
                  'GHF_SJ' => $contact_mobile,
                  // nor_code
                  'GHF_NSRSBH' => $nor_code,
                  // 购货方企业类型
                  'GHFQYLX' => '01',
                  // 开票类型  1 正票 2 红票
                  'KPLX' => ($type == 'main_ticket') ? 1 : 2,
                  // 操作代码
                  'CZDM' => ($type == 'main_ticket') ? 10 : 20,
                  // 合计不含税金额
                  'HJBHSJE' => '',
                  // 合计税额
                  'HJSE' => '',
                  // 请求流水号
                  'trade_no' => $invoice->no,
                  // 价税合计金额
                  'KPHJJE' => '',
                  // 购货方，银行账户
                  'GHF_YHZH' => !empty($invoice->bank_number) ? ($invoice->bank_number)['bank_name'] . ' ' . ($invoice->bank_number)['bank_account'] : '',
                  // 购货方 固定电话
                  'GHF_GDDH' => !empty($invoice->address_mobile) ? ($invoice->address_mobile)['mobile'] : '',
                  // 购货方地址
                  'GHF_DZ' => !empty($invoice->address_mobile) ? ($invoice->address_mobile)['address'] : '',
                  // 发票代码
                  'YFP_DM' => $invoice->invoice_code,
                  // 发票编号
                  'YFP_HM' => $invoice->invoice_no,
              ];
          }
```
