# CES : Customer Effort Score
________________________________
Customer Effort Score is the amount of effort put by the customer to resolve his issue. It gives a score for effort put up by the customer.  
94% customer with low effort score intent to use the company product again.  

## Setup
________________________________
to set up and run code
```
pip install -r requirement.txt
```
in any python envioronment with version >=3.7.  

## Table of Content  
1. [Introduction](#ces--customer-effort-score)  
2. [Prompting for tagging Dataset](#prompting-for-tagging-dataset)
3. [Reward Model Training](#reward-model-trainer)    
4.[Result]  
5.
6.
## Prompting for tagging Dataset  
____________________________________
Based on the prompt used and the agent conversation would be tagged in these metrics by GPT 3.5-turbo  
```
    You will are given two conversation between the user and the agent classify the conversation on the basic of following features . 
    These features are responsible for calculating Customer Effort score . 
    Based on the features mentioned below which conversation would have lower CES score , 
   
    Number of interaction: if the interaction between agent and the user is higher this states that user need to put more effort to resolve its issue thus        score would increase. 
    If the number of interaction is 9- 14 classify it as medium and if higher than this classify it as hight and below this classify as low.
    
    Emotional cues - Evaluate the conversation step by step and find how the customer feels basically sentiment analysis. 
    Emotion like frustrated, sad, angry, unsatisfied, confused, dissatisfied, annoyed  etc. 
    On the basis on analysis classify as high if the emotion is highly negative for customer , if customer is bit negative classify as average and if         
    customer show no negative emotion classify this as low 
   
    Message Content - if the message send by the user is larger in length than send by an average user  ,  this state the user is putting more effort and         thus the score would increase and vice - versa. 
    To calculate message content count the number of words send by the user if the number of words in each conversation of use is between 10-20 classify it       as medium and if the number increase classify as hight and decrease classify as low.
    
    Issue resolution - Check whether the issue faced by the customer has been solved or not.If the issue has been resolved give classify as low and if not 
    resolved classify as high . 
    If there is a situation where the issue may have been resolved when agent suggest something classify as high
    
    Agent Error - Agents may lack sufficient knowledge about products, services, policies, or procedures, resulting in incorrect or incomplete responses to   
    customer inquiries. 
    If agent lack knowledge this makes user to invest more time thus score increases. if agent is able to deliver and show proper knowledge classify as low , 
    if it commits a bit error such as I would transfer your concerns to your team mark it as high and anywhere in between of such is marked medium .

```
a) Number of Intraction  
b) Emotional Cues  
c) Message Content  
d) Issue Resolution  
e) Agent Error  

## Reward Model Trainer  
_________________________________
The dataset would contain ranks between the two conversation and al label would mention which coversation would have lower CES score based on the metrics defined.These would be then passed through the model TRL(Transformer Reinforcement Learning) where a scaler score would be generated. The loss function used 
will be this:  

<img align ="left" width="200" alt="Screenshot 2023-07-21 at 11 33 29 AM" src="https://github.com/ShoaibShama/Decesion-Tree-Implementation/assets/98227015/7f88e0b7-b013-4ce8-b3d4-cd902c7cd131">

Their would be two forward passes one for the conversation which is ranked higher (i.e prompt accepted) and one for conversation which is lower ranked (i.e Prompt rejected). Then loss would be calculated by using this function  
<br/><br/>
<p align="centre"> <img width="448" alt="Screenshot 2023-07-21 at 11 33 46 AM" src="https://github.com/ShoaibShama/Decesion-Tree-Implementation/assets/98227015/f65e17ea-c62a-4a5f-92c8-12e5971aa971"></p>
<br/><br/>  


## Result  
___________________________
<img width="1093" alt="Screenshot 2023-07-14 at 1 09 51 PM" src="https://github.com/ShoaibShama/Decesion-Tree-Implementation/assets/98227015/2875d48d-1837-4917-8b66-bd277d79dbe0">  
<img width="1106" alt="Screenshot 2023-07-14 at 1 10 56 PM" src="https://github.com/ShoaibShama/Decesion-Tree-Implementation/assets/98227015/784346e6-b686-4ce5-93c0-27b69f7080ea">
<img width="1106" alt="Screenshot 2023-07-14 at 1 11 44 PM" src="https://github.com/ShoaibShama/Decesion-Tree-Implementation/assets/98227015/01cda896-cbe1-46d9-8c91-67756e3df9c6">

| Model  | Accuracy | Training Loss | Evaluation Loss | 
| ------------- | ------------- | ----------- | ----------- |
| bigBIRD  | 0.6336  | 0.6931 | 0.6931 |
| Longformer  | 0.536  | 0.6921 | 0.6931 |
| Roberta-base | 0.5619 | 0.6928 | 0.6931 |


