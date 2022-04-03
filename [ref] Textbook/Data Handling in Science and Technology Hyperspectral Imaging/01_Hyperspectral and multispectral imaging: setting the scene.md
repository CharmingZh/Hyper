### Images: basic concepts, spatial resolution, and spectral information

- `Images`：图像是由任何有能力以相关方式获得表面 `X-Y` 方向信息的设备产生的表面的双维表示；
- `Pixel`：像素是图像的空间细分，它们是独特的信息片段，也是空间相关的。换句话说，一个像素受到周围像素的影响，其中一个像素包含的信息取决于周围像素包含的信息；
- `Spatial Resolution`：空间分辨率必须始终以相对距离来衡量。例如，有一个 $10×10$ 像素的相机。如果该相机拍摄的是 $10×10$ 米的照片，实际的空间分辨率应该说是 $0.01 m^2$；而如果同一台相机拍摄的是 $0.5\times0.5m$ 的照片，空间分辨率应该说是 $0.0025m^2$。这将直接解决设定每个像素所包含的表面信息的面积的需要。

### Spectral information: types of images

有不同的方法来对图像进行分类，其中一个方法是考虑一个像素所能包含的信息量。这些信息可能来自于相对于参照物的简单的强度差异，每个像素产生一个单一的信息通道（例如，扫描电子显微镜（SEM）图像），现在常见的是，每个像素包含更多信息通道的图像：`RGB`空间图像、多光谱图像 `MultiSpectral Imaging (MSI)`和高光谱图像 `HyperSpectral Imaging (HSI)`:

- `Color-space images`：色域图像是试图模仿人类视觉的图像，它们通常由三个光谱信息通道组成，即：红色 ($\approx 700nm$)、绿色 ($\approx 550\,nm$) 和蓝色 ($\approx 450 nm$)，它们结合起来能够在所谓的RGB色彩空间中重现真实的色彩。`RGB`并不是用于这类图像的唯一颜色空间：如**色相饱和度颜色空间**或**亮度-红到绿-蓝梯度（L*a*b*）**。
- `MultiSpectral Imaging (MSI)`：**多波段**或**多光谱图像**是指在特定的波数或波长下捕捉单个图像，通常由特定的薄膜或LED拍摄，跨越电磁波谱（通常是可见光（VIS）和近红外（NIR）区域。多光谱图像可以被认为是高光谱图像的特殊情况，其收集的波长范围不能被认为是连续的。也就是说，多光谱图像不是在某个波长范围内的连续测量，而是在**离散**的**特定波长**中包含信息。

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h0wgg3j3naj21340u0qay.jpg" alt="image-20220403124155932" style="zoom: 35%;" />

> 上图是一张在18个不同的波长下被测量的10欧元纸币的多光谱图像，它每一个像素都包含了在18个不同波长下收集的规格信息。从图中可以看出：
>
> - 当样品被不同波长的光照射时，可以获得不同的信息。
> - 每个单独的图像可以被认为是一个单通道图像。
>
> 因此，必须把每个单独图像中获得的颜色强度的相应值放在一起。

- ` HyperSpectral Imaging (HSI)`：高光谱图像是指每个像素都测量一个连续光谱的图像。通常情况下，光谱分辨率是以纳米或波数为单位的。

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h0wgfxwno7j20wl0u078y.jpg" alt="image-20220403125006131" style="zoom: 33%;" />

> 用高光谱相机在$940-1600~nm$（近红外，NIR）的波长范围内测量的饼干图像，光谱分辨率为$4~nm$。显示了在两个像素中获得的光谱和在1475纳米获得的假彩色图像（单通道图像）。所选的单通道图像突出了饼干中有意添加水的三个区域。这种水在可见光区域是看不见的，而水是在近红外中可以被发现的主要元素之一。

高光谱图像是唯一一种我们可以谈论光谱分辨率（在遥感领域也被称为辐射分辨率）的图像类型。光谱分辨率被定义为在一个特定的波长范围内测量的不同波长之间的间隔或分离（差距）。显然，在一个较小的波长范围内获得的波段（或光谱通道）越多，光谱分辨率就越高。

### 高光谱图像的结构

**高光谱**或多光谱图像可以被看作是一个**超立方体**。该超立方体由三个维度来定义。**两个空间 $X$ 和 $Y$ 和一个光谱 $\lambda$。**一个超立方体 **D** 将有尺寸 $X\times Y\times\lambda$。这个结构包含了所有与被测表面有关的信息。也就是说，高光谱数据立方体通常是多分量系统，它们往往包含一种以上成分的混合信息。此外，它还包含像光谱噪声、空间干扰和冗余信息这样的假象。因此，从噪声中提取有用信息就非常必要。

![image-20220403131124452](https://tva1.sinaimg.cn/large/e6c9d24egy1h0wgfukzoij21110u0q7c.jpg)

> **Comprehensive ﬂowchart of the analysis of hyperspectral and multispectral images.** 
>
> - 数据预处理：为了后续应用模型获得最佳结果，合适的数据预处理方法会起到至关重要的作用。
>
>    - 错误或遗漏的数据值（如死像素$^{[7]}$）、非信息背景或极端离群值的存在；
>    - 空间和光谱伪影（如散射或大气影响）的存在;
>
>    都是在建模之前甚至在建模期间必须考虑的方面。有很多算法可以帮助我们最大限度的减少数据中的伪影，或者突出起信息，然而适当的预处理方法有时候并不明显，有时也需要把不同的预处理方法进行组合。
>
> 名词解释：ANN, artiﬁcial neural networks; AsLS, asymmetric least squares; CLS, classical least squares; EMSC, extended multiplicative scatter correction; FSW-EFA; ﬁxed size windowevolving factor analysis; ICA, independent component analysis; LDA, linear discriminant analysis; MCR, multivariate curve resolution; MLR, multiple linear regression; MSC, multiplicative scatter correction; NLSU, nonlinear spectral unmixing; OPA, orthogonal projection approaches; PCA, principal component analysis; PLS-DA, partial least squares-discriminant analysis; PLS, partial least squares; SIMCA, soft independent modeling of class analogy; SIMPLISMA, simple-to-use interactive self-modeling mixture analysis; SNV, standard normal variate; SVM, support vectors machine; WLS, weighted least squares. 
>
> from **Hyperspectral image analysis. A tutorial, Analytica Chimica Acta 896 (2015) 34e51. doi:10.1016/j.aca.2015.09.030.**

### 参考文献

[1] O. university Press, Oxford dictionary, (n.d.). https://www.oxforddictionaries.com/.

[2] I.C.I.C. Torres, J.M. Amigo Rubio, R. Ipsen, J.M. Amigo, R. Ipsen, Using fractal image analysis to characterize microstructure of low-fat stirred yoghurt manufactured with microparticulated whey protein, Journal of Food Engineering 109 (2012) 721e729, https:// doi.org/10.1016/j.jfoodeng.2011.11.016.

[3] R.C. Gonzalez, R.E. Woods, S.L. Eddins, Digital Image Processing Using Matlab, 2004, https://doi.org/10.1117/1.3115362.

[4] M. Vidal, J.M.M. Amigo, R. Bro, F. van den Berg, M. Ostra, C. Ubide, Image analysis for maintenance of coating quality in nickel electroplating baths e real time control, Analytica Chimica Acta 706 (2011) 1e7, https://doi.org/10.1016/j.aca.2011.08.007. 14

SECTION j I Introduction

[5] L. de Moura Franc¸a, J.M. Amigo, C. Cairo´s, M. Bautista, M.F. Pimentel, J. Manuel Amigo, C. Cair os, M. Bautista, M. Fernanda Pimentel, Evaluation and assessment of homogeneity in images. Part 1: unique homogeneity percentage for binary images, Chemometrics and Intelligent Laboratory Systems 171 (2017), https://doi.org/10.1016/ j.chemolab.2017.10.002.

[6] N.C. da Silva, L. de Moura Franc¸a, J.M. Amigo, M. Bautista, M.F. Pimentel, Evaluation and assessment of homogeneity in images. Part 2: homogeneity assessment on single channel non-binary images. Blending end-point detection as example, Chemometrics and Intelligent Laboratory Systems 180 (2018), https://doi.org/10.1016/j.chemolab.2018.06.011.

[7] M. Vidal, J.M. Amigo, Pre-processing of hyperspectral images. Essential steps before image analysis, Chemometrics and Intelligent Laboratory Systems 117 (2012) 138e148, https:// doi.org/10.1016/j.chemolab.2012.05.009.

[8] NASA, Landsat Science, (n.d.). https://www.oxforddictionaries.com/.

[9] J.M. Amigo, H. Babamoradi, S. Elcoroaristizabal, Hyperspectral image analysis. A tutorial, Analytica Chimica Acta 896 (2015) 34e51, https://doi.org/10.1016/j.aca.2015.09.030.

[10] D.L. Massart, B.G.M. Vandeginste, J.M.C. Buydens, S. de Jong, P.J. Lewi, J. SmeyersVerberke, L.M.C. Buydens, S. De Jong, J. Smeyers-Verbeke, Handbook of Chemometrics and Qualimetrics, 1997. https://doi.org/10.1016/S0922-3487(97)80056-1.

[11] A.K. Smilde, R. Bro, Principal component analysis (tutorial review), Analytical Methods 6 (2014) 2812e2831. https://doi.org/10.1039/c3ay41907j.

[12] J.M. Amigo, J. Cruz, M. Bautista, S. Maspoch, J. Coello, M. Blanco, Study of pharmaceutical samples by NIR chemical-image and multivariate analysis, Trends in Analytical Chemistry 27 (2008). https://doi.org/10.1016/j.trac.2008.05.010.

[13] F. Franch-Lage, J.M. Amigo, E. Skibsted, S. Maspoch, J. Coello, Fast assessment of the surface distribution of API and excipients in tablets using NIR-hyperspectral imaging, International Journal of Pharmaceutics 411 (2011) 27e35. https://doi.org/10.1016/j.ijpharm. 2011.03.012.

[14] A. de Juan, R. Tauler, Multivariate curve resolution (MCR) from 2000: progress in concepts and applications, Critical Reviews in Analytical Chemistry 36 (2006) 163e176. https://doi. org/10.1080/10408340600970005.

[15] C. Ravn, E. Skibsted, R. Bro, Near-infrared chemical imaging (NIR-CI) on pharmaceutical solid dosage forms-comparing common calibration approaches, Journal of Pharmaceutical and Biomedical Analysis 48 (2008) 554e561. https://doi.org/10.1016/j.jpba.2008.07.019.

[16] S. Wold, W.J. Dunn, E. Johansson, J.W. Hogan, D.L. Stalling, J.D. Petty, T.R. Schwartz, Application of Soft Independent Method of Class Analogy (SIMCA) in Isomer Speciﬁc Analysis of Polychlorinated Biphenyls, 2009, pp. 195e234. https://doi.org/10.1021/bk1985-0284.ch012.

[17] A.F.H. Goetz, L.C. Rowan, M.J. Kingston, Mineral identiﬁcation from orbit: initial results from the shuttle multispectral infrared radiometer, Science 218 (1982) 1020e1024. https:// doi.org/10.1126/science.218.4576.1020, 80-.

[18] J.S. MacDonald, S.L. Ustin, M.E. Schaepman, The contributions of Dr. Alexander F.H.

Goetz to imaging spectrometry, Remote Sensing of Environment 113 (2009). https://doi.org/

10.1016/j.rse.2008.10.017.

[19] G. Vane, A.F.H. Goetz, J.B. Wellman, Airborne imaging spectrometer: a new tool for remote sensing, IEEE Transactions on Geoscience and Remote Sensing GE-22 (2013) 546e549. https://doi.org/10.1109/tgrs.1984.6499168.

[20] J.B. Campbell, R.H. Wynne, Introduction to Remote Sensing, 2011. Hyperspectral and multispectral imaging: setting the scene Chapter j 1.1

15

[21] S.C. Yoon, B. Park, Hyperspectral image processing methods, in: Food Eng. Ser., 2015, pp. 81e101. https://doi.org/10.1007/978-1-4939-2836-1_4.

[22] L. Wang, C. Zhao, Hyperspectral Image Processing, 2015. https://doi.org/10.1007/978-3662-47456-3.

[23] A. Pelizzari, R.A. Goncalves, M. Caetano, Computational Intelligence for Remote Sensing, 2008. https://doi.org/10.1007/978-3-540-79353-3.

[24] S. Martin, An Introduction to Ocean Remote Sensing, 2013. https://doi.org/10.1017/ CBO9781139094368.

[25] P.S. Thenkabail, Remote Sensing Handbook: Remote Sensing of Water Resources, Disasters, and Urban Studies, 2015. https://doi.org/10.1201/b19321.

[26] P.S. Thenkabail, R.B. Smith, E. De Pauw, Hyperspectral vegetation indices and their relationships with agricultural crop characteristics, Remote Sensing of Environment (2000). https://doi.org/10.1016/S0034-4257(99)00067-X.

[27] B. Park, R. Lu, Hyperspectral Imaging Technology in Food and Agriculture, 2015. https:// doi.org/10.1007/978-1-4939-2836-1.

[28] S. Delwiche, J. Qin, K. Chao, D. Chan, B.-K. Cho, M. Kim, Line-scan hyperspectral imaging techniques for food safety and quality applications, Applied Sciences 7 (2017) 125. https://doi.org/10.3390/app7020125.

[29] R. Vejarano, R. Siche, W. Tesfaye, Evaluation of biological contaminants in foods by hyperspectral imaging: a review, International Journal of Food Properties 20 (2017) 1264e1297. https://doi.org/10.1080/10942912.2017.1338729.

[30] J.M. Amigo, I. Martı´, A. Gowen, Hyperspectral imaging and chemometrics. A perfect combination for the analysis of food structure, composition and quality, Data Handling in Science and Technology 28 (2013) 343e370. https://doi.org/10.1016/B978-0-444-59528-7. 00009-0.

[31] S. Munera, J.M. Amigo, N. Aleixos, P. Talens, S. Cubero, J. Blasco, Potential of VIS-NIR hyperspectral imaging and chemometric methods to identify similar cultivars of nectarine, Food Control 86 (2018). https://doi.org/10.1016/j.foodcont.2017.10.037.

[32] X. Zou, J. Zhao, Nondestructive Measurement in Food and Agro-Products, 2015. https://doi.

org/10.1007/978-94-017-9676-7.

[33] Y. Liu, H. Pu, D.W. Sun, Hyperspectral imaging technique for evaluating food quality and safety during various processes: a review of recent applications, Trends in Food Science & Technology 69 (2017) 25e35. https://doi.org/10.1016/j.tifs.2017.08.013.

[34] D. Wu, D.W. Sun, Advanced applications of hyperspectral imaging technology for food quality and safety analysis and assessment: a review e Part II: fundamentals, Innovative Food Science and Emerging Technologies 19 (2013) 1e14. https://doi.org/10.1016/j.ifset. 2013.04.014.

[35] J.X. Wu, D. Xia, F. Van Den Berg, J.M. Amigo, T. Rades, M. Yang, J. Rantanen, A novel image analysis methodology for online monitoring of nucleation and crystal growth during solid state phase transformations, International Journal of Pharmaceutics 433 (2012) 60e70. https://doi.org/10.1016/j.ijpharm.2012.04.074.

[36] A.A. Gowen, C.P. O’Donnell, P.J. Cullen, S.E.J. Bell, Recent applications of Chemical Imaging to pharmaceutical process monitoring and quality control, European Journal of Pharmaceutics and Biopharmaceutics 69 (2008) 10e22. https://doi.org/10.1016/j.ejpb.2007.

10.013.

[37] M. Khorasani, J.M. Amigo, C.C. Sun, P. Bertelsen, J. Rantanen, Near-infrared chemical imaging (NIR-CI) as a process monitoring solution for a production line of roll compaction 16

SECTION j I Introduction

and tableting, European Journal of Pharmaceutics and Biopharmaceutics 93 (2015). https:// doi.org/10.1016/j.ejpb.2015.04.008.

[38] A.V. Ewing, S.G. Kazarian, Recent advances in the applications of vibrational spectroscopic imaging and mapping to pharmaceutical formulations, Spectrochimica Acta Part A: Molecular and Biomolecular Spectroscopy 197 (2018) 10e29. https://doi.org/10.1016/j.saa. 2017.12.055.

[39] P. Chemistry, Near promising future of near infrared hyperspectral imaging in forensic sciences, Journal of Indexing and Metrices 25 (2014) 6e9. https://doi.org/10.1255/nirn. 1443.

[40] G.J. Edelman, E. Gaston, T.G. Van Leeuwen, P.J. Cullen, M.C.G. Aalders, Hyperspectral imaging for non-contact analysis of forensic traces, Forensic Science International 223 (2012) 28e39. https://doi.org/10.1016/j.forsciint.2012.09.012.

[41] C. Cucci, M. Picollo, Reﬂectance Spectroscopy Safeguards Cultural Assets, SPIE Newsroom, 2013. https://doi.org/10.1117/2.1201303.004721.

[42] M. Bacci, C. Cucci, A.A. Mencaglia, A.G. Mignani, Innovative sensors for environmental monitoring in museums, Sensors 8 (2008) 1984e2005. https://doi.org/10.3390/s8031984.

[43] G. Lu, B. Fei, Medical hyperspectral imaging: a review, Journal of Biomedical Optics 19 (2014) 010901. https://doi.org/10.1117/1.jbo.19.1.010901.

[44] M. Vidal, A. Gowen, J.M. Amigo, NIR hyperspectral imaging for plastics classiﬁcation, Journal of Indexing and Metrices (2012). https://doi.org/10.1255/nirn.1285.

[45] D. Caballero, M. Bevilacqua, J. Amigo, Application of hyperspectral imaging and chemometrics for classifying plastics with brominated ﬂame retardants, Journal of Spectral Imaging 8 (2019). https://doi.org/10.1255/jsi.2019.a1.