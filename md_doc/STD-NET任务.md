## 注意事项：

1. @Bad boy @尛辉 师弟，都注意一下，每次参数一定要做记录，输出到log文件中也好还是固定一份参数脚本文件也好，不然的话自己写错了也没办法知道原因的



|      数据集       | SRNET                 | SRNETC64  |      | STD-NET                                              |
| :---------------: | --------------------- | --------- | ---- | ---------------------------------------------------- |
|    bb_QF90JU04    | ing                   | DONE 0.88 |      | DONE 0.87<br />cylinder 等待<br />ladder 等待        |
|    bb_MiPOD02     | ing<br />有收敛迹象   | NO        |      | ing<br />cylinder没有收敛迹象<br />ladder 有收敛迹象 |
|    bb_MiPOD04     | DONE 0.85             | NO        |      | ing<br />cylinder有收敛迹象<br />ladder 没有收敛迹象 |
| alaska256_MiPOD02 | ing<br />没有收敛迹象 |           |      |                                                      |
| alaska256_MiPOD04 | ing<br />有收敛迹象   |           |      |                                                      |
|                   |                       |           |      |                                                      |
|                   |                       |           |      |                                                      |



数据集

## bb：SRNET，STD-NET

`/data1/lily/datasets/BOSS_BOWS2/bb_256` pgm

`/data1/lily/datasets/BOSS_BOWS2/bb_256_MiPOD02` pgm

`/data1/lily/datasets/BOSS_BOWS2/bb_256_MiPOD04` pgm

MiPOD只用训练srnet

1. ### MiPOD02

   ~~数据集出问题，重新生成中。~~

   ~~训练srnetc64 显卡5   valid_acc: 0.5~~保存模型，随后finetune

   ~~训练srnet~~

   ```
   python 2_test.py --work_name '2_test_srnet_462000' --load_path '/data1/lily/STD-NET-official/bb_MIPOD02_log/20220201-1_train_val_srnet/models/model_462000.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/'
   ```

   > Model:462000, test_acc: 0.72739995, test_loss: 5.5221157, auc: 0.81026016, weight_auc: 0.8606818285714286, P_MD: 0.5691999999999999 , time:20220204-131717

   

   训练STD-NET cylinder  没有收敛，重新训练, 依旧没有收敛。从04cylinder微调后收敛

   ```
   python 7_train_from_scratch_decom.py --log_root '../../bb_MiPOD02_log/' --work_name '7_train_from_scratch_cylinder_second' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```

   ```
   python 6_finetune_decom_srnet.py --log_root '../../bb_MiPOD02_log/' --work_name '6_finetune_decom_04cylinder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/bb_MiPOD04_log/20220202-003725-7_train_from_scratch_cylinder/models/model_491750.ckpt' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```

   ```
   python 2_test.py --work_name '2_test_cylinder_finetune' --load_path '/data1/lily/STD-NET-official/bb_MiPOD02_log/20220205-110026-6_finetune_decom_04cylinder/models/model_167125.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```

   > Model:167125, test_acc: 0.72660017, test_loss: 0.5322985, auc: 0.82320368, weight_auc: 0.8713589714285714, P_MD: 0.5779999999999998 , time:20220208-173834

   训练STD-NET ladder 

   ```
   python 7_train_from_scratch_decom.py --log_root '../../bb_MiPOD04_log/' --work_name '7_train_from_scratch_cylinder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   ```
   python 2_test.py --work_name '2_test_scratch_ladder' --load_path '/data1/lily/STD-NET-official/bb_MiPOD02_log/20220202-003329-7_train_from_scratch_ladder/models/model_492625.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   > Model:492625, test_acc: 0.7183999, test_loss: 0.54551876, auc: 0.8214792000000001, weight_auc: 0.8702993142857144, P_MD: 0.5739999999999998 , time:20220205-112258

2. ### MiPOD04:

   ~~训练出srnet和srnetc64~~

   使用srnet 训练2_test.py

   ```
   python 2_test.py --work_name '2_test_srnet_412125' --load_path '/data1/lily/STD-NET-official/log_dir/20220117-1_train_val_srnet_bb_MiPOD04/models/model_412125.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/'
   ```

   > Model:412125, test_acc: 0.85200024, test_loss: 0.42840487, auc: 0.94178224, weight_auc: 0.9583725714285716, P_MD: 0.2792 , time:20220204-130544

   ~~3_inspect_ma.py 显卡：2  ing~~  

   ~~需要增加1000-2000的，显卡6 ing~~

   ~~从中挑选合适的数据~~

   迁移到02 使用15w 使用那组参数

   再使用50w  flag为True 仿照6脚本

   使用提供的两个配置文件 训练 7脚本

   如果02不收敛 迁移学习

   

   训练STD-NET cylinder 

   ```
   python 7_train_from_scratch_decom.py --log_root '../../bb_MiPOD04_log/' --work_name '7_train_from_scratch_cylinder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```

   ```
   python 2_test.py --work_name '2_test_scratch_cylinder' --load_path '/data1/lily/STD-NET-official/bb_MiPOD04_log/20220202-003725-7_train_from_scratch_cylinder/models/model_491750.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```

   > Model:491750, test_acc: 0.82759994, test_loss: 0.36368003, auc: 0.9279096800000001, weight_auc: 0.9484083999999998, P_MD: 0.3475999999999998 , time:20220205-114602

   训练STD-NET ladder 等待显卡 没有收敛，重新训练 。没有收敛 进行微调。

   ```
   python 7_train_from_scratch_decom.py --log_root '../../bb_MiPOD04_log/' --work_name '7_train_from_scratch_ladder_second' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   ```
   python 6_finetune_decom_srnet.py --log_root '../../bb_MiPOD04_log/' --work_name '6_finetune_decom_02ladder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/bb_MiPOD02_log/20220202-003329-7_train_from_scratch_ladder/models/model_492625.ckpt' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   ```
   python 2_test.py --work_name '2_test_ladder_finetune' --load_path '/data1/lily/STD-NET-official/bb_MiPOD04_log/20220205-111222-6_finetune_decom_02ladder/models/model_130375.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   > Model:130375, test_acc: 0.8316001, test_loss: 0.3720196, auc: 0.9308385599999999, weight_auc: 0.9505619428571428, P_MD: 0.3304 , time:20220208-174706

## bb_QF90_JU04：SRNETC64分解实验

` /data1/hulh/BOSS_BOWS2/bb_256_qf90` mat

` /data1/hulh/BOSS_BOWS2/bb_256_qf90_juniward04` mat

1. ~~训练出srnetc64~~ 

2. ```
   python 2_test.py --work_name '2_test_491750' --load_path '/data1/lily/STD-NET-official/log_dir/20220120-1_train_val_srnetc64_BBQF90JU04/models/model_491750.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/'
   ```
   
   > Model:491750, test_acc: 0.88379997, test_loss: 0.3730901, auc: 0.96056488, weight_auc: 0.9717946285714286, P_MD: 0.20399999999999974 , time:20220204-133952
   
3. ~~3_inspect_ma.py 显卡：4  ing~~
   ~~需要增加1000-2000的，显卡0 ing~~

4. ~~export_table_errorbar.py 生成excel文件~~

5. ~~从中挑选出合适的数据~~

6. ~~5_decom.py 执行结果为分解后的模型~~

   ```
   python 5_decom.py --log_root '../../bb_QF90JU04_log/' --work_name '5_decom_v1' --data_path '/data1/hulh/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/log_dir/20220120-1_train_val_srnetc64_BBQF90JU04/models/model_491750.ckpt' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v1.cfg'
   ```

   

   ```
   python 5_decom.py --log_root '../../bb_QF90JU04_log/' --work_name '5_decom_v3' --data_path '/data1/hulh/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/log_dir/20220120-1_train_val_srnetc64_BBQF90JU04/models/model_491750.ckpt' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v3.cfg'
   ```

   

7. 6_finetune_decom_srnet.py  （15w参数）

   ```
   python 6_finetune_decom_srnet.py --log_root '../../bb_QF90JU04_log/' --work_name '6_finetune_decom_srnet' --data_path '/data1/hulh/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220201-155230-5_decom/model_decom_finetune.ckpt' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v1.cfg'
   
   python 2_test.py --work_name '2_test_final_126875' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220201-180316-6_finetune_decom_srnet/models/model_126875.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v1.cfg'
   
   Model:126875, test_acc: 0.8739999, test_loss: 0.3520079, auc: 0.95720752, weight_auc: 0.9694174857142859, P_MD: 0.21799999999999986 , time:20220204-125006
   ```

   

8. 6_finetune_decom_srnet.py  （25w参数）

   ```
   python 6_finetune_decom_srnet.py --log_root '../../bb_QF90JU04_log/' --work_name '6_finetune_decom_srnet_second' --data_path '/data1/hulh/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220201-155230-5_decom/model_decom_finetune.ckpt' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v1.cfg'
   
   python 2_test.py --work_name '2_test_final_second_159250' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220202-231238-6_finetune_decom_srnet_second/models/model_159250.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v1.cfg'
   
   Model:159250, test_acc: 0.8792, test_loss: 0.35951784, auc: 0.9574260800000001, weight_auc: 0.9694700571428574, P_MD: 0.21919999999999973 , time:20220204-102141
   ```
   
   

   ```
   python 6_finetune_decom_srnet.py --log_root '../../bb_QF90JU04_log/' --work_name '6_finetune_decom_srnetc64_v3' --data_path '/data1/hulh/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220222-233918-5_decom_v3/model_decom_finetune.ckpt' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v3.cfg'
   
   python 2_test.py --work_name '2_test_v3' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220223-230736-6_finetune_decom_srnetc64_v3/models/model_114625.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v3.cfg'
   
   Model:114625, test_acc: 0.8776, test_loss: 0.35089, auc: 0.9577852799999999, weight_auc: 0.9697734857142858, P_MD: 0.21240000000000003 , time:20220225-134845
   
   ```

对v1和v3从头训练

```
python 7_train_from_scratch_decom.py --log_root '../../bb_QF90JU04_log/' --work_name '7_train_from_scratch_v1' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v1.cfg'

python 2_test.py --work_name '2_test_scratch_v1' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220225-160051-7_train_from_scratch_v1/models/model_462000.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v1.cfg'

Model:462000, test_acc: 0.85500026, test_loss: 0.37084705, auc: 0.9360375999999999, weight_auc: 0.9541197714285715, P_MD: 0.3019999999999998 , time:20220228-144347
```



```
python 7_train_from_scratch_decom.py --log_root '../../bb_QF90JU04_log/' --work_name '7_train_from_scratch_v3' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v3.cfg'

python 2_test.py --work_name '2_test_scratch_v3' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220225-160311-7_train_from_scratch_v3/models/model_467250.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v3.cfg'

Model:467250, test_acc: 0.8621998, test_loss: 0.34276304, auc: 0.95450896, weight_auc: 0.9674589714285712, P_MD: 0.24519999999999953 , time:20220228-145128

```



## 模型复杂度分析

```
# srnetc64
python 6_finetune_decom_srnet.py --log_root '../../srnetc64_2/' --data_path '/data1/hulh/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/log_dir/20220120-1_train_val_srnetc64_BBQF90JU04/models/model_491750.ckpt' 

# v1 v3
python 6_finetune_decom_srnet.py --log_root '../../v3_para/' --data_path '/data1/hulh/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220223-230736-6_finetune_decom_srnetc64_v3/models/model_114625.ckpt' --load_config '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220131-002455-4_detect/bb_QF90JU04_v3.cfg'

# cylinder ladder
python 6_finetune_decom_srnet.py --log_root '../../cylinder/' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220204-141122-7_train_from_scratch_cylinder/models/model_467250.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

python 6_finetune_decom_srnet.py --log_root '../../ladder/' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220207-105915-7_train_from_scratch_ladder/models/model_489125.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
```





## bb_QF90_JU04：SRNET，STD-NET

1. 1_train_val.py 

   ```
   python 1_train_val.py --log_root '../../bb_QF90JU04_log/' --work_name '1_train_val_srnet' --data_path '/data1/hulh/BOSS_BOWS2/'
   ```

   ```
   python 2_test.py --work_name '2_test_srnet' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220202-1_train_val_srnet/models/model_423500.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' 
   ```

   > Model:423500, test_acc: 0.88379997, test_loss: 0.35973534, auc: 0.9603377600000002, weight_auc: 0.9716203999999998, P_MD: 0.21479999999999988 , time:20220205-203842

4. bb_QF90_JU04：STD-NET cylinder

   ```
   python 7_train_from_scratch_decom.py --log_root '../../bb_QF90JU04_log/' --work_name '7_train_from_scratch_cylinder' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```

   ```
   python 2_test.py --work_name '2_test_cylinder' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220204-141122-7_train_from_scratch_cylinder/models/model_467250.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```

   > Model:467250, test_acc: 0.869, test_loss: 0.3363014, auc: 0.9578964799999999, weight_auc: 0.9697779428571425, P_MD: 0.2195999999999998 , time:20220209-094318

2. bb_QF90_JU04：STD-NET ladder 

   ```
   python 7_train_from_scratch_decom.py --log_root '../../bb_QF90JU04_log/' --work_name '7_train_from_scratch_ladder' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   ```
   python 2_test.py --work_name '2_test_ladder' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220207-105915-7_train_from_scratch_ladder/models/model_489125.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   > Model:489125, test_acc: 0.851, test_loss: 0.35054567, auc: 0.9382051199999999, weight_auc: 0.9556356571428571, P_MD: 0.30879999999999985 , time:20220210-095723

## Alaska256

`/data1/liqs/tensor_decom/datasets/alaskav2/spatial/ALASKA_v2_TIFF_256_GrayScale/` tif

`/data1/lily/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale_MiPOD_04/` pgm

由于两者格式不同，需要在`libs/decom_srnet/gen_data.py`中做修改。

1. MiPOD02

   ~~训练srnetc64 显卡1  ing  。valid_acc: 0.5367502~~

   训练srnet 

   ```
   python 1_train_val.py --log_root '../../alaska256_MIPOD02_log/' --work_name '1_train_val_srnet' --data_path '/data1/'
   ```

   训练结果没有收敛。 使用alaskaMiPOD04进行

   ```
   python 6_finetune_decom_srnet.py --log_root '../../alaska256_MiPOD02_log/' --work_name '6_finetune_decom_04srnet' --data_path '/data1/' --load_path '/data1/lily/STD-NET-official/alaska256_MiPOD04_log/20220202-1_train_val_srnet/models/model_83125.ckpt' 
   ```

   ```
   python 2_test.py --work_name '2_test_srnet' --load_path '/data1/lily/STD-NET-official/alaska256_MiPOD02_log/20220205-200659-6_finetune_decom_04srnet/models/model_236250.ckpt' --data_path '/data1/' 
   ```

   > Model:236250, test_acc: 0.65025085, test_loss: 14.721067, auc: 0.7261800956622664, weight_auc: 0.7988444538662209, P_MD: 0.668436873747495 , time:20220208-222458

   训练STD-NET cylinder 

   ```
   python 7_train_from_scratch_decom.py --log_root '../../alaska256_MiPOD02_log/' --work_name '7_train_from_scratch_cylinder' --data_path '/data1/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```

   ```
   python 2_test.py --work_name '2_test_cylinder' --load_path '/data1/lily/STD-NET-official/alaska256_MiPOD02_log/20220210-095158-7_train_from_scratch_cylinder/models/model_405125.ckpt' --data_path '/data1/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```

   > Model:405125, test_acc: 0.5890788, test_loss: 3.7824442, auc: 0.6410451915855759, weight_auc: 0.7218156933678409, P_MD: 0.8115230460921843 , time:20220215-132833

   训练STD-NET ladder 没有收敛。

   ```
   python 7_train_from_scratch_decom.py --log_root '../../alaska256_MiPOD02_log/' --work_name '7_train_from_scratch_ladder' --data_path '/data1/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   ```
   python 6_finetune_decom_srnet.py --log_root '../../alaska256_MiPOD02_log/' --work_name '6_finetune_decom_ladder_2' --data_path '/data1/' --load_path '/data1/lily/STD-NET-official/alaska256_MiPOD04_log/20220210-094733-7_train_from_scratch_ladder/models/model_477750.ckpt' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   ```
   python 2_test.py --work_name '2_test_ladder' --load_path '/data1/lily/STD-NET-official/alaska256_MiPOD02_log/20220215-140031-6_finetune_decom_ladder_2/models/model_126000.ckpt' --data_path '/data1/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```

   > Model:126000, test_acc: 0.60776585, test_loss: 2.2952316, auc: 0.6701578859121047, weight_auc: 0.7468878137035595, P_MD: 0.7855210420841684 , time:20220217-100048

2. MiPOD04

   ~~训练srnetc64 显卡3  ing。 valid_acc: 0.6535~~
   
   训练srnet 
   
   ```
   python 1_train_val.py --log_root '../../alaska256_MIPOD04_log/' --work_name '1_train_val_srnet' --data_path '/data1/'
   ```
   
   ```
   python 2_test.py --work_name '2_test_srnet' --load_path '/data1/lily/STD-NET-official/alaska256_MIPOD04_log/20220202-1_train_val_srnet/models/model_83125.ckpt' --data_path '/data1/'
   ```
   
   > Model:83125, test_acc: 0.66638285, test_loss: 0.8750692, auc: 0.7659155886924149, weight_auc: 0.8261753861286845, P_MD: 0.6722444889779559 , time:20220205-123903
   
   训练STD-NET cylinder
   
   ```
   python 7_train_from_scratch_decom.py --log_root '../../alaska256_MiPOD04_log/' --work_name '7_train_from_scratch_cylinder' --data_path '/data1/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```
   
   ```
   python 2_test.py --work_name '2_test_cylinder' --load_path '/data1/lily/STD-NET-official/alaska256_MiPOD04_log/20220211-125314-7_train_from_scratch_cylinder/models/model_412125.ckpt' --data_path '/data1/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
   ```
   
   > Model:412125, test_acc: 0.68847656, test_loss: 1.0330615, auc: 0.7795404134923153, weight_auc: 0.8370624156988469, P_MD: 0.6408817635270543 , time:20220215-134926
   
   训练STD-NET ladder 
   
   ```
   python 7_train_from_scratch_decom.py --log_root '../../alaska256_MiPOD04_log/' --work_name '7_train_from_scratch_ladder' --data_path '/data1/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```
   
   ```
   python 2_test.py --work_name '2_test_ladder' --load_path '/data1/lily/STD-NET-official/alaska256_MiPOD04_log/20220210-094733-7_train_from_scratch_ladder/models/model_477750.ckpt' --data_path '/data1/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
   ```
   
   > Model:477750, test_acc: 0.6846693, test_loss: 1.1672794, auc: 0.7774390213292317, weight_auc: 0.8357478735025161, P_MD: 0.634569138276553 , time:20220213-154549
   
3. Alaska512

   `/data1/liqs/datasets/alaskav2/ALASKA_v2_TIFF_512_GrayScale`

   `/data1/lily/datasets/alaskav2/ALASKA_v2_TIFF_512_GrayScale_MiPOD_02`	MiPOD_04	HILL_02	HILL_04

   1. MiPOD02

      训练srnetc64 显卡2 ing（出错：构建模型的时候出错）

   2. MiPOD04 （数据集还没有生成完毕 78055）

   3. HILL02

   4. HILL04





No pair constraint 对比实验 error

1. 完全noPC训练400 epochs，是否能够收敛，如果不能够收敛，进行2；

2. 使用PC 训练300 epochs，再用noPC 100 epochs提升性能。

对于QF95的实验，如不收敛，可使用QF75训练好的模型直接在QF95上进行微调。

bb_QF75JU04

1. SRNET

使用《 ImageNet Pre-trained CNNs for JPEG Steganalysis 》中的参数：

```
noPC_max_iter = 400000
noPC_learning_rate = [0.0001, 0.001, 0.00001, 0.000002, 0.000001]
noPC_learning_rate_boundaries = [200000, 260000, 300000, 340000]
```

```
python 1_train_val.py --log_root '../../bb_QF75JU04_log/' --work_name '1_train_val_srnet' --data_path '/data1/lily/datasets/BOSS_BOWS2/'
```

```
python 2_test.py --work_name '2_test_srnet' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/20220207-1_train_val_srnet/models/model_336000.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/'
```

> Model:336000, test_acc: 0.91859984, test_loss: 0.22210747, auc: 0.97730896, weight_auc: 0.9837837714285715, P_MD: 0.12559999999999993 , time:20220215-002441

使用原来的参数： 没有收敛, 前面加100 epoch 的PC

```
max_iter = 500000
learning_rate = [0.001, 0.0001]
learning_rate_boundaries = [400000]
```

```
python 1_train_val.py --log_root '../../bb_QF75JU04_log/' --work_name '1_train_val_srnet_original' --data_path '/data1/lily/datasets/BOSS_BOWS2/'
```



2. STD-NET cylinder



```
python 7_train_from_scratch_decom.py --log_root '../../bb_QF75JU04_log/' --work_name '7_train_from_scratch_cylinder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
```

```
python 2_test.py --work_name '2_test_noPC_cylinder' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/20220209-145318-7_train_from_scratch_cylinder/models/model_384125.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
```

> Model:384125, test_acc: 0.8535997, test_loss: 0.3543549, auc: 0.9336070399999999, weight_auc: 0.9520538285714287, P_MD: 0.30959999999999976 , time:20220212-084930

3. STD-NET ladder

```
python 7_train_from_scratch_decom.py --log_root '../../bb_QF75JU04_log/' --work_name '7_train_from_scratch_ladder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
```

```
python 2_test.py --work_name '2_test_noPC_ladder' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/20220211-135037-7_train_from_scratch_ladder/models/model_179375.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
```

> Model:179375, test_acc: 0.6034, test_loss: 0.6592776, auc: 0.6516936, weight_auc: 0.7274482285714288, P_MD: 0.8508 , time:20220214-223750

bb_QF75UE04

1. SRNET



2. STD-NET cylinder



3. STD-NET ladder



bb_QF90JU04

1. SRNET

使用《 ImageNet Pre-trained CNNs for JPEG Steganalysis 》中的参数： 没有收敛

```
noPC_max_iter = 400000
noPC_learning_rate = [0.0001, 0.001, 0.00001, 0.000002, 0.000001]
noPC_learning_rate_boundaries = [200000, 260000, 300000, 340000]
```

```
python 1_train_val.py --log_root '../../bb_QF90JU04_log/' --work_name '1_train_val_noPC_srnet' --data_path '/data1/hulh/BOSS_BOWS2/'
```

finetune 

```
python 6_finetune_decom_srnet.py --log_root '../../bb_QF90JU04_log/' --work_name '6_finetune_noPC_srnet' --data_path '/data1/hulh/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220202-1_train_val_srnet/models/model_259875.ckpt' 
```

```
python 2_test.py --work_name '2_test_noPC_srnet' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220216-010706-6_finetune_noPC_srnet/models/model_18375.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' 
```

>Model:18375, test_acc: 0.8837999, test_loss: 0.33185634, auc: 0.9613067200000001, weight_auc: 0.9723304, P_MD: 0.20639999999999958 , time:20220217-101546

2. STD-NET cylinder

没有收敛

```
python 7_train_from_scratch_decom.py --log_root '../../bb_QF90JU04_log/' --work_name '7_train_from_scratch_noPC_cylinder' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
```

finetune 

```
python 6_finetune_decom_srnet.py --log_root '../../bb_QF90JU04_log/' --work_name '6_finetune_noPC_cylinder' --data_path '/data1/hulh/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220204-141122-7_train_from_scratch_cylinder/models/model_208250.ckpt' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
```

```
python 2_test.py --work_name '2_test_noPC_cylinder' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220216-122741-6_finetune_noPC_cylinder/models/model_75250.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
```

>Model:75250, test_acc: 0.86680025, test_loss: 0.33593467, auc: 0.9519939200000002, weight_auc: 0.9655456000000002, P_MD: 0.2567999999999999 , time:20220217-112204

3. STD-NET ladder



```
python 7_train_from_scratch_decom.py --log_root '../../bb_QF90JU04_log/' --work_name '7_train_from_scratch_noPC_ladder' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
```





# No pair constraint 对比实验

1. 完全noPC训练400 epochs，是否能够收敛，如果不能够收敛，进行2；

2. 使用PC 训练300 epochs，再用noPC 100 epochs提升性能。

对于QF95的实验，如不收敛，可使用QF75训练好的模型直接在QF95上进行微调。

## bb_QF75JU04

### 1. SRNET

```
max_iter = 500000
learning_rate = [0.001, 0.0001]
learning_rate_boundaries = [400000]
```

```
# 400noPC  没有收敛
python 1_train_val.py --log_root '../../bb_QF75JU04_log/' --work_name 'PC_noPC_srnet' --data_path '/data1/lily/datasets/BOSS_BOWS2/'
```

```
# 300PC + 100 noPC  收敛
python 1_train_val_noPC.py --log_root '../../bb_QF75JU04_log/' --work_name 'noPC_srnet' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/PC_noPC_srnet/PC/models/model_262500.ckpt'

python 2_test.py --work_name '2_test_PCnoPC_srnet' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/noPC_srnet/models/model_32375.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/'

Model:32375, test_acc: 0.92059994, test_loss: 0.25509554, auc: 0.9800191999999999, weight_auc: 0.9857014857142857, P_MD: 0.12160000000000015 , time:20220221-100424

```



### 2. STD-NET cylinder

```
# 训练400次noPC 正在训练 没有收敛
python 1_train_val_noPC.py --log_root '../../bb_QF75JU04_log/' --work_name 'noPC_cylinder_whole' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'
```



```
# 300次PC + 100次noPC 训练结束
python 1_train_val.py --log_root '../../bb_QF75JU04_log/' --work_name 'PC_noPC_cylinder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

python 2_test.py --work_name '2_test_PCnoPC_cylinder' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/PC_noPC_cylinder/noPC/models/model_59500.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

Model:59500, test_acc: 0.9093996, test_loss: 0.27039164, auc: 0.9706672000000001, weight_auc: 0.9789950857142858, P_MD: 0.16559999999999975 , time:20220221-103744

```



### 3. STD-NET ladder

```
# 训练400次noPC 正在训练 没有收敛
python 1_train_val_noPC.py --log_root '../../bb_QF75JU04_log/' --work_name 'noPC_ladder_whole' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
```



```
# 300次PC + 100次noPC 训练结束
python 1_train_val.py --log_root '../../bb_QF75JU04_log/' --work_name 'PC_noPC_ladder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

python 2_test.py --work_name '2_test_PCnoPC_ladder' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/PC_noPC_ladder/noPC/models/model_57750.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'


Model:57750, test_acc: 0.9169999, test_loss: 0.24279958, auc: 0.97943568, weight_auc: 0.9853112000000003, P_MD: 0.10840000000000005 , time:20220227-215426

```



## bb_QF75UE04

### 1. SRNET

```
# 400noPC  没有收敛
python 1_train_val_noPC.py --log_root '../../bb_QF75UE04_log/' --work_name 'noPC_srnet_whole' --data_path '/data1/lily/datasets/BOSS_BOWS2/'

# 300次PC + 100次noPC 
python 1_train_val.py --log_root '../../bb_QF75UE04_log/' --work_name 'PC_noPC_srnet' --data_path '/data1/lily/datasets/BOSS_BOWS2/'

python 2_test.py --work_name '2_test_PCnoPC_srnet' --load_path '/data1/lily/STD-NET-official/bb_QF75UE04_log/PC_noPC_srnet/noPC/models/model_44625.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/'


Model:44625, test_acc: 0.9656005, test_loss: 0.14906804, auc: 0.99502528, weight_auc: 0.9964466285714286, P_MD: 0.02599999999999969 , time:20220226-123938

```



### 2. STD-NET cylinder

```
# 训练400次noPC 没有收敛
python 1_train_val_noPC.py --log_root '../../bb_QF75UE04_log/' --work_name 'noPC_cylinder_whole' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

# 300次PC + 100次noPC 
python 1_train_val.py --log_root '../../bb_QF75UE04_log/' --work_name 'PC_noPC_cylinder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

python 2_test.py --work_name '2_test_PCnoPC_cylinder' --load_path '/data1/lily/STD-NET-official/bb_QF75UE04_log/PC_noPC_cylinder/noPC/models/model_8750.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

Model:8750, test_acc: 0.9628007, test_loss: 0.1433657, auc: 0.99524848, weight_auc: 0.9966060571428571, P_MD: 0.021199999999999997 , time:20220226-162356

```

### 3. STD-NET ladder

```
# 训练400次noPC 没有收敛
python 1_train_val_noPC.py --log_root '../../bb_QF75UE04_log/' --work_name 'noPC_ladder_whole' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

# 300次PC + 100次noPC 
python 1_train_val.py --log_root '../../bb_QF75UE04_log/' --work_name 'PC_noPC_ladder' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

python 2_test.py --work_name '2_test_PCnoPC_ladder' --load_path '/data1/lily/STD-NET-official/bb_QF75UE04_log/PC_noPC_ladder/noPC/models/model_77000.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

Model:77000, test_acc: 0.9654005, test_loss: 0.13784683, auc: 0.9948846400000001, weight_auc: 0.9963461714285716, P_MD: 0.019999999999999907 , time:20220226-183245

```



## bb_QF90JU04

### 1. SRNET

```
# 400次noPC 没有收敛
python 7_train_from_scratch_decom.py --log_root '../../bb_QF90JU04_log/' --work_name 'noPC_srnet_whole' --data_path '/data1/hulh/BOSS_BOWS2/'

# 300次PC + 100次noPC 正在训练
 python 1_train_val.py --log_root '../../bb_QF90JU04_log/' --work_name 'PC_noPC_srnet' --data_path '/data1/lily/datasets/BOSS_BOWS2/'
 
 python 2_test.py --work_name '2_test_PCnoPC_srnet' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/PC_noPC_srnet/noPC/models/model_70875.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' 
 
 Model:70875, test_acc: 0.8650002, test_loss: 0.36239183, auc: 0.95419632, weight_auc: 0.967103085714286, P_MD: 0.2508 , time:20220226-191210

```



### 2. STD-NET cylinder

```
# 400次noPC 没有收敛
python 7_train_from_scratch_decom.py --log_root '../../bb_QF90JU04_log/' --work_name 'noPC_cylinder' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

# 300次PC + 100次noPC 正在训练
python 1_train_val_noPC.py --log_root '../../bb_QF90JU04_log/' --work_name 'noPC_cylinder' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220204-141122-7_train_from_scratch_cylinder/models/model_262500.ckpt'

python 2_test.py --work_name '2_test_PCnoPC_cylinder' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/noPC_cylinder/models/model_87500.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

Model:87500, test_acc: 0.8573998, test_loss: 0.35339952, auc: 0.9448570399999999, weight_auc: 0.9601988571428571, P_MD: 0.2919999999999998 , time:20220222-090446

```



### 3. STD-NET ladder

```
# 400次noPC 正在训练 没有收敛
python 1_train_val_noPC.py --log_root '../../bb_QF90JU04_log/' --work_name 'noPC_ladder_whole' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

# 300次PC + 100次noPC 正在训练
python 1_train_val_noPC.py --log_root '../../bb_QF90JU04_log/' --work_name 'noPC_ladder' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220207-105915-7_train_from_scratch_ladder/models/model_262500.ckpt'

python 2_test.py --work_name '2_test_PCnoPC_ladder' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/noPC_ladder/models/model_41125.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

Model:41125, test_acc: 0.82680005, test_loss: 0.39221278, auc: 0.92359344, weight_auc: 0.9452505142857144, P_MD: 0.3531999999999996 , time:20220222-090729

```





# efficient net

QF75-BB-JU04, QF95-BB-JU04, BB-HILL04, QF75-ALA2-JU04, QF95-ALA2-JU04, ALA2-HILL04

```
python main.py --data '/data1/lily/datasets/Fish/' --arch 'efficientnet-b0' --lr 0.01 --seed 1234 --gpu 0 --batch-size 32 --image_size 224
```

没有设置输出类为20   Acc@1 52.500 Acc@5 82.143

设置输出类为20   Acc@1 46.071 Acc@5 80.000

调整lr=0.001后  Acc@1 61.607 Acc@5 88.393

调整其他后       Acc@1 56.250 Acc@5 85.893

```
python main.py --data '/data1/lily/EfficientNet-PyTorch/examples/imagenet/data/' --arch 'efficientnet-b0' --lr 0.001 --wd 0.01 --seed 1234 --gpu 1 --batch-size 32 --pretrained --print_freq 320 --cover_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75/' --stego_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75_juniward04/' --log_root '/data1/lily/EfficientNet-PyTorch/bb_QF75JU04'
```

秋实师兄代码：

```
优化器第一组：
optimizer = optim.AdamW(model.parameters(), lr=1e-4, weight_decay=1e-2, eps=1e-5)
scheduler = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=20, eta_min=(1e-4) * 0.05)

优化器第二组：
optimizer = optim.SGD(model.parameters(), lr=1e-3, momentum=0.9, weight_decay=0)
scheduler = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=20, eta_min=(1e-3) * 0.05)

```



## bb_QF75JU04

随机加载数据 

```
optimizer = torch.optim.AdamW(model.parameters(), args.lr, weight_decay=args.weight_decay)
scheduler = lr_scheduler.ReduceLROnPlateau(optimizer, 'min', factor=0.5, patience=2)
```

noPC

```
optimizer = torch.optim.AdamW(model.parameters(), 0.001, weight_decay=0.01)
scheduler = lr_scheduler.ReduceLROnPlateau(optimizer, 'min', factor=0.5, patience=2)
```

PC

```
# 1. 没有收敛
optimizer = torch.optim.AdamW(model.parameters(), 0.001, weight_decay=0.01)
scheduler = lr_scheduler.ReduceLROnPlateau(optimizer, 'min', factor=0.5, patience=2)

python main.py --arch 'efficientnet-b0' --lr 0.001 --wd 0.01 --seed 1234 --gpu 1 --batch-size 32 --pretrained --print_freq 320 --cover_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75/' --stego_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75_juniward04/' --log_root '/data1/lily/EfficientNet-PyTorch/bb_QF75JU04_PC' --shuffle False


# 2. 没有收敛
optimizer = torch.optim.SGD(model.parameters(), lr=0.1, momentum=0.9)
scheduler = lr_scheduler.ReduceLROnPlateau(optimizer, 'min', factor=0.5, patience=4)

python main.py --arch 'efficientnet-b0' --lr 0.01 --wd 0.01 --momentum 0.9 --seed 1234 --gpu 1 --batch-size 32 --pretrained --print_freq 320 --cover_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75/' --stego_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75_juniward04/' --log_root '/data1/lily/EfficientNet-PyTorch/bb_QF75JU04_PC_2' --shuffle False

# 3. 没有收敛
optimizer = torch.optim.AdamW(model.parameters(), 0.01, weight_decay=0.0001)
scheduler = lr_scheduler.ReduceLROnPlateau(optimizer, 'min', factor=0.5, patience=2)

python main.py --arch 'efficientnet-b0' --lr 0.01 --wd 0.0001 --seed 1234 --gpu 7 --batch-size 32 --pretrained --print_freq 320 --cover_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75/' --stego_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75_juniward04/' --log_root '/data1/lily/EfficientNet-PyTorch/bb_QF75JU04_PC_3' --shuffle False --epochs 200

# 4. 使用保营师兄代码，加上CrossEntropyLoss Adam CosineAnnealingLR
optimizer = optim.Adam(parameters, lr=1e-3, weight_decay=1e-5)
optimizer = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=20)

# 5. 用秋实师兄代码和参数2 PC+noPC 收敛
python train_loader.py --work_name 'bb_qf75ju04_PCnoPC_para2'



```



## bb_HILL_04

PC

```
# 1 100 次PC  没有收敛
optimizer = torch.optim.AdamW(model.parameters(), 0.001, weight_decay=0.01)
scheduler = lr_scheduler.ReduceLROnPlateau(optimizer, 'min', factor=0.5, patience=2)

python main.py --arch 'efficientnet-b0' --lr 0.001 --wd 0.01 --seed 1234 --gpu 1 --batch-size 32 --pretrained --print_freq 320 --cover_root '/data1/lily/datasets/BOSS_BOWS2/bb_256/' --stego_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_HILL04/' --log_root '/data1/lily/EfficientNet-PyTorch/bb_HILL04_PC_1' --shuffle False

# 2 在datasets中增加了normalization 再进行100次PC 没有收敛
optimizer = torch.optim.AdamW(model.parameters(), 0.01, weight_decay=0.0001)
scheduler = lr_scheduler.ReduceLROnPlateau(optimizer, 'min', factor=0.5, patience=10)

python main.py --arch 'efficientnet-b0' --lr 0.01 --wd 0.0001 --seed 1234 --gpu 5 --batch-size 32 --pretrained --print_freq 320 --cover_root '/data1/lily/datasets/BOSS_BOWS2/bb_256/' --stego_root '/data1/lily/datasets/BOSS_BOWS2/bb_256_HILL04/' --log_root '/data1/lily/EfficientNet-PyTorch/bb_HILL04_PC_2' --shuffle False

# 3. 用秋实师兄代码 PC两组参数都没有收敛
# 4. 
```





## bb_QF95JU04

PC和noPC都没有收敛

```
# 1. noPC 参数1 没有收敛
python train_dataloader.py --work_name 'bb_qf95ju04_noPC_para1'

# 1. noPC 参数2 没有收敛
python train_dataloader.py --work_name 'bb_qf95ju04_noPC'
```



## alaska_QF75JU04

```
# 1. noPC 参数1 收敛
python train_dataloader.py --work_name 'alaska_qf75ju04_noPC_para1'
Epoch: 1 Valid Acc: 91.59569138276554 Valid auc: 0.9647355796261864 Valid wauc: 0.9730733215285424 P_MD: 0.1123246492985972


# 1. noPC 参数2 收敛
python train_dataloader.py --work_name 'alaska_qf75ju04_noPC_para2'
```



## alaska_QF95JU04

```
# 1. noPC 参数1 没有收敛
python train_dataloader.py --work_name 'alaska_qf95ju04_noPC_para1'

# 2. noPC 参数2 没有收敛
python train_dataloader.py --work_name 'alaska_qf95ju04_noPC_para2'

# 3. PC 参数1 正在训练 75epochs 没有收敛
python train_dataloader.py --work_name 'alaska_qf95ju04_PC_para1'

# 4. PC 参数2 正在训练 75epochs 没有收敛
python train_dataloader.py --work_name 'alaska_qf95ju04_PC_para2'
```





## alaska_HILL04

```
# 1. noPC 参数1 没有收敛
python train_dataloader.py --work_name 'alaska_hill04_noPC_para1'

# 2. noPC 参数2 没有收敛
python train_dataloader.py --work_name 'alaska_hill04_noPC_para2'

# 3. PC 参数1 正在训练 75epochs 没有收敛
python train_dataloader.py --work_name 'alaska_hill04_PC_para1'

# 4. PC 参数2 正在训练 75epochs 没有收敛
python train_dataloader.py --work_name 'alaska_hill04_PC_para2'
```





汇报

```
para1：
optimizer = optim.AdamW(model.parameters(), lr=1e-4, weight_decay=1e-2, eps=1e-5)
scheduler = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=epochs, eta_min=(1e-4) * 0.05)
para2：
optimizer = optim.SGD(model.parameters(), lr=1e-3, momentum=0.9, weight_decay=0)
scheduler = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=epochs, eta_min=(1e-3) * 0.05)
batch size 都是 32

noPC：全部使用para1.

PC：
ALA_QF95JU04 b0和b4的pretrain用的para1，其余用的para2
```







```
srnet_ala2_qfvari_ju02
wauc:  0.6520151597211028
39920
warning !!!
P_MD(05%): 0.931463

srnet_ala2_qfvari_ju04
wauc:  0.8320729779054242
39920
0.05 part_tpr: [0.2629258517034068, 0.26683366733466934]
0.05 part_fpr: [0.04969939879759519, 0.05020040080160321]
P_MD(05%): 0.734729


srnet_ala2_qfvari_ue02
wauc:  0.7503959931027243
39920
0.05 part_tpr: [0.07264529058116233, 0.12965931863727456]
0.05 part_fpr: [0.03767535070140281, 0.051402805611222444]
P_MD(05%): 0.876167


srnet_ala2_qfvari_ue04
wauc:  0.8707036532268659
39920
0.05 part_tpr: [0.4223446893787575, 0.4230460921843687]
0.05 part_fpr: [0.04969939879759519, 0.05]
P_MD(05%): 0.576954



alaqf75ue02
wauc:  0.7211603173423858
39920
warning !!!
P_MD(05%): 0.653507



alaqf75ue04
wauc:  0.9284866433410768
39920
0.05 part_tpr: [0.48697394789579157, 0.493186372745491]
0.05 part_fpr: [0.049498997995991986, 0.05020040080160321]
P_MD(05%): 0.508589


bbqf75ju04
wauc:  0.9856333714285713
10000
0.05 part_tpr: [0.8968, 0.8968]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.103200



bbqf75ju02
wauc:  0.9233824
10000
0.05 part_tpr: [0.5604, 0.5604]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.439600


bbqf75ue02
wauc:  0.9770393142857142
10000
0.05 part_tpr: [0.83, 0.83]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.170000



bbqf75ue04  更换多个模型后找到一个合适的
wauc:  0.996184742857143
10000
0.05 part_tpr: [0.9808, 0.9808]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.019200



bbqf95ju04
wauc:  0.8982612571428574
10000
0.05 part_tpr: [0.47, 0.47]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.530000



bbqf95ue04
wauc:  0.9842409142857144
10000
0.05 part_tpr: [0.866, 0.866]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.134000


bbsu04
wauc:  0.9804281714285714
10000
0.05 part_tpr: [0.8328, 0.8328]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.167200


bbhill04
wauc:  0.9613445142857142
10000
0.05 part_tpr: [0.72, 0.72]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.280000


bbhill02
wauc:  0.8955124571428572
10000
0.05 part_tpr: [0.5052, 0.5052]
0.05 part_fpr: [0.0492, 0.05]
P_MD(05%): 0.494800


bbqf90ju04
wauc:  0.8344157142857145
10000
warning !!!
P_MD(05%): 0.652000


bbsu02
wauc:  0.5958274285714287
10000
0.05 part_tpr: [0.0544, 0.0556]
0.05 part_fpr: [0.0496, 0.0512]
P_MD(05%): 0.945300


alaqf75ju02
wauc:  0.8051011771610787
39920
0.05 part_tpr: [0.2343687374749499, 0.23567134268537074]
0.05 part_fpr: [0.04959919839679359, 0.05030060120240481]
P_MD(05%): 0.764887


alaqf75ju04
wauc:  0.9316037313676881
39920
0.05 part_tpr: [0.6213426853707414, 0.6220440881763527]
0.05 part_fpr: [0.04979959919839679, 0.05]
P_MD(05%): 0.377956


bb512qf75ju04 
wauc:  0.9769361714285717
10000
0.05 part_tpr: [0.9064, 0.9064]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.093600




bbqf90ju04  hulh  
wauc:  0.9673206857142859
10000
0.05 part_tpr: [0.754, 0.754]
0.05 part_fpr: [0.0496, 0.05]
P_MD(05%): 0.246000







```









/data1/liqs/CALPA_output/exp/srnet_bb_qf90ju04 这个也顺便测一下吧，模型你挑一个最好的就行

==bb_qf75ju04:==
/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75/
/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75_juniward04/
clapa: /data2/wuwl/psm_output/expA/srnet_qf75_juniward_40/output_another_try2/initial_train_output/juniward_04_pruned_05/Model_403375.ckpt
/data2/wuwl/psm_output/expA/srnet_qf75_juniward_40/output_another_try2/detect_output/srnet_juniward_04_threshold05.cfg



==bb_qf75ju02:== 
/data1/lily/datasets/BOSS_BOWS2/bb_256_qf75/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward02/
clapa: /data2/liqiushi/psm_output/expB/srnet_qf75_juniward_20/output/initial_train_output_revision/Model_483875.ckpt
/data2/liqiushi/psm_output/expB/srnet_qf75_juniward_20/output/detect_output/srnet_juniward_02_threshold05_revision.cfg



bbqf75ue04: 出错
/data1/liqs/datasets/BOSS_BOWS2/bb_256_qf75/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd04/
/data2/wuwl/psm_output/expB/srnet_qf75_uerd_40/output/initial_train_output_revision/Model_795375.ckpt
/data2/wuwl/psm_output/expB/srnet_qf75_uerd_40/output/detect_output/srnet_uerd_04_threshold05.cfg

![image-20220416042711616](C:\Users\lly\AppData\Roaming\Typora\typora-user-images\image-20220416042711616.png)



bbqf75ue02: 出错
/data1/liqs/datasets/BOSS_BOWS2/bb_256_qf75/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd02/
/data2/wuwl/psm_output/expB/srnet_qf75_uerd_20/output/initial_train_output_revision/Model_498750.ckpt
/data2/wuwl/psm_output/expB/srnet_qf75_uerd_20/output/detect_output/srnet_uerd_02_threshold05.cfg

报错：![image-20220416043417981](C:\Users\lly\AppData\Roaming\Typora\typora-user-images\image-20220416043417981.png)



bbqf95ju04 ：
/data1/liqs/datasets/BOSS_BOWS2/bb_256_qf95/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_qf95_juniward04/
/data2/wuwl/psm_output/expB/srnet_qf95_juniward_40/output/initial_train_output/Model_856625.ckpt
/data2/wuwl/psm_output/expB/srnet_qf95_juniward_40/output/detect_output/srnet_juniward_04_threshold05.cfg

报错：![image-20220416043923239](C:\Users\lly\AppData\Roaming\Typora\typora-user-images\image-20220416043923239.png)



bbqf95ue04 ：
/data1/liqs/datasets/BOSS_BOWS2/bb_256_qf95/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_qf95_uerd04/
/data2/wuwl/psm_output/expB/srnet_qf95_uerd_40/output/initial_train_output/Model_660625.ckpt
/data2/wuwl/psm_output/expB/srnet_qf95_uerd_40/output/detect_output/srnet_uerd_04_threshold05.cfg

报错：![image-20220416044157059](C:\Users\lly\AppData\Roaming\Typora\typora-user-images\image-20220416044157059.png)



bbsu04
/data1/liqs/datasets/BOSS_BOWS2/bb_256/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_suniward04/
/data2/wuwl/psm_output/expB/srnet_suniward_40/output/initial_train_output/Model_500500.ckpt
/data2/wuwl/psm_output/expB/srnet_suniward_40/output/detect_output/srnet_suniward_04_threshold05.cfg

报错：![image-20220416044606890](C:\Users\lly\AppData\Roaming\Typora\typora-user-images\image-20220416044606890.png)



bbsu02
/data1/liqs/datasets/BOSS_BOWS2/bb_256/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_suniward02/
没有收敛

bbhill04
/data1/liqs/datasets/BOSS_BOWS2/bb_256/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_hill04/
/data2/wuwl/psm_output/expB/srnet_hill_40/output_another_try1/initial_train_output/Model_418250.ckpt
/data2/wuwl/psm_output/expB/srnet_hill_40/output_another_try1/detect_output/srnet_hill_04_th05.txt

报错：![image-20220416044845541](C:\Users\lly\AppData\Roaming\Typora\typora-user-images\image-20220416044845541.png)



bbhill02
/data1/liqs/datasets/BOSS_BOWS2/bb_256/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_hill02/
/data2/wuwl/psm_output/expB/srnet_hill_20/output/initial_train_output_revision/Model_1272250.ckpt
/data2/wuwl/psm_output/expB/srnet_hill_20/output/detect_output/srnet_hill_02_threshold05.cfg

alaqf75ju04
/data1/lily/datasets/alaskav2/qf75/
/data1/lily/datasets/alaskav2/qf75_ju04/



alaqf75ju02
/data1/lily/datasets/alaskav2/qf75/
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qf75_ju02/

==alaqf75ue04==
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qf75/
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qf75_uerd04/
/data1/liqs/CALPA_output/exp/srnet_ala2_qfvari_ue04/output/initial_train_output/Model_511000.ckpt
/data1/liqs/CALPA_output/exp/srnet_ala2_qfvari_ue04/output/detect_output/srnet_qfvari_ue04_threshold05.cfg

==alaqf75ue02==
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qf75/
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qf75_uerd02/
/data1/liqs/CALPA_output/exp/srnet_ala2_qfvari_ue02/output/initial_train_output/Model_675500.ckpt
/data1/liqs/CALPA_output/exp/srnet_ala2_qfvari_ue02/output/detect_output/srnet_qfvari_ue02_threshold05.cfg

==alaqfvarju04==
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qfvarious/
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qfvarious_ju04/
/data1/liqs/CALPA_output/exp/srnet_ala2_qfvari_juniward_40/output/initial_train_output/Model_444500.ckpt
/data1/liqs/CALPA_output/exp/srnet_ala2_qfvari_juniward_40/output/detect_output/srnet_qfvari_ju04_threshold05.cfg

==alaqfvarju02==
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qfvarious/
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qfvarious_ju02/
/data1/liqs/CALPA_output/exp/srnet_ala2_qfvari_ju02/output/initial_train_output/Model_189000.ckpt
/data1/liqs/CALPA_output/exp/srnet_ala2_qfvari_ju02/output/detect_output/srnet_qfvari_ju02_threshold05.cfg

==alaqfvarue04==
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qfvarious/
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qfvarious_uerd04/

==alaqfvarue02==
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qfvarious/
/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qfvarious_uerd02/



==bbqf90ju04== 

/data1/liqs/CALPA_output/exp/srnet_bb_qf90ju04/output/initial_train_output_1/Model_353500.ckpt

/data1/liqs/CALPA_output/exp/srnet_bb_qf90ju04/output/detect_output/srnet_bb_su02_threshold05.cfg



==bbsu02==  

/data1/liqs/datasets/BOSS_BOWS2/bb_256/
/data1/liqs/datasets/BOSS_BOWS2/bb_256_suniward02/

/data1/liqs/CALPA_output/exp/srnet_bb_su02/output/initial_train_output/Model_1750.ckpt

/data1/liqs/CALPA_output/exp/srnet_bb_su02/output/detect_output/srnet_bb_su02_threshold05.cfg



==alaqf75ju02 3==

/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qf75/

/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qf75_ju02/

/data1/chenyl/CALPA_output/exp/srnet_ala2_qf75_ju02/output/detect_output/srnet_qf75_ju02_threshold05.cfg

/data1/chenyl/calpa_logfiles/ala_qf75ju02/Model_612500.ckpt



==alaqf75ju04 2==

/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qf75/

/data1/liqs/tensor_decom/datasets/alaskav2/jpeg2spatial/qf75_ju04/

/data1/chenyl/CALPA_output/exp/srnet_ala2_qf75_ju04/output/detect_output/srnet_qf75_ju04_threshold05.cfg

/data1/chenyl/calpa_logfiles/ala_qf75ju04/Model_780500.ckpt



bb512qf75ju04 
/data1/hulh/BOSS_BOWS2/bb_512_qf75/
/data1/hulh/BOSS_BOWS2/bb_512_qf75_juniward04/
/data1/liqs/CALPA_output/exp/srnet_bb512_qf75ju04/output/initial_train_output_51/Model_6125.ckpt
/data1/liqs/CALPA_output/exp/srnet_bb512_qf75ju04/output/detect_output/srnet_bb_su02_threshold05.cfg



==bb_512_qf75_juniwar04==

/data1/hulh/BOSS_BOWS2/bb_512_qf75/

/data1/hulh/BOSS_BOWS2/bb_512_qf75_juniward04/

/data1/liqs/CALPA_output/exp/srnet_bb512_qf75ju04/output/detect_output/srnet_bb_su02_threshold05.cfg

/data1/hulh/std-net/log_dir/1_need_log/20220324-162135-7_train_from_scratch_calpa_bb_512_qf75_juniwar04/models/model_291375.ckpt



==bbqf90ju04 hulh==

/data1/hulh/std-net/log_dir/1_need_log/20220324-162224-7_train_from_scratch_calpa_bb_256_qf90_juniwar04/models/model_447125.ckpt

/data1/liqs/CALPA_output/exp/srnet_bb_qf90ju04/output/detect_output/srnet_bb_su02_threshold05.cfg







## 测试300epochs PC的最佳模型

```
python 2_test.py --work_name 'pc_' --load_path '' --data_path '/data1/lily/datasets/BOSS_BOWS2/'
```

### bbqf75ju04

srnet

```
python 2_test.py --work_name 'pc_srnet' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/PC_noPC_srnet/PC/models/model_205625.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/'

Model:205625, test_acc: 0.912, test_loss: 0.26843655, auc: 0.9754284000000001, weight_auc: 0.9823925142857143, P_MD: 0.11760000000000004 , time:20220416-150515

```

cylinder

```
python 2_test.py --work_name '0416_test_pc_cylinder' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/PC_noPC_cylinder/PC/models/model_198625.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

Model:198625, test_acc: 0.8878, test_loss: 0.32459694, auc: 0.9625956, weight_auc: 0.9730981142857145, P_MD: 0.20440000000000003 , time:20220416-151121

```

ladder

```
python 2_test.py --work_name '0416_test_pc_ladder' --load_path '/data1/lily/STD-NET-official/bb_QF75JU04_log/PC_noPC_ladder/PC/models/model_255500.ckpt' --data_path '/data1/lily/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

Model:255500, test_acc: 0.914, test_loss: 0.26300794, auc: 0.9756468800000001, weight_auc: 0.9825778285714284, P_MD: 0.13239999999999996 , time:20220416-151232

```

### bbqf75ue04

srnet 

```
python 2_test.py --work_name '0416_test_pc_srnet' --load_path '/data1/lily/STD-NET-official/bb_QF75UE04_log/PC_noPC_srnet/PC/models/model_235375.ckpt' --data_path '/data1/liqs/datasets/BOSS_BOWS2/'

Model:235375, test_acc: 0.96999997, test_loss: 0.1315361, auc: 0.9955643200000001, weight_auc: 0.9968316571428573, P_MD: 0.018000000000000016 , time:20220416-152932

```

cylinder 

```
python 2_test.py --work_name '0416_test_pc_cylinder' --load_path '/data1/lily/STD-NET-official/bb_QF75UE04_log/PC_noPC_cylinder/PC/models/model_260750.ckpt' --data_path '/data1/liqs/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

Model:260750, test_acc: 0.9676, test_loss: 0.14079127, auc: 0.99422808, weight_auc: 0.9958772, P_MD: 0.022800000000000042 , time:20220416-153205

```

ladder

```
python 2_test.py --work_name '0416_test_pc_ladder' --load_path '/data1/lily/STD-NET-official/bb_QF75UE04_log/PC_noPC_ladder/PC/models/model_206500.ckpt' --data_path '/data1/liqs/datasets/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

Model:206500, test_acc: 0.9568, test_loss: 0.1679208, auc: 0.9927336800000001, weight_auc: 0.9948097714285715, P_MD: 0.02959999999999996 , time:20220416-153710

```

### bbqf90ju04

srnet

```
python 2_test.py --work_name '0416_test_pc_srnet' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/PC_noPC_srnet/PC/models/model_247625.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/'

Model:247625, test_acc: 0.8678, test_loss: 0.35387996, auc: 0.9521608000000001, weight_auc: 0.9655968000000001, P_MD: 0.24360000000000004 , time:20220416-154124

```

cylinder 

```
python 2_test.py --work_name '0416_test_pc_cylinder' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220204-141122-7_train_from_scratch_cylinder/models/model_208250.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

Model:208250, test_acc: 0.8504, test_loss: 0.35694334, auc: 0.9425564799999999, weight_auc: 0.9587575999999999, P_MD: 0.3136 , time:20220416-154927

```

ladder  

```
python 2_test.py --work_name '0416_test_pc_ladder' --load_path '/data1/lily/STD-NET-official/bb_QF90JU04_log/20220207-105915-7_train_from_scratch_ladder/models/model_246750.ckpt' --data_path '/data1/hulh/BOSS_BOWS2/' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

Model:246750, test_acc: 0.8282, test_loss: 0.39750442, auc: 0.9227210399999999, weight_auc: 0.9442845714285715, P_MD: 0.34919999999999995 , time:20220416-155208


```







## ALA-QF75JU04 接着跑no PC

/data1/chenyl/calpa_logfiles/ala_qf75ju04/Model_350000.ckpt

```
python 6_finetune_decom_srnet.py --log_root '../../alaska256_QF75JU04_log/' --work_name '6_finetune_decom_srnet' --load_path '/data1/chenyl/calpa_logfiles/ala_qf75ju04/Model_350000.ckpt' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'
```



srnet /data1/liqs/tensor_decom/alaska_nopc/srnet_alav2qf75ju04/model_350000.ckpt

```
python 6_finetune_decom_srnet.py --log_root '../../alaska256_QF75JU04_log/' --work_name '6_finetune_srnet' --load_path '/data1/liqs/tensor_decom/alaska_nopc/srnet_alav2qf75ju04/model_350000.ckpt' 

# model 98000 val_acc:0.86
python 2_test.py --work_name '0417_test_nopc_srnet' --load_path '/data1/lily/STD-NET-official/alaska256_QF75JU04_log/20220416-172847-6_finetune_srnet/models/model_98000.ckpt'

Model:98000, test_acc: 0.86132264, test_loss: 0.3498852, auc: 0.9468067648322697, weight_auc: 0.9617737980971964, P_MD: 0.26623246492985975 , time:20220417-080705

```

cylinder /data1/chenyl/cylinder_ala_qf75ju04_logfiles/train_from_scratch_tucker_350000.ckpt

```
python 6_finetune_decom_srnet.py --log_root '../../alaska256_QF75JU04_log/' --work_name '6_finetune_cylinder' --load_path '/data1/chenyl/cylinder_ala_qf75ju04_logfiles/train_from_scratch_tucker_350000.ckpt' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

# model 42000 val_acc: 0.844
python 2_test.py --work_name '0417_test_nopc_cylinder' --load_path '/data1/lily/STD-NET-official/alaska256_QF75JU04_log/20220416-172606-6_finetune_cylinder/models/model_42000.ckpt' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_cylinder.cfg'

Model:42000, test_acc: 0.8352707, test_loss: 0.3889717, auc: 0.9290154808615226, weight_auc: 0.9487216631946976, P_MD: 0.3395791583166332 , time:20220417-081059

```

ladder /data1/chenyl/ladder_ala_qf75ju04_logfiles/train_from_scratch_tucker_350000.ckpt

```
python 6_finetune_decom_srnet.py --log_root '../../alaska256_QF75JU04_log/' --work_name '6_finetune_ladder' --load_path '/data1/chenyl/ladder_ala_qf75ju04_logfiles/train_from_scratch_tucker_350000.ckpt' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

# model 80500 val_acc: 0.8395
python 2_test.py --work_name '0417_test_nopc_ladder' --load_path '/data1/lily/STD-NET-official/alaska256_QF75JU04_log/20220416-173348-6_finetune_ladder/models/model_80500.ckpt' --load_config '/data1/lily/STD-NET-official/generated_cfg_and_model/cfgs/bb_qf75ju04_ladder.cfg'

Model:80500, test_acc: 0.8372243, test_loss: 0.39005354, auc: 0.9302503102397179, weight_auc: 0.9497127270573211, P_MD: 0.3323647294589178 , time:20220417-081505

```

