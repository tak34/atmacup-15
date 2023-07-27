2023年7月に行われた機械学習コンペatmacup#15で10位（661チーム中）となった。その解法をまとめる。
https://www.guruguru.science/competitions/21/

# 概要
seenとunseenそれぞれに対しモデルを作成した。
  
# 特徴抽出
- 人数に関する情報（membersなど）で比をとったもの
- エピソード数（100でclip）
- 放送開始・終了の年と、両者の差
- genres, studiosのone-hot encoding
- producersとlicensersのone-hotをSVDで次元削減したもの（参照：https://www.guruguru.science/competitions/21/discussions/7885664e-2acd-4191-833f-e1b21d34afc4/）
- user-id, type, source, ratingのtarget encoding（user-idのtarget encodingはseenのみに使用）  
- trainとtest全データにおけるuserが視聴したanimeのone-hot encodingを行い、SVDで次元削減（n_components=10）したもの
- animeを原作ごとにグループ化し、anime2vecを行ったもの  
  （参照①：https://www.guruguru.science/competitions/21/discussions/04b127cc-c527-4f9d-9057-109ea54a05eb/）  
  （参照②：https://www.guruguru.science/competitions/21/discussions/b46b4dda-6ce7-4a9b-99f9-0c3caabb3ba7/）

# モデリング
lightGBM

# 後処理
seed違いとパラメータ違いで予測結果を出しアンサンブル。

# その他：Shake upについて
本解法の予測結果は、上位50位の中で一番shake up量が大きかった（+24位）。考えられる原因として、今回作成したモデルはunseenのCVスコアがfoldごとに大きく異なっていた（max: 1.430, min: 1.357）。publicでは下振れを引いていて、privateでは上振れを引いていた？


  
