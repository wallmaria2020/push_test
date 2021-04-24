## 何ができるのか
Audio-Driven Emotional Video Portraits 
ビデオポートレート、Emotional Video Portraits（EVP）により、音声情報とターゲットビデオが与えられると、感情をコントロールし、自由に感情を変更できる、被写体が話せるポートレート(ビデオ)を作成できる。

本研究では、音声情報によって働く感情的でダイナミックで高品質なビデオポートレートを作成するためのシステム、EVPを提案している。

### 先行研究
音声情報と顔の一部のパーツ（口など）との関係に着目した方法のみ。

自然な人間の顔の最も重要な特徴の1つである顔と対応する感情には、注目されていない。

ビデオベースの感情操作可能な顔生成ツールは存在しない。

### EVPシステムを成功させる上での問題点
・音声情報から感情要素を抽出するのは難しい

・音声情報から作成した顔情報と元のターゲットビデオ内の顔情報とを組み合わせるのは難しい←大きく異なる顔の動きになる可能性があるため

### 提案手法での重要な点
EVPでは、2つの重要な構成要素、
Cross-Reconstructed Emotion Disentanglement  

Target-Adaptive Face Synthesis. 

を用いることで、音声情報ベースのポートレート上の感情操作が可能となっている。

### Cross-Reconstructed Emotion Disentanglement
ターゲットビデオからランドマーク情報を、音声情報から意味的要素と感情要素を抽出
そして感情的でダイナミックな2D顔画像のランドマークを生成する。図3を参照

![cr1](https://user-images.githubusercontent.com/75981777/115942337-b6204900-a4e4-11eb-9a79-c744ccbdf6ff.png)

### Target-Adaptive Face Synthesis
ここでは、新しく生成された顔のランドマークをターゲットビデオの自然な頭の向きに合わせる。

音声情報では向き情報が与えられないため、生成されたランドマークと、ターゲットビデオの頭のポーズや動きの大きな変動との間にギャップがある。

そのため、3D対応の位置合わせアルゴリズムを使用して、2Dランドマークをターゲットビデオに投影する。次に、Edge-to-Videoネットワークを学習させ、高品質の感情的なビデオポートレートを生成。図2の右部分を参照

![tafs](https://user-images.githubusercontent.com/75981777/115942563-d7356980-a4e5-11eb-8132-7820f066ec61.png)


### 　実験方法
60人の俳優/女優と8つの感情カテゴリを持つ高品質の感情的な視聴覚データセットでるMEADで、評価した。

比較として、3つの既存手法を引用した。

1.The method of Chen et al. Lele Chen, Ross K Maddox, Zhiyao Duan, and Chenliang Xu. Hierarchical cross-modal talking face generation with dynamic pixel-wise loss. In Proceedings of the IEEE Confer- ence on Computer Vision and Pattern Recognition (CVPR), 2019. 2, 3, 5, 6, 7

2.The method of Song et al. Linsen Song, Wayne Wu, Chen Qian, Ran He, and Chen Change Loy. Everybody’s talkin’: Let me talk as you want. arXiv preprint arXiv:2001.05201, 2020. 2, 3, 6, 7

3.Wang et al. Kaisiyuan Wang, Qianyi Wu, Linsen Song, Zhuoqian Yang, Wayne Wu, Chen Qian, Ran He, Yu Qiao, and Chen Change Loy. Mead: A large-scale audio-visual dataset for emotional talking-face generation. In ECCV, 2020. 2, 3, 4, 6, 7, 8 

1は、ランドマークに基づいて顔の動きを合成し、attention技術を使用して品質向上させる画像ベースの方法
2は、3D顔モデルを適用して音声ベースのビデオポートレートの編集を実現する動画ベースの方法です。
3は、感情を操作する能力を備えた最初の話す顔生成アプローチを提案する最も関連性の高い方法

## タスクの処理精度

評価方法

定性的評価

合成された話す顔のビデオが現実的であるかどうか、顔の動きが音声と同期するかどうか、生成された顔の感情の正確さという3つの異なる基準で評価された。

(1)	視聴覚同期とビデオ品質に基づいて特定のビデオを判断、1から5のスコアで被験者が評価

(2)	背景音のない本当の感情的なビデオクリップを見せた後、音声なしで生成されたビデオの感情カテゴリを選択しその精度を確認

提案手法は既存手法よりも評価が高かった。

![evp_result1](https://user-images.githubusercontent.com/75981777/115942371-f1bb1300-a4e4-11eb-8a08-d0c947191d09.png)

定量的評価

提案手法が、既存手法よりも有効であると判断した。
視聴覚同期の精度 (M-LD, M-LVD)、顔の表情の精度 (F-LD, F-LVD) 
動画の精度 (SSIM, PSNR, FID). 

![evp_result2](https://user-images.githubusercontent.com/75981777/115942376-f4b60380-a4e4-11eb-8f72-6cad51018b1f.png)

EVPで導入した4つの損失関数の、精度への寄与を評価
評価結果から、導入した全ての損失関数が有効であったと判断した。
Lcla: 分類損失(様々ある感情要素の中から抽出させる感情要素に対して働く)
Lcon:コンテンツ損失（音声サンプルの使用量を制御させるために働く）
Lself:自己再構成損失（エンコーダー、デコーダーにより働く）
Lcross:相互再構成損失（複数ある音声サンプルによって再構成させる場合に働く）
Lcla 　式4、Lcon 式5、Lself 式3、Lcross 式２

![result_loss](https://user-images.githubusercontent.com/75981777/115942465-61310280-a4e5-11eb-948f-85a670478cb8.png)

## 導入する際のコストについて
ソースコードについて
著者らが、近日中にソースコードの公開を行う予定
