**SENG 637- Dependability and Reliability of Software Systems\***

**Lab. Report \#5 – Software Reliability Assessment**
| Group \#: 7 |
| -------------- |
| Student Names: |
| Carissa Chung |
| Benjamin Reid |
| Braden Tink |
| Christian Valdez |
| Alton Wong |

# Introduction

Using failure data, we can assess the reliability of a system under test (SUT). Our group utilized the C-SFRAT reliability growth assessment tool to generate plots of the failure rate and reliability of the SUT. Reliability Demonstration Char (RDC) is a good method for checking whether the target mean time to failure (MTTF) is met. It is based on collecting failure data at various time points.

# Assessment Using Reliability Growth Testing

### Laplace Test

The **Laplace test** was used to determine the trend in the failure rate of a system. Failure data were collected and plotted to achieve a **90% confidence level**. The **x-axis** displays the failure count, while the **y-axis** shows the Laplace value.

<img width="889" alt="Screenshot_2024-04-17_at_10 26 29_AM" src="https://github.com/BradenTink/SENG-637/assets/69766712/451ecf0a-5a7d-4fc5-8b7e-5bcd30d190b7">
<br>
Figure 1. Laplace Test for Failure Data

<br>

<br>
When the rate of change is **positive**, this indicates that the time between failures is decreasing, suggesting that failures are occurring more frequently. This could imply that the system or component is degrading or undergoing a wear-out process. Conversely, a **negative rate of change** suggests that the time between failures is increasing, implying that failures are becoming less frequent.

When the **Laplace value** is **above the red line**, it indicates that the system is likely becoming less reliable over time.

When the **Laplace value** is **below the red line**, this suggests an improvement in reliability.

| Interval | Trend Description                                   | Above Upper Threshold? | Interpretation                            |
| -------- | --------------------------------------------------- | ---------------------- | ----------------------------------------- |
| 0-30     | Decreasing sharply                                  | No                     | Improvement in reliability                |
| 30-50    | Initially significant improvement, then fluctuating | Yes and No             | Variable reliability, periods of concern  |
| 50-60    | Increasing sharply                                  | Yes                    | Deterioration in reliability              |
| 60-80    | Generally high with fluctuations                    | Yes                    | Continued concern for reliability         |
| 80-92    | Decreasing                                          | No                     | Indications of improvement in reliability |

### Model Ranking

To identify the optimal model, we assessed the **Akaike Information Criterion (AIC)** and the **Bayesian Information Criterion (BIC)**.

> The AIC is more suited when the primary concern is **prediction accuracy**, as it tends to select models that fit the data well. The BIC is often preferred in scenarios where model selection aims to identify the **true underlying model**, as it penalizes free parameters more stringently, reducing the risk of overfitting as the sample size grows.

Models scoring the lowest in AIC or BIC are preferred for their blend of **simplicity** and **accuracy**.

Analysis revealed that the **Discrete Weibull Type 3 (DW3) model with covariate F** led with an **AIC of 122.199** and a **BIC of 127.935**, followed by the **Geometric Model (GM) with covariate F**, which recorded an **AIC of 125.323** and a **BIC of 129.625**.

| Model                | AIC     | BIC     |
| -------------------- | ------- | ------- |
| DW3 with covariate F | 122.199 | 127.935 |
| GM with covariate F  | 125.323 | 129.625 |

Below are the graphs for DW3 and GM. We can see that both models are fitting close to the true values.

![Screenshot 2024-04-17 185119](https://github.com/BradenTink/SENG-637/assets/69766712/c13afce4-22e5-4b94-a478-1967853c4f43)

Figure 2. MVF Graph for DW3 and GM
<br>

<br>
**Observations:**

- **Closeness to the Imported Data**: The lines follow the step function. This indicates that both models are likely providing a good fit to the observed data.
- **Consistency Across Time**: Both models are consistent with each other and with the imported data throughout most of the range.
- **Divergence at the Tail**: There's a noticeable divergence at the beginning of the model.

### Failure Intensity

<img width="1075" alt="Screenshot_2024-04-17_at_10 32 38_AM" src="https://github.com/BradenTink/SENG-637/assets/69766712/fd831d9c-b4c1-43a0-8555-7bdafa345f65">
Figure 3. Failure intensity Graph
<br>

<br>
<img width="1075" alt="Screenshot_2024-04-17_at_10 32 38_AM" src="https://github.com/BradenTink/SENG-637/assets/69766712/421bc12f-4bd7-4337-8774-bda90fcd993d">
Figure 4. Failure intensity Graph with DW3 and GM
<br>

<br>

| Interval | Trend                                                     |
| -------- | --------------------------------------------------------- |
| 0-4      | High failure intensity, decreasing trend                  |
| 4-19     | Fluctuating failure intensity, general decrease           |
| 19-25    | High failure intensity with sharp increases and decreases |
| 25-32    | Lower failure intensity, fluctuating with slight increase |

Explanation:

- **0-4**: The testing starts with a very high failure count (peaking at time 2), which suggests a high failure intensity. However, it decreases by time 4, showing an improvement in reliability as initial defects are presumably being fixed.

- **4-19**: There are fluctuations in the failure count, with several points (e.g., time 9 and 19) showing increased failure intensity, but the overall trend is a general decrease, which might indicate that the software is becoming more reliable as testing progresses and bugs are fixed.

- **19-25**: There is a noticeable spike at times 19 and 20, followed by a drop, which could indicate the discovery and fixing of a batch of defects or the software being subjected to more rigorous tests. The trend is quite variable with sharp increases and decreases in failure intensity.

- **25-32**: Post time 25, the failure counts are lower than the earlier spikes but show some fluctuation. This could suggest a stabilization in the number of failures, with ongoing discovery and fixing of defects as the testing progresses.

# Assessment Using Reliability Demonstration Chart

<img width="1322" alt="Screenshot 2024-04-17 at 9 29 03 AM" src="https://github.com/BradenTink/SENG-637/assets/59784141/f42d3461-f5e6-4bcd-9ae0-3851694d8413">
Figure 5. Risk Profile selected for the failure data, beta = 0.1, alpha = 0.1, and gamma = 2.
<br>

<br>
In determining the minimum Mean Time To Failure (MTTFmin) necessary for acceptance using the provided failure data from the dataset, failure-dataset-a5.csv, an iterative process was employed. This involved plotting the data and systematically adjusting the Failure Intensity Overload (FIO) values until all data points unequivocally surpassed the reject boundary and a single point is touching the reject boundary line (refer to Figure 6 below). It's worth noting that the discrimination ratio (gamma) used was 2, along with a developer's risk (alpha) of 0.1 and a user's risk (beta) of 0.1. By ensuring that the majority of data points reside within the accept boundary region, while allowing for a few to linger within the continue testing boundary, the integrity and reliability of the system are steadfastly maintained.

![MTTFmin_RDC](https://github.com/BradenTink/SENG-637/assets/59784141/cd1a97a7-263a-4949-b98f-4825c3b51cbe)
Figure 6. Reliability Demonstration Chart with MTTFmin of 0.182 intervals and a FIO of 170 failures per 31 intervals.

Upon doubling the MTTFmin (0.365) and deriving the resultant RDC, its depiction on Figure 7 below yields a discernible outcome. It becomes evident that the system fails to meet the acceptability criteria, as a substantial proportion of data points reside distinctly above the boundary delineating the reject region. This observation indicates a deficiency in reliability, thus rendering the system unsuitable for deployment under the current conditions. The prevalence of data points beyond the reject region boundary underscores the imperative need for further optimization or recalibration to attain the requisite operational integrity and compliance with acceptability standards.

![double_MTTFmin_RDC](https://github.com/BradenTink/SENG-637/assets/59784141/c2c31220-b1f7-4b1c-a210-644dd804cfbf)
Figure 7. Reliability Demonstration Chart with MTTFmin of 0.365 intervals and a FIO of 85 failures per 31 intervals.

Alternatively, when the MTTFmin was halved and the resulting data points were examined, a clear trend emerged: all of them fell within the acceptable range (see Figure 8 below). This straightforward observation indicates a significant improvement in system reliability, confirming its adherence to predefined acceptability standards. With the adjusted MTTF value now at 0.091, the system demonstrates a satisfactory level of acceptability, instilling confidence in its operational effectiveness and suitability for deployment. This underscores the importance of MTTF in gauging system reliability and emphasizes the impact of precise parameter adjustments on system performance and acceptance status.

![half_MTTFmin_RDC](https://github.com/BradenTink/SENG-637/assets/59784141/b23f3cd6-e42e-4e01-ae91-713b2f0af364)
Figure 8. Reliability Demonstration Chart with MTTFmin of 0.091 intervals and a FIO of 340 failures per 31 intervals.

The Reliability Demonstration Curve (RDC) is a powerful tool used to assess the reliability of systems and products, offering several advantages. One of its primary benefits is the clear visual representation it provides, allowing stakeholders to easily understand and interpret reliability data. This graphical depiction enables quantitative analysis, facilitating comparisons between different systems or products and aiding informed decision-making. Additionally, RDCs can predict failure rates over time by extrapolating the curve, which assists organizations in planning maintenance, replacements, or upgrades. Moreover, the standardization of RDCs within industries ensures consistent evaluation and comparison of reliability across various products or systems. Lastly, RDCs are commonly employed to assess compliance with reliability requirements specified in contracts, regulations, or standards, serving as a means to verify performance against predefined criteria.

However, the use of RDCs is not without its limitations and drawbacks. Constructing an accurate RDC necessitates sufficient historical failure data, which may not always be available, particularly for new or complex systems. Moreover, RDCs typically assume a constant failure rate over time, which may not always hold true in real-world scenarios where failure rates may vary due to factors such as wear and tear, environmental conditions, or usage patterns. Consequently, RDCs may offer a general indication of reliability but may not capture all potential failure modes or factors influencing reliability, leading to inaccuracies in long-term predictions. Additionally, interpreting RDCs and understanding the implications of different curve shapes and parameters can be challenging for individuals without a background in reliability engineering or statistics. Finally, there is a risk of overconfidence in the reliability of a system if RDCs are solely relied upon for assessment, especially if uncertainties and limitations associated with the curve are not adequately considered. Therefore, while RDCs offer valuable insights into system reliability, they should be used judiciously in conjunction with other reliability assessment methods and considerations to ensure comprehensive and accurate evaluation.

# Comparison of Results

The Demonstration Chart shows the results as a failure unless a lower MTTF is used. The growth trend is variable for different sections of the chart, but the Laplace is positive is the final section, indicating the reliabilty should be improving as the program runs.

# Discussion on Similarity and Differences of the Two Techniques

The Demonstration Chart is based on the inter failure times only and target failure rate, whereas the Growth Analysis is based on the inter failure times and/or failure count and target failure rate. RDC should be used when failure data is limited to a few failures, time of failures are known, and one wants to find out what is the
trend for reliability of the system. The growth test requires an approprate model, and is used to predict the reliability over time.
Both are for prediciting reliability and trend, but go about it differently.

# How the team work/effort was divided and managed

Each team member tried both sections of the lab to try to get it to work. Since there were difficulties, the most complete of each section was used to assemble the final report.

# Difficulties encountered, challenges overcome, and lessons learned

There were quite a few difficulties using CASRE and SRTAT. The provided excel sheet did not import well into SRTAT, and using CASRE was a bit tricky.
In addition, although C-SFRAT worked well to explore various models, it did not have the Laplace test functionality. Thus, the provided data set had to be reformatted to work with SFRAT which provided the Laplace test functionality.

For Part 2, understanding the functionality of the Excel document was a bit tricky. The range of the RDC was limited to 16 for the x and y axis but since the data set had 31 data points, the plot had to be adjusted. Adjusting just the min/max of the x and y axis was not enough as the reject, continue test, and accept regions did not expand to the increased max range of the axis. Thus, modifications to the plot data sheet in the Excel document had to be made to expand these regions which required further exploration.

# Comments/feedback on the lab itself

The difficulties with CASRE and SRTAT made this lab the most frustrating, so the purpose of the lab was slightly lost in the difficulties with the tools.
