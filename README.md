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
4. [Result](#result)  
5. [Calculation of CES](#calculation-of-ces)
6. [Finding CES for every Turn](#finding-ces-for-every-turn)
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
<p align="center"> <img width="448" alt="Screenshot 2023-07-21 at 11 33 46 AM" src="https://github.com/ShoaibShama/Decesion-Tree-Implementation/assets/98227015/f65e17ea-c62a-4a5f-92c8-12e5971aa971"></p>

<br/> <br/> <br/>  


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

## CALCULATION OF CES
_______________________________________
<p align="center"><img width="668" alt="Screenshot 2023-07-21 at 1 09 18 PM" src="https://github.com/ShoaibShama/Decesion-Tree-Implementation/assets/98227015/bcdd0e82-3fa5-4607-a718-0a217cd01658"></p>   
<br/>
<p>Where these were the weightage of the classes</p>

* Low = 0.6   
* Medium= 1.2 
* High=1.8 
* Chat Abandoned=1
  

The score was calculated on a scale of 10
<br/> <br/>
<h2 align="center">CES Scores with this formula</h2>
<p align="center"><img width="725" alt="Screenshot 2023-07-21 at 1 11 09 PM" src="https://github.com/ShoaibShama/Decesion-Tree-Implementation/assets/98227015/d530ecf9-d7bd-40d6-8854-d0bddcadf71e"></p>

## Finding CES for every Turn
_________________________________________
We would tag the user agent conversation based on this prompt.
```
use this framework to classify the messages in the given user angent conversation in the categories 
    listed below. \n\
    3 RULES for framework 1 FOR \"USER\" MESSAGES ARE GIVEN BELOW. If the message is from user, 
    only follow these rules,
    dont go outside these rules.Classify the most sutable message type form below:\n\
    1) Sentiment of the user: Assert the tone and language of the user to analyse their emotional state. choose the 
                                  the most sutaible class from these 3 classes given below:\n\
                        low:- if the user message has low negative sentiment or the user has just shared 
                              some information.
                     medium:- if the user seems uncertain i.e emotion like troubled, not able to understand 
                              or similar emotion.
                       high:- if the user has negative emotion state such as frustrated, angry, sad, 
                              disturbed or use some profane/offensive language.
    2) Repetition of context by user: Check if the user has tried multiple troubleshooting steps suggested 
                                      by the agents for support or their is a repeat of information shared 
                                      by user. choose the most sutaible class from these 3 classes given below:\n\
                                     low:- if the user is just replying or writing discourse makers words.
                                  medium:- if the user has shared information first time or giving new 
                                           information.
                                    high:- if their is repeat of information shared by user ot user has tried 
                                           multiple troubleshooting steps for their issure or is triying multiple 
                                           troubleshooting steps.
    3) Issue Resolvement: Check if the issue of user has been resolved or not. If User is thanking the agent 
                          for help and feel satisfied or similar statement that states issue resolvement. 
                          choose the most sutaible class from these 3 classes given below: \n\
                          low:- if the issue has been resolved.
                       medium:- if the user say he will try doing troubleshooting steps as suggested by agent but issue has not been resolved.
                         high:- if the user issue has not been resolved.
    \n\
    3 Rules for framework 2 FOR \"AGENT\" MESSAGE ARE GIVEN BELOW. if the messae is from Agent, only follow 
    these rules dont go outside this rules. Classify the most sutaible message from type from below:\n\
    1) Chat abandoned: if the user has left the chat with the agent . Messages of agent like "are you there ?"  
                       or "we havent heard from you " or similar message stating this context. 
                       choose the most sutaible class from the 3 classes given below:
                           low:- if the user replies to the chat of agent and agent messages context 
                                 suggest respoiding to queries of user.
                          high:- if the agent is left out when he asked some question and user didnt 
                                 answer or message such as "we havent heard from you" or messages of 
                                 similar context stating chat has been abandoned.
    2) Agent Switching: if the agent has switched the user to the new agent this is agent switching. 
                        Messages like "i am transferring to another agent" or similar messages suggest agent 
                        switching . choose the most sutaible class from the 3 classes given below:
                    low:- if the agent is having conversation by its own.
                   high:- if their is agent switching i.e agent reply with message "i am transferring to 
                          another agent" or similar context stating introduction to another agent.
    3) Redirecting: if the agent redirect to an external link to the user to use a portal or to fill details 
                    somewhere or to open a website or to send email somewhere or similar advice. 
                    choose the most sutaible class from these 2 class given below.
                    low:- if agent listen to the user query and reply or reply message with similar contex.
                   high:- if their is redirecting, i.e agent redirect to an external link or suggest to open 
                   website or suggest similar operation to fill some details.
```
### Rule for calculating CES per turn
After reviewing all the distribution in the data, Issue Resolution is a matrix which reduces the CES score to greater extent. 
Also features like Chat abandoned, Agent Switching increase the CES to the greater extent. 
Thus, CES can be calculated as:
We would start the customer effort score by 50 and then look for features which either decrease score or which increase score.  
```
For Issue Resolvement if the Issue Resolvement is low: 
               if prev_ces<=40: 
                prev_ces= 20-(10*(prev_ces/40)) 
            elif prev_ces<=60: 
                prev_ces= 40-(10*((prev_ces)/60)) 
            elif prev_ces<=80: 
                prev_ces= 60-(10*((prev_ces)/80)) 
            else: 
                prev_ces= 80-(10*((prev_ces)/100)) 
If Issue Resolvement is medium 
             if prev_ces<=40
             prev_ces= 20-(5*(prev_ces/40)) 
            elif prev_ces<=60: 
                prev_ces= 40-(5*((prev_ces)/60)) 
            elif prev_ces<=80: 
                prev_ces= 60-(5*((prev_ces)/80)) 
            else: 
                prev_ces= 80-(5*((prev_ces)/100)) 
```
```
 If Chat Abandoned, Agent switching and Redirecting is high then we include like this  
if prev_ces<=20: 
                inc= 25 
            elif prev_ces<=40: 
                inc= 18 
            elif prev_ces<=60: 
                inc=12 
            else: 
                inc=4 
            prev_ces+=inc 
```
<h2 align="center">Final Output of the CES score in conversations:</h2>
<p align="center"><img width="662" alt="Screenshot 2023-07-21 at 1 21 50 PM" src="https://github.com/ShoaibShama/Decesion-Tree-Implementation/assets/98227015/cebc5ec0-f1ed-45ea-b878-49318befbdf6"></p>





