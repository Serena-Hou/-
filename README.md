# 流程概要
1. 根据关注的疾病找准基因列表，与患者的数据进行vlookup匹配找出匹配的基因
2. 在匹配结果里根据频率、遗传方式等筛选基因
3. 选择符合的基因标红

# 筛选顺序
1. 根据关注的疾病选择基因列表vlookup，筛选出有匹配基因的
2. 如果是三人的检测，首先筛选出患者的数据
3. 如果是三人的，将父母变异列纯合与半合子的都筛选掉，留下杂合或未发现变异或低质量位点的
4. 筛选frequency列将大于10的筛选掉，留下小于等于10或“-”的
5. 筛选AF-1与AF-2，留下ASN或者SAS频率小于等于0.05的变异
6. 在患者变异方式列筛选半合子或纯合变异，看是否有可以验证的
7. 在hgmg type列筛选DM和DM?的，看是否有可以验证的
8. 在患者变异列筛选杂合的，根据疾病和遗传方式选择合适的位点**显性新生和隐性复合杂合**

# 筛选要点
