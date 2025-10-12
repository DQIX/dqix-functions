# dqix-functions

dq9 function name sharing project using [resymgen](https://github.com/UsernameFodder/pmdsky-debug/blob/master/docs/resymgen.md) that comes with [pmdsky-debug](https://github.com/UsernameFodder/pmdsky-debug) project
Currently only a handful of functions from the jp version are provided

It includes some functions for division and decimal point processing, and by sharing them, the purpose is to omit some of the tedious discrimination work.
I'm not going to do a complete analysis.

See the issue for instructions on how to import.
https://github.com/DaisukeDaisukeTeam/dq9Functions/issues/2


# motivation
The dqix-functions repository was created to address the following issues:  
- Ghidra provides only limited and unintuitive methods for exporting information such as function names  
- Due to unexpected failures or other incidents, the analysis environment may need to be replaced frequently, which can result in the loss of previously identified functions  
- By importing this repository into Ghidra, simply viewing a function’s name can help infer its behavior, reducing the need to inspect the function’s implementation directly  

# Why resymgen?
- There are limited legal methods for sharing function information, and could unintentionally lead to noncompliant distribution.
- For this reason, resymgen was chosen as the format for sharing function data.
- Also, resymgen comes with a formatter that allows for clean merging.

# Contributing 
- Create a fork of this repository   
- Configure Git on your computer and clone the forked repository   
- Edit the symbol files in YAML format using your preferred text editor, such as VS Code, Sublime Text 4, or PhpStorm (paid)   

> [!TIP]
> (Optional, for Windows users) Download resymgen from the link below to check for syntax errors and reformat symbol files automatically    
> https://drive.google.com/file/d/1ISqVxmTNlTDi5T2dlgCUzkvo760OGkcZ/view?usp=sharing  
> Syntax errors can be checked with the following commands:  
```
resymgen.exe fmt .\symbols\arm9_ram.yml
resymgen.exe fmt .\symbols\arm9_field.yml
resymgen.exe fmt .\symbols\arm9_battle.yml
```

- Commit your changes  
- Open a pull request to request review and merging

# 

# Glossary

## LCG
Abbreviation for Linear congruential generator.
It is a random number that is updated using the following formula.
```
((seed * constant1) + constant2) mod (2^64 or 2^32)
```
https://en.wikipedia.org/wiki/Linear_congruential_generator
<!--
Linear congruential generatorの略です。
次のような式で更新される乱数です
(seed * 定数1) + 定数2 mod (2^64 or 2^32)
-->

## AT
This is the first random number in the game where the 32-bit LCG algorithm is used. It is used for the following purposes
- Monster spawn
- Field-related random numbers
- In-game animations
- Monster treasure chest drops
- Symbol movement direction determination
- Monster determination of encounters at sea
- Lottery for too stunned to move

will be updated with the following expression
```
rand = ((rand * 0x41C64E6D) + 0x3039 & 0xFFFFFFFF)
```
<!--
32bit LCGアルゴリズムが採用されているゲーム内の1つ目の乱数です。下記の目的で使用されています。
- モンスタースポーン
- フィールド関連の乱数
- ゲーム内のアニメーション
- モンスターの宝箱ドロップ
- シンボルの移動方向決定
- 海上のエンカウントのモンスター決定
- too stunned to moveの抽選
次の式で更新されます
-->

## BT

It is the second random number in the game and uses a 64-bit LCG algorithm.
In Japan, it is called a Hoimi table.
AT and BT share the same seed value, and in 3ds the initial seed is very narrow and the way it advances is predictable and consistent, allowing the initial seed to be identified and tracked

Some of the intended uses are
- Lottery for the great alchemical success
- Recovery on feeds
- Rewards from the Demon King or Map Boss
- Lottery for items from the Fountain
- Shuffling of member gestures and lottery for delayed animation start period
- Lottery for the number of encounters and types of the second and subsequent groups on the ground
- Consumed during super special moves
- Lottery for Zaoral
- Lottery for flee
- Lottery for messages of examine command
- Lottery for name plate color
- The number of enemies in normal encounters and the drawing of friends

will be updated with the following expression
```
seed(64bit) = ((seed * 0x5d588b656c078965) + 0x269ec3) & 0xFFFFFFFFFFFFFFFF
```

<!--
ゲームの2番目の乱数で、64-bit LCGアルゴリズムが採用されています。
日本ではホイミテーブルと呼ばれています。
ATとBTは同じシード値を共有し、3dsでは初期シードの幅が非常に狭く、進み方が予測可能で一貫性があるため、初期シードを特定して追跡することができます
使用目的の一部は次の通りです
- 錬金大成功のある錬金の抽選
- フィード上での回復
- 魔王or地図ボスの報酬
- 泉のアイテムの抽選
- メンバーのしぐさのシャッフルと、アニメーション開始遅延期間の抽選
- 地上での遭遇の数と、2グループ目以降の種類の抽選
- 超必殺技時に消費する
- ザオラルの抽選
- 逃げるの抽選
- examineコマンドのメッセージ抽選
- ネームプレートのカラーの抽選
- 通常エンカウントの敵の数や、仲間の抽選
次の式で更新されます
-->

### CT

It is the third random number in the game and is used to randomize combat actions.
An initial seed is generated at the start of each battle and is 48 bits wide, making it difficult to predict.
It is also used as a camera-related random number.

will be updated with the following expression
```
seed(64bit) = ((seed * 0x5d588b656c078965) + 0x269ec3) & 0xFFFFFFFFFFFFFFFF
```


<!--
ゲームの3番目の乱数で、戦闘の行動の乱数に使用されています。
また、カメラ関連の乱数としても使用されています。

戦闘開始時に毎回初期シードが生成され、32bitの幅を持つため、予測することは困難です。
次の式で更新されます
-->
