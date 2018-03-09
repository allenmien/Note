# Alphax推荐系统

- 当用户注册的过程中，可以引导用户选择关心的主题或者人物。然后在改主题或者人物的选择范围内，进行流行度算法的推荐。
  - 物品表（ITEM_META）的category字段，可以设置物品的类别。
  - 可以采用默认推荐，OFFLINE_CATEGORY_DEFAULT_REC输出数据（根据所设置的推荐策略产生的在不同物品类目下的默认推荐物品列表（离线产出））
  - 参数：
    - 默认推荐策略
      - simple_pv：基于简单行为PV的热度排行，对用户行为中的**物品按照全局出现次数**（count(a) group by item_id）进行排序
        -  retain_days：基于**过去N天**的全局用户行为数据进行pv计算，本参数决定了会在用户行为表中扫描**最近多少个分区**的数据 。
      - weighted_uv：基于行为uv加权的热度排行，针对用户行为中的不同行为类型进行加权求和后按照得分进行排序。
        - retain_days：N天
        - weight：有效行为权重。如view:1.0;click:0.8。
        - bucket_random：简单随机分桶的推荐。
        - simple_recent：按照物品表 update_datetime 排序，最新的排在最前面，可以设置时间衰减参数和阈值。
        - weighted_iteminfo：按照物品表的**item_info中的字段**排序，可以设置阈值和**计算公式**。
    - 分类目推荐
    - 推荐结果数