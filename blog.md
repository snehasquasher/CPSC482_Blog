# GPT-4V(ision) is a Generalist Web Agent, if Grounded

Sneha Sivakumar

May 6 2024 

## Paper Walkthrough

### Summary 

This paper is a continuation from the work in Mind2Web (Deng et al, 2023) which was a benchmark for building realistic web agents. The main contributions of this work involve the development of a generalist web agent SeeAct on real world websites. Unlike a lot of the previous work that has been on offline environments, SeeAct was built to perform on real-world webpages. Various grounding methods were explored:  Visual Grounding, Textual Grounding and Multiple Choice based grounding.This paper has demonstrated that GPT-4V presents a great potential for web agents with a successful task completion rate of about 51.1% on tasks presented with the oracle grounding method (manually ground its textual plans into actions on the websites). The paper also examines novel visul prompting methods like Set of Marks (citation) and showed that it still is pretty limited, and there is a major gap with oracle grounding. 


### Motivation

What are Agents? 

AI agents are ones that can follow language instructions to carry out diverse and complex tasks in real-world or simulated environments. The difference between AI as we have known of it and the contemporary understanding is that contemporary AI agents use language as a vehicle for both thought and communication, this was a quality that was once only unique to humans. 

AI is autonomous agents that can follow language instructions to carry out diverse and complex tasks in real-world or simulated environments.

In this paper, AI Web Agents were explored. The environment is the web environment, and agents are made to follow open-ended language instructions like ("Order me a red scarf on Amazon.com") and complete tasks as seen in the image below. 

![red scarf path](/imgs/image.png)

### Methodology


![Alt text](/imgs/image-3.png)

SeeAct initially utilizes an LMM (Large Multimodal Modal), such as GPT-4V, for Action Generation by enabling the model to visually interpret websites and create textual action plans. They specifically direct GPT-4V to simulate human interactions with a webpage, taking into account the task at hand, the webpage's layout, and action history. It is tasked with producing a description of the action based on its evaluation and reasoning. Following this, Action Grounding involves translating these textual plans into specific actions by identifying relevant HTML elements and the operations to be performed on them.

# The Environment

![Alt text](/imgs/image-6.png)

Each Test can be decomposed into a sequence of actions that are taken by the agent. At each time step t, we provide the agent with:
- **Task**
- **Current State of the Environment**
- **Action History**
- **Action Space**

The agent first generates a set of observations about the environment, and then decides the final Action `{ element, operation, value }` to be performed.

## Grounding Methods 

After action generation, and agent deciding the final action to be taken, the actual element grounding proves to be a major challenge. 

A major aspect of this work was exploring and evaluating the agent on several grounding methods.

![SeeAct Logo](/imgs/image-5.png)  
*Logo Source: Boyuan Zheng, Boyu Gou, Jihyung Kil, Huan Sun, Yu Su, The Ohio State University.*


## Grounding via Image Annotation

This is a purely vision based grounding method where all the interactable elements in the screen were annotated and numbered as bounding boxes. Then, the action was carried out based on the coordinates of the element. 
![Alt text](/imgs/prompt-visual.png)


## Challenges in Grounding via Image Annotation

- **Bounding Box Issues:** Difficulty in making bounding boxes and labeling when the target element is not visible in the viewport.
- **Label Association Errors:** The LMMs struggle with understanding the relative spatial positions and complex, dense layouts of webpage elements. This often leads to incorrect associations of labels with adjacent elements instead of the intended ones.

## Grounding via Element Attributes
Model generate attributes (textual descriptions) and the heuristic search on the HTML dom tree (e.g. describes the elements that are proximal to it, details regarding the specific element, and identifiable information)

![Alt text](/imgs/element.png)

## Challenges in Grounding via Element Attributes

- **Heuristic Limitations:** The grounding process is constrained by heuristic-based methods for locating elements, which rely heavily on textual and locality characteristics.

## Grounding via Textual Choices (Best Performing Grounding Method)
Action generation using textual choices whereby all the interactive elements from the HTML Dom Tree were being extracted and presented in a multiple choice option format for the agent to select. 

Post selection, the corresponding action, value and element pair would be executed using playwright. 

![Alt text](/imgs/text.png)

## Challenges in Grounding via Textual Choices

- **Model Limitations:** Unable to handle elements like pop-ups, iframes not visible in the HTML DOM Tree.
- **Textual Choice Challenges:** When faced with similar textual options, the model often selects the first choice that appears to match its intention. This issue arises because web pages can contain multiple elements with identical HTML information.
- **Slow Parsing:** Since the entire HTML dom tree is being extracted for its interactable elements like (spans, dropdowns, button elements), this parsing is extremely slow and slows the agent down significantly. 


## Results

![results](/imgs/image-2.png)

1. **SeeAct with GPT-4V as a Generalist Web Agent:** SeeAct when paired with GPT-4V, provided with oracle grounding, significantly outperforms traditional methods such as GPT-4 or FLAN-T5, establishing itself as a robust generalist web agent.
2. **Challenges in Grounding:** Grounding remains a substantial challenge. Even the best grounding strategy, SeeAct Choice, exhibits a 20-25% performance gap compared to oracle grounding.
3. **In-Context Learning vs. Supervised Fine-Tuning:** In-context learning with large models, including both LMMs (Large Multimodal Models) and LLMs (Large Language Models), demonstrates improved generalization to unseen websites. However, supervised fine-tuning maintains a competitive advantage on websites that were included during the training phase.

![Alt text](/imgs/image-4.png)

# Interesting Observations

1. **General & World Knowledge**
   - GPT-4V demonstrates substantial advantages in tasks requiring certain knowledge over finetuned models at a smaller scale, such as being able to type out a valid UK postal code without being told.

2. **World Model (for Websites)**
   - GPT-4V can predict the state transitions on a website (e.g., what would happen if I clicked this button). Based on its awareness of website state transitions, GPT-4V can conduct speculative planning involving a sequence of subsequent actions in the future to complete the given task.

3. **Error-Correction Awareness**
   - It realizes that the mobile phone number is invalid due to the wrong format and generates the description of the action to correct this error.


## Analysis & Areas of Improvement 

This paper has done a great job in curating one of the first generalist web agents to perform on real life online websites. However, there are many areas for improvement. 

First, I believe the path forward is in using visual grounding. While the text based HTML choices are the best performing element grounding method in the paper, it has several limitations: extremely high numbers of tokens, slow parsing as well as failure on real world websits that have more complex front-end designs. An improvement in visual grounding could come from training foundational models on website data to improve its ability to map interactive elements on a page to coordinates and bounding boxes accurately. This would drastically improve the speed of execution of the agent as well as reduce the number of tokens needed to pass into the LLM per call. 

Second, I think fine-tuning the agent on website trajectories could be another direction to take. Currently, the VLM does not have a very good understanding of webpages since we are using GPT4V which is mostly trained on text data. 

Finally, exploring agent memory could be an interesting direction. For example, going beyond just providing the previous action history, but augmenting the agent with information specific to a webpage (screenshots, documentation, past interactions) etc and getting it to perform with retrieval augmented generation. 
