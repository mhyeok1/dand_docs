---
layout: default
title: How-to-use
parent: Userguide
nav_order: 2
---

# Example

This page provides a general overview of dandelion, which can efficiently create extensive database with sampling chemical compound space near transition state from the example of 5 mother structures below.
We will focus on how to expand dataset from several mother structures.
<img width="628" alt="all" src="https://github.com/user-attachments/assets/6ff5bf37-7ce5-4980-a268-ee0f1d2c185d">


First, we have to prepare some mother structures to initiate. This is achieved through
9 geometry optimization using the GFN2-xTB level of theory on the PES, providing stable molecular
10 configurations as a starting point. Make sure that all of your prepared mother structures are in specific `input_path`. 

```
├── Cl7138
│   └── ClGeom-m7138-i1-c1-opt
│       └── struc.xyz
├── Cl7164
│   └── ClGeom-m7164-i1-c1-opt
│       └── struc.xyz
├── Cl7166
│   └── ClGeom-m7166-i1-c1-opt
│       └── struc.xyz
├── Cl7168
│   └── ClGeom-m7168-i1-c1-opt
│       └── struc.xyz
└── Cl7188
    └── ClGeom-m7188-i1-c1-opt
        └── struc.xyz
```

To run dandelion, your current conda environment should be **ts**, and you should move your current directory ( working directory ) to path where dandelion_sample.py is located.

You can enter this in terminal for more information:
``` python
$ dandelion_sample -h

usage: dandelion_sample [-h] -i INPUT_PATH -o OUTPUT_PATH -n MAX_WORKERS

Do SEGSM and NEB from mother structures, Other parameters can be set in each
modules

options:
  -h, --help            show this help message and exit
  -i INPUT_PATH, --input_path INPUT_PATH
                        Input path of mother structures
  -o OUTPUT_PATH, --output_path OUTPUT_PATH
                        Output path of dandelion
  -n MAX_WORKERS, --max_workers MAX_WORKERS
                        Number of worker processes
```

If you enter this:
```python
python dandelion_sample -i /path/to/your/prepared/mother/structures -o /path/to/the/output/you/want -n workers
```

Total 6 steps below will be executed automatically.

```


                                                     `;:`  BREAK 1 2
                                         .;:;         /    BREAK 3 4
        _____                   _      _;::;         `     ADD 1 3
        |  __ \                | |    | |';:;'
        | |  | | __ _ _ __   __| | ___| |  _  ___  _ __
        | |  | |/ _` | '_ \ / _` |/ _ \ | | |/ _ \| '_ \
        | |__| | (_| | | | | (_| |  __/ | | | (_) | | | |
        |_____/ \__,_|_| |_|\__,_|\___|_| |_|\___/|_| |_|

                   Chemical compound space sampling
           near transition state using xTB, SE-GSM and NEB
                          Ver. 0.6.2 by mlee



```

First step is to create GSM. In this process, dandelion generates possible driving coordinates(seeds) from each mother structures.

```
╔════════════════════════════════════════════════════════════════════╗
║                          1. Creating GSM                           ║
╚════════════════════════════════════════════════════════════════════╝

Arguments provided:
  input_path: /home/jjy1031/example/mother_strucs
  output_path: /home/jjy1031/example/outputs/1_gsm
  maxbreak: 2
  maxform: 2
  maxchange: 3
  minbreak: 0
  minform: 0
  minchange: 1
  ignore_single_change: True
  equiv_Hs: False

280 Seeds were generated from ClGeom-m7138-i1-c1-opt
276 Seeds were generated from ClGeom-m7164-i1-c1-opt
275 Seeds were generated from ClGeom-m7166-i1-c1-opt
276 Seeds were generated from ClGeom-m7168-i1-c1-opt
299 Seeds were generated from ClGeom-m7188-i1-c1-opt

Creating GSM finished!

```
The file `ISOMERS.txt` in created output folder will store some information about where bonds should be added or broken to generate possible driving coordinates from mother structures.

```
BREAK 5 6
ADD 4 6
ADD 3 6
```

Second step is to run GSM.

```
╔════════════════════════════════════════════════════════════════════╗
║                           2. Running GSM                           ║
╚════════════════════════════════════════════════════════════════════╝

Arguments provided:
  input_path: /home/jjy1031/example/output/1_gsm
  max_workers: 30

GSM on seeds: 100%|████████████████████████| 1406/1406 [4:57:13<00:00]
GSM finished!

```

Third step is to filter GSM. In this step, dandelion excludes some trivial pathways with strictly uphill energy trajectories, negligible energy variations, unfeasible structures, or those that are repetitive.

```
╔════════════════════════════════════════════════════════════════════╗
║                          3. Filtering GSM                          ║
╚════════════════════════════════════════════════════════════════════╝

Arguments provided:
  input_path: /home/jjy1031/example/outputs/1_gsm
  output_path: /home/jjy1031/example/outputs/2_gsm_filtered
  barrier_min: 5
  barrier_max: 200
  delta_e_min: 5

◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢
   mother: ClGeom-m7138-i1-c1-opt
Initial seeds:                   280
GSM success reactions:           115
Profile filtered reactions:       41
Structure filtered reactions:     38
Unique reactions:                 28

◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢
   mother: ClGeom-m7164-i1-c1-opt
Initial seeds:                   276
GSM success reactions:           111
Profile filtered reactions:       42
Structure filtered reactions:     38
Unique reactions:                 32

◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢
   mother: ClGeom-m7166-i1-c1-opt
Initial seeds:                   275
GSM success reactions:            96
Profile filtered reactions:       31
Structure filtered reactions:     28
Unique reactions:                 25

◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢
   mother: ClGeom-m7168-i1-c1-opt
Initial seeds:                   276
GSM success reactions:            87
Profile filtered reactions:       37
Structure filtered reactions:     35
Unique reactions:                 29

◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢◤◢
   mother: ClGeom-m7188-i1-c1-opt
Initial seeds:                   299
GSM success reactions:           102
Profile filtered reactions:       40
Structure filtered reactions:     33
Unique reactions:                 30

Filtering GSM finished!

```

The output generated from each mother structure is stored in `2_gsm_filtered`

```
├── ClGeom-m7138-i1-c1-opt
│   ├── gsm0019
│   │   ├── product.png
│   │   ├── product.xyz
│   │   ├── reactant.png
│   │   ├── reactant.xyz
│   │   ├── string.png
│   │   ├── string.xyz
│   │   ├── ts.png
│   │   └── ts.xyz
│   ├── gsm0043
│   │   ├── product.png
│   │   ├── product.xyz
│   │   ├── reactant.png
│   │   ├── reactant.xyz
│   │   ├── string.png
│   │   ├── string.xyz
│   │   ├── ts.png
│   │   └── ts.xyz
...

```

Fourth step is to run NEB. We can optimize some energy path using the concept of maximum force.

```
╔════════════════════════════════════════════════════════════════════╗
║                           4. Running NEB                           ║
╚════════════════════════════════════════════════════════════════════╝

Arguments provided:
  input_path: /home/jjy1031/example/output/2_gsm_filtered
  output_path: /home/jjy1031/example/output/3_neb
  max_workers: 30
  n_images: 10
  neb_fmax: 0.5
  cineb_fmax: 0.05
  steps: 500

Seeds: 100%|███████████████████████████████████| 144/144 [04:34<00:00]
xTB-NEB completed!

```

Fifth step is to filter some data. For example, the reaction that doesn't converge, 너무 붙어있는 데이터, 전이상태가 아닌 데이터 

```

╔════════════════════════════════════════════════════════════════════╗
║                          5. Filtering NEB                          ║
╚════════════════════════════════════════════════════════════════════╝



Arguments provided:
  input_path: /home/jjy1031/example/output/3_neb
  output_path: /home/jjy1031/example/output/4_neb_filtered

Mothers: 100%|█████████████████████████████████████| 5/5 [00:20<00:00]

40/53 rxns were saved to /home/jjy1031/example/output/4_neb_filtered/reactions.json
Filtering NEB finished!

```

Sixth step is to :
```

╔════════════════════════════════════════════════════════════════════╗
║                        6. Compiling samples                        ║
╚════════════════════════════════════════════════════════════════════╝



Arguments provided:
  input_path: /home/jjy1031/example/output/4_neb_filtered/reactions.json
  output_path: /home/jjy1031/example/output/xtb.h5
  fmax_threshold: 0.1

Compiling reactions: 100%|███████████████████████| 40/40 [00:03<00:00]
Compiling finished!

```

And there will be generated files in your output path.
We can easily see using tree :

```
(ts) [jjy1031@ne00 example]$ tree -L 2
.
├── 0_mothers.zip
├── mother_strucs
│   ├── Cl7138
│   ├── Cl7164
│   ├── Cl7166
│   ├── Cl7168
│   └── Cl7188
└── outputs
    ├── 1_gsm
    ├── 4_neb_filtered
    ├── xtb.h5
    └── xtb.h5.index.json
```

In outputs/1_gsm, there are many seeds generated by segsm from each mother structures

```
(ts) [jjy1031@ne00 outputs]$ tree -L 3
├── 1_gsm
│   ├── ClGeom-m7138-i1-c1-opt
│   │   ├── gsm0000
│   │   ├── gsm0001
│   │   ├── gsm0002
│   │   ├── gsm0003
│   │   ├── gsm0004
│   │   ├── gsm0005
│   │   ├── gsm0006
...
├── ClGeom-m7168-i1-c1-opt
│   │   ├── gsm0000
│   │   ├── gsm0001
│   │   ├── gsm0002
│   │   ├── gsm0003
│   │   ├── gsm0004
│   │   ├── gsm0005
│   │   ├── gsm0006
│   │   ├── gsm0007
...

```
In outputs/4_neb_filtered :
```
├── 4_neb_filtered
  └── reactions.json
```

Next step is to execute dandelion_refine.
You can enter -h or --help if u want more information:

```
(ts) [jjy1031@ne00 dandelion]$ dandelion_refine -h
usage: dandelion_refine [-h] -i INPUT_PATH -n MAX_WORKERS --orca ORCA

Refine force on obtained samples, Other parameters can be set in each modules

options:
  -h, --help            show this help message and exit
  -i INPUT_PATH, --input_path INPUT_PATH
                        Input path of working directory containing xtb.h5
  -n MAX_WORKERS, --max_workers MAX_WORKERS
                        Number of worker processes
  --orca ORCA           Path of the orca binary file
```

~~Make sure that the path of orca should be an executable file~~
If you enter like this:
```
(ts) [jjy1031@ne00 dandelion]$ dandelion_refine -i /home/jjy1031/example/outputs -n 10 --orca path/to/your/orca/executable/file
```

2 steps below will be executed automatically !
```
          ⢀⣀⣀⣀⣀⣀⡀       ⢀⢀⣀⢀⠞⠖⠁⠡⡂⡆ ⡠⢀⡀
         ⠺⢿⣿⣿⣿⣿⣿⣿⣷⣦⣠⣤⣤⣤⣄⣀⣀ ⡏⢸  ⢀ ⠣⠈ ⡠⡋⡨⡋⡂
           ⠙⢿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣷⣦⣄⡀⡎⢀⡰⢀⢎⠌⢀⠔⣐⠠⣄⣀
       ⢀ ⡔⢀⣴⣾⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⠿⠿⠿⣿⣿⣷⣄⠂ ⢊⠎ ⠠⠂⡀⠕⠌⠌ ⡄⡠⢄
    ⢀⡆⠄⠁⢈⢠⣾⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣀   ⣀⣿⣿⣿⣆⠐    ⡨⠒⠁⡀⢠⣦⠍⠇⡀⢲⠂⡄⠄
   ⠨⡀⠑⡈ ⢠⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⡄   ⠈  ⣬⠠⣰⣿ ⢳⢹⡄⡆⠄⢀⢼
 ⡄⠱⠈⠁⠑⢄⠐⣾⣿⣿⡿⠋⠁⣀⣠⣬⣽⣿⣿⣿⣿⣿⣿⠿⠿⠿⠿⠿⠿⠿⠿⠟⠁⡟⣅⡢⠁⠠⠜⡄⡑⢌⢧⡀ ⡀⣰⢁⡐⢁⢄⣡⣧⡤⠄
⠠⡐⠓⠂⠌  ⢀⣿⣿⡏⢀⣴⣿⠿⠛⠉⠉⠶⢸⣿⣿⠿⠁⠢⠨⢀⣻⣿⣿⣿⣿⢟⣿⣝⠂  ⠠⡠⢆⠈⡂⠱⡇ ⣅⠫⠂⡠⢂⡪⠋  ⠁⡆
⡶⠉ ⢀⡀⠁⡁⢸⣿⣿⢠⣾⡟⠁⣿⣿⡇ ⢀⠈⠉⠁    ⣀⠷⣹⣏⣷⢏⠹⠁    ⠈⢈ ⢇ ⢸⠱⢸⡏⡀⡶⡸⠎  ⠰⠁⡸
⢈⡕⡈⠁⠐⠂⢀⢸⣿⣿⣾⠏⣿⣿⡿⣻⣿⢞⡢⠄ ⠈ ⡀⡤⠂⠁⠉⠌       ⢀⢀⠠⠐⢄ ⡀⢆⠎⢹⣶⣷⣧⡈⠈⠉⠤⠂⠉⢀⠱⡀
⢠⡊    ⠁⣸⣿⣿⣿⣀⠉⡻⡏⠋⠁ ⠁⠒⠒⡀⣍⠍⠁ ⡀ ⢠⠂     ⢀⠈⠄⢀⠄⡒⠅⠈⢄⢡ ⢿⣿⣷⣿⡄ ⠐⠄⠤ ⠜⢀
⠐⠁ ⠤⠒⢠⣾⣿⣿⣿⣿⣿⣷⣄⢄  ⢀ ⡏ ⢰⣃⠊⡐⠐⠁⢀⠈  ⣀ ⠰⠢⢀⠂⡰⠈⠂  ⡱⠂⢂⡇⡈⠻⢿⣿⠇   ⡤⠄⣀⡰⠁
    ⠁⣾⣿⣿⣿⣿⣿⣿⣿⣿⣦ ⠄ ⠉   ⠸⠫⢞⠈⣰⠈ ⡐⢲⣿⡏       ⢠⡾ ⣀⠊⢱ ⠠⡀    ⢈⢀⡐⠤⣕⡄
    ⢰⣿⡿⠛⠉   ⠈⠙⠛         ⠈⠈ ⠻⠔⠁⢸⡍⡇      ⢀⣏ ⢀⠠⠆ ⠣⡀⠈⡠⡀⠉⠢⡤⠢⣈⡡⣢⠦
⠈⠁           ⢻⣇               ⢸⡇⡇      ⣼⡿⠉  ⢀⡇ ⠑⡄⠑⣌⢄ ⠙⢄⠠⡪⣅
             ⠈⣾⡆              ⢸⣏⡇     ⢠⣿⠇   ⠸⢌⢢⢄⡠⠣⠈⠢⡁⡈⣎⢢⡬⠃

               Energy refinement on samples using orca
                          Ver. 0.6.0 by mlee
```

```
╔════════════════════════════════════════════════════════════════════╗
║                         7. Refining forces                         ║
╚════════════════════════════════════════════════════════════════════╝



Arguments provided:
  input_path: /home/jjy1031/example/outputs/xtb.h5
  output_path: /home/jjy1031/example/outputs/wb97x.db
  max_workers: 10
  orca: ORCA

Created db file at /home/jjy1031/example/outputs/wb97x.db

Formulas: |                                 | 0/0 [00:00<?, ? hour/it]
wB97X calculation finished!
```

```
╔════════════════════════════════════════════════════════════════════╗
║                     8. Compiling final samples                     ║
╚════════════════════════════════════════════════════════════════════╝



Arguments provided:
  input_path: /home/jjy1031/example/outputs/wb97x.db
  output_path: /home/jjy1031/example/outputs/wb97x.h5

Compiled successfully!
```





