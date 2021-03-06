# gemx.io Http API Docmution

# 说明
    该API为GEMX(https://www.gmex.io)用于对外提供的http rest api接口文档说明.
    该api可供开发者进行行情数据的获取和交易服务的操作,请注意行情和交易是两个不同服务器。
    其中交易服务操作是需要用户提供相应的签名数据方可验证是否能获取到数据,关于签名的生产方法可参考下文中的"签名生成方法",
    行情服务的数据则无需签名可自由访问。

    GMEX官方的生产环境:
        官网地址: https://www.gmex.io
        行情服务: https://api-market.gmex.io/v1/rest
        交易服务: https://api-trade.gmex.io/v1/rest
    
    GMEX官方的模拟环境(供开发者测试使用):
        官网地址: https://simgo.gmex.io
        行情服务: https://market02.gmex.io/v1/rest
        交易服务: https://trade02.gmex.io/v1/rest
## 签名生成方法
> 计算公式: MD5(req+args+expires+API.SecretKey)

  例如:
  
    req="GetWalletsLog"
    
    args={AId:"102041501"}
    
    expires=1544167142509
    
    API.secretKey=bPQAAHKFnC%ywLNt2ydROYc58H%l47Be0bbw9NzNkbwd3ltgayNsOJg 
    
    (API.SecretKey为用户在官网申请API时生成的SecretKey)
    
  通过计算得到的签名:b094142522364fab85ef82b8f875ca89
    

## 行情API示例

* 获取服务器时间Time
```JavaScript
    // 请求
    http GET https://market02.gmex.io/v1/rest/Time 
    
    // 返回
    {
        "code": 0,              // 0成功,其它则为失败状态
        "data": "", 
        "time": 1544085815447   // 返回的服务器时间
    }
```

* 获取可订阅的交易对信息GetAssetD
```JavaScript
    // 请求
    http GET https://market02.gmex.io/v1/rest/GetAssetD
    
    // 返回
    {
        "code": 0,                                      // 0成功,其它则失败 
        "data": [                                       // 所有可订阅交易对列表
            {
                "DenyOpenAfter": 1546041600000,         // 交割时间
                "Expire": 9999999900000,                // 交易对的有效期
                "FeeMkrR": 0.0001,                      // 被动单手续费
                "FeeTkrR": 0.0003,                      // 主动单手续费    
                "FromC": "USDT",                       
                "FundingInterval": 28800,               // 结算间隔(毫秒)
                "FundingLongR": 5e-05,                  // 当前周期内的资金费率
                "FundingNext": 1544112000000,           // 下次结算时间戳
                "FundingPredictedR": 0,                 // 下个周期预测的资金费率
                "FundingShortR": 0.0005,                // 当前字段未使用
		"FundingTolerance": 59,                 // 偏移宽容度
		"FundingFeeR": 60,                      // Funding结算佣金
                "LiqR": 0,
                "LotSz": 1.0,                           // 最小委托量
                "MIR": 0.02,                            // 初始保证金率
                "MMR": 0.01,                            // 维持保证金率
                "MkSt": 1,                              // 市场状态
                "Mult": 0.0001,                         // 合约乘数
                "OpenInterest": 12718,                  // 最大持仓量
                "OrderMaxQty": 10000000.0,              // 最大委托量
                "OrderMinVal": 0.0001,                  // 委托的最小价值
                "PosLmtStart": 1000000000,             
                "PosOpenRatio": 1,
                "PrzIndex": 0.28,
                "PrzLatest": 0.3435,
                "PrzM": 0.3,                            // 标记价格
                "PrzMFairBasis": 27,
                "PrzMFairBasisCalc": 100,
                "PrzMFairBasisRate": 5,
                "PrzMMethod": "FairPrice",
                "PrzMax": 1000000.0,                    // 最大委托价格
                "PrzMaxChg": 1000,                      // 市价委托时撮合的最大次数
                "PrzMinInc": 0.0001,                    // 最小的价格变化
                "QuoteCoin": "USDT",                    // 报价货币
                "SettleCoin": "USDT",                   // 结算货币
                "SettleR": 0.001,                       // 结算费率
                "Sym": "XRP.USDTP",                     // 交易对名称
                "ToC": "XRP",
                "TotalVol": 3517956643,     
                "TrdCls": 3,                            // 交易对的类型 1.合约 2.现货 3.永续
                "Turnover": 0,
            }
        ]
    }
```
* 获取可订阅的指数信息GetCompositeIndex
```JavaScript
    // 请求
    
    http GET https://market02.gmex.io/v1/rest/GetCompositeIndex
    
    // 返回
    {
        "code": 0,   // 0成功,其它则失败
        "data": [
            {
                "At": 1544088829131,         // 时间(毫秒)
                "High24": 109.37213,         // 24小时的最高价格
                "Low24": 96.37748,           // 24小时的最低价格
                "Prz": 103.70592,            // 价格
                "Prz24": 107.479,            // 24小时的价格 
                "Sym": "GMEX_CI_ETH",        // 指数的名称 
                "Sz": 0.50887,               // 量
                "Turnover24": 44811810,      // 24小时的价值
                "Volume24": 430782           // 24小时的成交量
            },
            {
                "At": 1544088829132,
                "High24": 0.72193,
                "Low24": 0.6185,
                "Prz": 0.6622,
                "Prz24": 0.70777,
                "Sym": "GMEX_CI_ONT",
                "Sz": 0,
                "Turnover24": 1340805,
                "Volume24": 1985054
            }
        ]
    }
```

* 获取历史K线信息GetHistKLine
```JavaScript
    // 请求
    /**
     * 参数说明
     * Sym:交易对名称
     * Sec:K线的起始时间
     * Count: K线的数量
     * Type: K线的周期类型
     * Offset: 偏移量
     * */
     
    http POST https://market02.gmex.io/v1/rest/GetHistKLine  Sym='BTC1812' Sec:=1541987816 Count:=2 Offset:=0 Typ='1m'
    
    // 返回
    {
        "code": 0,   // 0成功,其它则失败
        "data": {
            "Count": 2,           // K线的数量
            "PrzClose": [         // 结束价格集合
                5462.5,          
                5480.5
            ],
            "PrzHigh": [          // 最高价格集合
                5464.5,
                5488
            ],
            "PrzLow": [           // 最低价格集合
                5462.5,
                5468.5
            ],
            "PrzOpen": [          // 开始价格集合
                5462.5,
                5468.5
            ],
            "Sec": [              // 时间集合
                1542536940,
                1542537000
            ],
            "Sym": "BTC1812",     // 交易对名称 
            "Turnover": [         // 价值集合
                4152192.5,
                9900955399.5
            ],
            "Typ": "1m",          // K线的周期,1m,3m,5m,15m,30m,1h,2h,4h,6h,8h,12h,1d,3d,1w, m:分钟 h:小时 d:天 w:周 M:月   
            "Volume": [           // 交易量集合
                760,
                1806176
            ]
        }
    }
```

* 指数的聚合行情GetIndexTick
```JavaScript
    // 请求
    /**
     * 参数哦说明
     * idx: 指数的名称,可根据获取到的可订阅指数里面获取指数的名称(接口GetCompositeIndex)
     * 
     * */
     
    http GET https://market02.gmex.io/v1/rest/GetIndexTick?idx=GMEX_CI_ETH
    
    // 返回
    {
        "code": 0,   // 0成功,其它则失败
        "data": {
            "At": 1544096824635,            // 时间
            "High24": 109.37213,            // 24小时最高
            "Low24": 96.37748,              // 24小时最低
            "Prz": 101.00231,               // 价格
            "Prz24": 108.54015,             // 24小时价格
            "RefThirdParty": {              // 采取的现货交易所的信息
                "binance": {                        
                    "prz": 100.38,          // 价格
                    "status": 0,            // 状态: 0:正常, 1:价格偏离, 2:长期价格偏离, 3:数据中断
                    "vol": 0                // 量
                },
                "bitfinex": {
                    "prz": 101.55126159,
                    "status": 0,
                    "vol": 0
                },
                "hitbtc": {
                    "prz": 102.46,
                    "status": 0,
                    "vol": 0
                },
                "huobi": {
                    "prz": 100.32,
                    "status": 0,
                    "vol": 0
                },
                "okex": {
                    "prz": 100.3003,
                    "status": 0,
                    "vol": 0
                }
            },
            "Sym": "GMEX_CI_ETH",           // 指数名称
            "Sz": 0,                        // 量
            "Turnover24": 46223338,         // 24小时指数价值
            "Volume24": 446536              // 24小时指数的交易量
        }
    }
```
* 交易对的聚合行情
```JavaScript
    // 请求
    /**
     * 参数说明
     * sym: 交易对名称 
     * 
     * */
     
    http GET https://market02.gmex.io/v1/rest/GetTick?sym=BTC1812
    
    // 返回
    {
        "code": 0,                         	// 0成功,其它则失败
        "data": {
            "At": 1544096989020,           	// 时间
            "High24": 3923.5,              	// 24小时最高
            "LastPrz": 3811,               	// 最新价格
            "Low24": 3671,                 	// 24小时最低
            "OpenInterest": 8380370,       	// 总持仓量 
            "Prz24": 3885,                 	// 24小时最初价格 
            "PrzAsk1": 3812,               	// 卖1价格
            "PrzBid1": 3810.5,             	// 买1价格
            "SettPrz": 4163,               	// 最新标记价格
            "Sym": "BTC1812",              	// 合约名称 
            "SzAsk": 402607560,            	// 总卖量
            "SzAsk1": 2193519,             	// 卖1总量
            "SzBid": 195668907,            	// 总买量
            "SzBid1": 2844882,             	// 买1总量
            "Turnover": 0.0118079244,      	// 总成交额
            "Turnover24": 659690.3038108406,  	// 24小时成交额
            "Volume": 72569054316,         	// 总成交量
            "Volume24": 2503022033,         	// 24小时总成交量
            "FundingLongR":0,               	// 当前周期内的资金费率
	    "FundingPredictedR":0,          	// 下个周期预测的资金费率
	    "FundingShortR":0               	// 当前字段未使用
        }
    }
```

* 获取交易对的成交记录
```JavaScript
    // 请求
    /**
     * 接口功能: 最多获取到最新的64条记录
     * 参数说明:
     * sym: 交易对名称 
     * 
     * */    
     
    http GET https://market02.gmex.io/v1/rest/GetTrades?sym=BTC1812
    
    // 返回
    {
        "code": 0,                                          // 0成功,其它的为失败
        "data": [           
            {       
                "At": 1544097003149,                        // 时间
                "Dir": -1,                                  // 交易方向 1买 -1卖
                "MatchID": "01CY1DM65TM4558969P8QTZ4DB",    // 撮合ID
                "Prz": 3811,                                // 价格
                "Sym": "BTC1812",                           // 交易对名称
                "Sz": 102,                                  // 成交量,大于0的为买,小于0为卖
                "Val": 0.0267646287                         
            },
            {
                "At": 1544097004951,
                "Dir": -1,
                "MatchID": "01CY1DM65T71BXMMJTS9GASWBJ",
                "Prz": 3812.5,
                "Sym": "BTC1812",
                "Sz": 10,
                "Val": 0.0026229508
            }
        ]
   }
```
* 获取交易对的20档行情
```JavaScript
    // 请求
    /**
     * 接口功能: 获取盘口20档的全部行情
     * 参数说明:
     * sym: 交易对名称 
     * 
     * */    
     
    http GET https://market02.gmex.io/v1/rest/GetOrd20?sym=BTC1812
    
    // 返回
    {
        "code": 0,                          // 0成功,其它为失败
        "data": {
            "Asks": [                       // 卖出的集合
                [   
                    3814.5,                 // 价格
                    2561002                 // 数量
                ],
                [
                    3815.5,
                    148788
                ]
            ],
            "At": 1544097366720,            // 时间
            "Bids": [                       // 买入的集合
                [           
                    3812.5,                 // 价格
                    2724280                 // 数量
                ],
                [
                    3810.5,
                    144027
                ]
            ],
            "Sym": "BTC1812"                // 交易对名称
        }
    }
```

## 交易API示例

* 获取服务器时间Time
```JavaScript
    // 请求
    http GET https://trade02.gmex.io/v1/rest/Time
    
    // 返回
    {
        "code": 0,                  // 0成功,其它则失败
        "data": "", 
        "time": 1544097935826       // 服务器的当前时间        
    }
```

* 获取用户信息GetUserInfo
```JavaScript
    // 请求
    /**
     * 接口功能: 获取用户信息取到UserID,根据UserID可生成AId
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args:空对象
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"req":"GetUserInfo", "username":"tt@gaea.com", "args":{}, "apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw", "expires":1544166435858, "signature":"7166be64f351c68318c835d4eb219cc3"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    //返回
    {
        "code": 0,                              // 0成功,其它则失败
        "data": {
            "UserID": "1020415",                // 用户的UserID
            "UserName": "tt@gaea.com"           // 用户的名称
        }
    }    

```

* 获取钱包信息GetWallets
```JavaScript
    // 请求
    /**
     * 接口功能: 获取用户的钱包信息,钱包分为币币钱包、合约钱包、我的钱包
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约钱包AId=UserID+'01',现货钱包AId=UserID+'02',我的钱包AId=USerID+'03' 例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"req":"GetWallets", "username":"tt@gaea.com", "args":{"AId":"102041501"}, "apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw", "expires":1544166944189, "signature":"ac5fdac94088cb6e79d5ed7cd20eba9e"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    // 返回
    {
        "code": 0,                                          // 0成功,其它则失败
        "data": [
            {
                "AId": "102041501",                         // 账号AId,合约AId=UserID+'01',现货AId=UserID+'02' 
                "Coin": "BTC",                              // 币种的名称 
                "Depo": 10000.1,                            // 入金金额 
                "Frz": 0,                                   // 冻结金额 
                "MI": 0,                                    // 委托保证金
                "MM": 0.0097475728,                         // 仓位保证金
                "PNL": -0.00286613663,                      // 已实现盈亏
                "RD": 0,                                    // 风险度
                "Spot": 0,                                  
                "Status": 2,                                // 账户状态, 1尚未激活 2正常状态 3强平状态 4接管
                "UId": "1020415",                           // 用户UserID
                "UPNL": 0.0097475728,                       // 未实现盈亏
                "WDrw": 0,                                  // 总出金金额
                "Wdrawable": 10000.0971338634
            },
            {
                "AId": "102041501",
                "Coin": "ETH",
                "Depo": 1.0,
                "Frz": 0,
                "MI": 0,
                "MM": 0,
                "PNL": 0,
                "RD": 0,
                "Spot": 0,
                "Status": 2,
                "UId": "1020415",
                "UPNL": 0,
                "WDrw": 0,
                "Wdrawable": 1
            }
        ]
    }
```

* 钱包日志GetWalletsLog
```JavaScript
    // 请求
    /**
     * 接口功能: 获取用户的钱包日志信息,钱包分为币币钱包、合约钱包、我的钱包
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约钱包AId=UserID+'01',现货钱包AId=UserID+'02',我的钱包AId=USerID+'03',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"req":"GetWalletsLog", "username":"tt@gaea.com", "args":{"AId":"102041501"}, "apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw", "expires":1544167142509, "signature":"b094142522364fab85ef82b8f875ca89"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    // 返回
    {
        "code": 0,  // 0成功,其它则失败
        "data": [
            {
                "AId": "102041501",                                    // 账号AId,合约AId=UserID+'01',现货AId=UserID+'02' 
                "At": 1544077973299,                                   // 时间
                "Coin": "BTC",                                         // 币种
                "DirtyFlag": 43,
                "Fee": 0,                                              // 手续费
                "Info": "FeeData :01CXT69HBDGPRFBTH1PWSRG1KH",
                "Op": 3,
                "Qty": -1.298532e-05,                                  // 数量
                "Seq": "01CXT69HBDBRS6WZ009XHBEEPE",
                "Stat": 4,
                "UId": "1020415",
                "Via": 8,                                               // 来源
                "WalBal": 10000.09713386336                             
            },
            {
                "AId": "102041501",
                "At": 1543911109980,
                "Coin": "BTC",
                "DirtyFlag": 43,
                "Fee": 0,
                "Info": "FeeData :Sym(BTC1903) Pos(421.00000000000) PrzI(8347.20737117506) PrzC(7800.00000000000)",
                "Op": 3,
                "Qty": -5.397435e-05,
                "Seq": "01CXT69HBDH99JCEBRE7WJZ53F",
                "Stat": 4,
                "UId": "1020415",
                "Via": 8,
                "WalBal": 10000.09714684869
            }
        ]
    }
```

* 历史订单GetHistOrders
```JavaScript
    // 请求
    /**
     * 接口功能: 获取用户的历史委托
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约AId=UserID+'01',现货AId=UserID+'02',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"req":"GetHistOrders", "username":"tt@gaea.com", "args":{"AId":"102041501"}, "apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw", "expires":1544167420498, "signature":"417377f8c05efdd8d6364dad48f70cb0"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    // 返回
    {
        "code": 0,  // 0成功,其它则失败
        "data": [
            {
                "AId": "102041501",
                "At": 1544077970837,
                "COrdId": "1544077970817",                      // 用户参考的订单编号,在订单回报时会带回来,需要保持唯一性
                "Dir": 1,                                       // 交易方向,1表示买 -1表示卖 
                "ErrTxt": "NOERROR",                            // 错误描述
                "FeeEst": 0,                                    // 
                "Frz": 0,
                "MM": 0,                                        // 仓位保证金
                "OType": 1,                                     // 委托方式,1限价 2市价 3条件限价/止盈止损 4条件市价/止盈止损 
                "OrdId": "01CXT69HBC5RZM9W6EVM31GSJ8",
                "Prz": 3850.5,                                  // 非条件或者非止盈止损时的价格
                "PrzF": 3850.5,                                 // 已成交的平均价格
                "PrzStop": 0,                                   // 已废弃
                "Qty": 500.0,                                   // 交易的量
                "QtyDsp": 0,                                    // 大于0小于委托的量则表示冰山的量
                "QtyF": 500.0,
                "Status": 4,                                    // 委托的状态,1.正在排队2.有效3.提交失败4.全部成交,其它值则代表执行失败,参考ErrCode,ErrTxt
                "StopPrz": 0,                                   // 条件单/止盈止损时会用到,表示价格的值 
                "Sym": "BTC1812",                               // 交易对名称
                "TraceMax": 0,
                "TraceMin": 0,
                "TraceRR": 0,
                "UId": "1020415",
                "UPNLEst": 0,
                "Until": 9223372036854775807,
                "Upd": 1544077973299,
                "Val": 0
            },
            {
                "AId": "102041501",
                "At": 1543912373321,
                "COrdId": "SN7",
                "Dir": 1,
                "ErrTxt": "NOERROR",
                "FeeEst": 0,
                "Frz": 0,
                "MM": 0,
                "OType": 1,
                "OrdId": "01CXT69HBCWCGAQXSPK26G070N",
                "Prz": 5000.0,
                "PrzChg": 10,                               // 成交的档位, 1档 5档 10档 
                "PrzF": 0,
                "PrzStop": 0,
                "Qty": 100.0,
                "QtyDsp": 0,
                "QtyF": 0,
                "Status": 4,
                "StopPrz": 0,
                "Sym": "BTC1833",
                "TraceMax": 0,
                "TraceMin": 0,
                "TraceRR": 0,
                "UId": "1020415",
                "UPNLEst": 0,
                "Until": 9223372036854775807,
                "Upd": 1543980443526,
                "Val": -0.02
            }
        ]
    }
```

* 获取持仓信息GetPositions
```JavaScript
    // 请求
    /**
     * 接口功能: 获取用户的持仓信息
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约AId=UserID+'01',现货AId=UserID+'02',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"req":"GetPositions", "username":"tt@gaea.com", "args":{"AId":"102041501"}, "apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw", "expires":1544167570475, "signature":"ea545fdf01aea172c2bbdce204a9186b"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    // 返回
    {
        "code": 0,  // 0成功,其它则失败
        "data": [
            {
                "ADLIdx": 9.018e-07,
                "AId": "102041501",                     // 账号AId,合约AId=UserID+'01',现货AId=UserID+'02' 
                "FeeEst": 3.60317e-05,                   
                "Lever": 0,                             // 杠杆的值
                "MI": 0,                                // 委托保证金
                "MMnF": 0.0006005285,                   // 保证金，被仓位使用并锁定的保证金
                "MgnISO": 0,
                "PId": "01CXT69HBDZXF36MW2NHPBDG43",    // 持仓ID
                "PrzBr": 0.0500138647,                  
                "PrzIni": 3850.5,                       // 开仓平均价格
                "PrzLiq": 0.0502638591,
                "ROE": 0.0750660581,
                "RPNL": -1.29853e-05,                   // 已实现盈亏 
                "Sym": "BTC1812",
                "Sz": 500.0,                            // 仓位(正数为多仓，负数为空仓) 
                "UId": "1020415",
                "UPNL": 0.0097475728,                   // 未实现盈亏 
                "Val": -0.1298532658
            }
        ]
    }
```

* 获取委托GetOrders
```JavaScript
    // 请求
    /**
     * 接口功能: 获取用户的当前委托
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约AId=UserID+'01',现货AId=UserID+'02',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"req":"GetOrders", "username":"tt@gaea.com", "args":{"AId":"102041501"}, "apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw", "expires":1544167683629, "signature":"a0efd3adad53143b1fb09c06bbc02811"}' | http POST https://trade02.gmex.io/v1/rest/Action
   
    // 返回
    {
        "code": 0,  // 0成功,其它则失败
        "data": [
            {
                "AId": "102041501",
                "At": 1544167207908,
                "COrdId": "1544167207911",
                "Dir": 1,
                "FeeEst": 0,
                "Frz": 0,
                "MM": 0,
                "OType": 1,
                "OrdId": "01CY1X0RRRDP8FWFA3AFCWR76M",
                "Prz": 3414.5,
                "PrzF": 0,
                "PrzStop": 0,
                "Qty": 1000.0,
                "QtyDsp": 0,
                "QtyF": 0,
                "Status": 2,
                "StopPrz": 0,
                "Sym": "BTC1812",
                "TraceMax": 0,
                "TraceMin": 0,
                "TraceRR": 0,
                "UId": "1020415",
                "UPNLEst": 0,
                "Until": 9223372036854775807,
                "Upd": 1544167207908,
                "Val": -0.29286864841
            }
        ]
    }
```

* 委托下单OrderNew
```JavaScript
    // 请求
    /**
     * 接口功能: 委托下单
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约AId=UserID+'01',现货AId=UserID+'02',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     *  Prz: 非条件或者非止盈止损时的价格
     *  Qty: 委托的数量
     *  Dir: 交易的方向,1买 -1卖
     *  DspQty: 冰山的数量,大于0小于委托量则在盘口显示的是冰山的值,等于0则显示实际委托的量
     *  PrzChg: 交的档位, 1档 5档 10档
     *  Otype: 委托的类型,1限价 2市价 3条件限价/止盈止损 4条件市价/止盈止损 
     *  OrdFlag: 标志位,0无 1如果立即成交则取消 2只减仓 4触发后平仓 8如果价格大于等于 16如果价格低于等于
     *  Sym: 交易对名称
     *  Tif: 生效时间,0一直有效 1剩余取消(FillAndKill=FAK) 2全部成交或取消(FilOrKill=FOK) ,
     *  StopPrz: 条件单/止盈止损时会用到,表示价格的值
     *  StopBy: 条件单/止盈止损时会用到,0标记价格 1最新成交价 2指数价格
     *  COrdId: 用户参考的订单编号,在订单回报时会带回来,确保唯一性
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"args":{"AId":"102041501","COrdId":"1544169579283102041501","Dir":1,"OType":1,"OrdFlag":0,"Prz":100,"PrzChg":1,"Qty":3000,"QtyDsp":0,"Sym":"BTC1812","Tif":0},"req":"OrderNew","expires":1544170179229,"signature":"5e6967d6b0715207243be37636d95c45","username":"tt@gaea.com","apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    // 返回
    {
        "code": 0,  // 0成功,其它则失败
        "data": {
            "AId": "102041501",
            "At": 1544169651038,
            "COrdId": "1544169579283102041501",
            "Dir": 1,
            "FeeEst": 0,
            "Frz": 0,
            "MM": 0,
            "OType": 1,
            "OrdId": "01CY1X0RRRK6G77QTMVDFB5FE7",
            "Prz": 100.0,
            "PrzChg": 1,
            "PrzF": 0,
            "PrzStop": 0,
            "Qty": 3000.0,
            "QtyDsp": 0,
            "QtyF": 0,
            "Status": 2,
            "StopPrz": 0,
            "Sym": "BTC1812",
            "TraceMax": 0,
            "TraceMin": 0,
            "TraceRR": 0,
            "UId": "1020415",
            "UPNLEst": 0,
            "Until": 9223372036854775807,
            "Val": -30.0,
            "WId": "102041501BTC"
        }
    }
```

* 撤销委托OrderDel
```JavaScript
    // 请求
    /**
     * 接口功能: 用户撤单操作
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约AId=UserID+'01',现货AId=UserID+'02',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     *  Sym: 交易对名称
     *  OrdId: 需要撤销的OrdId
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"args":{"AId":"102041501","OrdId":"01CY1X0RRRK6G77QTMVDFB5FE7","Sym":"BTC1812"},"req":"OrderDel","expires":1544170922227,"signature":"607c940ca12c7fab55035c95a0e06c83","username":"tt@gaea.com","apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    // 返回
    {
        "code": 0,   // 0成功,其它则失败
        "data": {
            "AId": "102041501",
            "At": 1544170337312,
            "ErrTxt": "NOERROR",
            "FeeEst": 0,
            "Frz": 0,
            "MM": 0,
            "OrdId": "01CY1X0RRRK6G77QTMVDFB5FE7",
            "Prz": 0,
            "PrzF": 0,
            "PrzStop": 0,
            "Qty": 0,
            "QtyDsp": 0,
            "QtyF": 0,
            "Status": 4,
            "StopPrz": 0,
            "Sym": "BTC1812",
            "TraceMax": 0,
            "TraceMin": 0,
            "TraceRR": 0,
            "UId": "1020415",
            "UPNLEst": 0,
            "Until": 9223372036854775807,
            "Val": 0,
            "WId": "102041501BTC"
        }
    }
```

* 调整杠杆PosLeverage
```JavaScript
    // 请求
    /**
     * 接口功能: 调整杠杆
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约AId=UserID+'01',现货AId=UserID+'02',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     *  Sym: 交易对的名称
     *  PId: 持仓Id,暂时可以传null
     *  Param: 杠杆的值
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"args":{"Sym":"BTC1812","AId":"102041501","PId":null,"Param":10},"req":"PosLeverage","expires":1544171021791,"signature":"5f8c60c9d6c572b06fb614465af3cac9","username":"tt@gaea.com","apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    // 返回
    {
        "code": 0,   // 0成功,其它则失败
        "data": {}
    }
```

* 增删资金PosTransMgn
```JavaScript
    // 请求
    /**
     * 接口功能: 逐仓时增加或减少资金
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约AId=UserID+'01',现货AId=UserID+'02',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     *  Sym: 交易对的名称
     *  PId: 持仓Id
     *  Param: 杠杆的值
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"args":{"Sym":"BTC1812","AId":"102041501","PId":"01CXT69HBDZXF36MW2NHPBDG43","Param":500},"req":"PosTransMgn","expires":1544171208181,"signature":"07ebfaf6202dfa1d1f74cbfa5a5ebf83","username":"tt@gaea.com","apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    // 返回
    {
        "code": 0,   // 0成功,其它则失败
        "data": {}
    }    
```

* 超时撤单CancelAllAfter
```JavaScript
    // 请求
    /**
     * 接口功能: 逐仓时增加或减少资金
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网申请的apikey
     * args: {
     *  AId: 账号AId,合约AId=UserID+'01',现货AId=UserID+'02',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     *  Sec: 超时时间
     * }
     * expires: 消息的有效时间
     * signature: 签名,参考签名生成方法
     * */ 
     
    echo '{"args":{"AId":"102041501","Sec":70},"req":"CancelAllAfter","expires":1544171689842,"signature":"b4b9f476392ca8ccffe41a66a045b825","username":"tt@gaea.com","apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw"}' | http POST https://trade02.gmex.io/v1/rest/Action
    
    // 返回
    {
        "code": 0,      // 0成功,其它则失败
        "data": {}
    }
```

* 获取用户风险限额
```js
    // 请求
    /**
     * 接口功能: 查询用户风险限额(内测中,不建议使用)
     * 参数说明:
     * req: Action类型
     * username: 用户名
     * apikey: 用户在官网上申请的apikey
     * args: {
     *  AId: 账号AId,合约AId=UserID+'01',现货AId=UserID+'02',例如UserID为1020415,获取的是合约的信息,AId则为"1020415"+"01"==>"102041501"
     *  Sym: 交易对名称
     * }
     * */
     
    echo '{"args":{"AId":"102041501","Sym":"BTC1812"},"req":"GetRiskLimit","expires":1544965066515,"signature":"12467507854c08c09e75142cef1c0f64","username":"tt@gaea.com","apikey":"bEwAA4NCzhexYsNtnyaYnhbMFQw"}' | http POST https://trade02.gmex.io/v1/rest/Action

    // 返回
    {
    "code": 0,
    "data": {
        "Base": 1000000,        // Base Risk Limit 当 Pos Val < Base 的时候
        "BaseMIR": 0.01,        // Initial Margin  Val < Base 的时候 MIR
        "BaseMMR": 0.005,       // Base Maintenance Margin Val < Base 的时候 MMR 
        "MaxOrdVal": 50000,     // 最大挂单量
        "PosSzMax": 1500000,    // 最大持仓
        "Step": 10,             // Step  StepS = math.Ceil((Val - Base)/Step) 表示递增次数
        "StepIR": 0.005,        // 每次递增的时候，MIR 的增量
        "StepMR": 0.005,        // StepM  每次递增的时候，MMR MIR 的增量
        "Sym": "BTC1812"      // 交易对
    }
}
```


## 委托状态码


| ErrCode| ErrTxt | 描述 |
|:------:|:------|:------|
| 0       |  NORERROR| 没有错误  |
| 1       |  GENERAL | 数据错误 |
| 2       |  DATA    | 数据错误 |
| 3       |  NOT_IMPLEMENTED     | 服务器未实现 |
| 4       |  NO_MARGIN     | 保证金不足 |
| 5       |  FATAL     | 致命错误 |
| 6       |  NOT_FOUND     | 未找到 |
| 7       |  UNKNOWN_DIR     | 未知的委托方向 |
| 8       |  INVALID_CODE     | 操作码错误 |
| 9       |  EXISTS     | 已存在 |
| 10      |  NOT_FOUND_ORD     | 没有找到订单号 |
| 11      |  PRZ_INVALID     | 价格错误 |
| 12      |  EXPIRED     | 已过期 |
| 13      |  NOT_SUFFICIENT     | 资金不足 |
| 14      |  WILLFILL     | 对于PostOnly，本委托会成交 |
| 15      |  EXECUTE_FAIL     | 对FillOrKill委托，这表示执行撮合失败 |
| 16      |  EXCEED_LIMIT_MINVAL     | 超过限制|
| 17      |  VAL_TOO_SMALL     | 委托价值太小 |
| 18      |  EXCEED_LIMIT_PRZ_QTY     | 价格或者数量超出限制 |
| 19      |  DENYOPEN_BY_POS     | 仓位超出限制 |
| 20      |  DENYOPEN_BY_RD     | 禁止开仓 |
| 21      |  TRADE_STOPED     |  交易暂停 |
| 22      |  EXCEED_PRZ_LIQ     | 超过强平价格 |
| 23      |  TOO_MANY_ORDER     | 太多的委托 |
| 24      |  DENYOPEN_BY_TIME     | 超出开仓时间限制 |
| 25      |  MD5_INVALID     | MD5签名验证错误 |
| 26      |  RATELIMIT     | 限速,每秒50次API调用 |
