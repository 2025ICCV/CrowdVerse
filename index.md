---
layout: default
---

## CrowdVerse: Boosting Crowd Understanding with a Realistic and Comprehensive Multitask Dataset

![pic](https://github.com/xzh0312/minima/blob/master/imgs/CrowdVerse.png?raw=true)

## Overviews

- To promote the development of crowd understanding tasks, we propose CrowdVerse, the largest real-world crowd dataset to date, which supports a wide range of crowd-related tasks.
  
- In CrowdVerse, over 1.15 million crowd images have been manually annotated with diverse labels, including crowd locations, trajectories, interaction groups, and textual descriptions, enabling the training and evaluation of diverse crowd-related tasks.
  
- We report benchmark results on various tasks using CrowdVerse, and extensive experiments demonstrate that the dataset's scale and diversity introduce heightened challenges across multiple tasks, providing ample opportunities for future research.
  

![pic](https://github.com/xzh0312/minima/blob/master/imgs/advantage.png?raw=true)

## Labels

### 1. Human Numbers

![pic](https://github.com/xzh0312/minima/blob/master/imgs/HumanNumbers.png?raw=true)

### 2. Localization

![pic](https://github.com/xzh0312/minima/blob/master/imgs/Localization.png?raw=true)

### 3. Human Interaction

![pic](https://github.com/xzh0312/minima/blob/master/imgs/HumanInteraction.png?raw=true)

### 4. Text Description

![pic](https://github.com/xzh0312/minima/blob/master/imgs/text_description.png?raw=true)

### Time-Continuous Scene

![pic](https://github.com/xzh0312/minima/blob/master/imgs/Time-ContinuousScene.png?raw=true)

### More Precise Labels

![pic](https://github.com/xzh0312/minima/blob/master/imgs/MorePreciseLabels.png?raw=true)

## Benchmark Experiments

- [Trajectory Prediction](#section1)
- [Image Captioning](#section2)
- [Social Interaction Detection](#section3)
- [Image-text Retrieval](#section4)
- [Background Segmentation](#section5)
- [Image-text Generation](#section6)

### 1. Trajectory Prediction <a id="section1"></a>

**1.1 Baselines**

**·Social LSTM:** It is the pioneering data-driven method for the trajectory prediction task. The motion pattern of each pedestrian is modeled with an LSTM. And the hidden states of pedestrians are pooled with neighboring pedestrians at each time-step using a pooling mechanism, namely, Social Pooling, which achieves modeling social interactions.

**·Social GAN:** It is the first generative model based method for the pedestrian trajectory prediction task. Generative Adversarial Network is used to generate more socially-acceptable trajectories. And a new pooling mechanism is proposed to model social interactions. 

**·STGAT:** It is one of the first graph based method for the pedestrian trajectory prediction task. The social interactions between pedestrians are modeled by a graph structure and then captured by the Graph Attention Networks. And it also uses two LSTMs to capture spatial and temporal dynamics.

**·STAR:** It is the first transformer based framework for the trajectory prediction task, which tackles the motion patterns and social interactions by only attention mechanism. STAR captures complex spatio-temporal interactions by interleaving between spatial and temporal Transformers.

**1.2 Implementation Details**

We predict the future 12 timestep trajectories (4.8 sec) given the historical 8 timestep trajectories (3.2 sec). We train the models using Adam optimizer with a learning rate 0.001. And the implementation details of all models are consistent with the baselines.

**1.3 Experimental Results**

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/TrajectoryPredictionCases.png?raw=true)

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/model_sheet1.png?raw=true){: .center90 }

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/model_sheet2.png?raw=true){: .center90 }

**1.4 Analysis**

We show the performance of baseline models on CrowdVerse and two widely used public pedestrian trajectory prediction datasets ETH and UCY. Following previous works, we use the same evaluation methodology. We predict the future 12 timestep trajectories (4.8 sec) given the historical 8 timestep trajectories (3.2 sec). And two metrics are used for evaluation: Average Displacement Error (ADE), Final Displacement Error (FDE). 

The experimental results of quantitative analysis are shown in the tables above. The overall experimental results show that the performances of baseline models on our proposed datasets are much worse than that on existing datasets ETH and UCY. These quantitative experimental results indicate that CrowdVerse is a very large-scale dataset, containing a wide range of scenes and a large number of hard cases with complex interactions.

In order to better understand the contribution of CrowdVerse, we visualize three challenging scenes. Case (a) shows a very dense crowd scene, in which the density of pedestrians far exceeds that of the existing datasets ETH and UCY. Case (b) shows a drastic scene, in which the behaviors of dancers change rapidly over time. Case (c) shows a scene from the first-person perspective rather than the top-down perspective and. There three kinds of challenging scenes are not contained in the existing datasets ETH and UCY. The performance of baseline models deteriorates significantly in these three scenes compared to the results of existing datasets ETH and UCY. Hence, our proposed datasets address the limitation of the lack of large-scale datasets with complex interactions for the trajectory prediction task, which can further help to design better models. 


### 2. Image Captioning <a id="section2"></a>

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Image_Captioning1.png?raw=true)

**2.1 Baselines**

We compare with several SOTAs. According to the trainable parameters size, they can be divided into:

**1)** Vision-Language Pre-train Model based on the Transformer:BLIP, BLIP-2. Through pretraining, they extensively learn feature representations and cross-modal interactions between vision and language, and are then fine-tuned for a variety of downstream tasks; 

**2)** Retrieval-Augmentated Lightweight Model: SmallCap, which generates a caption conditioned on an input image and related captions retrieved from a datastore to enhance generalization ability, demonstrating high utility in image captioning;

**3)** Training-free Controllable Model: ConZIC, with a novel sampling-based non-autoregressive language model named Gibbs-BERT, which can generate and continuously polish every word; 

**4)** Multi-modal Large LanguageModel: LLaVA, an end-to-end trained large multimodal
model that connects a vision encoder and LLM for general purpose visual and language, using language-only GPT-4 to generate multimodal language-image instruction-following data

**2.2 Implementation Details**

We select 50 samples from the proposed dataset and formulate a meta-evaluation based on off-the-shelf image captioning methods, including BLIP, BLIP-2, SmallCap, ConZIC and LLaVA. Note that the above models consist of an all-round model types ranging from vision-language pre-train model, retrieval-augmentated lightweight model, training-free controllable model and multi-modal large language model.

For evaluation, we utilize two methods to both qualitatively and quantitatively verify the quality of the captions: (1) GPT-4o and human as the judge, to rate each generated caption according to designed prompt and task, (2) CLIPscore, RefCLIPscore and other reference-based metrics: matching score between the generated caption and ground-truth image/text.

**·BLIP:** blip-image-captioning-large

**·BLIP2:** blip2-opt-2.7b

**·SMALLCAP:** SmallCap7M with COCO datestore

**·ConZIC:**
 --run_type "caption" --order "shuffle" --sentence_len 10 --caption_img_path "./examples/girl.jpg" --samples_num 3 --lm_model "bert-base-uncased" --match_model "openai/clip-vit-base-patch32"  --alpha 0.02 --beta 2.0 
 
**·LLaVa:**  llava-v1.5-7b
--temperature 0.2 --top_p None --num_beams 1 --max_new_tokens 512
prompt = "Please generate a description of this image, as detailed as possible."

**2.3 Experimental Results**

**Human-judge evaluation scores (1-5), CLIPscore (non-ref), RefCLIPscore and other metrics:**

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/sheeet1.png?raw=true)
### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Image_Captioning4.png?raw=true)

**2.4 Analysis**

**·BLIP, BLIP2, SMALLCAP:** Coarse-grained, with short length and low information content (correlated with the training dataset characteristics). 

**·ConZIC:** Lacks fluency in generation and exhibits poor accuracy (a drawback of increased diversity). 

**·LLaVa:** Moderate information content (with non-specific, ambiguous descriptions), demonstrates logical coherence, but may engage in non-visually grounded reasoning and exhibit repetition between sentences. 

**·GT (Ground Truth):** Rich in information content, clear in structure and logic, combines both coarse-grained and fine-grained details, and provides accurate visual reasoning information. These observations highlight the varying capabilities and limitations of different vision-language models in terms of their output characteristics, information density, and reasoning abilities.

Due to CLIP’s max input length limit of 77, we truncated captions exceeding this limit at the first sentence, which may introduce some inaccuracies in the results. This issue also suggests that as model capabilities improve, evaluation metrics for long texts need further updates. For reference-based metrics, we used the ground-truth captions as the reference, which includes both coarse-grained and fine-grained correct visual information as well as reasoning details.

As shown in the table, ConZIC achieved better results on CLIP-based metrics, attributed to its CLIP-oriented optimization strategy. However, due to its lack of contextual fluency, its scores for B@4, CIDEr, and SPICE are quite low. SmallCap’s accuracy is relatively poor (CLIPscore), as its generation quality heavily depends on the relevance of the retrieved captions. Among all baselines, LLaVa performed the best overall, which aligns with GPT-4’s evaluation results and our quantitative analysis.

It is noteworthy that reference-based metrics generally yield lower scores, primarily due to the comprehensive nature of the reference captions, which often involve intricate information mining and effective reasoning. Consequently, the challenge posed by candidate captions significantly increases under such evaluation metrics. This suggests that the ability of current general SOTA image captioning models to generate fine-grained long caption still needs improvement, and the corresponding evaluation metrics are lacking. The dataset we propose can effectively advance research and exploration in this direction.

```
**Appendix: Prompt for GPT**:
prompt_for_single_grading = '''
[System]
Please act as an impartial judge and evaluate the quality of the response provided by an AI assistant to describe the input image.
Your evaluation should consider factors such as Accuracy, Fluency, Amount of information, Level of detail, and Logical. Begin your evaluation by providing a short explanation.
 Be as objective as possible. After providing your explanation, please rate the response on a scale of 1 to 5 by strictly following this format: "[[rating]]", for example: "Rating: [[5]]".

[Evaluation Factors]
1. Accuracy: The description of the model's answer needs to match the input image and cannot contain inaccurate visual information and mismatched speculations.
2. Fluency: The model's answer needs to be a complete and fluent sentences.
3. Amount of information: The model description needs to contain enough information, including coarse-grained and fine-grained.
4. Level of detail: The model description needs to be as specific as possible, without missing out on subtle visual objects in the image.
5. Logical: The model needs to have certain language organization skills and excellent logic.

[The Start of Assistant’s Answer]
{answer}
[The End of Assistant’s Answer]
'''
```


### 3. Social Interaction Detection <a id="section3"></a>

<img src="https://github.com/xzh0312/minima/blob/master/imgs/Social_Interaction_Detection1.png?raw=true" alt="pic" style="display: block; margin: 0 auto; max-width: 70%;">

**3.1 Baselines**

The Human Interaction Detection task aims to identify and detect social interaction groups (SIGs), defined as groups emphasizing mutual influence between intentions. This concept originates from psychological research, where it is described as ”behavior attempting to influence or take into account the subjective experiences or intentions of others.” For instance, in classroom settings involving interactions between students and teachers or in sports scenarios where defensive and offensive players interact, a common pattern emerges: when an individual within a group exhibits a particular behavior or intention, other members respond accordingly. Thus, this task involves classifying social interaction groups within a crowd.

**3.2 Implementation Details**

ARG constructs a relational graph among individuals using appearance features extracted from each person, combined with spatial distance relations. JS enhances group activity recognition by incorporating social group labels. CAGNet integrates action recognition and interaction inference within a unified network, treating the predicted interactions as results of interaction group discovery. PRN introduces psychological theories to detect social interaction groups in images, achieving superior performance. 

**3.3 Experimental Results**

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Social_Interaction_Detection2.png?raw=true)

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Social_Interaction_Detection3.png?raw=true)

<p align="center">
    <span style="font-size: 16px;">Case 1</span>
</p>

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Social_Interaction_Detection5.png?raw=true)

<p align="center">
    <span style="font-size: 16px;">Case 2</span>
</p>

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Social_Interaction_Detection7.png?raw=true)

<p align="center">
    <span style="font-size: 16px;">Case 3</span>
</p>

**3.4 Analysis**

The table presents the experimental results of benchmark models on CrowdVerse. Due to class imbalance in task labels, where an individual interacts with only a few people while having no interactions with the majority, metrics such as Acc and AUC do not fully capture the model’s performance. Therefore, we focus on the results of P, R, and F1 score metrics. A comparison reveals that the PRN model achieved the highest F1 score (66.10) on SID dataset, out-performing other methods. However, on CrowdVerse, the F1 score of PRN was 52.57, which, although still superior to other methods, represents a decline compared to SID. This discrepancy can be attributed to the higher complexity of scenes in CrowdVerse, characterized by increased crowd density and variations in perspective, which make it challenging for the model to maintain the same level of performance. Moreover, other methods generally performed worse on CrowdVerse compared to their performance on SID, further indicating that CrowdVerse presents a greater challenge. Enhancing the model’s generalization ability on CrowdVerse has thus become a key research direction. 


### 4. Image-text Retrieval <a id="section4"></a>

**4.1 Baselines**

**·BLIP:** BLIP introduces a unified approach for vision language understanding and generation, which utilizes a novel MultiModal Mixture of Encoder-Decoder architecture, incorporating bootstrap captioning and image-text feature alignment. The model synergistically combines three modules: image encoder, text encoder, and image grounded text encoder/decoder.

**·BLIP-2:** BLIP-2 establishes an efficient bridge between vision encoders and large language models through a two-stage framework with a lightweight Q-Former serving as an interface between the vision encoder and language model, significantly reducing computational costs while maintaining performance.

<img src="https://github.com/xzh0312/minima/blob/master/imgs/Image_text_Retrieval1.png?raw=true" alt="pic" style="display: block; margin: 0 auto; max-width: 80%;">

**4.2 Implementation Details**

We select five scenarios that encompass a rich variety of contexts, including outdoor and indoor settings, as well as individual and crowd. For each scenario, we select several hard negatives from the dataset and adopt BLIP-2 for image-text retrieval. We utilized Recall@K as the evaluation metric to assess the retrieval effectiveness.

**4.3 Experimental Results**

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/insert_sheet.png?raw=true)

Retrieval results of scenario 1: **Classroom**.

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Classroom.png?raw=true){:width="30%"}

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Image_text_Retrieval3.png?raw=true)

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Image_text_Retrieval4.png?raw=true)

Retrieval results of scenario 2: **Marathon**.

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Marathon.png?raw=true){:width="30%"}

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Image_text_Retrieval6.png?raw=true)

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Image_text_Retrieval7.png?raw=true)

**4.4 Analysis**

We select hard negatives from two scenarios (classroom and marathon) from the proposed dataset, and image-text retrieval is performed using off-the-shelf BLIP-2. It can be observed that the retrieval performance is suboptimal, indicating that current models struggle with fine-grained retrieval based on long text. Furthermore, BLIP-2's ability to extract detailed information from text needs to be improved. This highlights the urgent need in the multimodal community for an image-text dataset specifically tailored to long-text retrieval.


### 5. Background Segmentation <a id="section5"></a>

<img src="https://github.com/xzh0312/minima/blob/master/imgs/Background_Segmentation1.png?raw=true" alt="pic" style="display: block; margin: 0 auto; max-width: 60%;">

**5.1 Implementation Details**

The content of this experiment involves using a semantic segmentation model to divide top-down view images, with humans removed, into walkable and non-walkable areas, marked in red and blue, respectively, as shown in the Figure. The segmentation results from the SAM model are taken as the ground truth. The F1-score (%) and IOU (%) of five classic semantic segmentation models—FCN, Deeplabv2, Deeplabv3, PSPNet, and U-Net—are compared on CrowdVerse. Taking the image shown in the Figure as an example, the processing results of the five semantic segmentation models are shown in the figures.

**5.2 Experimental Results**

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/FCN1.png?raw=true)

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/FCN2.png?raw=true)

**5.3 Analysis**

The F1-score is the harmonic mean of precision and recall, which considers both accuracy and coverage, with a higher F1-score indicating better performance in predicting the positive class. The Intersection over Union (IOU) measures the overlap between the predicted results and the true labels. The final results show that these five models do not perform well on CrowdVerse. Most of these models can only segment specific objects, such as people or cars, but struggle with segmenting scene classes. In particular, the segmentation of various categories and complex scenes has not been well trained, leading to most of these being classified as "background," resulting in images where the entire picture is either red or blue, indicating all walkable or non-walkable areas.

### 6. Image-text Generation <a id="section6"></a>

**6.1 Implementation Details**

We randomly selected a subset of samples from the dataset and generated images based on the textual descriptions. The generated images were quantitatively compared with the real images, and the quality of the generated images was evaluated by image-text matching scores.

**6.2 Experimental Results**

```
Description:
The image features a young woman posed against a bright yellow background. She has long, slightly wavy brown hair that frames her face, with lighter highlights adding dimension.
Her shirt is a floral pattern, predominantly in soft colors such as blue, pink, and white, with a collar and three-quarter length sleeves.
The shirt has a relaxed fit and a small bow tied at the waist, accentuating her silhouette. She is wearing fitted blue jeans that are paired with the shirt.
Her expression is confident with a slight smile, and she stands with one hand on her hip and the other held slightly out, creating a casual yet poised demeanor.
The vibrant yellow wall serves as a striking backdrop, contrasting nicely with her attire and enhancing the overall cheerful and lively vibe of the image.
```

<img src="https://github.com/xzh0312/minima/blob/master/imgs/Image_text_Generation1.png?raw=true" alt="pic" style="display: block; margin: 0 auto; max-width: 60%;">

```
Description:
The image depicts a lively street scene during twilight, likely in a bustling urban area. The cobblestone pavement is laid out in intricate patterns, adding a decorative element to the environment.
On either side of the street, there are numerous outdoor dining spaces with tables covered in orange tablecloths, where people are enjoying their meals.
A diverse crowd can be seen at the tables and walking along the street; some individuals appear to be engaged in conversation while others enjoy their food.
The atmosphere is vibrant, illuminated by warm, golden street lamps that enhance the evening ambiance.
Buildings line the street, showcasing a mix of architectural styles, and most have large windows with glowing lights, indicating that they are occupied.
One establishment is marked as \"Cafeteria S. Nicolau,\" and there are more food-related windows displaying treats and delicacies.
Additional pedestrians are visible, with some forming a line outside of a shop, suggesting popularity.
The overall scene conveys a festive and welcoming vibe, typical of a lively dining district in a city, rich in culture and social interaction.
```

### ![pic](https://github.com/xzh0312/minima/blob/master/imgs/Image_text_Generation2.png?raw=true)

**Settings and Quantitative Analysis**

The results of the text-to-image generation demonstrate that the generated images are highly similar to the real images in terms of layout, color, objects, actions, and spatial relationships. This indicates that our annotated caption effectively aids the model in reconstructing the images, capturing nearly all global and local visual information.





