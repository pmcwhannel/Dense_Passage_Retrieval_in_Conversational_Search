# TREC Conversational Assistance Track (CAsT) 

There are currently few datasets appropriate for training and evaluating models for Conversational Information Seeking (CIS). The main aim of TREC CAsT is to advance research on conversational search systems. The goal of the track is to create a reusable benchmark for open-domain information centric conversational dialogues. 

The track will run in 2019 and establish a concrete and standard collection of data with information needs to make systems directly comparable. 

This is the first year of TREC CAsT, which will run as a track in [TREC](https://trec.nist.gov/). This year we aim to focus on candidate information ranking in context:
* Read the dialogue context: Track the evolution of the information need in the conversation, identifying salient information needed for the current turn in the conversation
* Retrieve Candidate Response Information: Perform retrieval over a large collection of paragraphs (or knowledge base content) to identify relevant information

## Year 2
 * Year 2 is happening in 2020
 * See the [TREC Boaster](https://docs.google.com/presentation/d/1ECg07GchcbngdLls6qo4CNULj-odiHOLZBTZLxJ_RS8/edit?usp=sharing) for an initial preview

## Year 1 Task Guidelines
* [Year 1 task guidelines](https://docs.google.com/document/d/1SH6UdZ9xQUZzhxnCzlsJvXtPEUCuiow5MXlrsA22pKs/edit?usp=sharing)
* Comments and feedback are welcome.

## Year 1 Submission
* The [submission form](http://ir.nist.gov/trecsubmit/converse.html) is now available!
* The [Script](https://trec.nist.gov/act_part/scripts/19.scripts/check_cast.pl) is available to check the validity of your runs.
* Note: If you registered, but your name is not in the list, please verify that you have submitted the dissemination form.

# Data

## Topics
 * [Training topics year 1 V1.0](https://github.com/daltonj/treccastweb/tree/master/2019/data/training) - 30 example training topics
  * [Training judgments](https://github.com/daltonj/treccastweb/blob/master/2019/data/training/train_topics_mod.qrel) - We provide limited (incomplete) training data for 5 topics (approximately 50 turns). These are judged from the baseline retrieval run (below).  The judgments are graded on a three point scale (2 very relevant, 1 relevant, and 0 not relevant). 

 * [Evaluation topics year 1 V1.0](https://github.com/daltonj/treccastweb/tree/master/2019/data/evaluation) - 50 evaluation topics
 
 * Additional resources: MS MARCO Conversational Search Sessions  [Conversational Search data](https://github.com/microsoft/MSMARCO-Conversational-Search) and [train data](https://msmarco.blob.core.windows.net/conversationalsearch/ann_session_train.tar.gz) is released.
 
### Resolved Topic Annotations
 * To facilitate work on passage ranking only we performed manual resolution of coreference as well as conversational ambiguity for topics.  We make these available to participants who may not have access to automatic methods. Runs using this data manual runs. The annotations are provided in a tab separated format with the turn id (query id) and the rewritten query in text form.
 * TRAIN: Sample annotations on [two training queries](https://github.com/daltonj/treccastweb/blob/master/2019/data/training/train_topic_sample_annotated_resolved_v1.0.tsv) (for exemplars) 
 * EVALUTION: Complete annotations on the [evaluation topics](https://github.com/daltonj/treccastweb/blob/master/2019/data/evaluation/evaluation_topics_annotated_resolved_v1.0.tsv) for the year 1 evaluation queries.  

## Baselines
 * [Indri search interface](http://boston.lti.cs.cmu.edu/Services/treccast19) - We provide an Indri index of the CAsT collection.  See the [help page](http://boston.lti.cs.cmu.edu/Services/treccast19/help-db.html) for details on indexing parameters and statistics. It includes a standard [batch search](http://boston.lti.cs.cmu.edu/Services/treccast19_batch/) API limited to 50 queries per batch.)
 * Baseline retrieval - We provide the queries and run files in [trec eval](https://github.com/usnistgov/trec_eval) format: [train queries](https://github.com/daltonj/treccastweb/blob/master/2019/data/training/train_topics.query), [train run file](http://boston.lti.cs.cmu.edu/vaibhav2/cast/train_topics.teIn), [test queries](https://github.com/daltonj/treccastweb/blob/master/2019/data/test_topics.query), [test run file](http://boston.lti.cs.cmu.edu/vaibhav2/cast/test_topics.teIn) - We provide an Indri baseline run with Query Likelihood run, including both the topics and run files. Queries are generated by running AllenNLP coreference resolution to perform rewriting and stopwords are removed using the Indri stopword list.  
 
## Collection
 * The corpus is a combination of three standard TREC collections: MARCO Ranking passages, Wikipedia (TREC CAR), and News (Washington Post)
 * The [MS MARCO Passage Ranking collection](https://msmarco.blob.core.windows.net/msmarcoranking/collection.tar.gz) - This file only includes the passage id and passage text.  For convenience, we also provide a passage id -> URL mapping file in TSV format [pid to URL file](http://boston.lti.cs.cmu.edu/vaibhav2/cast/marco_pas_url.tsv). 
 * The [TREC CAR paragraph collection v2.0](http://trec-car.cs.unh.edu/datareleases/v2.0/paragraphCorpus.v2.0.tar.xz)
 * The [TREC Washington Post Corpus version 2](https://ir.nist.gov/wapo/WashingtonPost.v2.tar.gz): Note this is behind a password and requires an organizational agreement, to obtain it see: https://ir.nist.gov/wapo/
  
### Document ID format
 * The document id format is `[collection_id_paragraph_id]` with collection id and paragraph id separated by an underscore.
 * The collection ids are in the set: `{MARCO, CAR, WAPO}`. 
 * The paragraph ids are: standard provided by MARCO and CAR. For WAPO the paragraph ID is `[article_id-paragraph_index]` where the paragraph_index is the *starting from 1-based* index of the paragraph using the provided paragraph markup separated by a single dash. 
 * Example WaPo combined document id: `[WAPO_903cc1eab726b829294d1abdd755d5ab-1]`, or CAR: `[CAR_6869dee46ab12f0f7060874f7fc7b1c57d53144a]`
 
### Duplicate handling
 * Early analysis found that both the MARCO and WaPo corpora both contain a significant number of near duplicate paragraphs. We have run near-dupliate detection to cluster results; only one result per duplicate cluster will be evaluated.  It is suggested that you remove dupliates (keeping the canonical document) from your indices.
 * A README with the [process and file format](http://boston.lti.cs.cmu.edu/Services/treccast19/duplicate_description.txt).
 * Washington Post [duplicate file](http://boston.lti.cs.cmu.edu/Services/treccast19/wapo_duplicate_list_v1.0.txt)
 * MARCO [duplicate file](http://boston.lti.cs.cmu.edu/Services/treccast19/duplicate_list_v1.0.txt)
 * Note: The tools in the repository below require these files as input for processing the collection and perform deduplication when the data is generated.
 
## Code and tools
* [TREC-CAsT Tools](https://github.com/gla-ial/trec-cast-tools) repository with code and scripts for processing data. 
* The tools contain scripts for parsing the collection into standard indexing formats. It also provides APIs for working with the topics (in text, json, and protocol buffer formats).
* Note: This will evolve over time, it currently contains topic definition files and scripts for reading and loading topics. 

## Year 1 Planning slides 
* [Year 1 planning information](https://docs.google.com/presentation/d/1z053BrUeozwTtDEimX4t1ik0Mc5eTl5rTct53mAKd4s/edit?usp=sharing)
* Comments and feedback are welcome.

**Information Needs**
* ~50-100 topics with manually defined trajectories
* Start from initial general topic
* Conversation evolves across ‘realistic’ facets for ~10 turns 
* Manually created topics from crowdsourcing

# News
 - June 29: Training data and complete baseline runs
 - June 26: Additional resources released (baseline, collection index, collections tools)
 - June 14: Evaluation topics released
 - May 23: Training topics released
 - April 18th: Guidelines released
 - November 13: Announcement that the track will run next year. 
 - March 19: Sample topic data for conversational and MARCO sessions available


# Contact
* Twitter: [@treccast](https://twitter.com/treccast)
* Slack: [treccast.slack.com](https://join.slack.com/t/treccast/shared_invite/enQtNDgwOTE0NTY3MDQyLTljNTM0YmZmYzY0NzJiODNiYWYyMmZjMGRmZTNlNTZlZGVhY2JiNzlkMjc0ODc3NjU0NzkzMTlhYzFmNWFkNTk)
* Google groups [trec-cast@googlegroups.com](https://groups.google.com/forum/#!forum/trec-cast)

# Important Dates
* Training data release: May 23rd
* Test topic release: June 12th
* Run submission: ~~August 16th~~ August 18th **(Extended deadline)**


# Evaluation 
Forthcoming

# Organizers
 * [Jeff Dalton](http://www.dcs.gla.ac.uk/~jeff/), University of Glasgow
 * [Chenyan Xiong](https://www.linkedin.com/in/chenyan-xiong-4a103257/), Microsoft Research
 * [Jamie Callan](http://www.cs.cmu.edu/~callan/), Carnegie Mellon University
 
# Advisory Committee
  * Laura Dietz, University of New Hamsphire
  * Jimmy Lin, University of Waterloo
  * Julia Kiseleva, Microsoft Research
  * Vanessa Murdock, Amazon Research
  * Paul Bennett, Microsoft Research
  * Zhiting Hu, CMU
  * Anton Leuski, USC
