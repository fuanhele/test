# 隐写分析对比实验——lily



模型：[Yedroudj-Net](https://github.com/yedmed/steganalysis_with_CNN_Yedroudj-Net)，[ZhuNet](https://github.com/1204BUPT/Zhu-Net-image-steganalysis)，[SiaStegNet](https://github.com/SiaStg/SiaStegNet)

> **注意：尽量不要改动源代码，尤其是读数据和网络结构。任何改动都请记录下来！**

数据集：

256x256

1. BB数据集：

   * HILL 0.4, 0.2

     

   * MIPOD 0.4, 0.2

   * CMD-HILL 0.4

   * CMD-MIPOD 0.4

   > HILL: 
   >
   > /data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/   bb_256_hill04
   >
   > MIPOD: 
   >
   > /data2/liqs/datasets/BOSS_BOWS2/spatial/moxb/cover  MiPOD04_oes  cmd_mipod_04

2. ALASKA-2 数据集

   * HILL 0.4, 0.2

   * MIPOD 0.4, 0.2

   * CMD-HILL 0.4

   * CMD-MIPOD 0.4

     > /data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale
     >
     > /data2/liqs/datasets/alaskav2/spatial/HILL40
     >
     > /data2/liqs/datasets/alaskav2/spatial/moxb/ala2_cmd_mipod_04

512x512

1. BB数据集：
   * HILL 0.4
   * MIPOD 0.4
2. ALASKA-2 数据集
   * HILL 0.4
   * MIPOD 0.4





# 实验过程记录

## 待做实验

好的，你先跑EFN-B4，跑完之后把KeNet的suniward给跑了吧

## 首先训练SiaStegNet

cd /home/lily/SiaStegNet/

1. BB HILL 0.4

   ```
   python train.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_hill04/' --list-file './src/datas/bb/spatial' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/bb256_hill04_default' --gpu-id 0
   ```

2. BB256 hill02

   ```
   python train.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_hill02/' --list-file './src/datas/bb/spatial' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/bb256_hill02_default' --gpu-id 3
   ```

3. BB256 MiPOD04

   ```
   python train.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_mipod04/' --list-file './src/datas/bb/spatial' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/bb256_MiPOD04_default' --gpu-id 4
   ```

4. BB256 MiPOD02

   ```
   python train.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_mipod02/' --list-file './src/datas/bb/spatial' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/bb256_MiPOD02_default' --gpu-id 7
   ```

5. BB256 CMD_MiPOD04

   ```
   python train.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/moxb/cover/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/moxb/cmd_mipod_04/' --list-file './src/datas/bb/spatial' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/bb256_CMD_MiPOD04_default' --gpu-id 0
   ```

6. BB256 CMD_HILL04 数据集还未生成
   /data2/liqs/datasets/BOSS_BOWS2/bb_256_cmd_hill04
   /data2/liqs/datasets/alaskav2/spatial/CMD_HILL04

   ```
   python train.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/moxb/cover/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_cmd_hill04/' --list-file './src/datas/bb/spatial' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/bb256_CMD_HILL04_default' --gpu-id 3 --data-type 'bb'
   ```

7. ALA_256 HILL 0.4  DONE

   ```
   python train.py --cover-dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego-dir '/data2/liqs/datasets/alaskav2/spatial/HILL40/' --list-file './src/datas/alaskav2/spatial' --data-type 'ala' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/ala256_hill04_default' --gpu-id 5
   ```

8. ALA_256 hill02  DONE未收敛

   ```
   python train.py --cover-dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego-dir '/data2/liqs/datasets/alaskav2/spatial/HILL20/' --list-file './src/datas/alaskav2/spatial' --data-type 'ala' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/ala256_hill02_default' --gpu-id 6
   ```

9. ALA_256 MiPOD 0.4

   ```
   python train.py --cover-dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego-dir '/data2/liqs/datasets/alaskav2/spatial/moxb/mipod_04/' --list-file './src/datas/alaskav2/spatial' --data-type 'ala' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/ala256_MiPOD04_default' --gpu-id 3
   ```

10. ALA_256 MiPOD02

    ```
    python train.py --cover-dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego-dir '/data2/liqs/datasets/alaskav2/spatial/moxb/mipod_02/' --list-file './src/datas/alaskav2/spatial' --data-type 'ala' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/ala256_MiPOD02_default' --gpu-id 4
    ```

11. ALA_256 CMD_HILL04

    ```
    python train.py --cover-dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego-dir '/data2/liqs/datasets/alaskav2/spatial/CMD_HILL04/' --list-file './src/datas/alaskav2/spatial' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/ala256_CMD_HILL04_default' --gpu-id 6 --data-type 'ala'
    ```

    

12. ALA_256 CMD_MiPOD04

    ```
    python train.py --cover-dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego-dir '/data2/liqs/datasets/alaskav2/spatial/moxb/ala2_cmd_mipod_04/' --list-file './src/datas/alaskav2/spatial' --data-type 'ala' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/ala256_CMD_MiPOD04_default' --gpu-id 7
    ```

13. ALA_512 HILL04

    ```
    python train.py --cover-dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_512_GrayScale/' --stego-dir '/data2/lily/backup/lily/datasets/alaskav2/ALASKA_v2_TIFF_512_GrayScale_HILL_04/' --list-file './src/datas/alaskav2/spatial' --data-type 'ala' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/ala512_HILL04_default' --gpu-id 2
    ```

14. ALA_512 MiPOD04

    ```
    
    ```

    

15. ALA_suniward02
    
      ``` python
      python train.py --cover-dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego-dir '/data2/liqs/datasets/alaskav2/spatial/SUNIWARD20/' --list-file './src/datas/alaskav2/spatial' --data-type 'ala' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/ala256_suniward02_default' --gpu-id 4
      ```

## 训练Zhu-Net

1. BB HILL 0.4

   ```
   python Main.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_hill04' --work-name 'bb256_hill04_default'
   ```

2. BB HILL 0.2

   ```
   python Main.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_hill02' --work-name 'bb256_hill02_default'
   ```

3. BB MiPOD 0.4

   ```
   python Main.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/moxb/cover' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/moxb/MiPOD04_oes' --work-name 'bb256_MiPOD04_default'
   ```

4. BB MiPOD 0.2

   ```
   python Main.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/moxb/cover' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/moxb/MiPOD02_oes' --work-name 'bb256_MiPOD02_default'
   ```

5. BB256 CMD_MiPOD04

   ```
   python Main.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/moxb/cover/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/moxb/cmd_mipod_04/' --work-name  'bb256_CMD_MiPOD04_default'
   ```

6. BB256 CMD_HILL04 数据集还未生成

python Main.py --cover-dir '' --stego-dir '' --work-name  'bb256_CMD_MiPOD04_default' --data_type 'ala'



## 训练Yedroudj-Net

1. BB HILL 0.4       0.84

   ``` python
   python yed.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_hill04' --work_name 'bb256_hill04_default' --gpuNum '5'
   ```

2. BB HILL 0.2      0.739

   ```
   python yed.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_hill02' --work_name 'bb256_hill02_default' --gpuNum '6'
   ```

3. BB MiPOD 0.4

   ```
   python yed.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/moxb/cover' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/moxb/MiPOD04_oes' --work_name 'bb256_MiPOD04_default' --gpuNum '3'
   ```

4. BB MiPOD 0.2

   ```
   python yed.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/moxb/cover' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/moxb/MiPOD02_oes' --work_name 'bb256_MiPOD02_default' --gpuNum '4'
   ```







## JPEG domain B0

模型：EfficientNet-B0, B4  pc+nopc

* 使用Yedrou-Net读图像方式

* 将灰度图复制成三通道

数据集：

256X256

### BB:

* QF75-JU 0.4, 0.2   0.83 0.63
  
  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75

  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward04

  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward02

  ``` python
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward04' --work_name 'bb_QF75_JU04_para1' --data_type 'bb'
  # nopc
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward04' --data_type 'bb' --train_strategy 'nopc' --weights_dir '/data2/lily/output/efficient-YedrouNet-steg/20220628-bb_QF75_JU04_para1'
  
  # pcnopc para4
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward04' --work_name 'bb_QF75_JU04_para4' --data_type 'bb' --train_strategy 'pcnopc' --para 'para4'
  
  # pcnopc para2
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward04' --work_name 'bb_QF75_JU04_para2' --data_type 'bb' --train_strategy 'pcnopc' --para 'para2'
  
  # pcnopc_conti para1
   python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward04' --work_name 'bb_QF75_JU04_para1_continue' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
  
  ```
  
  ```python
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward02/' --work_name 'bb_QF75_JU02_para1' --data_type 'bb'

  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward02' --data_type 'bb' --train_strategy 'nopc' --weights_dir '/data2/lily/output/efficient-YedrouNet-steg/20220628-bb_QF75_JU02_para1'
  ```

  ```python
  # 增加 bb_qf75ju04的单通道 从头训练
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward04' --work_name 'bb_QF75_JU04_scratch_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
  ```
  
  
  
* QF95-JU 0.4, 0.2    0.58  0.5
  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95

  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_juniward04

  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_juniward02

  ``` python
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_juniward04' --work_name 'bb_QF95_JU04_para1' --data_type 'bb'
  
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_juniward04' --data_type 'bb' --train_strategy 'nopc' --weights_dir '/data2/lily/output/efficient-YedrouNet-steg/20220628-bb_QF95_JU04_para1'
  ```

  ``` python
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_juniward02' --work_name 'bb_QF95_JU02_para1' --data_type 'bb'
  
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_juniward02' --data_type 'bb' --train_strategy 'nopc' --weights_dir '/data2/lily/output/efficient-YedrouNet-steg/20220628-bb_QF95_JU02_para1'
  ```

  

* QF75-UERD 0.4, 0.2    0.93  0.78

  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd04
  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd02

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd04' --work_name 'bb_QF75_UERD04_para1' --data_type 'bb'
  
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd04' --data_type 'bb' --train_strategy 'nopc' --weights_dir '/data2/lily/output/efficient-YedrouNet-steg/20220628-bb_QF75_UERD04_para1'
  ```

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd02' --work_name 'bb_QF75_UERD02_para1' --data_type 'bb'
  
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd02' --data_type 'bb' --train_strategy 'nopc' --weights_dir '/data2/lily/output/efficient-YedrouNet-steg/20220628-bb_QF75_UERD02_para1'
  ```

  

* QF95-UERD0.4, 0.2    0.79  0.61
  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_uerd04

  /data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_uerd04

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_uerd04' --work_name 'bb_QF95_UERD04_para1' --data_type 'bb' --train_strategy 'pcnopc'
  ```

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95_uerd02' --work_name 'bb_QF95_UERD02_para1' --data_type 'bb' --train_strategy 'pcnopc'
  ```

  

### ALA   

全是mat文件，所以读取数据那里需要更改

* QF75-JU 0.4, 0.2
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qf75   mat
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_ju04  mat
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_ju02   mat

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_ju04' --work_name 'ala_QF75_JU04_para1' --data_type 'ala' --train_strategy 'pcnopc' --para 'para1'
  ```

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_ju02' --work_name 'ala_QF75_JU02_para1' --data_type 'ala' --train_strategy 'pcnopc' --para 'para1'
  ```

  

* QFVari-JU 0.4, 0.2   mat
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious_ju04
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious_ju02

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious_ju04' --work_name 'ala_QFvari_JU04_para1' --data_type 'ala' --train_strategy 'pcnopc' --para 'para1'
  ```

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious_ju02' --work_name 'ala_QFvari_JU02_para1' --data_type 'ala' --train_strategy 'pcnopc' --para 'para1'
  ```

  

* QF75-UERD 0.4, 0.2   mat
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_uerd04
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_uerd02

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_uerd04' --work_name 'ala_QF75_UERD04_para1' --data_type 'ala' --train_strategy 'pcnopc' --para 'para1'
  ```

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_uerd02' --work_name 'ala_QF75_UERD02_para1' --data_type 'ala' --train_strategy 'pcnopc' --para 'para1'
  ```

  

* QFVari-UERD 0.4, 0.2   mat
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious_uerd04
  /data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious_uerd02

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious_uerd04' --work_name 'ala_QFvari_UERD04_para1' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious_uerd02' --work_name 'ala_QFvari_UERD02_para1' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

  

* QF95-JU 0.4

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf95' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf95_ju04' --work_name 'ala_QF95_JU04_para1' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

  

* QF95-JU0.2

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf95' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf95_ju02' --work_name 'ala_QF95_JU02_para1' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

* QF95-UERD04

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf95' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf95_uerd04' --work_name 'ala_QF95_UERD04_para1' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

  







```
    def read_file(self, cover_listfile, stego_listfile, cover_dir, stego_dir, seed=None):

        with open(cover_listfile) as cf:
            cover_lines = cf.readlines()
        with open(stego_listfile) as sf:
            stego_lines = sf.readlines()
        assert len(cover_lines) == len(stego_lines)

        stego_list = [os.path.join(stego_dir, a.strip()) for a in stego_lines]
        cover_list = [os.path.join(cover_dir, a.strip()) for a in cover_lines]

        # image_label_list = []
        cover_path_list = []
        stego_path_list = []

        for cover_file, stego_file in zip(cover_list, stego_list):
            if "moxb" in cover_file and "BOWS2_" in cover_file:
                cover_file = cover_file.replace("BOWS2_", "BOWS_")
            if "moxb" in stego_file and "BOWS2_" in stego_file:
                stego_file = stego_file.replace("BOWS2_", "BOWS_")
            # image_label_list.append([cover_file, 0])
            # image_label_list.append([stego_file, 1])
            cover_path_list.append([cover_file, 0])
            stego_path_list.append([stego_file, 1])
        return cover_path_list, stego_path_list

    def nopc(self):
        # 在保证每批类别balance的情况下，去掉成对约束
        random.shuffle(self.cover_list)
        random.shuffle(self.stego_list)
```



训练0.81 验证0.83 测试0.82

F.softmax和torch.nn.Softmax效果一样，放在计算acc前后结果也一样。

使用sigmoid会和使用softmax略有区别 wauc会高0.01，pmd会高0.002，acc一样

使用outputs.sigmoid().squeeze(1)和使用torch.nn.functional.sigmoid输出相同。

使用 outputs.softmax(dim=1)[:, 1:].sum(dim=1)和使用普通的softmax相同。





## JPEG domain B4

### BB_qf75

- [ ] bb_qf75_ju04

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward04' --work_name 'bb_QF75_JU04_b4_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

- [ ] bb_qf75_ju02

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_juniward02' --work_name 'bb_QF75_JU02_b4_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

- [ ] bb_qf75_uerd04

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd04' --work_name 'bb_QF75_UERD04_b4_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

- [ ] bb_qf75_uerd02

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75_uerd02' --work_name 'bb_QF75_UERD02_b4_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

- [ ] bb_256_qf75_uerd02_fixkey

   ``` python
   python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/fix_key/bb_256_qf75_uerd02_fixkey' --work_name 'bb256_QF75_UERD02_fixkey_b4_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
   ```

- [ ] bb_256_qf75_uerd04_fixkey

   ``` python
   python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/fix_key/bb_256_qf75_uerd04_fixkey' --work_name 'bb256_QF75_UERD04_fixkey_b4_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
   ```

 - [ ] bb_256_qf95_uerd04_fixkey

   ``` python
   python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/fix_key/bb_256_qf95_uerd04_fixkey' --work_name 'bb256_QF95_UERD04_fixkey_b4_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
   ```

### ala

- [ ] ala_qf75_ju04

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_ju04' --work_name 'ala_QF75_JU04_b4_para1' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

- [ ] ala_qf75_ju02

  ```
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_ju02' --work_name 'ala_QF75_JU02_b4_para1' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'
  ```

- [ ] ala_qf75_uerd04

  ```
  
  ```


- [ ] ala_qfvari_uerd02
  ``` python
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qfvarious_uerd04' --work_name 'ala_QFVari_UERD04_b4_para1' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'
  ```





## bb_256_qf75_ju04_rand_seed

/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/bb_256_qf75_ju04_rand_seed/

KeNet  不跑jpeg域的 /data2/liqs/datasets/BOSS_BOWS2/bb_256_hill04_randseed

```
python train.py --cover-dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego-dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_hill04_randseed/' --list-file './src/datas/bb/spatial' --model 'kenet' --ckpt-dir '/data2/lily/output/SiaStegNet/bb256_hill04_randseed' --gpu-id 6 --data-type 'bb'
```

B0

```
python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/bb_256_qf75_ju04_rand_seed' --work_name 'bb_QF75_JU04_rand_seed_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
```

B4

```
python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/bb_256_qf75_ju04_rand_seed' --work_name 'bb_QF75_JU04_rand_seed_b4_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
```

对比收敛结果

random seed epoch5:0.69   100:0.809

## bb_256_hill04_randseed

B0单通道随机初始化，从头训练，不使用预训练模型

```
python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_hill04_randseed' --work_name 'bb_HILL04_rand_seed_para1' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'
```




## 2022-7-17 SRNet

STD-NET项目，配置tensorflow 1.12环境时，每次增加：
   `export LD_LIBRARY_PATH="/usr/local/nvidia/lib:/usr/local/nvidia/lib64:/usr/local/cuda-9.0/lib64:/usr/local/cuda-9.0/extras/CUPTI/lib64"` 

使用 EFN-B4, SRNet训练一下数据集，

B4: fix_key 1.25*5 + 

### Fix_key

* BB-SPATIAL: /data2/liqs/datasets/BOSS_BOWS2/spatial/fix_key/
  * bb_256_mipod02_fixkey  B4:done SRNet:done
  * bb_256_mipod04_fixkey  B4:done SRNet:done

   ```python
   python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/fix_key/bb_256_mipod04_fixkey' --work_name 'bb256_mipod04_fixkey_para1' --data_type 'bb_spatial' --train_strategy 'pcnopc_conti' --para 'para1'

   python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/fix_key/bb_256_mipod02_fixkey' --work_name 'bb256_mipod02_fixkey_para1' --data_type 'bb_spatial' --train_strategy 'pcnopc_conti' --para 'para1'


   python 1_train_val.py --log_root 'bb_256_mipod04_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/fix_key/bb_256_mipod04_fixkey/'
   /data2/lily/output/STD-NET/bb_256_mipod04_fixkey/20220720-1_train_val_srnet/models/model_397250.ckpt

   python 1_train_val.py --log_root 'bb_256_mipod02_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/fix_key/bb_256_mipod02_fixkey/'
   /data2/lily/output/STD-NET/bb_256_mipod02_fixkey/20220720-1_train_val_srnet/models/model_357000.ckpt
   ```


* BB-JPEG: /data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/fix_key/
  * bb_256_qf75_uerd02_fixkey B4:done  SRNet:done
  * bb_256_qf75_uerd04_fixkey B4:done  SRNet:done
  * bb_256_qf95_uerd04_fixkey B4:ing  SRNet:done
  ```python
  B4 已经训练结束，现在训练SRNet
  python 1_train_val.py --log_root 'bb_256_qf75_uerd04_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/fix_key/bb_256_qf75_uerd04_fixkey/'
  /data2/lily/output/STD-NET/bb_256_qf75_uerd04_fixkey/20220718-1_train_val_srnet/models/model_388500.ckpt

  python 1_train_val.py --log_root 'bb_256_qf75_uerd02_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/fix_key/bb_256_qf75_uerd02_fixkey/'
  /data2/lily/output/STD-NET/bb_256_qf75_uerd02_fixkey/20220718-1_train_val_srnet/models/model_292250.ckpt

  python 1_train_val.py --log_root 'bb_256_qf95_uerd04_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/fix_key/bb_256_qf95_uerd04_fixkey/'
  /data2/lily/output/STD-NET/bb_256_qf95_uerd04_fixkey/20220718-1_train_val_srnet/models/model_425250.ckpt

  ```

* ALA2-SPATIAL:  /data2/liqs/datasets/alaskav2/spatial/fix_key/
  * MIPOD02_fixkey
  * MIPOD04_fixkey SRNet:ing
  ```python
    python 1_train_val.py --log_root 'ala_256_MiPOD04_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego_dir '/data2/liqs/datasets/alaskav2/spatial/fix_key/MIPOD04_fixkey/' 
    /data2/lily/output/STD-NET/ala_256_MiPOD04_fixkey/20220726-1_train_val_srnet/models/model_770000.ckpt

    python 1_train_val.py --log_root 'ala_256_MiPOD02_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego_dir '/data2/liqs/datasets/alaskav2/spatial/fix_key/MIPOD02_fixkey/'
    /data2/lily/output/STD-NET/ala_256_MiPOD02_fixkey/20220727-1_train_val_srnet/models/model_658000.ckpt

  ```
* ALA2-JPEG: /data2/liqs/datasets/alaskav2/jpeg2spatial/fix_key
  * QF75_UERD02_fixkey SRNet:ing
  * QF75_UERD04_fixkey B4:ing SRNet:ing
  * QF95_UERD04_fixkey SRNet:ing
  ```python
  # QF75_UERD04_fixkey

  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/fix_key/QF75_UERD04_fixkey' --work_name 'ala256_qf75_UERD04_fixkey' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'

  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/fix_key/QF75_UERD02_fixkey/' --work_name 'ala256_qf75_UERD02_fixkey' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'

  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf95' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/fix_key/QF95_UERD04_fixkey/' --work_name 'ala256_qf95_UERD04_fixkey' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'

  python 1_train_val.py --log_root 'ala_256_qf75_UERD04_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75/' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/fix_key/QF75_UERD04_fixkey/'
  /data2/lily/output/STD-NET/ala_256_qf75_UERD04_fixkey/20220720-1_train_val_srnet/models/model_721000.ckpt

  python 1_train_val.py --log_root 'ala_256_qf75_UERD02_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75/' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/fix_key/QF75_UERD02_fixkey/'
  /data2/lily/output/STD-NET/ala_256_qf75_UERD02_fixkey/20220721-1_train_val_srnet/models/model_822500.ckpt

  python 1_train_val.py --log_root 'ala_256_qf95_UERD04_fixkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf95/' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/fix_key/QF95_UERD04_fixkey/'
  /data2/lily/output/STD-NET/ala_256_qf95_UERD04_fixkey/20220721-1_train_val_srnet/models/model_773500.ckpt

  ```



### Rand_key

* BB-SPATIAL: /data2/liqs/datasets/BOSS_BOWS2/spatial/
  * bb_256_hill02_randkey
  * bb_256_hill04_randkey
  ``` python
  python 1_train_val.py --log_root 'bb_256_HILL04_randkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_hill04_randkey/'
  /data2/lily/output/STD-NET/bb_256_HILL04_randkey/20220728-1_train_val_srnet/models/model_394625.ckpt

  python 1_train_val.py --log_root 'bb_256_HILL02_randkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/spatial/bb_256_hill02_randkey/'
  /data2/lily/output/STD-NET/bb_256_HILL02_randkey/20220728-1_train_val_srnet/models/model_372750.ckpt
  
  ```
* BB-JPEG: /data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/
  * bb_256_qf75_juniward02_randkey  B4:ing SRNet:Done
  * bb_256_qf75_juniward04_randkey  SRNet:ing B4:done
  * bb_256_qf95_juniward04_randkey  B4:ing SRNet:ing

  ``` python
  python 1_train_val.py --log_root 'bb_256_qf75_JU04_randkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/bb_256_qf75_juniward04_randkey/'
  /data2/lily/output/STD-NET/bb_256_qf75_JU04_randkey/20220721-1_train_val_srnet/models/model_420875.ckpt

  python 1_train_val.py --log_root 'bb_256_qf75_JU02_randkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/bb_256_qf75_juniward02_randkey/'
  /data2/lily/output/STD-NET/bb_256_qf75_JU02_randkey/20220725-1_train_val_srnet/models/model_371000.ckpt

  python 1_train_val.py --log_root 'bb_256_qf95_JU04_randkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95/' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/bb_256_qf95_juniward04_randkey/'
  /data2/lily/output/STD-NET/bb_256_qf95_JU04_randkey/20220726-1_train_val_srnet/models/model_390250.ckpt


  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf75' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/bb_256_qf75_juniward02_randkey/' --work_name 'bb256_QF75_JU02_randkey' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'

  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/BOSS_BOWS2/bb_256_qf95' --stego_dir '/data2/liqs/datasets/BOSS_BOWS2/jpeg2spatial/bb_256_qf95_juniward04_randkey/' --work_name 'bb256_QF95_JU04_randkey' --data_type 'bb' --train_strategy 'pcnopc_conti' --para 'para1'

  ```

* ALA2-SPATIAL:  /data2/liqs/datasets/alaskav2/spatial/
  * HILL20_randkey
  * HILL40_randkey
  ``` python
  python 1_train_val.py --log_root 'ala_256_HILL04_randkey/' --work_name '1_train_val_srnet' --cover_dir '/data2/liqs/datasets/alaskav2/ALASKA_v2_TIFF_256_GrayScale/' --stego_dir '/data2/liqs/datasets/alaskav2/spatial/HILL40_randkey/'

  ```

* ALA2-JPEG: /data2/liqs/datasets/alaskav2/jpeg2spatial/  
  * qf75_ju04_randseed
  * QF75_JUNIWARD20_randkey
  * QF95_JUNIWARD40_randkey
  ```python
  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75_ju04_randseed/' --work_name 'ala256_QF75_JU04_randkey' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'

  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf75' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/QF75_JUNIWARD20_randkey/' --work_name 'ala256_QF75_JU02_randkey' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'

  python train_dataloader_yedrouNet.py --cover_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/qf95' --stego_dir '/data2/liqs/datasets/alaskav2/jpeg2spatial/QF95_JUNIWARD40_randkey/' --work_name 'ala256_QF95_JU04_randkey' --data_type 'ala' --train_strategy 'pcnopc_conti' --para 'para1'

  ```

