# CS277-Final-Project



目标：推动金融领域的 AI 系统，在 **公司披露文件**（SEC filings，如 10-K, 10-Q, 8-K, 委托书（DEF-14A）、财报会议记录等）中，做出“多步、证据驱动”的检索与推理，以支持可解释、准确的投资研究结论。



## 目标与动机

- 传统的检索系统（或金融信息检索系统）可能只能做关键词匹配或简单排序，不容易在复杂、长文档、多种类型文件中给出高质量、有解释性的答案。
- 本任务希望 AI 系统能够跨越这些限制，对复杂问题进行“推理 + 检索”，即不只是“哪个文件最可能含有答案”，还要“在哪些段落里可以找到答案”，并且能给出证据支撑其选取。
- 这样的能力在机构级别的投资研究中非常重要：用户不只是要答案，还要知道“答案从哪里来”、“依据是什么”。



这是一个 **两阶段（two-stage）检索任务**，具体包括：

| 阶段                                     | 任务内容                                                     | 输入 / 输出                                                |
| ---------------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------- |
| **Document-Level Ranking（文件级排序）** | 给定一个金融问题（查询），将一组候选文件类型（例如 10-K、10-Q、8-K、DEF-14A、earnings transcript 等）按其包含答案的可能性排序 | 输入：问题 + 候选文件类型；输出：这些文件类型的排序        |
| **Chunk-Level Ranking（片段级排序）**    | 在某个已选定的文件内部，选出若干最相关的段落（passage／chunk）并排序 | 输入：某个文件 + 问题；输出：排名前若干段落（通常前 5 段） |



<img src="img/task.png" alt="image-20251015210549937" style="zoom:50%;" />



## 数据集

数据集来源：https://arxiv.org/pdf/2508.14052

**文件语料**

- 从美国 SEC 的 EDGAR 系统获取公司披露文件（2023–2024 年间）。
- 涵盖多种常见文件类型：10-K、10-Q、8-K、财报会议记录（earnings transcripts）、DEF-14A（代理表决说明书/委托书）等。
- 每个文档按段落切分为多个 chunk。特别地，表格（tables）被视为一个整体单元，并与其上下文一起作为一个 chunk 处理（以保留结构完整性）。
- 为控制规模与焦点，作者选取了 S&P 500 上市公司作为主要构建对象。

**查询构造**

- 作者邀请有金融背景的两位专家，为每家公司分别在若干维度（共 10 类别，例如 管理层评论、行业竞争、风险挑战、运营指标、财务结果等）写查询。
- 每个专家为每家公司在每个类别写十个查询（共 20 个候选），然后通过交叉验证选择最终 10 个高质量查询。
- 这样可以保证查询覆盖不同类型的投资／分析兴趣点，且具有实际可用性。

**人工标注 / 排序标签**

- 对于每个查询，标注者首先在文档类型级别对五种文件类别（10-K、10-Q、8-K、earnings、DEF-14A）进行排序。
- 之后，在被认为最可能包含答案的文档内，对其各段落 chunk 进行标注，使用 0 / 1 / 2 三等级（0 = 无关，1 = 部分相关，2 = 直接相关）分配相关性分数。
- 标注由两位 annotator 独立作业，若存在分歧，则通过仲裁 (adjudication) 加以解决。

**规模与统计**

- 全数据集中共有大约 **26,000** 个查询-文档对的标注样本。
- 涵盖多个公司、多个文档类型与多个查询领域。
- 在论文中也划分了训练／验证／测试集。
- 在实验评估中，作者展示部分在 S&P 500 公司子集上的结果。





## Previous work and Related work

TBD





## Method

TBD



## Timetable and workload division

TBD





## References

```
@misc{choi2025finagentbenchbenchmarkdatasetagentic,
      title={FinAgentBench: A Benchmark Dataset for Agentic Retrieval in Financial Question Answering}, 
      author={Chanyeol Choi and Jihoon Kwon and Alejandro Lopez-Lira and Chaewoon Kim and Minjae Kim and Juneha Hwang and Jaeseon Ha and Hojun Choi and Suyeol Yun and Yongjin Kim and Yongjae Lee},
      year={2025},
      eprint={2508.14052},
      archivePrefix={arXiv},
      primaryClass={cs.IR},
      url={https://arxiv.org/abs/2508.14052}, 
}
```

