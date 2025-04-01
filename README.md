# Additional Experiments

We have added the following experiments: \
***simulation data***
* Ablation studies on base and retraining model size (ajY7) (Table 2)
* Simulation on classification dataset (6Mdq) (Table 1)
* Simulation on non-linear settings (ajY7, nhFX)
    * Homogeneous model with vanilla CP, compared to the KnnR [1] (Table 2)
    * Heterogeneous model with CQR, compared to the KnnR [1] (Table 3)

***real data***
* Real data with baseline (ajY7)
    * Regression task: compared to CQRR[2] and CQR[3] (Table 4)
    * Classification task: compared to Conftr[4] and ConfTS [5] (Table 5)
* Ablation studies on base and retraining model size (ajY7)  (Table 6,7,8)

The experiments illustrate that:
* Our retraining framework is robust to the size of the retraining model. The improvement in efficiency is primarily dependent on the size of the base model (as shown in Thm 5.4).
* Our framework is orthogonal to existing CP methods. As a result, it can be applied to various baseline CP methods to enhance their efficiency, including interval-based predictors such as CQR.
* The retraining strategy enhances the efficiency of various CP methods across simulated classification data as well as nonlinear homogeneous and heterogeneous data.

[1] Papadopoulos, H., Gammerman, A., & Vovk, V. (2008, February). Normalized nonconformity measures for regression conformal prediction. In Proceedings of the IASTED International Conference on Artificial Intelligence and Applications (AIA 2008) (pp. 64-69).\
[2] Sesia, M., & Candès, E. J. (2020). A comparison of some conformal quantile regression methods. Stat, 9(1), e261.\
[3] Romano, Y., Patterson, E., & Candes, E. (2019). Conformalized quantile regression. Advances in neural information processing systems, 32.\
[4] Noorani, S., Romero, O., Fabbro, N. D., Hassani, H., & Pappas, G. J. (2024). Conformal Risk Minimization with Variance Reduction. arXiv preprint arXiv:2411.01696.\
[5] Dabah, L., & Tirer, T. (2024). On Temperature Scaling and Conformal Prediction of Deep Classifiers. arXiv preprint arXiv:2402.05806.

## Simulation

#### Table 1: Performance of the Retraining Framework on the Simulation Classification Dataset with APS and RAPS methods
| Method | APS Coverage | APS Length | RAPS Coverage | RAPS Length |
|---------|----------------|--------------|-----------------|------------------|
|Base 4 Layer|  91.60±0.01| 7.09±0.04| 90.53±0.01| 6.15±0.06|
|2 Layer Retrain | 90.93±0.01 | 5.44±0.19 | 90.37±0.01 | 5.20±0.11 |
|3 Layer Retrain| 90.95±0.01 | 5.80±0.23 | 90.00±0.01 | 5.38±0.19 |
|4 Layer Retrain| 90.48±0.01 | 5.75±0.20 | 90.46±0.01 | 5.83±0.23 |
|5 Layer Retrain| 90.22±0.01 | 5.68±0.15 | 89.81±0.01 | 5.55±0.22 |
|6 Layer Retrain| 90.14±0.01 | 5.71±0.18 | 89.84±0.01 | 5.66±0.11 |

* The retraining framework enhances the efficiency of the base model for APS and RAPS methods on the simulation classification dataset.
* The data is simulated using a logistic model where each class score is given by $s\_j = \mathbf{w}\_{(j)}^{\top} \mathbf{x} + b\_{(j)}$. The softmax probabilities are then computed as $p_j = \frac{\exp(s_j)}{\sum_{k=1}^K \exp(s_k)}$, ensuring that $\sum_{j=1}^K p_j = 1$. In this simulation, we generate 60 classes.


#### Table 2: Ablation Study on Base and Retraining Model Size for the Homogeneous Model $Y = 1 + sin(X) +sin^2(X) + \epsilon, \epsilon\sim N(0,1)$.
| Base RF depth | Retrain RF depth | Coverage | Length |
|---------|---------|----------------|--------------|
| 4 | 0（vanilla） |  89.96±0.62| 1.18±0.04|
| 4 | 2 |  89.92±1.09| 0.81±0.03|
| 4 | 4 |  90.01±1.15| 0.56±0.03|
| 5 | 0（vanilla） |  90.00±0.47| 0.87±0.05|
| 5 | 2 |  89.77±1.09| 0.59±0.03|
| 5 | 4 |  90.11±1.13| 0.48±0.02|
| 6 | 0（vanilla） |  89.90±0.79| 0.53±0.02|
| 6 | 2 |  89.92±1.29| 0.47±0.02|
| 6 | 4 |  89.94±1.45| 0.43±0.02|
| 10 | 0（vanilla） |  89.77±1.08| 0.3709±0.0099|
| 10 | 3 |  89.80±1.43| 0.3692±0.0143|
| 15 | 0（vanilla） |  89.81±1.06| 0.3940±0.0103|
| 15 | 3 | 89.79±1.53| 0.3928±0.0170|
| 20 | 0（vanilla） |  89.79±1.08| 0.3986±0.0106|
| 20 | 3 |  89.73±1.42| 0.3969±0.0161|
|  | KnnR | （91.90 ± 1.81） | （0.3701±0.005）|

* The base and retraining models are implemented as Random Forests, with model size controlled by adjusting the maximum tree depth.
* As the base model size increases, the improvement from the retraining strategy decreases. However, it can still improve efficiency even with a large base model, as retraining a smaller model can help reduce variance, similar to an ensemble approach.
* Since KnnR [1] is a transductive CP method based on KNN regression, it is computationally expensive to implement on real data. Due to time limitations, we have only conducted experiments on a 1-dimensional simulation dataset (Table 2,3). After retraining, the base model is comparable to KnnR.











#### Table 3: Comparision of Vanilla CQR, Retrained CQR and KnnR in Terms of Coverage, Length, and Group Coverage at Different Values of $\lambda$, Based on the Heterogenous Model $Y = 1 + sin(X) +sin^2(X) + \lambda\,I_{\{U < 0.1\}}\epsilon_X, \epsilon_x \sim N(0, \sigma^2_x=1+sin^2(x))$
|  |  | **Coverage** |  |  | **Length** |  |  | **Group Coverage** |  |
|-------|------|--------|---------|------|---------|--------|-------|---------|---------|
|  $\lambda$     | CQR Vanilla | CQR Retrain | KnnR | CQR Vanilla | CQR Retrain | KnnR | CQR Vanilla | CQR Retrain | KnnR |
| 5     | 89.63±0.52 | 89.72±0.53 | 90.80±2.79 | 1.34±0.12 | 0.95±0.07 | 2.55±0.20 | 82.49±1.94 | 84.41±1.28 | 82.39±5.77 |
| 10    | 89.39±0.30 | 89.77±0.47 | 90.80±2.79 | 1.09±0.10 | 0.85±0.09 | 4.07±0.30 | 82.52±1.38 | 83.93±0.56 | 82.39±5.77 |
| 15    | 89.47±0.24 | 89.67±0.51 | 90.80±2.79 | 0.95±0.07 | 0.79±0.11 | 5.05±0.36 | 82.97±1.29 | 83.87±0.61 | 82.39±5.77 |
| 20    | 89.43±0.16 | 89.62±0.52 | 90.80±2.79 | 0.83±0.08 | 0.77±0.16 | 5.73±0.40 | 83.21±1.36 | 83.81±0.61 | 82.39±5.77 |
| 25    | 89.44±0.37 | 89.72±0.60 | 90.70±2.79 | 0.78±0.15 | 0.79±0.25 | 6.24±0.43 | 82.88±0.99 | 83.78±0.58 | 82.39±5.77 |

* Our retraining framework is orthogonal to the CP methods, so it is also applicable to the interval-based predictor (e.g. CQR) here.
* Compared to the KnnR, CQR is more robust to the outliers (as $\lambda$ controls the intensity of the outlier influence) and our framework also inherits this property.


## Real data

#### Table 4: Application of the Scalable Retraining Framework to Different Baseline Conformal Prediction Models on Real Regression Data
| Dataset | Vanilla CP Coverage | Vanilla CP Length | Retrain CP Coverage | Retrain CP Length | CQR Coverage | CQR Length | Retrain CQR Coverage | Retrain CQR Length | CQRR Coverage      | CQRR Length         | Retrain CQRR Coverage | Retrain CQRR Length |
|---------|---------------------|-------------------|---------------------|-------------------|--------------|------------|----------------------|--------------------|--------------------|---------------------|-----------------------|---------------------|
| SYN | 89.97±0.49 | 0.70±0.05 | 90.48±0.66 | 0.57±0.04  | 89.81±1.03   | 0.71±0.08  | 90.71±0.21           | 1.07±0.17 | 89.65±0.75 | 0.73±0.08  | 90.09±0.67 | 0.71±0.02 |
| COM     | 90.45±2.13          | 3.39±0.37         | 89.20±2.24          | 2.46±0.34         | 90.10±2.19   | 3.88±0.27  | 89.90±1.55           | 1.91±0.06          | 90.95±1.90         | 3.83±0.29           | 90.45±0.86            | 2.07±0.12           |
| BIKE    | 90.07±0.58          | 2.35±0.05         | 89.01±0.39          | 2.84±0.06         | 89.87±0.40   | 2.53±0.05  | 89.95±1.11           | 2.99±0.06          | 89.86±0.44         | 2.51±0.06           | 90.01±1.05            | 2.91±0.10           |
| GLA     | 91.81±2.31          | 4.89±0.64         | 91.82±6.83          | 2.00±0.81         | 90.00±2.32   | 4.89±0.59  | 93.18±5.75           | 2.07±0.98          | 91.36±3.91         | 4.79±1.51    | 90.00±7.82            | 2.27±1.57           |
| DIA     | 87.66±2.43          | 5.73±0.41         | 90.65±3.73          | 4.09±0.27         | 89.35±2.45   | 5.74±0.37  | 89.87±4.30           | 3.42±0.30          | 90.13±1.90         | 6.83±1.07          | 89.61±4.42            | 3.38±0.50           |
| CAN     | 88.87±4.48| 5.13±0.70| 88.35±4.28 | 2.46±0.40 | 91.65±3.37 | 5.31±0.34 | 88.35±5.35 | 2.03±0.79 | 92.87±2.01  | 7.97±2.03 | 87.13±5.14 | 2.05±0.56  |
| WINE    | 91.65±1.20          | 1.94±0.13         | 89.47±3.36          | 0.56±0.05         | 90.34±1.29   | 2.01±0.13  | 88.79±2.01           | 0.68±0.08          | 89.60±0.85         | 4.18±3.30         | 89.03±1.47            | 0.71±0.08           |
| ABA     | 89.86±1.63          | 1.72±0.23         | 90.45±1.92          | 1.21±0.35         | 89.62±1.61   | 1.43±0.11  | 89.55±1.54           | 1.66±0.06          | 89.81±1.34         | 1.48±0.19           | 90.43±1.64            | 1.68±0.09           |
| SEO     | 90.04±0.53          | 2.31±0.11         | 90.26±1.20          | 2.78±0.19         | 89.01±1.18   | 2.21±0.12  | 90.35±0.79           | 2.55±0.17          | 88.96±1.07         | 2.19±0.13           | 90.31±1.01            | 2.53±0.20           |
| CON     | 90.63±2.46          | 3.08±0.16         | 90.72±2.76          | 1.11±0.13         | 90.92±2.56   | 3.03±0.20  | 90.24±0.94           | 1.23±0.14          | 91.01±1.72         | 5.36±3.74    | 91.69±1.82            | 1.29±0.18           |

* Setting：5 Layer Base Model + 3 Layer Retraining model
* Since our framework is orthogonal to different CP methods, we apply it to CQR and CQRR on regression datasets, retraining improves the efficiency of these methods.




#### Table 5: Application of the Scalable Retraining Framework to Different Baseline Conformal Prediction Models on CIFAR10 (Top x represents the Top x accuracy of the base classifier)
|Method|Top1|Top5|Coverage|Length|
|------|--------|-------|--------|-------|
|APS|58.986±0.007|96.213±0.003|90.666±0.005|3.185±0.034|
|Retrain APS|-|-|90.000±0.011|3.066±0.093|
|RAPS|-|-|90133±0.006|3.079±0.007|
|Retrain RAPS|-|-|89.760±0.012|3.006±0.064|
|Conftr APS|69.166±0.011|0.976±0.003|91.411±0.006|2.391±0.027|
|Retrain Conftr APS |-|-|90.822±0.007|2.382±0.070|
|Conftr RAPS |-|-|90.770±0.005|2.271±0.032|
|Retrain Conftr RAPS |-|-|90.230±0.005|2.268±0.043|
|ConfTS APS |59.688±0.014|95.832±0.002|90.328±0.005|3.042±0.030|
|Retrain ConfTS APS |-|-|90.128±0.007|3.036±0.100|
|ConfTS RAPS |-|-|90.239±0.006|2.998±0.034|
|Retrain ConfTS RAPS |-|-|90.231±0.012|2.967±0.104|


* settings: 
    * Base Model : 4 Layer convolutional layer + 2 Layer Fully Connected Networks
    * Retrain Model : 2 Layer convolutional layer + 2 Layer Fully Connected Networks
    * Conformal Training (Conftr) : Pretrained Base Model + Conformal Training Finetune
    * Conformal Temperature Scaling (ConfTS) : Pretrained Base Model + Conformal Temperature Scaling Finetune

* Since our framework is orthogonal to different CP methods, we apply it to Conftr and ConfTS on CIFAR 10, retraining improves the efficiency of these methods.



#### Table 6: Ablation Study on the Impact of Base and Retraining Model Sizes Using a Simulation Regression Dataset (5-Layer Base Model with 2~4 Layer Retraining Models)
| Dataset | Vanilla CP Coverage | Vanilla CP Length | Retrain CP (2 Layer) Coverage | Retrain CP (2 Layer) Length | Retrain CP (3 Layer) Coverage | Retrain CP (3 Layer) Length | Retrain CP (4 Layer) Coverage | Retrain CP (4 Layer) Length |
|---------|---------------------|-------------------|-------------------------------|-----------------------------|-------------------------------|-----------------------------|-------------------------------|-----------------------------|
| SYN | 89.98±0.50 | 0.70±0.05 | 90.80±0.65 | 0.56±0.04 | 90.43±0.66 | 0.57±0.04 | 90.79±0.75 | 0.57±0.04 |
| COM | 90.45±2.13 | 3.39±0.37 | 89.70±1.93 | 2.33±0.26 | 89.55±2.88 | 2.43±0.28 | 89.70±3.14 | 2.47±0.40 |
| BIKE | 90.07±0.58 | 2.35±0.05 | 89.31±0.58 | 2.92±0.04 | 89.35±0.56 | 2.89±0.06 | 89.44±0.39 | 2.86±0.06 |
| GLA | 91.82±2.32 | 4.89±0.64 | 90.00±1.11 | 3.57±0.82 | 92.73±4.41 | 1.99±0.67 | 90.91±4.31 | 2.11±0.71 |
| DIA | 87.66±2.43 | 5.73±0.41  | 89.48±3.03 | 4.03±0.23 | 91.56±2.93 | 4.13±0.35 | 90.13±3.08 | 4.08±0.36 |
| CAN | 88.87±4.48 | 5.13±0.70 | 89.91±2.31 | 2.93±0.26 | 88.52±3.45 | 2.55±0.16 | 89.74±3.83 | 2.52±0.37 |
| WINE | 91.65±1.20 | 1.94±0.13 | 89.84±3.00 | 0.73±0.06 | 89.78±1.97 | 0.64±0.06 | 89.47±1.52 | 0.56±0.02 |
| ABA | 89.86±1.63 | 1.72±0.23 | 90.50±2.02 | 1.22±0.34 | 90.69±1.76 | 1.24±0.37 | 90.19±1.57 | 1.20±0.35 |
| SEO | 90.04±0.53 | 2.30±0.11 | 90.12±1.20 | 2.85±0.16 | 90.12±1.10 | 2.79±0.18 | 90.41±0.73 | 2.80±0.18 |
| CON | 90.63±2.46 | 3.07±0.16 | 91.01±2.32 | 1.40±0.07 | 91.21±0.94 | 1.28±0.04 | 90.43±2.01 | 1.18±0.09 |


* All sizes of the retraining model can improve the efficiency of the base model, the improvement is robust to the retraining model size.


#### Table 7: Ablation Study on the Impact of Base and Retraining Model Sizes Using a Simulation Regression Dataset (2-Layer Base Model with 1~3 Layer Retraining Models)

| Dataset | Vanilla CP Coverage | Vanilla CP Length | Retrain CP (1 Layer) Coverage | Retrain CP (1 Layer) Length | Retrain CP (2 Layer) Coverage | Retrain CP (2 Layer) Length | Retrain CP (3 Layer) Coverage | Retrain CP (3 Layer) Length |
|---------|-----------|-----------|----------|---------|-------|----------|------------|-----------|
| SYN| 90.36±0.91| 1.80±0.38| 90.65±0.81| 1.31±0.17| 90.80±0.65| 1.42±0.18| 90.74±0.96| 1.38±0.15|
| COM| 90.90±1.99| 3.86±0.68| 90.00±1.17| 2.79±0.43| 90.00±1.05| 2.52±0.48| 89.65±1.41| 2.51±0.42|
| BIKE| 90.19±0.67| 2.87±0.22| 89.96±0.56| 2.81±0.18| 89.94±1.04| 2.70±0.20| 89.97±0.70| 2.60±0.26|
| GLA| 89.55±3.08 | 5.32±1.03| 94.55±4.22| 4.41±1.49| 89.54±5.30| 2.56±0.93| 90.91±3.80| 2.41±0.82|
| DIA| 90.78±0.64| 5.99±0.66| 90.13±2.37| 5.07±0.71| 90.13±2.86| 4.05±0.39| 89.61±2.05| 4.08±0.32|
| CAN| 89.91±1.18 | 5.64±0.39 | 90.09±4.49| 3.76±0.61| 90.61±4.06| 2.75±0.39| 89.39±4.84| 2.04±0.64|
| WINE| 90.09±1.91| 2.43±0.19 | 89.72±2.52| 2.12±0.18| 89.72±2.07| 0.87±0.11| 89.47±1.96| 0.63±0.07|
| ABA| 89.81±1.47| 2.37±0.35| 90.41±1.12| 1.50±0.24| 90.60±1.49| 0.85±0.11| 90.24±1.08| 0.83±0.12|
| SEO| 90.06±0.72 | 3.52±0.37| 89.91±0.87| 2.16±0.13| 90.41±0.21| 1.82±0.15| 90.14±0.26| 1.69±0.13|
| CON| 89.95±1.31| 3.10±0.38| 90.82±1.70| 2.75±0.27| 90.24±3.20| 1.09±0.09| 90.14±2.67| 0.70±0.02|

* Compared to Table 6, as the expressiveness of the base model decreases, the potential for improvement in efficiency increases.
* All sizes of the retraining model can improve the efficiency of the base model, the improvement is robust to the retraining model size.


#### Table 8: Ablation Study on the Impact of Base and Retraining Model Sizes Using a Simulation Regression Dataset (1~3 Layer Base Model with 2 Layer Retraining Models)

| Dataset | Vanilla CP (1 Layer) Coverage | Vanilla CP (1 Layer) Length | + Retrain CP Coverage | + Retrain CP Length | Vanilla CP (2 Layer) Coverage | Vanilla CP (2 Layer) Length| + Retrain CP Coverage | + Retrain CP Length | Vanilla CP (3 Layer) Coverage | Vanilla CP (3 Layer) Length| + Retrain CP Coverage | + Retrain CP Length |
|---------|-----------|-----------|----------|---------|-------|----------|------------|-----------|------------|-----------|------------|-----------|
| SYN| 90.45±0.76| 2.16±0.01| 90.30±0.78| 1.60±0.13| 90.35±0.91| 1.80±0.38| 90.80±1.15| 1.41±0.20|89.91±0.70| 0.88±0.07|89.86±1.45| 1.00±0.13|
| COM| 89.75±1.65| 4.36±0.43| 89.30±1.26| 3.71±1.26| 90.90±1.98| 3.86±0.68| 90.75±1.22| 2.52±0.45|90.90±1.87| 2.78±0.36|89.35±2.05| 2.78±0.34|
| BIKE| 90.56±0.93| 4.14±0.16| 89.83±0.54| 2.82±0.35| 90.19±0.67| 2.87±0.22| 90.38±0.45| 2.72±0.24|90.04±0.25| 2.41±0.06|90.00±0.75| 2.64±0.08|
| GLA| 91.82±5.10 | 3.80±1.16| 90.91±6.59| 2.83±0.81| 91.36±2.65| 1.83±0.32| 90.91±4.31| 1.75±0.31|90.91±3.80| 5.02±0.75|90.00±5.49| 2.57±0.50|
| DIA| 91.30±2.20| 4.23±0.46| 90.26±2.29| 4.13±0.27| 90.78±1.26| 4.37±0.53| 91.82±2.42| 4.24±0.21|90.00±3.09| 5.95±0.44|91.30±2.45| 4.14±0.26|
| CAN| 89.91±2.31 | 3.85±0.63 | 90.61±3.03| 2.90±0.08| 89.74±3.54| 2.47±0.46| 89.22±3.42| 2.60±0.24|90.43±3.11| 5.20±0.57|91.83±2.73| 2.74±0.43|
| WINE| 91.96±2.50| 1.81±0.21 | 89.72±3.21| 0.68±0.13| 90.28±1.24| 0.52±0.02| 90.22±1.05| 0.72±0.04|89.78±0.93| 1.92±0.11|90.40±1.09| 0.72±0.05|
| ABA| 90.77±1.23| 2.61±0.15| 90.77±1.23| 1.45±1.06| 89.81±1.47| 2.37±0.35| 90.45±1.56| 0.85±0.12|89.35±1.53| 2.02±0.14|91.58±1.02| 1.23±0.16|
| SEO| 90.13±0.74 | 4.43±0.32| 89.80±0.53| 3.07±0.83| 90.06±0.72| 3.52±0.37| 90.35±0.57| 1.82±0.12|90.06±0.55| 2.70±0.20|90.75±0.45| 2.63±0.10|
| CON| 91.98±0.90| 2.72±0.19| 90.43±1.99| 1.36±0.20| 88.89±3.43| 2.04±0.34| 89.47±2.10| 1.12±0.13|90.43±1.20| 3.23±0.20|88.99±2.62| 1.19±0.05|

* As the expressiveness of the base model decreases, the potential for improvement in efficiency increases.




## Additional Questions from Reviewer nhFX

> In the case study, one may start from the data-generating distribution $Y \sim f(X)+g(X,\epsilon)$ [...]. Does this imply $g(X,\epsilon)=\beta^{\top}X+\epsilon$, $E(\epsilon)=0$. Are there other cases where the proposed scheme applies exactly?

You are correct! This may also be extended to the general non-linear residual case, i.e., $g(X,\epsilon)=h(X)+\epsilon$, where $h(X)$ is non-linear.
Specifically, the special linear residual case study in Sec. 5 is primarily for simplicity in the proof, as the linear case provides more direct intuition and has many practical applications, such as in kernel methods and NTK. However, the results are not limited to linear residual cases. The key reason is that if the residual $r(x)=y-\hat{f}(x)$ contains non-linear term, one can also apply some non-linear estimator $\hat{r}(x)$ during retraining stage, such that $Var(r(x)-\hat{r}(x)) \leq Var(r(x))$, provided the signal in the residual $r(x)$ is large enough.


> The concept of the hidden power of CP should have been defined more explicitly.

The "hidden power" suggests that if the performance of the "black-box" model has been improved, the gap between oracle optimality of CP when the true regression function is known and practical inefficiency may also be reduced. 
Specifically, in many cases, the non-conformity score function designed for the model is optimal. This means that CP provides the smallest possible prediction set while maintaining the coverage guarantee when the regression function is known. However, in practice, the efficiency of CP is suboptimal due to the use of an estimated regression function from a "black-box" model. As a result, there is a gap between oracle optimality and practical inefficiency. Therefore, this paper focuses on unleashing this hidden power by retraining an additional adapter without modifying the "black-box" model, using a loss function derived from a distributional perspective.


> The model seems designed for regression tasks but is used for regression and classification in the experiments. How does the concept of residual generalise to the classification setup?

In the classification setup, the quantity we are interested in is the underlying true posterior probability $p(Y\mid X=x)$, which is a vector. The residual is defined as the difference between the estimated $\hat{p}(Y\mid X=x)$ and the underlying true posterior probability $p(Y\mid X=x)$. For example, if the number of classes is large (e.g., 1k in ImageNet), then $p(Y\mid X=x)$ is a high-dimensional sparse vector. However, many "black-box" models utilize a softmax layer, which disrupts this sparsity structure. Our retraining framework has the potential to recover such sparsity (Figure 4b).



> Are the covariate shift and model misspecification mentioned in the introduction between training and calibration or calibration and test sets? What happens when they affect all sets?

In the case of covariate shift, training data are from the source domain, calibration and test data are from the target domain. If it happens when they affect all sets, we may need the likelihood ratio to do re-weighting as suggested in Tibshirani, et al. (2019).

For model misspecification,  all data (training, calibration, and test) come from the same data distribution, but the "black box" model may not have enough representation power compared to the data-generating model.

Tibshirani, R. J., Foygel Barber, R., Candes, E., & Ramdas, A. (2019). Conformal prediction under covariate shift. Advances in neural information processing systems, 32.

> Does model misspecification cause the marginal intervals to be inefficient even when replacing the empirical quantile with its population counterpart?

You are correct! If $f(x)-\hat{f}(x) \neq 0$, there is room for improvement even when replacing the empirical quantile with its population counterpart.

> The proposed training scheme sets the mean of the conformity score distribution to zero. Does it also reduce the variance?

It may reduce variance if the signal in the residual is large enough. Specifically, $Var(R-\hat{g}(X))^2 \leq Var(R)$ implies $Var(\hat{g}(X)) \leq 2Cov(R,\hat{g}(X))$,  where $R=f(X)-\hat{f}(X)$ is the residual and $\hat{g}(X)=argmin E(R-g(X))^2$ is the retrained model.
