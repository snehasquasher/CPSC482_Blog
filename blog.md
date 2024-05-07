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
…

### Methodology

![Alt text](/imgs/image-3.png)

SeeAct initially utilizes an LMM (Large Multimodal Modal), such as GPT-4V, for Action Generation by enabling the model to visually interpret websites and create textual action plans. They specifically direct GPT-4V to simulate human interactions with a webpage, taking into account the task at hand, the webpage's layout, and action history. It is tasked with producing a description of the action based on its evaluation and reasoning. Following this, Action Grounding involves translating these textual plans into specific actions by identifying relevant HTML elements and the operations to be performed on them.
…

### Results

![results](/imgs/image-2.png)

1. **SeeAct with GPT-4V as a Generalist Web Agent:** SeeAct when paired with GPT-4V, provided with oracle grounding, significantly outperforms traditional methods such as GPT-4 or FLAN-T5, establishing itself as a robust generalist web agent.
2. **Challenges in Grounding:** Grounding remains a substantial challenge. Even the best grounding strategy, SeeAct Choice, exhibits a 20-25% performance gap compared to oracle grounding.
3. **In-Context Learning vs. Supervised Fine-Tuning:** In-context learning with large models, including both LMMs (Large Multimodal Models) and LLMs (Large Language Models), demonstrates improved generalization to unseen websites. However, supervised fine-tuning maintains a competitive advantage on websites that were included during the training phase.

![Alt text](/imgs/image-4.png)

## Analysis 

## Challenges in Grounding via Textual Choices

- **Model Limitations:** Unable to handle elements like pop-ups, iframes not visible in the HTML DOM Tree.
- **Textual Choice Challenges:** When faced with similar textual options, the model often selects the first choice that appears to match its intention. This issue arises because web pages can contain multiple elements with identical HTML information.

## Grounding via Image Annotation

- **Bounding Box Issues:** Difficulty in making bounding boxes and labeling when the target element is not visible in the viewport.
- **Label Association Errors:** The LMMs struggle with understanding the relative spatial positions and complex, dense layouts of webpage elements. This often leads to incorrect associations of labels with adjacent elements instead of the intended ones.

## Grounding via Element Attributes

- **Heuristic Limitations:** The grounding process is constrained by heuristic-based methods for locating elements, which rely heavily on textual and locality characteristics.

…

# Interesting Observations

1. **General & World Knowledge**
   - GPT-4V demonstrates substantial advantages in tasks requiring certain knowledge over finetuned models at a smaller scale, such as being able to type out a valid UK postal code without being told.

2. **World Model (for Websites)**
   - GPT-4V can predict the state transitions on a website (e.g., what would happen if I clicked this button). Based on its awareness of website state transitions, GPT-4V can conduct speculative planning involving a sequence of subsequent actions in the future to complete the given task.

3. **Error-Correction Awareness**
   - It realizes that the mobile phone number is invalid due to the wrong format and generates the description of the action to correct this error.


## Analysis & Areas of Improvement 

Analysis of the selected paper, organized as bullet points, in particular:
What interests you the most about this work?
What are the places for improvements, for example, what are the ways you can think of to further improve the performance of the proposed method?

* …

* …