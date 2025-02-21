---
title: 影像组学特征鲁棒性分析指南（非正态数据适用）
date: '2025-02-17 13:39:21'
updated: '2025-02-21 18:01:48'
permalink: >-
  /post/guidelines-for-robustness-analysis-of-imagingomic-features-nonnormal-data-apply-132riu.html
comments: true
toc: true
---

# 影像组学特征鲁棒性分析指南（非正态数据适用）

### **一、数据准备**

1. **数据整理**

    * **配对结构**：确保每个受试者（如001、002）的1.5T和3.0T数据一一对应。
    * 宽格式转长格式（便于分析）：
    * |Subject|FieldStrength|Feature1|Feature2|...|
      | ---------| ---------------| ----------| ----------| -----|
      |001|1.5T|0.9238|0.3586|...|
      |001|3.0T|0.4995|0.1065|...|
2. **数据清洗**

    * 检查缺失值、极端异常值（如超出3倍IQR），必要时剔除或插补。

---

### **二、非参数统计方法**

#### **1. Wilcoxon符号秩检验（配对样本）**

* **目的**：检验1.5T和3.0T数据的**中位数**是否存在显著差异。
* **适用性**：无需正态假设，适合小样本、偏态数据。
* **解读：p &lt; 0.05表示差异显著**
* **操作示例（Python）** ：

  ```python
  from scipy.stats import wilcoxon

  # 以特征"original_shape_Elongation"为例
  group_1_5T = df[df["FieldStrength"] == "1.5T"]["original_shape_Elongation"]
  group_3_0T = df[df["FieldStrength"] == "3.0T"]["original_shape_Elongation"]
  stat, p_value = wilcoxon(group_1_5T, group_3_0T)
  print(f"Wilcoxon检验p值: {p_value:.4f}")  # p < 0.05表示差异显著
  ```

#### **2. 效应量计算（Cliff’s Delta）**

* **目的**：量化差异的实际大小（范围：[-1, 1]）。
* **解读**：

  * |d| ≥ 0.147（小效应），≥ 0.33（中效应），≥ 0.474（大效应）。
* **操作示例**：

  ```python
  from effsize import cliffs_delta

  d, res = cliffs_delta(group_1_5T, group_3_0T)
  print(f"Cliff's Delta = {d:.3f} ({res})")
  ```

---

### **三、一致性评估**

#### **1. Bland-Altman图（中位数版本）**

* **改进**：用**中位数差异**和**百分位数范围**代替均值和标准差。
* **操作示例（Python）** ：

  ```python
  import numpy as np
  import matplotlib.pyplot as plt

  diff = group_1_5T - group_3_0T
  median_diff = np.median(diff)
  lower = np.percentile(diff, 2.5)
  upper = np.percentile(diff, 97.5)

  plt.figure(figsize=(8, 5))
  plt.scatter((group_1_5T + group_3_0T)/2, diff, alpha=0.7)
  plt.axhline(median_diff, color="red", label="中位数差异")
  plt.axhline(lower, color="grey", linestyle="--", label="95%百分位区间")
  plt.axhline(upper, color="grey", linestyle="--")
  plt.xlabel("(1.5T + 3.0T)/2")
  plt.ylabel("差异 (1.5T - 3.0T)")
  plt.title("Bland-Altman图（中位数版）")
  plt.legend()
  plt.show()
  ```

#### **2. Kendall和谐系数（W）**

* **目的**：评估两组数据的一致性（无需正态假设）。
* **范围**：0（无一致性）到1（完全一致），​`>0.7`​表示强一致性
* **操作示例（Python）** ：

  ```python
  from pingouin import kendall

  # 数据需为宽格式（列名为1.5T和3.0T）
  df_wide = pd.DataFrame({"1.5T": group_1_5T, "3.0T": group_3_0T})
  kendall_w = kendall(df_wide)
  print(f"Kendall W = {kendall_w:.3f}")  # W > 0.7表示强一致性
  ```

---

### **四、多重比较校正**

* **问题**：检验多个特征时，假阳性率升高（如20个特征，α=0.05时至少1个误报的概率≈64%）。
* **解读：**  校正后p值 < 0.05，说明该特征在两种磁场强度下差异显著。
* **解决方法**：

  ```python
  from statsmodels.stats.multitest import multipletests

  # 假设p_values是多个特征的p值列表
  p_values = [0.01, 0.04, 0.001, 0.2, ...]
  _, corrected_p, _, _ = multipletests(p_values, method="fdr_bh")  # FDR校正
  print("校正后p值:", corrected_p)
  ```

---

### **五、结果解释与报告**

1. **统计显著性**

    * 若校正后p值 < 0.05，说明该特征在两种磁场强度下差异显著。
    * 需结合效应量（如Cliff’s Delta）判断差异的实际意义。
2. **一致性强度**

    * **Kendall W &gt; 0.7**：数据一致性较好，鲁棒性高。
    * **W &lt; 0.4**：一致性差，需谨慎使用该特征。
3. **Bland-Altman图解读**

    * 若差异点均匀分布在零线附近，无系统性偏移，说明无显著偏差。
    * 若差异随均值增大而扩大，可能需标准化处理。

|数据类型|有差异|无差异|
| ----------------| -------------------------------------------------------------------------| --------|
|Wilcoxon|< 0.05|>=0.05|
|Cliff’s Delta|\|d\| ≥ 0.147（小效应），≥ 0.33（中效应），≥ 0.474（大效应）。|-|
|Kendall_W|< 0.4|> 0.7|
|corrected_p|< 0.05|=0.05|

---

### **六、完整分析流程（Python示例）**

```python
import pandas as pd
import numpy as np
from scipy.stats import wilcoxon
from scipy.stats import kendalltau
from statsmodels.stats.multitest import multipletests
import matplotlib.pyplot as plt

# Kendall和谐系数
def kendall_coefficient(data):
    return kendalltau(data["1.5T"], data["3.0T"])[0]

def cliffs_delta(group1, group2):
    """
    计算 Cliff's Delta 效应量
    """
    n1 = len(group1)
    n2 = len(group2)
    delta = 0
    for x in group1:
        for y in group2:
            if x > y:
                delta += 1
            elif x < y:
                delta -= 1
    delta /= (n1 * n2)
    return delta

# ---------------------------
# 1. 数据加载与预处理
# ---------------------------

import os

def load_and_preprocess_data(filepath):
    """
    加载数据并提取受试者ID、磁场强度和组织名称信息
    """
    # 读取Excel文件
    df = pd.read_excel(filepath)
  
    # 提取文件名（不包含扩展名）作为组织名称
    organization_name = os.path.splitext(os.path.basename(filepath))[0]
  
    # 从Segment_Name中提取受试者ID和磁场强度
    df[["SubjectID", "FieldStrength"]] = df["Segment_Name"].str.extract(r"(\d{3})-(.*?)-")
    df["FieldStrength"] = df["FieldStrength"].map({"1.5": "1.5T", "3.0": "3.0T"})
  
    # 删除不需要的列
    df = df.drop(columns=["Segment_Name"])
  
    return df, organization_name

# ---------------------------
# 2. 统计分析主函数
# ---------------------------

def analyze_radiomics_robustness(df):
    """
    执行完整的鲁棒性分析流程
    """
    # 获取所有特征列（排除元数据列）
    metadata_columns = ["SubjectID", "FieldStrength"]
    features = [col for col in df.columns if col not in metadata_columns]
  
    # 初始化结果存储
    results = {
        "Feature": [],
        "Wilcoxon_p": [],
        "Cliffs_d": [],
        "Kendall_W": [],
        "Median_Diff": [],
        "IQR_Diff": []
    }

    # 遍历每个特征
    for feature in features:
        try:
            # 提取配对数据
            paired_data = df.pivot(index="SubjectID", columns="FieldStrength", values=feature)
            paired_data = paired_data.dropna()  # 删除缺失值
        
            # 添加验证逻辑
            if len(paired_data) < 1:
                print(f"特征 {feature} 数据不足，无法分析。")
                continue

            group_1_5T = paired_data["1.5T"]
            group_3_0T = paired_data["3.0T"]
        
            # Wilcoxon符号秩检验
            if len(group_1_5T) >= 3:  # 最小样本量要求
                stat, p_value = wilcoxon(group_1_5T, group_3_0T)
            else:
                p_value = np.nan
            
            # Cliff's Delta效应量
            d = cliffs_delta(group_1_5T, group_3_0T)
        
            # Kendall和谐系数
            kendall_w = kendall_coefficient(paired_data).round(3)
        
            # 差异描述统计
            diff = group_1_5T - group_3_0T
            median_diff = np.median(diff)
            iqr_diff = np.percentile(diff, 75) - np.percentile(diff, 25)
        
            # 存储结果
            results["Feature"].append(feature)
            results["Wilcoxon_p"].append(p_value)
            results["Cliffs_d"].append(d)
            results["Kendall_W"].append(kendall_w)
            results["Median_Diff"].append(median_diff)
            results["IQR_Diff"].append(iqr_diff)
        
        except Exception as e:
            print(f"特征 {feature} 分析失败: {str(e)}")
            continue
  
    # 转换为DataFrame
    results_df = pd.DataFrame(results)
  
    # 多重比较校正（FDR校正）
    wilcoxon_p = results_df["Wilcoxon_p"].dropna()
    if len(wilcoxon_p) > 0:
        _, corrected_p, _, _ = multipletests(
            wilcoxon_p,
            method="fdr_bh"
        )
        results_df["Corrected_p"] = np.nan
        results_df.loc[results_df["Wilcoxon_p"].notna(), "Corrected_p"] = corrected_p
    else:
        print("没有足够的 Wilcoxon p 值用于校正。")
        results_df["Corrected_p"] = np.nan
  
    return results_df

# ---------------------------
# 3. 可视化函数
# ---------------------------

def plot_bland_altman(df, feature, organization_name):
    """
    生成Bland-Altman图（中位数版本）
    """
    paired_data = df.pivot(index="SubjectID", columns="FieldStrength", values=feature).dropna()
    group_1_5T = paired_data["1.5T"]
    group_3_0T = paired_data["3.0T"]
  
    diff = group_1_5T - group_3_0T
    median_diff = np.median(diff)
    lower = np.percentile(diff, 2.5)
    upper = np.percentile(diff, 97.5)
  
    plt.figure(figsize=(10, 6))
    plt.scatter((group_1_5T + group_3_0T)/2, diff, alpha=0.7, edgecolor="k")
    plt.axhline(median_diff, color="red", label="Median Difference")
    plt.axhline(lower, color="grey", linestyle="--", label="95% Limits of Agreement")
    plt.axhline(upper, color="grey", linestyle="--")
    plt.xlabel("Mean of 1.5T and 3.0T")
    plt.ylabel("Difference (1.5T - 3.0T)")
    plt.title(f"Bland-Altman Plot: {organization_name} - {feature}")
    plt.legend()
    plt.grid(alpha=0.3)
    plt.show()

# ---------------------------
# 4. 主程序执行
# ---------------------------

if __name__ == "__main__":
    # 数据加载
    df, organization_name = load_and_preprocess_data("Brain-T1.xlsx")
  
    # 执行分析
    results_df = analyze_radiomics_robustness(df)
  
    # 保存结果到Excel
    results_df.to_excel("鲁棒性分析结果.xlsx", index=False)
    print("分析结果已保存到 鲁棒性分析结果.xlsx")
  
    # 可视化示例（以第一个特征为例）
    if len(results_df) > 0:
        first_feature = results_df.iloc[0]["Feature"]
        plot_bland_altman(df, first_feature, organization_name=organization_name)
    else:
        print("没有可分析的特征")
```

---

### **七、注意事项**

4. **小样本问题**：若样本量极小（如n < 10），非参数检验效力降低，建议增加样本量或使用贝叶斯方法。
5. **临床意义优先**：即使统计显著，若差异在设备误差范围内（如MRI磁场波动允许的差异），可认为数据鲁棒。
6. **特征选择**：优先关注与研究目标相关的关键特征（如形态学指标），避免“钓鱼式”分析。

通过以上步骤，您可系统评估影像组学特征在1.5T和3.0T下的鲁棒性，为医学影像技术的标准化和临床决策提供可靠依据。

‍

‍

‍

‍

‍
