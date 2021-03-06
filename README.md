<!-- markdownlint-disable MD029 -->

# TreeNewBee

这是个吹牛逼的项目,anyway，就是这个意思

谨以此项目名称纪念各种对我们吹牛逼但是没有实现的团队和个人，不仅限于搬砖。非要逼我们自己来做，ok，let's do it.

## 使用说明和步骤

1. 准备工作

   ```shell
   1. git clone https://github.com/JCCDex/TreeNewBee.git

   2. cd src

   3. npm i

   4. cd strategy

   5. config.js填写相关配置
   ```

2. 套利交易

   理论上基于[arbitrage](https://github.com/JCCDex/TreeNewBee/blob/master/src/strategy/factory/arbitrage.js)工厂函数, 可以构造任意两两交易所之间的套利行为, 但是目前除了`okex-weidex`和`huobi-weidex`, 其他处于未测试状态.

   ```shell
   # okex和weidex之间搬砖
   # 执行周期: 30s, 默认周期
   node okex_arbitrage.js

   # 执行周期: 60s
   node okex_arbitrage.js -p 60

   # 指定配置文件
   path=""
   node okex_arbitrage.js -p 60 -f $path

   # huobi和weidex之间搬砖
   # 执行周期: 30s, 默认周期
   node huobi_arbitrage.js

   # 执行周期: 60s
   node huobi_arbitrage.js -p 60

   # 指定配置文件
   path=""
   node huobi_arbitrage.js -p 60 -f $path
   ```

3. 网格交易

   ```shell
   # 以xrp/usdt为例

   # 交易对
   pair="xrp/usdt"
   # 数量上限
   amountCeiling=100
   # 数量下限
   amountFloor=10
   # 价格上限
   priceCeiling=0.15
   # 价格下限
   priceFloor=0.1
   # 挂单数量
   tradingQuantity=10
   # 买单
   type="buy"
   # 卖单
   # type="sell"
   # 配置文件(可选)
   path=""

   # huobi
   node huobi_grid_trading.js -p $pair -H $amountCeiling -L $amountFloor -h $priceCeiling
   -l $priceFloor -q $tradingQuantity -t $type -f $path

   # okex
   node okex_grid_trading.js -p $pair -H $amountCeiling -L $amountFloor -h $priceCeiling
   -l $priceFloor -q $tradingQuantity -t $type -f $path

   # 网格自动交易，成交后按照指定收益率自动挂单

   # 卖出收益率(5%)
   sellProfit=0.05

   # 买进收益率(5%)
   buyProfit=0.05

   # 轮询周期(默认 60\*1000ms, 单位 ms)
   timer=60*1000

   # huobi
   node huobi_auto_grid_trading.js -p $pair -H $amountCeiling -L $amountFloor -h $priceCeiling
   -l $priceFloor -q $tradingQuantity -t $type -f $path -SP $sellProfit -BP $buyProfit -T $timer

   # weidex
   node weidex_auto_grid_trading.js -p $pair -H $amountCeiling -L $amountFloor -h $priceCeiling
   -l $priceFloor -q $tradingQuantity -t $type -f $path -SP $sellProfit -BP $buyProfit -T $timer

   # okex
   node okex_auto_grid_trading.js -p $pair -H $amountCeiling -L $amountFloor -h $priceCeiling
   -l $priceFloor -q $tradingQuantity -t $type -f $path -SP $sellProfit -BP $buyProfit -T $timer

   ```

4. 映射

   ```shell
   # 缩放倍数
   scaling=100
   # 每笔最大数量
   amountLimit=10
   # 执行周期, 60s
   period=60
   # 配置文件(可选)
   path=""

   # huobi->weidex
   node huobi2weidex.js -s $scaling -l $amountLimit -p $period -f $path
   # 若映射前取消所有挂单
   node huobi2weidex.js -s $scaling -l $amountLimit -p $period -f $path -c

   # okex->weidex
   node okex2weidex.js -s $scaling -l $amountLimit -p $period -f $path
    # 若映射前取消所有挂单
   node okex2weidex.js -s $scaling -l $amountLimit -p $period -f $path -c
   ```
