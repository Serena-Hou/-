#  流程概要

  ```
  1. 根据关注的疾病找准基因列表，与患者的数据进行vlookup匹配找出匹配的基因
  2. 在匹配结果里根据重点关注、频率、遗传方式、各评价指标等筛选基因
  3. 选择符合的基因标红并填充黄色
  4. 以OMIM为主要参考数据库查询疾病与基因对应关系及症状
  ```

---

# 筛选流程

## 首先选择医生重点关注的基因与重点关注疾病对应基因，不论任何评价指标，选择完后再进行以下流程

1. 如果是三人的检测，首先筛选出患者的数据

2. 在`hgmd type`列筛选<kbd>DM</kbd>和<kbd>DM?</kbd>的，看`hgmd disease`列该变异关联的疾病是否有与重点关注疾病相关，看是否有可以验证的，如果没有，去掉填充蓝色，便于后续根据颜色筛选

   > 如果是DM的，同时要看一下hgmd change列hgmd所记载的变异是否与患者的变异一致，是否是同样的变异形式和变异为同样的碱基

3. 将含<kbd>>-和-></kbd>的位点标注颜色然后筛选掉

4. `ClinVar_significance`列筛选`Likely pathogenic` 和`pathogenic`的，看是否有可选位点

5. ACMG列筛选`致病`和`疑似致病`的，看是否有可选位点

6. 如果是三人的，将父母变异列纯合、半合子与低质量位点的都筛选掉，留下杂合或未发现变异的

   ```
   PCDH19基因突变造成的癫痫症只会发生于女性，因此又叫做女性限定的癫痫及智力障碍综合征，英文叫做（EPILEPSY, FEMALE-RESTRICTED, WITH MENTAL RETARDATION；EFMR），顾名思义，这是一种同时有癫痫发作及智力障碍的疾病，而且只有女性杂合子受累，男性半合子不受累。“细胞干扰”机制是解释这种特殊遗传方式的主要假说，该假说推测当个体表达两种不同PCDH19蛋白时，细胞与细胞之间正常的相互作用会被干扰，从而发病。男性嵌合体患者的发现似乎进一步证实了该假说。PCDH19基因相关癫痫具有外显率不全和表型异质性特点。
   ```

   >如果是女孩关注癫痫的，患者是PCDH19基因的杂合变异，父亲有可能是半合子，此时父亲的半合子不要筛选掉，需保留

7. 筛选`frequency`列将大于10的筛选掉，留下<kbd>小于等于10和“-”</kbd>的

8. 筛选英文疾病`Disease`列，留下有疾病注释的

9. 筛选掉HGMD注释DP、DFP的

10. 筛选`AF-1`与`AF-2`，留下ASN或者SAS频率小于等于<kbd>0.05</kbd>和没有频率数据的变异

    > 如果最后选择没有合适的位点，可将频率适当提高再筛选

11. 筛选掉`gnomAD_genome_ALL`、`gnomAD_exome_ALL`与`DB_ExAC_All_frequency`列频率大于<kbd>0.1</kbd>的位点

12. 在患者变异方式列筛选半合子或纯合变异，看是否有可以验证的

13. 在患者变异列筛选`纯合`和`半合子`变异位点，看是否可以验证

14. 在预测孟德尔遗传方式列筛选`A_denovo`与`XL_denovo`，看是否有`显性新生`的位点

15. 根据关注的疾病选择基因列表与患者的基因列表进行`vlookup`，筛选出有匹配基因的，如果有多个症状的，同时筛选，可根据主要症状与非主要症状决定符合哪些症状的是基因是必需保留的，哪些是可以选择保留的

    * 明确写了重点关注的，找寻与重点关注相近的疾病并对应基因，**具体方法请查找[GeneReviews](https://www.ncbi.nlm.nih.gov/books/NBK1116/)该疾病下鉴别诊断`Differential Diagnosis`部分内容**

    - 只写了临床症状的，根据症状找出对应的基因列表**需学会区分主要症状/次要症状/并发症状**

16. 在患者变异列筛选杂合，看是否有`隐性复合杂合`的位点

17. 如果没有家族史，筛选`incomplete penetrance`列看是否有显性外显不全的位点变异，**如果患者有家族史，明显可能是常显的情况，需在筛选时关注患者和患病家属杂合，未患病患者未发现变异即符合家系共分离的变异位点**

    ```
    常见的家族性疾病基因有PRRT2,NF1,NF2,SOD1,PKD1
    ```

18. 选择的基因确保与患者的`发病年龄`符合
    ```
    Fetal胎儿/Embryonal胚胎：妊娠8周-出生前
    Neonate新生儿：0-28天
    Infant婴儿：0-1岁
    Toddler幼儿：1-3岁
    Child儿童：2-10岁
    Juvenile青少年：5-15岁
    Young adult青年：16-40岁
    Adult成年：16岁及以上
    Middle age中年：40-60岁
    Late onset老年发病：60岁及以上
    ```

19. 可疑位点需查询OMIM症状看是否包含患者的临床症状，**查询时不只看临床症状表格，还需看下方全文部分**

    > OMIM症状总结表格很随意，有些症状在既往患者中出现但不一定会总结到表格里，只在文献内容总结列表里提到；搜索时先阅读首页下方的OMIM Search Help会帮助搜索
    >

20. 经常有大片段变异的基因要关注`CNV_Result`和`KSCNV_Result`，看是否有显性大片段变异和隐性大片段变异可与点变异组成复合杂合的情况，特别可疑的选做验证

## 筛选要点

1. 所选的位点尽可能为外显子，有明确的<kbd>c.与p.</kbd>

2. 优先选择<span style='color:red'>基因LOF</span>的变异,包括`起始密码子变异、无义变异、移码变异、剪接位点变异、外显子缺失`，即`PVS1证据`致病可能较大

3. 选择depth数值高、frequence越接近<kbd>50/50</kbd>的位点

   > frequence越偏离50/50，例如80/20、70/30、90/10，数据越有可能是不一致位点，depth一般100以上

4. 尽量不选择无明确关联疾病的基因。**有的基因对应疾病有英文名称，没有中文翻译，注意不要略过**

5. 有些基因经常会报出很多位点，如非特殊新生或复合杂合，尽量不选这些位点

   ```
   例如PSPH,HERC2,FLG,TTN,FANCD2,DSPP,KMT2C,MUC5B等
   ```

6. 有些具有动态突变基因的会有正常范围内的三核苷酸重复数增加或减少呈现出缺失或者插入`CAGCAG`形式的变异，不选择这类位点

   ```
   例如ATXN1,CACNA1A,HTT,DMPK,RAI1,SMARCA2,HECR2,FLG等
   ```

7. 如果过滤数据中找不到重点关注基因，再看未过滤数据做最终确认

   > 须是重点客户的关注的特别明确的基因没有找到变异再看未过滤数据

8. 尽量不选择splicing（除+1，-1特殊位置外）、UTR（除特殊基因外）和intron的位点，除非是写明重点关注的基因或UTR区致病的特殊基因

   ```
   UTR区致病的基因包括SOX9,AR,SOX3,SHH,ATOH7,ANKRD26,AXIN1,DIP2B,PITX1,PTF1A,ZNF713,HCFC1,PDE6H,IFITM5,EIF4A3,POMP,DHRS3,PIGM,ENG等
   ```

9. 如果写明的症状或疾病没有可选的基因列表，通过[诊断工具](http://192.168.100.120:54321/)、[OMIM](http://www.omim.org/)、[HPO](http://compbio.charite.de/hpoweb/showterm?id=HP:0000246#id=HP_0000246)、[FindZebra](http://www.findzebra.com/)、[Phenomizer](http://compbio.charite.de/phenomizer/)、[罕见病辅助诊断系统](https://www.genomcan.cn/#/)、[Genotype Phenotype Search](https://www.diseasegps.org/)等工具查询到基因列表与患者基因列表做对比。

10. 患者数据里的外显率可能不全，如果某个位点很符合患者的症状，可输入**`基因+penetrance`**在谷歌里搜索

    > 例如<kbd>SCN1A penetrance</kbd>，或在基因或疾病界面搜索`penetrance（外显率）`

11. 尽量不选择易感基因位点

12. 熟知特殊基因的特殊致病形式：
    ```
    1. 印记基因导致的疾病，需做MLPA才可检测出，二代测序无法检测：
         6q24.2	     Transient neonatal diabetes mellitus, 6q24 related
         chr7	     Russell-Silver syndrome
         11p15.5	 Russell-Silver syndrome
         11p15.5	 Beckwith-Wiedemann syndrome
         11p15.5	 Isolated hemihyperplasia (OMIM 235000)
         11p15.5	 Isolated Wilms tumor (see Wilms Tumor Predisposition)
         14q32	     Temple syndrome (OMIM 616222)
         14q32	     Kagami-Ogata syndrome (OMIM 608149)
         15q11.2-q13 Prader-Willi syndrome
         15q11.2-q13 Angelman syndrome
         20q13	     Pseudohypoparathyroidism 1B (OMIM 603233)
         chr20	     UPD(20)mat 6
    2. 动态突变导致的疾病
         AFF2	     Fragile X syndrome, FRAXE type (OMIM 309548)
         AR	         Spinal bulbar muscular atrophy
         ARX	     Partington syndrome (OMIM 309510)
         ATN1	     Dentatorubral-pallidoluysian atrophy
         ATXN1	     Spinocerebellar ataxia type 1
         ATXN2	     Spinocerebellar ataxia type 2
         ATXN2	     L-dopa responsive parkinsonism ALS type 13 (OMIM 183090)
         ATXN3	     Spinocerebellar ataxia type 3
         ATXN7	     Spinocerebellar ataxia type 7
         ATXN8OS     Spinocerebellar ataxia type 8	
         ATXN8	     Spinocerebellar ataxia type 8
         ATXN10	     Spinocerebellar ataxia type 10
         BEAN1	     Spinocerebellar ataxia type 31 (OMIM 117210)
         C9orf72     Frontotemporal dementia, ALS type 1, hereditary ataxia	
         CACNA1A     Spinocerebellar ataxia type 6
         CD40LG	     X-linked hyper IgM syndrome
         CNBP	     Myotonic dystrophy type 2
         COMP	     Pseudo-achondroplasia
         CSTB	     Unverricht-Lundborg disease
         DIP2B	     Mental retardation type 12A (OMIM 136630)
         DMPK	     Myotonic dystrophy type 1
         EIF4A3	     Richieri Costa Pereira syndrome (OMIM 268305)
         FMR1	     Fragile X syndrome, fragile X-associated tremor/ataxia syndrome
         FOXL2	     Blepharophimosis, ptosis, epicanthus inversus
         FXN	     Friedreich ataxia
         HOXA13	     Hand-foot-genital syndrome
         HOXD13	     Polysyndactyly，Syndactyly type V (OMIM 186300)
         HTT	     Huntington disease
         IL11RA	     Craniosynostosis and dental anomalies (OMIM 614188)	
         JPH3	     Huntington disease-like 2
         MUC1	     Medullary cystic kidney disease
         MYH7	     Laing distal myopathy
         NOP56	     Spinocerebellar ataxia type 36
         PABPN1	     Oculopharyngeal muscular dystrophy
         PAX2	     Renal coloboma syndrome
         PHOX2B	     Congenital central hypoventilation syndrome
         PPP2R2B     Spinocerebellar ataxia type 12
         RUNX2	     Cleidocranial dysplasia
         SOX3	     Mental retardation, X-linked, with isolated growth hormone deficiency 
         TBP	     Spinocerebellar ataxia type 17
         TBX1	     Tetralogy of Fallot (OMIM 602054)
         TCF4	     Fuchs endothelial corneal dystrophy (OMIM 613267)
         ZIC2	     Holoprosencephaly type 5 (see Holoprosencephaly Overview)
         ZIC3	     VACTERL (OMIM 300265)
         RAPGEF2     Epilepsy, familial adult myoclonic, 7 
         TNRC6A      Epilepsy, familial adult myoclonic, 6
         SAMD12      Epilepsy, familial adult myoclonic, 1 
    3. 特殊基因变异导致的疾病
       - 面肩肱肌营养不良1型/Facioscapulohumeral muscular dystrophy 1/FSHD1，需用特殊检测方法检测，
         一代与二代均做不了
       - 色素失禁症受假基因与主要由4-10号外显子缺失影响需用一代长PCR做，二代与MLPA均不能区分真假基因
       - CYP21A2受假基因影响需用MLPA与一代结合做
       - 血友病甲由于F8基因有可能倒位致病，二代只能检测位点变异
       - 地中海贫血变异复杂，需用专门试剂盒做
       - 脊髓性肌萎缩即SMA只能选择用MLPA与单基因做
       - 染色体11q12重复导致的脊髓小脑共济失调20型
    ```

## 注意事项

1. **禁止选择父母三人都是杂合的位点**
2. **除非特殊情况，禁止选择类似`c.2286-4T>-`和`c.1688-9->G`包含<kbd>>-/-></kbd>的位点，为poly结构影响导致测序报出的变异，无法验证出**
3. **除耳聋等需选择频率很高的致病位点外，避免选择频率很高的SNP**
4. **有的基因在UTR区突变会导致疾病，注意对特定基因不要过滤掉UTR区变异，见基因列表**
5. **有的基因同义变异会致病，注意关注HGMD注释，具体同义变异见整理的`同义突变`表格**
6. **如果需要评价位点致病性的强弱，可根据ACMG评分指标，二代数据给出的评价因没有查询文献所以不准确,详见[ACMG指南中文版](http://acmg.cbgc.org.cn/doku.php?id=start)**
7. **同义变异即使在剪切位置+1、-1的位置也可能会影响剪切导致疾病**
8. **没有见过的疾病要充分查阅相关的一切信息，确保考虑到一切信息，没有遗漏特殊情况**

---

# 各种类型疾病对应基因列表及选点时需注意内容

## 癫痫

1. 高热惊厥-基因列表
2. 癫痫-基因列表
3. 婴儿痉挛-基因列表
4. 局灶癫痫-基因列表

## 发育迟缓相关疾病含有基因列表

| 发育倒退            | 发育迟缓         |
| :------------------ | ---------------- |
| 智力障碍            | 脑畸形           |
| 喉软骨软化/发育不全 | 皮埃尔罗宾序列征 |
| 脑瘫                | 自闭症           |
| 钙化                | 喂养困难         |
| 多动                |                  |

## 肌肉病

1. 坏死性肌病-基因列表
2. 轴空病-基因列表
3. 肌病-基因列表
4. 肌肉异常-基因列表
5. 中央核性肌病-基因列表
6. 肌营养不良-基因列表
7. 坏死性肌病-基因列表
8. 脂质沉积性肌病-基因列表
9. 肌无力-基因列表
10. 肌电图异常-基因列表
## 代谢相关疾病含有基因列表

   ```
- 血乳酸的正常值为1.0士0.5mmol／L，但在危重病人，<2mmol／L均可视为正常
- 血氨正常值18-72μmol/L
- 血钾：3.5〜5.6mmol/L，血钠：137〜148mmol/L，血氯：98〜110mmol/L，
  血钙：2.10〜2.55mmol/L，血磷：1.45〜1.78mmol/L。
   ```
| 尿素循环障碍 | C5-OH升高      |
| ------------ | -------------- |
| 羧酸代谢异常 | 胆固醇代谢异常 |
| 血脂代谢异常 | 高血脂         |
| 代谢异常     | 低磷血症       |
| 低钙血症尿症 | 有机酸尿血症   |
| 糖尿病       | 脂肪代谢异常   |
| 肌酸代谢异常 | 肉碱异常       |
| 低血糖       |                |
|              |                |

## 消化

   ```
- 总胆红素（TBIL）正常值1.71～21μmol/L（0.1mg/dl～1.0mg/dl），
  直接/结合/（DBIL）胆红素正常值0～7.32μmol/L （0～0.2mg/dl），
  间接/非结合（IBIL）胆红素正常值0～13.68μmol/L（0～0.8mg/dl）
- 谷丙转氨酶（谷氨酸-丙酮酸转氨酶 ，ALT/GPT）的正常值范围是0-40U/L，
  谷草转氨酶（天门冬氨酸氨基转移酶，AST/GOT）的正常值范围是5-35U/L
- 黄疸有生理性和病理性之分。生理性黄疸是指单纯因胆红素代谢特点引起的暂时性黄疸，在出生后2～3天出现，
  4～6天达到高峰，7～10天消退，早产儿持续时间较长，除有轻微食欲不振外，无其他临床症状。若生后24小时即出现黄疸，
  每日血清胆红素升高超过5mg/dl或每小时>0.5mg/dl；持续时间长，足月儿>2周，早产儿>4周仍不退，
  甚至继续加深加重或消退后重复出现，或生后一周至数周内才开始出现黄疸，均为病理性黄疸。
   ```
1. 肝脏异常-基因列表
2. 黄疸-基因列表
3. 腹胀-基因列表
4. 疝气-基因列表
5. 胆汁淤积-基因列表

## 心血管

   ```
卵圆孔一般在生后第1年闭合，若大于3岁的幼儿卵圆孔仍不闭合称卵圆孔未闭，成年人中有20%～25%的卵圆孔不完全闭合
   ```
1. 先心病/心血管系统形态异常-基因列表
2. 心悸-基因列表
3. 心律失常-基因列表
4. 马凡综合征-基因列表

## 肾病

```
尿蛋白检测：
正常尿液里不含或只含微量蛋白质，检查结果呈阴性（－），当尿液里的蛋白质含量上升到一定量是检查结果就呈阳性（+）。尿蛋白2+（++、两个加号）是做24小时尿蛋白定量检测的结果。同时还可以表述为尿蛋白两个加号、尿蛋白+2，几种表述都是一个意思。尿蛋白两个加号的意思是1L尿液里含有蛋白质1.0～2.0g，即1.0～2.0g/24H。尿蛋白1+和3+的意思分别又是尿蛋白为0.2—1.0g/24H：+、尿蛋白为2.0—4.0g/24H：+++。尿蛋白四个加号：尿蛋白>4．0g/24H：++++；阴性或中性的情况：尿蛋白<0．1g/24H：－； 尿蛋白为0．1—0．2g/24H：±。
```

1. 肾脏异常-基因列表
2. Alport综合征-基因列表
3. 水肿浮肿-基因列表
4. 佝偻病-基因列表
5. 淀粉样变性-基因列表

## 线粒体病

1. 线粒体核基因-基因列表

## 耳聋

1. 耳聋有`双基因`致病形式，如GJB2与GJB3

```
- 有的致病位点在人群中的携带率很高，选择时不要因为频率高就过滤掉，尤其先天性感音神经性耳聋。
- 以下为高频率致病位点：
  GJB2:rs72474224,NM_004004.5:c.109G>A(p.Val37Ile),Chr13:20763612,
  1000 Genomes Project=0.01538(T)(EAS=0.0734),gnomAD(east asian)=0.08345
```

2. 听力障碍-基因列表

## 血液与免疫

```
ANKRD26基因突变导致AD遗传的血小板减少症2型，目前已知的几个致病变异都位于该基因的 5'UTR 的保守位点，研究表明这几个变异造成ANKRD26表达升高，从而造成疾病。
```

1. 免疫缺陷-基因列表
2. 贫血-基因列表
3. 血液及免疫缺陷-基因列表
4. 类风湿关节炎-基因列表
5. 缺铁性贫血-基因列表

## 眼病

1. 蓝巩膜-基因列表
2. 视力下降-基因列表
3. 黄斑变性-基因列表
4. 黄斑异常-基因列表

## 面部外观与皮肤异常

| 白化病       | 牛奶咖啡斑     |
| ------------ | -------------- |
| 小头畸形     | 唇腭裂         |
| 面部异常     | 面部不对称     |
| 红皮病       | 外胚层发育不良 |
| 鱼鳞病       | 黑棘皮         |
| 头发异常     | 牙齿异常       |
| 招风耳       | 外耳异常       |
| 通贯掌或断掌 | 鼻梁低         |
| 毛发颜色异常 |                |
|              |                |

## ALS+CMT+HSP+SCA

```
- 腱反射含义:
  - 0 反射消失；
  - + 反射减退：肌肉收缩存在，但无相应关节活动；
  - ++ 反射正常：肌肉收缩并导致关节活动；
  - +++ 反射增强：可为正常或病理状况；
  - +++ 反射亢进并伴有阵挛：为病理状况。
```

1. 肌张力障碍-基因列表
2. 肌萎缩侧索硬化-基因列表
3. 痉挛性截瘫-基因列表
4. 腓骨肌萎缩-基因列表
5. 共济失调-基因列表
6. 周围神经病-基因列表

## 内分泌与性腺

```
醛固酮增多症（OMIM:103900）：是由 CYP11B1/CYP11B2基因融合造成。CYP11B1和CYP11B2位于8号染色体上，二者呈串联排列，序列上有95%的相似度，因姐妹染色体间的非平衡交叉而产生CYP11B1/CYP11B2融合基因，融合的基因具有CYP11B1的5'端和CYP11B2的编码区
```

1. 矮小-基因列表
2. 卵子/泡成熟障碍-基因列表
3. 性反转-基因列表
4. 小阴茎/尿道下裂-基因列表
5. 男性生殖器异常-基因列表
6. 女性生殖器异常-基因列表
7. 隐睾-基因列表
8. 假性甲状旁腺功能减退症-基因列表
9. 流产-基因列表
10. 阴蒂肥大-基因列表
## 脑白质病

1. 神经和脑白质脱髓鞘-基因列表
2. 脑白质异常-基因列表
## 骨

1. 成骨不全-基因列表
2. 佝偻病-基因列表
3. 尤文肉瘤-基因列表
4. 多指-基因列表

## 呼吸

```
纤毛运动障碍有双基因致病形式，CCDC40与CCDC39
```

| 肺间质纤维化 | 肺炎       |
| ------------ | ---------- |
| 发热         | 肺部异常   |
| 口臭         | 鼻窦炎     |
| 咯血         | 反复肺出血 |
| 肺纤维化     | 肺泡浸润   |

​	

***

# 常用数据库列表

1. 综合与基础知识类

   OMIM：https://www.omim.org/

   GeneReviews：https://www.ncbi.nlm.nih.gov/books/NBK1116/

   中文版GeneReviews：https://genereviews.nrdrs.org.cn/paper/index

   Orphanet：https://www.orpha.net/consor/cgi-bin/index.php

   NORD：https://rarediseases.org/for-patients-and-families/information-resources/rare-disease-information/

   HPO：https://hpo.jax.org/app/

   CHPO：http://www.chinahpo.org/

   MitoMap：https://www.mitomap.org/MITOMAP

   GeneCards：https://www.genecards.org/

   ​	

2. 症状匹配数据库

   诊断助手：http://192.168.100.120:54321/

   前诊断助手：http://120.27.13.182:8778/

   罕见病辅助诊断系统：https://www.genomcan.cn/#/

   Phenolyzer：http://phenolyzer.wglab.org/#getstart

   Phenomizer：http://compbio.charite.de/phenomizer/

   Genotype Phenotype Search：http://compbio.charite.de/phenomizer/

   FindZebra：http://www.findzebra.com/

   OMIM：https://www.omim.org/

   Decipher：https://decipher.sanger.ac.uk/phenogram

   Monarch Initiative：https://monarchinitiative.org/

   ​			

3. 蛋白预测网站：

   PolyPhen-2：http://genetics.bwh.harvard.edu/pph2/

   SIFT：https://sift.bii.a-star.edu.sg/

   MutationTaster：http://www.mutationtaster.org/

   ​	

4. 查询序列网站：

   Ensembl：http://grch37.ensembl.org/Homo_sapiens/Info/Index

   NCBI：https://www.ncbi.nlm.nih.gov/

   UCSC：https://genome.ucsc.edu/

   ​	

5. 查询变异网站：

   康旭HGMD查询-基因：http://192.168.1.200/hgmd/

   康旭HGMD查询-疾病：http://192.168.1.200/hgmd/disease.php

   ClinVar：https://www.ncbi.nlm.nih.gov/clinvar/

   LOVD：http://www.lovd.nl/3.0/home

   Genomic Variants：http://dgv.tcag.ca/dgv/app/home

   Decipher：https://decipher.sanger.ac.uk/disorders#syndromes/overview

   

6. 查询变异频率网站：

   1000 Genomes Browser：https://www.ncbi.nlm.nih.gov/variation/tools/1000genomes/

   ExAC Browser：http://exac.broadinstitute.org/

   GnomAD：https://gnomad.broadinstitute.org/

   

7. 文献下载网站：

   Sci-Hub：https://sci-hub.se/

   百度学术：https://xueshu.baidu.com/

   谷歌学术：https://scholar.google.com.hk/?hl=zh-CN

   全国图书馆参考咨询联盟：http://www.ucdrs.superlib.net/

   PubMed：https://www.ncbi.nlm.nih.gov/pubmed/

   

8. 其他数据库：

   GTR：https://www.ncbi.nlm.nih.gov/gtr/all/?term

   > 国外机构基因检测信息汇总

   SFARI：https://gene.sfari.org/news/

   > 自闭症基因数据库

   SCN1A variants：http://www.molgen.ua.ac.be/SCN1AMutations/Mutations/Default.cfm

   > SCN1A基因变异数据库

   Mutalyzer：https://www.mutalyzer.nl/

   > 位点位置转换数据库

   Domino：https://wwwfbm.unil.ch/domino/index.html

   > 基因显隐性评分	
