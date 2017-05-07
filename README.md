
Steps:    
1. unzip backup_dqn.zip    
2. for debug, please open python command line     
3. copy code line by line from test_dqn.py and execute    
4. the last line      
 " BrainDQN_Nature.trainQNetwork(brain,article_batch,article_lens, decoder_output,abstracts4this,model,initW,sess)"    
   means to train dqn    



others to be mentioned:
1. because it loads pre-trained models for dqn, so it needs to get these data somewhere. For this , I have stored them in the tmp/textsum/backup/textsum.
   users only follows the above command and do not need to care about these things.
2. dqn model is more like a plugin based on original lstm model. 

   createQNetwork #method to create dqn graph
   createTrainingMethod # method for the gradient update
   getActions   #get the best action among all word list
   getQvalues  #get the qvalues(log probability of all tokens, vocab_sized ) at timestep t
   trainQNetwork  #method to train dqn net

3. the process is that first train the original lstm, then load a model and decoder all sentences, finally train dqn 

4. if modifying BrainDQN_Nature code and want it to apply immediate change in command session, please modify those method which has no  indent in decode_rl/BrainDQN_Nature.py and this is because they are module methods not class methods and can be runtime-deployed and changes can be loaded. Now I make two bunches of methods, one defined below module and the other one defined in class level. When it finished, I will override class method with module method.    



problems:
1. how to define top ranked  word list for each timestep? now if each timestep's topk words is different, class will be different so there is no sense traing classifier for different class set. Now I just let the model select whole vocab list since the class is the same for each timestep.
2. how to set decoder initial state to dqn(lstm)? right now I just let one sentence go to lstm and get the hidden state of the last node as the initial state for the decoder initial state setting in the next iteration.
3. The model needs to learn by observing memory and getting next state so how to calculate reward for the next state(or observation)? now I just let the next state(the modified sentence) go through the model and get highest log probability of the tokens(best token) as the reward. But I think this should be corrected or normalized since the BLEU score's range is different with reward(QVlaue_pred_reward variable in the code). 
4. is BLEU score the right and reseanable way for the reward(or loss) for the current states?
# thesis_ATTN_BN
# thesis_ATTN_BN
