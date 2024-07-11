# CTIKG

## Introduction
CTIKG (Cyber Threat Intelligence Knowledge Graph) is an innovative tool with Large Language Model (LLM) and machine learning techniques. Its goal is to effectively build a security-oriented knowledge graph from CTI articles.

With the fast-paced development of the Internet infrastructure and the increasing cyber threats that target organizations, knowledge of these threats is aggressively collected and shared through CTI. However, the information provided by traditional blacklists is limited and the manual effort required to extract knowledge from CTI articles is not scalable. 

By uilizing LLM agent and segment processing with dual memory design, CTIKG constructs a security-oriented knowledge graph from numerous CTI articles, which has higher effectiveness than SOTA methods. It reveals the relationships among security-related entities and their behaviors across multiple articles, and further provides security-oriented community detection to help security analysts understand the knowledge of cyber threats more efficiently.

## Updates
- **[2024/07/10]** The paper “CTIKG: LLM-Powered Knowledge Graph Construction from Cyber Threat Intelligence” has been accepted by COLM 2024.
- **[2024/04/30]** Introduced CTIKG for knowledge graph construction.


## Publication
Liangyi Huang and Xusheng Xiao. CTIKG: LLM-Powered Knowledge Graph Construction from Cyber Threat Intelligence
In Proceedings of the First Conference on Language Modeling (COLM 2024), Philadelphia, PA, USA, Oct 2024.

## Citation
```BibTeX
@inproceedings{
huang2024ctikg,
title={CTIKG: LLM-Powered Knowledge Graph Construction from Cyber Threat Intelligence},
author={Liangyi Huang and Xusheng Xiao},
booktitle={Proceedings of the First Conference on Language Modeling (COLM 2024)},
year={2024},
location={Philadelphia, PA, USA},
month={October}
}
```

## Overview
CTIKG contains three main components. The overview figure is shown below.

![image](https://i.imgur.com/klouXZu.jpeg)

1. Short-term Memory Construction: Short-term Memory Construction deploys multiple
LLM agents to extract all the knowledge within single CTI article segment in the form
of triples. This phase employs four types of LLM agents: worker, integrator, refiner, and
checker.
2. Long-term Memory Construction: As CTIKG uses segment processing to overcome LLMs’
token limitation, the merger agent is responsible for assembling the triples from different
article segments into one long-term memory.
3. Knowledge Graph Construction: CTIKG builds a knowledge graph using multiple articles.
To ensure consistency across different articles for the same entity (e.g., a specific malware),
the tool continues using a merger agent to achieve a unified entity description


## Dual Memory Design
CtiKG uses a dual memory design to overcome the problems of using LLM directly. Specifically, instead of using the whole article as input, CtiKG divides the article into segments and extracts the triples from each segment. The triples for a segment form the short-term memory. Meanwhile, CtiKG keeps a long list of all triples extracted from different segments of the article, which is called the long-term memory.


## Short-Term Memory Construction
To construct the short-term memory of a text segment, CtiKG
deploys four types of LLM agents: Worker, Integrator, Refiner and
Checker.

- Worker Agent. The worker agent receives the text segment as input and extracts the security-related triples from the text.

- Integrator Agent. The integrator agent is used to extract a final triple from the results of the worker agents. Due to the randomness of LLM, multiple workers may use different triples to express the same entity relation in the text. The integrator summarizes the valid triples with identical meanings and outputs a representative triple.

- Refiner Agent. The refiner agent is designed to perform the normalization and resolution tasks. The refiner agent takes the triples from the integrator agent as input and refines the triples to ensure that the entities in the triples are normalized and resolved.

## Long-Term Memory Construction
CTIKG uses the merger agent to further refine the triples in short-term memory to fit the context of long-term memory. After refinement, CTIKG directly adds the triples into the long-term memory.

- Merger Agent. This agent determines whether the subject or object of the triples in short-term memory and long-term memory refer to the same entities. If so, this agent will modify the subject or the object of the triples in the short-term memory to unify the representation of the same entities.

## Retry Mechanism
-Checker Agent. The checker agent is designed to check whether the output of LLM agents is correct, based on whether the output of other agents contains the example text, or refuse performing the task. If the output of an LLM agent is found to be incorrect, CtiKG then instructs the LLM agent to re-perform the task.

## Workflow
- Create a Dataframe to store the article in the 'sentence' column.
- Run the 'Knowledge Graph Construction.ipynb' to extract the triples from the article.
- Check local directory for the 'Result.xlsx' file which contains the extracted triples.

## Evaluation
We evaluate the performance of CtiKG on sentences and articles benchmark datasets. The results show that CtiKG outperforms the state-of-the-art methods in terms of precision, recall, and F1-score.

## Performance On Single Sentence
Overall, CTIKG can achieves 91.89% precision and 89.39% recall, and outperforms the SOTA methods such as Extractor, REBEL, and KnowGL. In addition, the evaluation about different LLM model demonstrates that CTIKG with YI model can achieve the best performance than other LLM models such as GPT4 and GPT3.5.
<table class="tg">
<thead>
  <tr>
    <th class="tg-c3ow" rowspan="2">Tactics </th>
    <th class="tg-c3ow" colspan="2">CTIKG</th>
    <th class="tg-c3ow" colspan="2">CTIKG with GPT4</th>
    <th class="tg-c3ow" colspan="2">CTIKG with GPT3.5</th>
    <th class="tg-c3ow" colspan="2">Extractor</th>
    <th class="tg-c3ow" colspan="2">REBEL</th>
    <th class="tg-c3ow" colspan="2">KnowGL</th>
  </tr>
  <tr>
    <th class="tg-0pky">Precision</th>
    <th class="tg-0pky">Recall</th>
    <th class="tg-0pky">Precision</th>
    <th class="tg-0pky">Recall</th>
    <th class="tg-0pky">Precision</th>
    <th class="tg-0pky">Recall</th>
    <th class="tg-0pky">Precision</th>
    <th class="tg-0pky">Recall</th>
    <th class="tg-0pky">Precision</th>
    <th class="tg-0pky">Recall</th>
    <th class="tg-0pky">Precision</th>
    <th class="tg-0pky">Recall</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0pky">Initial Access</td>
    <td class="tg-dvpl">91.89%</td>
    <td class="tg-dvpl">85.61%</td>
    <td class="tg-dvpl">71.59%</td>
    <td class="tg-dvpl">69.84%</td>
    <td class="tg-dvpl">81.77%</td>
    <td class="tg-dvpl">82.20%</td>
    <td class="tg-dvpl">73.65%</td>
    <td class="tg-dvpl">75.00%</td>
    <td class="tg-dvpl">17.86%</td>
    <td class="tg-dvpl">17.46%</td>
    <td class="tg-dvpl">4.55%</td>
    <td class="tg-dvpl">1.52%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Execution</td>
    <td class="tg-dvpl">88.27%</td>
    <td class="tg-dvpl">82.16%</td>
    <td class="tg-dvpl">82.23%</td>
    <td class="tg-dvpl">82.01%</td>
    <td class="tg-dvpl">65.35%</td>
    <td class="tg-dvpl">67.61%</td>
    <td class="tg-dvpl">84.98%</td>
    <td class="tg-dvpl">83.75%</td>
    <td class="tg-dvpl">17.44%</td>
    <td class="tg-dvpl">17.25%</td>
    <td class="tg-dvpl">13.26%</td>
    <td class="tg-dvpl">5.87%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Persistence</td>
    <td class="tg-dvpl">95.37%</td>
    <td class="tg-dvpl">98.15%</td>
    <td class="tg-dvpl">74.25%</td>
    <td class="tg-dvpl">70.99%</td>
    <td class="tg-dvpl">77.47%</td>
    <td class="tg-dvpl">76.54%</td>
    <td class="tg-dvpl">83.68%</td>
    <td class="tg-dvpl">80.67%</td>
    <td class="tg-dvpl">33.33%</td>
    <td class="tg-dvpl">30.86%</td>
    <td class="tg-dvpl">24.07%</td>
    <td class="tg-dvpl">12.35%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Privilege Escalation</td>
    <td class="tg-dvpl">95.03%</td>
    <td class="tg-dvpl">85.83%</td>
    <td class="tg-dvpl">72.51%</td>
    <td class="tg-dvpl">71.18%</td>
    <td class="tg-dvpl">71.02%</td>
    <td class="tg-dvpl">67.36%</td>
    <td class="tg-dvpl">91.52%</td>
    <td class="tg-dvpl">87.39%</td>
    <td class="tg-dvpl">21.53%</td>
    <td class="tg-dvpl">10.56%</td>
    <td class="tg-dvpl">18.06%</td>
    <td class="tg-dvpl">5.56%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Defense Evasion</td>
    <td class="tg-dvpl">87.39%</td>
    <td class="tg-dvpl">85.61%</td>
    <td class="tg-dvpl">93.56%</td>
    <td class="tg-dvpl">88.06%</td>
    <td class="tg-dvpl">78.17%</td>
    <td class="tg-dvpl">80.00%</td>
    <td class="tg-dvpl">79.85%</td>
    <td class="tg-dvpl">82.64%</td>
    <td class="tg-dvpl">16.67%</td>
    <td class="tg-dvpl">9.72%</td>
    <td class="tg-dvpl">16.11%</td>
    <td class="tg-dvpl">6.90%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Credential Access</td>
    <td class="tg-dvpl">92.01%</td>
    <td class="tg-dvpl">88.10%</td>
    <td class="tg-dvpl">77.99%</td>
    <td class="tg-dvpl">77.62%</td>
    <td class="tg-dvpl">74.60%</td>
    <td class="tg-dvpl">76.90%</td>
    <td class="tg-dvpl">82.99%</td>
    <td class="tg-dvpl">76.96%</td>
    <td class="tg-dvpl">22.98%</td>
    <td class="tg-dvpl">16.95%</td>
    <td class="tg-dvpl">15.69%</td>
    <td class="tg-dvpl">10.19%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Discovery</td>
    <td class="tg-dvpl">95.91%</td>
    <td class="tg-dvpl">94.22%</td>
    <td class="tg-dvpl">88.33%</td>
    <td class="tg-dvpl">87.50%</td>
    <td class="tg-dvpl">76.89%</td>
    <td class="tg-dvpl">83.75%</td>
    <td class="tg-dvpl">79.26%</td>
    <td class="tg-dvpl">75.97%</td>
    <td class="tg-dvpl">10.00%</td>
    <td class="tg-dvpl">9.95%</td>
    <td class="tg-dvpl">8.33%</td>
    <td class="tg-dvpl">10.22%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Lateral Movement</td>
    <td class="tg-dvpl">94.62%</td>
    <td class="tg-dvpl">95.26%</td>
    <td class="tg-dvpl">89.81%</td>
    <td class="tg-dvpl">88.89%</td>
    <td class="tg-dvpl">79.80%</td>
    <td class="tg-dvpl">80.21%</td>
    <td class="tg-dvpl">88.04%</td>
    <td class="tg-dvpl">80.07%</td>
    <td class="tg-dvpl">31.25%</td>
    <td class="tg-dvpl">17.36%</td>
    <td class="tg-dvpl">26.39%</td>
    <td class="tg-dvpl">14.93%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Collection</td>
    <td class="tg-dvpl">94.76%</td>
    <td class="tg-dvpl">92.94%</td>
    <td class="tg-dvpl">85.50%</td>
    <td class="tg-dvpl">87.90%</td>
    <td class="tg-dvpl">82.83%</td>
    <td class="tg-dvpl">84.92%</td>
    <td class="tg-dvpl">85.30%</td>
    <td class="tg-dvpl">76.22%</td>
    <td class="tg-dvpl">28.33%</td>
    <td class="tg-dvpl">17.40%</td>
    <td class="tg-dvpl">17.89%</td>
    <td class="tg-dvpl">11.11%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Command and Control</td>
    <td class="tg-dvpl">89.08%</td>
    <td class="tg-dvpl">90.04%</td>
    <td class="tg-dvpl">95.05%</td>
    <td class="tg-dvpl">96.17%</td>
    <td class="tg-dvpl">70.23%</td>
    <td class="tg-dvpl">76.35%</td>
    <td class="tg-dvpl">79.00%</td>
    <td class="tg-dvpl">76.71%</td>
    <td class="tg-dvpl">23.10%</td>
    <td class="tg-dvpl">14.31%</td>
    <td class="tg-dvpl">15.74%</td>
    <td class="tg-dvpl">6.20%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Exfiltration</td>
    <td class="tg-dvpl">93.84%</td>
    <td class="tg-dvpl">90.62%</td>
    <td class="tg-dvpl">88.01%</td>
    <td class="tg-dvpl">86.63%</td>
    <td class="tg-dvpl">82.37%</td>
    <td class="tg-dvpl">87.71%</td>
    <td class="tg-dvpl">89.12%</td>
    <td class="tg-dvpl">84.57%</td>
    <td class="tg-dvpl">16.07%</td>
    <td class="tg-dvpl">10.20%</td>
    <td class="tg-dvpl">6.59%</td>
    <td class="tg-dvpl">6.82%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Impact</td>
    <td class="tg-dvpl">84.49%</td>
    <td class="tg-dvpl">84.10%</td>
    <td class="tg-dvpl">92.27%</td>
    <td class="tg-dvpl">97.10%</td>
    <td class="tg-dvpl">71.92%</td>
    <td class="tg-dvpl">71.01%</td>
    <td class="tg-dvpl">73.36%</td>
    <td class="tg-dvpl">72.27%</td>
    <td class="tg-dvpl">23.41%</td>
    <td class="tg-dvpl">18.62%</td>
    <td class="tg-dvpl">17.39%</td>
    <td class="tg-dvpl">10.94%</td>
  </tr>
  <tr>
    <td class="tg-0pky">Average</td>
    <td class="tg-dvpl">91.89%</td>
    <td class="tg-dvpl">89.39%</td>
    <td class="tg-dvpl">84.26%</td>
    <td class="tg-dvpl">83.66%</td>
    <td class="tg-dvpl">76.04%</td>
    <td class="tg-dvpl">77.88%</td>
    <td class="tg-dvpl">82.56%</td>
    <td class="tg-dvpl">79.35%</td>
    <td class="tg-dvpl">21.83%</td>
    <td class="tg-dvpl">15.89%</td>
    <td class="tg-dvpl">15.34%</td>
    <td class="tg-dvpl">8.55%</td>
  </tr>
</tbody>
</table>

## Performance on Article
The performance of CTIKG on the article benchmark dataset is also evaluated. The results show that CTIKG achieves 86.88% precision and 70.86% recall, and outperforms the SOTA methods such as Extractor, REBEL, and KnowGL. In addition, even on the complex article such as chain type article, CTIKG can still achieve 82.38% precision and 62.89% recall.

<table>
  <thead>
    <tr>
      <th rowspan="2">Type</th>
      <th colspan="2">CTIKG</th>
      <th colspan="2">Extractor</th>
      <th colspan="2">REBEL</th>
      <th colspan="2">KnowGL</th>
    </tr>
    <tr>
      <th>Precision</th>
      <th>Recall</th>
      <th>Precision</th>
      <th>Recall</th>
      <th>Precision</th>
      <th>Recall</th>
      <th>Precision</th>
      <th>Recall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Standalone</td>
      <td>90.21%</td>
      <td>79.63%</td>
      <td>50.83%</td>
      <td>34.03%</td>
      <td>31.67%</td>
      <td>6.11%</td>
      <td>39.58%</td>
      <td>9.81%</td>
    </tr>
    <tr>
      <td>Chain</td>
      <td>82.38%</td>
      <td>62.89%</td>
      <td>66.00%</td>
      <td>38.67%</td>
      <td>46.67%</td>
      <td>16.67%</td>
      <td>0.00%</td>
      <td>0.00%</td>
    </tr>
    <tr>
      <td>Overview</td>
      <td>88.04%</td>
      <td>70.08%</td>
      <td>50.97%</td>
      <td>50.13%</td>
      <td>15.64%</td>
      <td>5.05%</td>
      <td>5.28%</td>
      <td>2.27%</td>
    </tr>
    <tr>
      <td>Average</td>
      <td>86.88%</td>
      <td>70.86%</td>
      <td>55.93%</td>
      <td>40.94%</td>
      <td>31.32%</td>
      <td>9.28%</td>
      <td>14.95%</td>
      <td>4.03%</td>
    </tr>
  </tbody>
  </table>

## Tool Requirements
Python Version >= 3.8.0

Python Library:

scalellm==0.0.5

openai==0.28.0

numpy >= 1.23.4

scikit-learn >= 1.1.2

simpletransformers >= 0.63.9

hdbscan >= 0.8.28

torch >= 1.12.1

networkx >= 2.8.7

cdlib >= 0.2.6
