### Manually inspected false negatives (Comments that were actually toxic but got classified as non-toxic)

Below is a list of manually inspected false negatives including model weights, values of the vectorized comment (TFIDF encoded Uni- and Bigrams) and  the category to which I assigned them (representing why they were missclassified). While many other features have been tried, for this list the only used features are TFIDF encoded word Uni- and Bigrams, as other features gave no significant improvement and only using Uni- and Bigrams increases readability for inspecting the comments a lot. All of the listed comments are of one of these categories: 'Lack of tox. Words', 'disagree', 'N-Tox. Ass. Neutral words', 'No tox. Words'. A quick explaination for these categories:
* No tox. Words: There were no explicit toxic words in the comment
* Lack of tox. Words: There were some words that could be considered toxic, but not enough to have the model classify the comment as toxic.
* N-Tox. Ass. Neutral words: A relatively high non-toxicity being associated with certain neutral words (compared to most other neutral words). 

Also, the comments have been lemmatized.

An example from the list: 
```
b =  [-1.05166094]
Screw you and your destruction of wildlife all for taint beef . 
True label: 1

-                                   ---- Result: 0.85 ----
                          sum toxic : 1.36  |  -0.51 : sum non-toxic                      

           screw you - (w=1.90/v=0.30) 0.57 | -0.11 (w=-0.49/v=0.23) - all for             
               screw - (w=2.62/v=0.21) 0.55 | -0.10 (w=-0.39/v=0.26) - beef                
                your - (w=1.35/v=0.09) 0.13 | -0.10 (w=-0.42/v=0.23) - wildlife            
                 you - (w=0.65/v=0.07) 0.04 | -0.06 (w=-0.24/v=0.26) - taint               
         destruction - (w=0.11/v=0.23) 0.03 | -0.04 (w=-0.15/v=0.30) - of wildlife         
                 all - (w=0.19/v=0.09) 0.02 | -0.03 (w=-0.17/v=0.20) - and your            
                 and - (w=0.29/v=0.05) 0.01 | -0.03 (w=-0.13/v=0.25) - destruction of      
             you and - (w=0.07/v=0.18) 0.01 | -0.01 (w=-0.21/v=0.07) - for                 
        wildlife all - (w=0.01/v=0.40) 0.00 | -0.01 (w=-0.03/v=0.39) - your destruction    
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.05) - of                  


Category : N-Tox. Ass. Neutral words
Category2: N-Tox. Ass. Stopword in Bigrams
Words    : ['all for, beef, wildlife']              
```
Here, the comment is "Screw you and your destruction of wildlife all for taint beef ." w denotes the weights of the model and v denotes vector of TFIDF values of the comment. b denotes the models bias term. For all features where the corresponding TFIDF values is not 0, the featurname, weight, TFIDF value and result of weight * TFIDF-value are printed. On the left, all features with a positive weight are listed, on the right all features with a negative weight. Informally, the left side can be regarded as "features which push the model towards classifying the comment as toxic" and the same for the right side but with "non-toxic". In this example, the result of all negative and positive features is 0.85 which is below -b, so this comment is classified as non-toxic. 
Below that are the missclassification-categories assigned to this example and also sometimes what specific word(s) are representative of the problem.
For this example, the classifier correctly managed to identify "screw you" and "screw" as toxic words, so the sum of toxic words ends up being 1.36. However, due to associating non-toxicity with neutral words ("beef", "wildlife", "all for") and associating non-toxicity with bigrams containing stopwords ("all for"), the sum of non-toxicity ends up at -0.51, so all in all the comments get classified as non-toxic despite actually being toxic.

``` 
b =  [-1.05166094]
Wow . Sara Fredericks be a hate fill monster . 
True label: 1

-                                   ---- Result: 0.99 ----
                          sum toxic : 1.20  |  -0.21 : sum non-toxic                      

             monster - (w=1.66/v=0.25) 0.42 | -0.07 (w=-0.21/v=0.35) - fredericks          
             be hate - (w=1.03/v=0.28) 0.29 | -0.05 (w=-0.16/v=0.29) - sara                
           hate fill - (w=1.01/v=0.29) 0.29 | -0.04 (w=-0.11/v=0.35) - sara fredericks     
                hate - (w=0.82/v=0.17) 0.14 | -0.03 (w=-0.18/v=0.20) - fill                
                  be - (w=0.76/v=0.04) 0.03 | -0.02 (w=-0.05/v=0.41) - fredericks be       
                 wow - (w=0.11/v=0.19) 0.02 | 0.00 (w=0.00/v=0.00) -                     
        fill monster - (w=0.00/v=0.41) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : N-Tox. Ass. To name
Category2: 
Words    : ['Sara Fredericks']

------------------------------------------------------------------

no much Chinese please . 
True label: 1

-                                   ---- Result: 0.86 ----
                          sum toxic : 0.87  |  -0.01 : sum non-toxic                      

             chinese - (w=0.94/v=0.42) 0.40 | -0.01 (w=-0.03/v=0.17) - much                
        much chinese - (w=0.36/v=0.69) 0.25 | 0.00 (w=0.00/v=0.00) -                     
             no much - (w=0.49/v=0.42) 0.21 | 0.00 (w=0.00/v=0.00) -                     
                  no - (w=0.06/v=0.19) 0.01 | 0.00 (w=0.00/v=0.00) -                     
              please - (w=0.01/v=0.31) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : No tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

Another brilliant lefty who have choose to ruin his life for a petty political prank . What be this knuckleheads learn in school . It certainly be not common sense . 
True label: 1

-                                   ---- Result: 0.47 ----
                          sum toxic : 1.10  |  -0.64 : sum non-toxic                      

        knuckleheads - (w=1.12/v=0.21) 0.24 | -0.07 (w=-0.46/v=0.15) - brilliant           
   this knuckleheads - (w=0.51/v=0.25) 0.13 | -0.06 (w=-0.61/v=0.10) - what be             
               lefty - (w=0.83/v=0.15) 0.12 | -0.05 (w=-0.24/v=0.23) - for petty           
        it certainly - (w=0.68/v=0.16) 0.11 | -0.05 (w=-0.75/v=0.06) - be not              
        common sense - (w=0.57/v=0.14) 0.08 | -0.04 (w=-0.40/v=0.11) - sense               
                 his - (w=0.88/v=0.07) 0.06 | -0.04 (w=-0.33/v=0.11) - choose              
                ruin - (w=0.39/v=0.14) 0.06 | -0.03 (w=-0.22/v=0.15) - certainly be        
               petty - (w=0.31/v=0.16) 0.05 | -0.03 (w=-0.30/v=0.10) - who have            
           choose to - (w=0.33/v=0.13) 0.04 | -0.03 (w=-0.20/v=0.15) - in school           
          not common - (w=0.20/v=0.21) 0.04 | -0.03 (w=-0.13/v=0.21) - lefty who           
                  be - (w=0.76/v=0.05) 0.04 | -0.03 (w=-0.71/v=0.04) - not                 
             another - (w=0.38/v=0.09) 0.03 | -0.02 (w=-0.15/v=0.16) - have choose         
                this - (w=0.69/v=0.05) 0.03 | -0.02 (w=-0.21/v=0.11) - common              
                what - (w=0.26/v=0.06) 0.01 | -0.02 (w=-0.24/v=0.09) - life                
               prank - (w=0.07/v=0.21) 0.01 | -0.02 (w=-0.56/v=0.04) - in                  
            life for - (w=0.07/v=0.17) 0.01 | -0.02 (w=-0.19/v=0.11) - certainly           
           school it - (w=0.06/v=0.19) 0.01 | -0.01 (w=-0.14/v=0.10) - school              
               learn - (w=0.10/v=0.11) 0.01 | -0.01 (w=-0.06/v=0.22) - ruin his            
             be this - (w=0.08/v=0.12) 0.01 | -0.01 (w=-0.21/v=0.04) - for                 
     petty political - (w=0.00/v=0.26) 0.00 | -0.01 (w=-0.04/v=0.18) - to ruin             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.14/v=0.04) - it                  
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.15/v=0.03) - have                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.04/v=0.10) - political           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.10/v=0.03) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.19) - learn in            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.06) - who                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.23) - another brilliant   
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.15) - his life            


Category : Lack of tox. Words
Category2: 
Words    : ['knuckleheads']

------------------------------------------------------------------

Ranger be you call Alaskan Natives domestic terrorist . Because last time I check I have not every hear of any Alaska Native strap on a suicide vest . I do not see them stockpile bomb ingredient and build bomb to blow up government agency and park garage . I have not see any Waco or Ruby Ridge standoff . I do not see any Aryan Nation race type group . I have not hear of any religious cult issue and mass poison . So Ranger tell me what be make you think Alaskan Natives be domestic terrorist . Or be this just racist bullshit -PRON- be peddle . 
True label: 1

-                                   ---- Result: 1.03 ----
                          sum toxic : 1.91  |  -0.88 : sum non-toxic                      

            bullshit - (w=7.01/v=0.08) 0.53 | -0.06 (w=-0.71/v=0.08) - not                 
           terrorist - (w=1.66/v=0.11) 0.19 | -0.06 (w=-0.51/v=0.11) - of any              
              racist - (w=1.94/v=0.05) 0.10 | -0.06 (w=-0.49/v=0.12) - have not            
                bomb - (w=0.63/v=0.12) 0.08 | -0.05 (w=-0.34/v=0.16) - not see             
              ranger - (w=0.38/v=0.17) 0.06 | -0.04 (w=-0.49/v=0.08) - strap               
                 any - (w=0.38/v=0.13) 0.05 | -0.03 (w=-0.52/v=0.05) - or be               
             bomb to - (w=0.49/v=0.10) 0.05 | -0.03 (w=-0.21/v=0.13) - domestic            
                  be - (w=0.76/v=0.06) 0.05 | -0.03 (w=-0.30/v=0.09) - stockpile           
   terrorist because - (w=0.43/v=0.10) 0.04 | -0.03 (w=-0.61/v=0.04) - what be             
         just racist - (w=0.44/v=0.10) 0.04 | -0.02 (w=-0.25/v=0.10) - strap on            
             natives - (w=0.28/v=0.15) 0.04 | -0.02 (w=-0.24/v=0.10) - build bomb          
              poison - (w=0.57/v=0.07) 0.04 | -0.02 (w=-0.57/v=0.04) - do                  
           religious - (w=0.66/v=0.06) 0.04 | -0.02 (w=-0.28/v=0.08) - not every           
                blow - (w=0.63/v=0.06) 0.04 | -0.02 (w=-0.20/v=0.10) - see                 
          natives be - (w=0.39/v=0.09) 0.04 | -0.02 (w=-0.23/v=0.08) - vest                
       bullshit pron - (w=0.32/v=0.11) 0.03 | -0.02 (w=-0.58/v=0.03) - think               
             blow up - (w=0.44/v=0.07) 0.03 | -0.02 (w=-0.29/v=0.06) - tell me             
               aryan - (w=0.30/v=0.10) 0.03 | -0.02 (w=-0.20/v=0.08) - ridge               
              do not - (w=0.55/v=0.05) 0.03 | -0.02 (w=-0.31/v=0.05) - be make             
            standoff - (w=0.29/v=0.09) 0.03 | -0.02 (w=-0.19/v=0.08) - time check          
                 you - (w=0.65/v=0.04) 0.03 | -0.01 (w=-0.37/v=0.04) - issue               
           and build - (w=0.35/v=0.07) 0.02 | -0.01 (w=-0.14/v=0.10) - waco                
             suicide - (w=0.35/v=0.07) 0.02 | -0.01 (w=-0.17/v=0.08) - government agency   
            and mass - (w=0.25/v=0.09) 0.02 | -0.01 (w=-0.18/v=0.07) - this just           
                pron - (w=0.94/v=0.02) 0.02 | -0.01 (w=-0.16/v=0.08) - garage              
            make you - (w=0.34/v=0.06) 0.02 | -0.01 (w=-0.15/v=0.08) - ingredient          
                them - (w=0.59/v=0.03) 0.02 | -0.01 (w=-0.13/v=0.09) - be peddle           
  domestic terrorist - (w=0.09/v=0.19) 0.02 | -0.01 (w=-0.26/v=0.04) - build               
                this - (w=0.69/v=0.02) 0.01 | -0.01 (w=-0.08/v=0.13) - hear of             
         be domestic - (w=0.14/v=0.10) 0.01 | -0.01 (w=-0.15/v=0.07) - see them            
          agency and - (w=0.17/v=0.08) 0.01 | -0.01 (w=-0.18/v=0.06) - agency              
                 and - (w=0.29/v=0.04) 0.01 | -0.01 (w=-0.08/v=0.12) - race type           
             see any - (w=0.08/v=0.14) 0.01 | -0.01 (w=-0.18/v=0.05) - check               
           you think - (w=0.23/v=0.05) 0.01 | -0.01 (w=-0.22/v=0.04) - last                
          group have - (w=0.14/v=0.08) 0.01 | -0.01 (w=-0.07/v=0.11) - racist bullshit     
       alaska native - (w=0.13/v=0.08) 0.01 | -0.01 (w=-0.15/v=0.05) - park                
       any religious - (w=0.10/v=0.10) 0.01 | -0.01 (w=-0.10/v=0.08) - and park            
               every - (w=0.24/v=0.04) 0.01 | -0.01 (w=-0.07/v=0.10) - religious cult      
                type - (w=0.18/v=0.05) 0.01 | -0.01 (w=-0.15/v=0.05) - have                
            you call - (w=0.13/v=0.07) 0.01 | -0.01 (w=-0.23/v=0.03) - time                
                race - (w=0.16/v=0.05) 0.01 | -0.01 (w=-0.12/v=0.06) - mass                
           last time - (w=0.13/v=0.07) 0.01 | -0.01 (w=-0.12/v=0.06) - native              
                just - (w=0.24/v=0.03) 0.01 | -0.01 (w=-0.13/v=0.05) - or                  
                what - (w=0.26/v=0.03) 0.01 | -0.01 (w=-0.06/v=0.10) - terrorist or        
             to blow - (w=0.07/v=0.08) 0.01 | -0.01 (w=-0.13/v=0.04) - tell                
               group - (w=0.11/v=0.05) 0.01 | -0.00 (w=-0.08/v=0.06) - issue and           
             because - (w=0.14/v=0.03) 0.00 | -0.00 (w=-0.22/v=0.02) - on                  
             be this - (w=0.08/v=0.05) 0.00 | -0.00 (w=-0.14/v=0.04) - government          
                  up - (w=0.14/v=0.03) 0.00 | -0.00 (w=-0.03/v=0.12) - alaskan             
           ranger be - (w=0.04/v=0.10) 0.00 | -0.00 (w=-0.12/v=0.03) - me                  
             me what - (w=0.05/v=0.07) 0.00 | -0.00 (w=-0.04/v=0.10) - up government       
        because last - (w=0.03/v=0.11) 0.00 | -0.00 (w=-0.04/v=0.09) - hear                
                call - (w=0.08/v=0.04) 0.00 | -0.00 (w=-0.13/v=0.03) - make                
         park garage - (w=0.02/v=0.09) 0.00 | -0.00 (w=-0.03/v=0.11) - aryan nation        
             pron be - (w=0.02/v=0.03) 0.00 | -0.00 (w=-0.04/v=0.09) - ruby                
     alaskan natives - (w=0.00/v=0.20) 0.00 | -0.00 (w=-0.03/v=0.11) - on suicide          
              alaska - (w=0.01/v=0.04) 0.00 | -0.00 (w=-0.03/v=0.07) - not hear            
        call alaskan - (w=0.00/v=0.12) 0.00 | -0.00 (w=-0.06/v=0.03) - so                  
          every hear - (w=0.00/v=0.12) 0.00 | -0.00 (w=-0.03/v=0.05) - nation              
          type group - (w=0.00/v=0.12) 0.00 | -0.00 (w=-0.10/v=0.01) - to                  
             vest do - (w=0.00/v=0.12) 0.00 | -0.00 (w=-0.01/v=0.07) - peddle              
         garage have - (w=0.00/v=0.11) 0.00 | -0.00 (w=-0.01/v=0.10) - ingredient and      
             or ruby - (w=0.00/v=0.11) 0.00 | -0.00 (w=-0.00/v=0.11) - poison so           
             waco or - (w=0.00/v=0.11) 0.00 | -0.00 (w=-0.00/v=0.07) - cult                
          any alaska - (w=0.00/v=0.11) 0.00 | -0.00 (w=-0.00/v=0.11) - ruby ridge          
        suicide vest - (w=0.00/v=0.11) 0.00 | -0.00 (w=-0.00/v=0.11) - think alaskan       
          check have - (w=0.00/v=0.10) 0.00 | -0.00 (w=-0.00/v=0.05) - be you              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.03) - of                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.11) - mass poison         


Category : N-Tox. Ass. To negations
Category2: N-Tox. Ass. Stopword in Bigrams
Words    : ['not, of any, have not, not see']

------------------------------------------------------------------

When Trump go on The View in he say that he have hire investigator to look into Obama have birthplace . He say that You will not believe what -PRON- be find and that he would be release their report soon . We do not know then what we know now that when Trump promise to release proof for any of his lie -PRON- have lie in fact -PRON- have Trump speak for -PRON- be lie through my tooth . There be no report no investigation no investigator there be always and only Trump have filthy racist lie . Here he be on The View https watch?v emkDpm_vQDg . 
True label: 1

-                                   ---- Result: 0.68 ----
                          sum toxic : 1.74  |  -1.06 : sum non-toxic                      

                 lie - (w=1.20/v=0.20) 0.24 | -0.05 (w=-1.05/v=0.05) - https               
              filthy - (w=2.60/v=0.08) 0.22 | -0.04 (w=-0.52/v=0.08) - be release          
               trump - (w=0.90/v=0.14) 0.12 | -0.04 (w=-0.37/v=0.11) - view                
              racist - (w=1.94/v=0.06) 0.11 | -0.04 (w=-0.34/v=0.11) - have filthy         
                pron - (w=0.94/v=0.10) 0.09 | -0.04 (w=-0.36/v=0.10) - only trump          
                  he - (w=0.49/v=0.16) 0.08 | -0.04 (w=-0.36/v=0.10) - report              
                  be - (w=0.76/v=0.08) 0.06 | -0.03 (w=-0.29/v=0.11) - lie here            
             his lie - (w=0.63/v=0.09) 0.06 | -0.03 (w=-0.25/v=0.12) - he say              
     have birthplace - (w=0.44/v=0.12) 0.05 | -0.03 (w=-0.38/v=0.08) - tooth               
          birthplace - (w=0.40/v=0.11) 0.04 | -0.03 (w=-0.46/v=0.06) - investigation       
          racist lie - (w=0.35/v=0.11) 0.04 | -0.03 (w=-0.27/v=0.10) - trump speak         
           proof for - (w=0.37/v=0.10) 0.04 | -0.03 (w=-0.71/v=0.04) - not                 
          to release - (w=0.37/v=0.08) 0.03 | -0.02 (w=-0.44/v=0.05) - be on               
                 his - (w=0.88/v=0.03) 0.03 | -0.02 (w=-0.34/v=0.07) - when                
              be lie - (w=0.40/v=0.07) 0.03 | -0.02 (w=-0.50/v=0.04) - find                
         lie through - (w=0.26/v=0.10) 0.03 | -0.02 (w=-0.38/v=0.06) - go on               
           then what - (w=0.25/v=0.08) 0.02 | -0.02 (w=-0.13/v=0.16) - when trump          
       filthy racist - (w=0.15/v=0.12) 0.02 | -0.02 (w=-0.56/v=0.04) - in                  
           be always - (w=0.28/v=0.06) 0.02 | -0.02 (w=-0.33/v=0.06) - proof               
               we do - (w=0.31/v=0.05) 0.02 | -0.02 (w=-0.39/v=0.05) - speak               
             to look - (w=0.26/v=0.06) 0.02 | -0.02 (w=-0.21/v=0.09) - soon we             
          through my - (w=0.17/v=0.09) 0.02 | -0.02 (w=-0.40/v=0.05) - through             
          trump have - (w=0.31/v=0.05) 0.02 | -0.02 (w=-0.18/v=0.11) - say that            
            not know - (w=0.30/v=0.05) 0.02 | -0.02 (w=-0.22/v=0.08) - have lie            
                look - (w=0.38/v=0.04) 0.02 | -0.02 (w=-0.14/v=0.12) - release             
       release proof - (w=0.12/v=0.13) 0.02 | -0.02 (w=-0.17/v=0.10) - always and          
              do not - (w=0.55/v=0.03) 0.01 | -0.02 (w=-0.27/v=0.06) - there               
                what - (w=0.26/v=0.06) 0.01 | -0.02 (w=-0.20/v=0.08) - look into           
         not believe - (w=0.23/v=0.06) 0.01 | -0.01 (w=-0.14/v=0.10) - lie pron            
           fact pron - (w=0.17/v=0.08) 0.01 | -0.01 (w=-0.15/v=0.08) - have                
                 you - (w=0.65/v=0.02) 0.01 | -0.01 (w=-0.29/v=0.04) - believe             
        believe what - (w=0.17/v=0.08) 0.01 | -0.01 (w=-0.57/v=0.02) - do                  
                only - (w=0.39/v=0.04) 0.01 | -0.01 (w=-0.07/v=0.16) - the view            
               obama - (w=0.29/v=0.05) 0.01 | -0.01 (w=-0.07/v=0.16) - investigator        
               he be - (w=0.32/v=0.04) 0.01 | -0.01 (w=-0.12/v=0.09) - view in             
                 any - (w=0.38/v=0.04) 0.01 | -0.01 (w=-0.18/v=0.06) - he would            
               their - (w=0.44/v=0.03) 0.01 | -0.01 (w=-0.12/v=0.09) - trump promise       
             he have - (w=0.24/v=0.05) 0.01 | -0.01 (w=-0.10/v=0.11) - that he             
         https watch - (w=0.16/v=0.07) 0.01 | -0.01 (w=-0.22/v=0.05) - on                  
           report no - (w=0.10/v=0.11) 0.01 | -0.01 (w=-0.15/v=0.07) - for any             
                 and - (w=0.29/v=0.03) 0.01 | -0.01 (w=-0.19/v=0.05) - that you            
               watch - (w=0.18/v=0.05) 0.01 | -0.01 (w=-0.14/v=0.07) - on the              
           that when - (w=0.12/v=0.07) 0.01 | -0.01 (w=-0.08/v=0.12) - into obama          
             in fact - (w=0.15/v=0.06) 0.01 | -0.01 (w=-0.16/v=0.06) - you will            
                soon - (w=0.16/v=0.06) 0.01 | -0.01 (w=-0.14/v=0.06) - now that            
           know then - (w=0.06/v=0.10) 0.01 | -0.01 (w=-0.21/v=0.04) - for                 
                then - (w=0.17/v=0.04) 0.01 | -0.01 (w=-0.09/v=0.10) - their report        
                  go - (w=0.20/v=0.03) 0.01 | -0.01 (w=-0.10/v=0.09) - in he               
           what pron - (w=0.09/v=0.06) 0.01 | -0.01 (w=-0.14/v=0.06) - what we             
          have trump - (w=0.07/v=0.08) 0.01 | -0.01 (w=-0.08/v=0.11) - my tooth            
                  no - (w=0.06/v=0.09) 0.01 | -0.01 (w=-0.14/v=0.06) - promise             
                know - (w=0.08/v=0.07) 0.01 | -0.01 (w=-0.10/v=0.08) - lie in              
    investigation no - (w=0.05/v=0.11) 0.01 | -0.01 (w=-0.11/v=0.07) - there be            
          obama have - (w=0.07/v=0.06) 0.00 | -0.01 (w=-0.10/v=0.07) - promise to          
                  my - (w=0.10/v=0.04) 0.00 | -0.01 (w=-0.07/v=0.10) - release their       
              of his - (w=0.07/v=0.05) 0.00 | -0.01 (w=-0.15/v=0.04) - fact                
            know now - (w=0.03/v=0.10) 0.00 | -0.01 (w=-0.14/v=0.04) - would be            
            and that - (w=0.07/v=0.05) 0.00 | -0.01 (w=-0.19/v=0.03) - would               
                into - (w=0.08/v=0.04) 0.00 | -0.01 (w=-0.08/v=0.07) - and only            
                 now - (w=0.08/v=0.04) 0.00 | -0.01 (w=-0.08/v=0.06) - pron have           
                  we - (w=0.04/v=0.06) 0.00 | -0.00 (w=-0.06/v=0.07) - that                
           speak for - (w=0.03/v=0.07) 0.00 | -0.00 (w=-0.06/v=0.07) - say                 
              any of - (w=0.03/v=0.06) 0.00 | -0.00 (w=-0.10/v=0.03) - to                  
             pron be - (w=0.02/v=0.07) 0.00 | -0.00 (w=-0.11/v=0.03) - the                 
             here he - (w=0.01/v=0.09) 0.00 | -0.00 (w=-0.03/v=0.09) - for pron            
               be no - (w=0.01/v=0.05) 0.00 | -0.00 (w=-0.03/v=0.09) - trump go            
             we know - (w=0.01/v=0.06) 0.00 | -0.00 (w=-0.04/v=0.06) - hire                
            find and - (w=0.00/v=0.09) 0.00 | -0.00 (w=-0.04/v=0.05) - will not            
              always - (w=0.00/v=0.05) 0.00 | -0.00 (w=-0.02/v=0.11) - investigator to     
         report soon - (w=0.00/v=0.13) 0.00 | -0.00 (w=-0.04/v=0.04) - here                
         tooth there - (w=0.00/v=0.13) 0.00 | -0.00 (w=-0.02/v=0.11) - no report           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.10) - no investigation    
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.09) - have hire           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.03) - will                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.07) - be find             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.12) - hire investigator   
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.02) - of                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.12) - view https          


Category : Failed to learn tox. Words
Category2: 
Words    : ['filthy racist, racist lie']

------------------------------------------------------------------

He should be execute not give year . 
True label: 1

-                                   ---- Result: 1.04 ----
                          sum toxic : 1.32  |  -0.28 : sum non-toxic                      

             execute - (w=2.14/v=0.38) 0.82 | -0.11 (w=-0.33/v=0.33) - not give            
          be execute - (w=0.36/v=0.46) 0.17 | -0.07 (w=-0.40/v=0.17) - year                
           he should - (w=0.28/v=0.32) 0.09 | -0.06 (w=-0.71/v=0.09) - not                 
                  he - (w=0.49/v=0.15) 0.08 | -0.04 (w=-0.08/v=0.50) - give year           
              should - (w=0.35/v=0.17) 0.06 | -0.00 (w=-0.01/v=0.19) - give                
           should be - (w=0.25/v=0.22) 0.06 | 0.00 (w=0.00/v=0.00) -                     
                  be - (w=0.76/v=0.06) 0.05 | 0.00 (w=0.00/v=0.00) -                     


Category : N-Tox. Ass. To negations
Category2: 
Words    : ['not give, not']

------------------------------------------------------------------

The Clinton have be a crime family . 
True label: 1

-                                   ---- Result: -0.01 ----
                          sum toxic : 0.19  |  -0.20 : sum non-toxic                      

             clinton - (w=0.36/v=0.27) 0.10 | -0.09 (w=-0.23/v=0.37) - the clinton         
                  be - (w=0.76/v=0.07) 0.05 | -0.07 (w=-0.36/v=0.19) - have be             
               crime - (w=0.07/v=0.28) 0.02 | -0.02 (w=-0.10/v=0.24) - family              
        clinton have - (w=0.04/v=0.38) 0.01 | -0.01 (w=-0.15/v=0.09) - have                
        crime family - (w=0.01/v=0.51) 0.00 | -0.01 (w=-0.11/v=0.07) - the                 
            be crime - (w=0.01/v=0.44) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : Lack of tox. Words
Category2: 
Words    : ['crime, crime family']

------------------------------------------------------------------

I love how fellow troglodyte vote in agreement with you despite your falsehood . Let me see our middle class taxis be lower and this also benefit the wealthy but yes they do rise for that make over $ 200k boo hoo . Our corporate taxis be already in the range and have be for a long time . So where be this tax increase under Wynne or Trudeau name them . You believe Trump can get rid of half just a guess of federal revenue and it will benefit society . You think that be how it work . Do tell . How doe the fact that within the next year we will ALL lose another of job due to technology . Tell me how that factor in for you . What about the fact that globally we be add about much people a year but require little labour . How doe that factor in let me tell you . Should we always just let the reap the benefit of technology none of them have invent . Simple mind answer be for the far right among us . 
True label: 1


-                                   ---- Result: -0.25 ----
                          sum toxic : 1.39  |  -1.64 : sum non-toxic                      

          troglodyte - (w=1.09/v=0.08) 0.09 | -0.09 (w=-0.87/v=0.11) - how doe             
              let me - (w=0.53/v=0.10) 0.05 | -0.05 (w=-0.44/v=0.12) - benefit             
                 you - (w=0.65/v=0.08) 0.05 | -0.05 (w=-0.38/v=0.12) - factor in           
                  be - (w=0.76/v=0.07) 0.05 | -0.04 (w=-0.35/v=0.11) - taxis be            
             me tell - (w=0.57/v=0.07) 0.04 | -0.04 (w=-0.39/v=0.10) - technology          
            love how - (w=0.57/v=0.07) 0.04 | -0.03 (w=-0.72/v=0.05) - much people         
                 let - (w=0.35/v=0.10) 0.03 | -0.03 (w=-0.56/v=0.06) - in                  
           long time - (w=0.68/v=0.05) 0.03 | -0.03 (w=-0.51/v=0.06) - already in          
                 rid - (w=0.66/v=0.05) 0.03 | -0.03 (w=-0.28/v=0.10) - factor              
                them - (w=0.59/v=0.05) 0.03 | -0.03 (w=-0.37/v=0.07) - of half             
            wynne or - (w=0.36/v=0.09) 0.03 | -0.02 (w=-0.33/v=0.07) - tell how            
                your - (w=1.35/v=0.02) 0.03 | -0.02 (w=-0.32/v=0.07) - society you         
        tax increase - (w=0.48/v=0.06) 0.03 | -0.02 (w=-0.26/v=0.09) - be for              
            you what - (w=0.45/v=0.06) 0.03 | -0.02 (w=-0.31/v=0.07) - we always           
            this tax - (w=0.39/v=0.06) 0.02 | -0.02 (w=-0.31/v=0.07) - make over           
                this - (w=0.69/v=0.04) 0.02 | -0.02 (w=-0.67/v=0.03) - tax                 
                mind - (w=0.60/v=0.04) 0.02 | -0.02 (w=-0.29/v=0.07) - do tell             
               trump - (w=0.90/v=0.03) 0.02 | -0.02 (w=-0.28/v=0.07) - despite your        
            will all - (w=0.39/v=0.06) 0.02 | -0.02 (w=-0.40/v=0.05) - year                
           them have - (w=0.35/v=0.06) 0.02 | -0.02 (w=-0.29/v=0.07) - 200k                
             year we - (w=0.35/v=0.06) 0.02 | -0.02 (w=-0.30/v=0.07) - see our             
              be how - (w=0.41/v=0.05) 0.02 | -0.02 (w=-0.22/v=0.09) - trudeau name        
            the next - (w=0.43/v=0.04) 0.02 | -0.02 (w=-0.57/v=0.03) - do                  
         the wealthy - (w=0.32/v=0.06) 0.02 | -0.02 (w=-0.41/v=0.05) - be already          
         but require - (w=0.20/v=0.09) 0.02 | -0.02 (w=-0.58/v=0.03) - long                
         simple mind - (w=0.24/v=0.07) 0.02 | -0.02 (w=-0.36/v=0.05) - wealthy             
              labour - (w=0.31/v=0.06) 0.02 | -0.02 (w=-0.38/v=0.04) - we will             
           this also - (w=0.24/v=0.07) 0.02 | -0.02 (w=-0.30/v=0.06) - trump can           
              me how - (w=0.28/v=0.06) 0.02 | -0.02 (w=-0.32/v=0.05) - doe that            
             trudeau - (w=0.43/v=0.04) 0.02 | -0.02 (w=-0.26/v=0.07) - falsehood           
              little - (w=0.59/v=0.03) 0.02 | -0.02 (w=-0.47/v=0.04) - under               
                 how - (w=0.12/v=0.13) 0.02 | -0.02 (w=-0.27/v=0.06) - them you            
              rid of - (w=0.30/v=0.05) 0.01 | -0.02 (w=-0.29/v=0.06) - the far             
          about much - (w=0.23/v=0.06) 0.01 | -0.02 (w=-0.34/v=0.05) - tell you            
             time so - (w=0.22/v=0.06) 0.01 | -0.02 (w=-0.21/v=0.07) - taxis               
           range and - (w=0.18/v=0.07) 0.01 | -0.02 (w=-0.19/v=0.08) - have invent         
              simple - (w=0.31/v=0.04) 0.01 | -0.01 (w=-0.19/v=0.08) - that within         
             another - (w=0.38/v=0.03) 0.01 | -0.01 (w=-0.30/v=0.05) - agreement           
             boo hoo - (w=0.17/v=0.07) 0.01 | -0.01 (w=-0.58/v=0.03) - think               
            how that - (w=0.21/v=0.06) 0.01 | -0.01 (w=-0.25/v=0.06) - far right           
                just - (w=0.24/v=0.05) 0.01 | -0.01 (w=-0.35/v=0.04) - but                 
             work do - (w=0.18/v=0.06) 0.01 | -0.01 (w=-0.34/v=0.04) - add                 
        in agreement - (w=0.16/v=0.07) 0.01 | -0.01 (w=-0.49/v=0.03) - that be             
                 and - (w=0.29/v=0.04) 0.01 | -0.01 (w=-0.48/v=0.03) - work                
            and have - (w=0.27/v=0.04) 0.01 | -0.01 (w=-0.29/v=0.05) - tell me             
             for you - (w=0.23/v=0.04) 0.01 | -0.01 (w=-0.30/v=0.05) - corporate           
        also benefit - (w=0.12/v=0.08) 0.01 | -0.01 (w=-0.21/v=0.06) - just let            
           next year - (w=0.19/v=0.05) 0.01 | -0.01 (w=-0.27/v=0.05) - range               
        lose another - (w=0.12/v=0.08) 0.01 | -0.01 (w=-0.21/v=0.06) - for                 
           you think - (w=0.23/v=0.04) 0.01 | -0.01 (w=-0.26/v=0.05) - what about          
               class - (w=0.23/v=0.04) 0.01 | -0.01 (w=-0.18/v=0.07) - all lose            
              should - (w=0.35/v=0.03) 0.01 | -0.01 (w=-0.13/v=0.10) - tell                
           fact that - (w=0.10/v=0.09) 0.01 | -0.01 (w=-0.31/v=0.04) - middle              
              me see - (w=0.12/v=0.07) 0.01 | -0.01 (w=-0.32/v=0.04) - and it              
            among us - (w=0.13/v=0.07) 0.01 | -0.01 (w=-0.14/v=0.09) - under wynne         
             already - (w=0.23/v=0.04) 0.01 | -0.01 (w=-0.30/v=0.04) - due                 
          within the - (w=0.16/v=0.05) 0.01 | -0.01 (w=-0.28/v=0.04) - you should          
               wynne - (w=0.15/v=0.05) 0.01 | -0.01 (w=-0.23/v=0.05) - benefit of          
             let the - (w=0.16/v=0.05) 0.01 | -0.01 (w=-0.23/v=0.05) - you believe         
              be add - (w=0.10/v=0.06) 0.01 | -0.01 (w=-0.27/v=0.04) - rise                
            with you - (w=0.14/v=0.05) 0.01 | -0.01 (w=-0.20/v=0.06) - of job              
             vote in - (w=0.12/v=0.05) 0.01 | -0.01 (w=-0.07/v=0.16) - that factor         
                none - (w=0.13/v=0.04) 0.01 | -0.01 (w=-0.21/v=0.05) - lower               
              invent - (w=0.10/v=0.06) 0.01 | -0.01 (w=-0.31/v=0.04) - next                
            year but - (w=0.10/v=0.06) 0.01 | -0.01 (w=-0.29/v=0.04) - guess               
                what - (w=0.26/v=0.02) 0.01 | -0.01 (w=-0.18/v=0.06) - reap                
              how it - (w=0.10/v=0.05) 0.01 | -0.01 (w=-0.32/v=0.03) - far                 
         revenue and - (w=0.08/v=0.06) 0.01 | -0.01 (w=-0.12/v=0.09) - me                  
          of federal - (w=0.08/v=0.06) 0.01 | -0.01 (w=-0.36/v=0.03) - have be             
            the fact - (w=0.06/v=0.08) 0.00 | -0.01 (w=-0.29/v=0.03) - believe             
              in let - (w=0.06/v=0.08) 0.00 | -0.01 (w=-0.16/v=0.06) - doe                 
         always just - (w=0.06/v=0.08) 0.00 | -0.01 (w=-0.15/v=0.06) - fact                
       of technology - (w=0.07/v=0.07) 0.00 | -0.01 (w=-0.46/v=0.02) - can                 
          just guess - (w=0.07/v=0.07) 0.00 | -0.01 (w=-0.19/v=0.05) - about               
            yes they - (w=0.08/v=0.06) 0.00 | -0.01 (w=-0.11/v=0.08) - to technology       
           that make - (w=0.10/v=0.05) 0.00 | -0.01 (w=-0.22/v=0.04) - answer              
             they do - (w=0.13/v=0.04) 0.00 | -0.01 (w=-0.14/v=0.06) - will benefit        
             despite - (w=0.10/v=0.05) 0.00 | -0.01 (w=-0.29/v=0.03) - where               
             revenue - (w=0.10/v=0.04) 0.00 | -0.01 (w=-0.13/v=0.07) - globally            
              in for - (w=0.08/v=0.06) 0.00 | -0.01 (w=-0.11/v=0.08) - the                 
        middle class - (w=0.08/v=0.05) 0.00 | -0.01 (w=-0.20/v=0.04) - require             
                 all - (w=0.19/v=0.02) 0.00 | -0.01 (w=-0.18/v=0.04) - within              
              people - (w=0.17/v=0.02) 0.00 | -0.01 (w=-0.13/v=0.06) - benefit the         
             be this - (w=0.08/v=0.05) 0.00 | -0.01 (w=-0.10/v=0.08) - require little      
             can get - (w=0.08/v=0.05) 0.00 | -0.01 (w=-0.11/v=0.06) - but yes             
                lose - (w=0.10/v=0.04) 0.00 | -0.01 (w=-0.19/v=0.04) - name                
           answer be - (w=0.06/v=0.06) 0.00 | -0.01 (w=-0.08/v=0.09) - half just           
         you despite - (w=0.04/v=0.09) 0.00 | -0.01 (w=-0.20/v=0.03) - yes                 
             get rid - (w=0.06/v=0.05) 0.00 | -0.01 (w=-0.16/v=0.04) - think that          
               we be - (w=0.08/v=0.04) 0.00 | -0.01 (w=-0.12/v=0.05) - our                 
                they - (w=0.14/v=0.02) 0.00 | -0.01 (w=-0.14/v=0.05) - among               
                  we - (w=0.04/v=0.07) 0.00 | -0.01 (w=-0.23/v=0.03) - time                
              fellow - (w=0.05/v=0.05) 0.00 | -0.01 (w=-0.20/v=0.03) - see                 
       believe trump - (w=0.04/v=0.07) 0.00 | -0.01 (w=-0.19/v=0.03) - us                  
             none of - (w=0.05/v=0.05) 0.00 | -0.00 (w=-0.18/v=0.03) - right               
                with - (w=0.12/v=0.02) 0.00 | -0.00 (w=-0.15/v=0.03) - vote                
            for long - (w=0.04/v=0.05) 0.00 | -0.00 (w=-0.14/v=0.03) - it                  
         the benefit - (w=0.04/v=0.05) 0.00 | -0.00 (w=-0.06/v=0.08) - that                
             it will - (w=0.05/v=0.04) 0.00 | -0.00 (w=-0.06/v=0.08) - or trudeau          
              in the - (w=0.09/v=0.02) 0.00 | -0.00 (w=-0.10/v=0.04) - of them             
             doe the - (w=0.03/v=0.05) 0.00 | -0.00 (w=-0.05/v=0.08) - job due             
            for that - (w=0.03/v=0.04) 0.00 | -0.00 (w=-0.09/v=0.05) - where be            
            and this - (w=0.02/v=0.05) 0.00 | -0.00 (w=-0.15/v=0.03) - have                
             it work - (w=0.01/v=0.05) 0.00 | -0.00 (w=-0.10/v=0.04) - love                
                 get - (w=0.03/v=0.02) 0.00 | -0.00 (w=-0.04/v=0.10) - the reap            
                half - (w=0.01/v=0.04) 0.00 | -0.00 (w=-0.10/v=0.04) - increase            
       our corporate - (w=0.00/v=0.08) 0.00 | -0.00 (w=-0.11/v=0.04) - about the           
              always - (w=0.00/v=0.04) 0.00 | -0.00 (w=-0.09/v=0.04) - federal             
          how fellow - (w=0.00/v=0.10) 0.00 | -0.00 (w=-0.05/v=0.07) - reap the            
            rise for - (w=0.00/v=0.08) 0.00 | -0.00 (w=-0.13/v=0.02) - make                
       little labour - (w=0.00/v=0.10) 0.00 | -0.00 (w=-0.04/v=0.08) - wealthy but         
       that globally - (w=0.00/v=0.10) 0.00 | -0.00 (w=-0.13/v=0.02) - or                  
             do rise - (w=0.00/v=0.09) 0.00 | -0.00 (w=-0.04/v=0.06) - so where            
         class taxis - (w=0.00/v=0.08) 0.00 | -0.00 (w=-0.04/v=0.07) - hoo                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.08) - increase under      
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.08) - people year         
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.07) - corporate taxis     
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.05/v=0.04) - society             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.10) - labour how          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.09) - our middle          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.06) - agreement with      
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.08) - name them           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.06) - the range           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.09) - globally we         
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.02) - so                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.07) - be lower            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.06) - another of          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.08) - guess of            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.10/v=0.01) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.08) - lower and           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.10) - your falsehood      
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.03) - also                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.04) - will                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.09) - over 200k           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.02) - much                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.04) - due to              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.03) - for the             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.06) - should we           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.09) - right among         
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.08) - add about           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.03) - job                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.03) - over                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.08) - benefit society     
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.06) - boo                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.06) - of                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.08) - federal revenue     


Category : Failed to learn tox. Words
Category2: 
Words    : ['troglodyte']

------------------------------------------------------------------

Gee of the plus ARs out there we be look at what that be use in a mass shoot . You be talk about literally a little than in a chance . The problem much definitely be with the class of government will protect everyone from everything idiocy that have be propagate by certain politician . 
True label: 1

-                                   ---- Result: 0.99 ----
                          sum toxic : 1.99  |  -1.00 : sum non-toxic                      

              idiocy - (w=8.20/v=0.12) 1.02 | -0.11 (w=-0.78/v=0.14) - definitely be       
               shoot - (w=2.13/v=0.09) 0.18 | -0.09 (w=-0.92/v=0.10) - be look             
           shoot you - (w=0.79/v=0.15) 0.12 | -0.08 (w=-0.90/v=0.09) - be talk             
                  be - (w=0.76/v=0.10) 0.08 | -0.05 (w=-0.31/v=0.17) - ars                 
     from everything - (w=0.46/v=0.16) 0.07 | -0.04 (w=-0.26/v=0.14) - mass shoot          
              you be - (w=0.91/v=0.06) 0.06 | -0.03 (w=-0.34/v=0.10) - be with             
          politician - (w=0.52/v=0.08) 0.04 | -0.03 (w=-0.38/v=0.09) - be use              
              little - (w=0.59/v=0.06) 0.03 | -0.03 (w=-0.56/v=0.06) - in                  
           propagate - (w=0.21/v=0.14) 0.03 | -0.03 (w=-0.23/v=0.13) - the class           
          chance the - (w=0.20/v=0.14) 0.03 | -0.03 (w=-0.26/v=0.11) - at what             
            everyone - (w=0.35/v=0.07) 0.03 | -0.03 (w=-0.19/v=0.15) - in mass             
                look - (w=0.38/v=0.06) 0.02 | -0.03 (w=-0.32/v=0.09) - chance              
         idiocy that - (w=0.13/v=0.18) 0.02 | -0.03 (w=-0.49/v=0.06) - that be             
                 you - (w=0.65/v=0.03) 0.02 | -0.02 (w=-0.35/v=0.07) - problem             
               class - (w=0.23/v=0.08) 0.02 | -0.02 (w=-0.14/v=0.16) - problem much        
     government will - (w=0.15/v=0.12) 0.02 | -0.02 (w=-0.15/v=0.15) - will protect        
          everything - (w=0.21/v=0.08) 0.02 | -0.02 (w=-0.25/v=0.08) - protect             
              use in - (w=0.14/v=0.11) 0.02 | -0.02 (w=-0.11/v=0.19) - literally little    
            there we - (w=0.11/v=0.14) 0.01 | -0.02 (w=-0.36/v=0.06) - have be             
           what that - (w=0.13/v=0.11) 0.01 | -0.02 (w=-0.21/v=0.09) - certain             
                plus - (w=0.16/v=0.09) 0.01 | -0.02 (w=-0.17/v=0.11) - gee                 
        propagate by - (w=0.09/v=0.17) 0.01 | -0.02 (w=-0.38/v=0.04) - of the              
        be propagate - (w=0.07/v=0.18) 0.01 | -0.02 (w=-0.35/v=0.05) - by                  
            the plus - (w=0.08/v=0.14) 0.01 | -0.02 (w=-0.19/v=0.08) - look at             
           literally - (w=0.11/v=0.11) 0.01 | -0.01 (w=-0.08/v=0.18) - protect everyone    
                what - (w=0.26/v=0.04) 0.01 | -0.01 (w=-0.27/v=0.05) - there               
                 use - (w=0.15/v=0.06) 0.01 | -0.01 (w=-0.10/v=0.13) - class of            
     much definitely - (w=0.05/v=0.14) 0.01 | -0.01 (w=-0.13/v=0.09) - little than         
                talk - (w=0.10/v=0.07) 0.01 | -0.01 (w=-0.08/v=0.15) - everyone from       
             than in - (w=0.06/v=0.12) 0.01 | -0.01 (w=-0.12/v=0.10) - mass                
                than - (w=0.13/v=0.05) 0.01 | -0.01 (w=-0.11/v=0.10) - definitely          
               we be - (w=0.08/v=0.07) 0.01 | -0.01 (w=-0.15/v=0.06) - with the            
                with - (w=0.12/v=0.04) 0.00 | -0.01 (w=-0.19/v=0.05) - about               
          by certain - (w=0.03/v=0.15) 0.00 | -0.01 (w=-0.20/v=0.05) - from                
                  we - (w=0.04/v=0.05) 0.00 | -0.01 (w=-0.09/v=0.10) - of government       
           out there - (w=0.01/v=0.10) 0.00 | -0.01 (w=-0.14/v=0.06) - government          
     about literally - (w=0.00/v=0.19) 0.00 | -0.01 (w=-0.10/v=0.08) - the problem         
  certain politician - (w=0.00/v=0.18) 0.00 | -0.01 (w=-0.11/v=0.06) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.15/v=0.03) - have                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.06) - that                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.05) - at                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.05/v=0.05) - out                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.18) - in chance           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.04) - much                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.08) - talk about          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.04) - will                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.06) - that have           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.05) - of                  


Category : N-Tox. Ass. Stopword in Bigrams
Category2: 
Words    : ['definetly be, be look, be talk']

------------------------------------------------------------------

Donald be a duck and go for a swim with his big Mouth . 
True label: 1

-                                   ---- Result: 0.50 ----
                          sum toxic : 0.86  |  -0.36 : sum non-toxic                      

               mouth - (w=1.43/v=0.19) 0.27 | -0.19 (w=-0.58/v=0.33) - be duck             
                duck - (w=0.61/v=0.23) 0.14 | -0.04 (w=-0.13/v=0.33) - swim with           
           donald be - (w=0.36/v=0.25) 0.09 | -0.04 (w=-0.17/v=0.24) - swim                
                 his - (w=0.88/v=0.10) 0.09 | -0.02 (w=-0.14/v=0.17) - donald              
            duck and - (w=0.28/v=0.29) 0.08 | -0.02 (w=-0.16/v=0.13) - big                 
           big mouth - (w=0.16/v=0.29) 0.05 | -0.02 (w=-0.09/v=0.19) - with his            
              go for - (w=0.20/v=0.21) 0.04 | -0.01 (w=-0.21/v=0.06) - for                 
                  be - (w=0.76/v=0.04) 0.03 | -0.01 (w=-0.02/v=0.37) - for swim            
             his big - (w=0.08/v=0.28) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                  go - (w=0.20/v=0.09) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                 and - (w=0.29/v=0.05) 0.01 | 0.00 (w=0.00/v=0.00) -                     
                with - (w=0.12/v=0.07) 0.01 | 0.00 (w=0.00/v=0.00) -                     
              and go - (w=0.03/v=0.20) 0.01 | 0.00 (w=0.00/v=0.00) -                     


Category : No tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

you miss the point totally nobody say that he do not have the constitutional right to do what she do they be question the appropriateness of her action . act like a jerk in front of a national tv audience be not proper demeanor for a elect delegate represent the democratic party and Hawaii . she bring shame and dishonor to us . 
True label: 1

-                                   ---- Result: 0.42 ----
                          sum toxic : 1.57  |  -1.15 : sum non-toxic                      

                jerk - (w=7.07/v=0.10) 0.70 | -0.15 (w=-1.00/v=0.15) - jerk in             
           like jerk - (w=0.94/v=0.15) 0.14 | -0.05 (w=-0.57/v=0.09) - do                  
               shame - (w=1.06/v=0.09) 0.09 | -0.04 (w=-0.30/v=0.14) - dishonor            
                 she - (w=0.69/v=0.12) 0.08 | -0.04 (w=-0.29/v=0.14) - demeanor            
            act like - (w=0.77/v=0.10) 0.08 | -0.04 (w=-0.30/v=0.13) - shame and           
           for elect - (w=0.27/v=0.13) 0.04 | -0.04 (w=-0.40/v=0.10) - proper              
            front of - (w=0.35/v=0.10) 0.03 | -0.04 (w=-0.24/v=0.15) - bring shame         
                 her - (w=0.52/v=0.06) 0.03 | -0.04 (w=-0.22/v=0.17) - and dishonor        
                  be - (w=0.76/v=0.04) 0.03 | -0.04 (w=-0.71/v=0.05) - not                 
    democratic party - (w=0.24/v=0.10) 0.02 | -0.04 (w=-0.33/v=0.11) - question the        
               to us - (w=0.24/v=0.10) 0.02 | -0.03 (w=-0.23/v=0.14) - nobody say          
                  he - (w=0.49/v=0.04) 0.02 | -0.03 (w=-0.75/v=0.04) - be not              
              do not - (w=0.55/v=0.04) 0.02 | -0.03 (w=-0.24/v=0.12) - the constitutional  
                 you - (w=0.65/v=0.03) 0.02 | -0.03 (w=-0.31/v=0.09) - do they             
                  tv - (w=0.22/v=0.09) 0.02 | -0.03 (w=-0.33/v=0.08) - miss                
             totally - (w=0.22/v=0.08) 0.02 | -0.03 (w=-0.25/v=0.10) - you miss            
         dishonor to - (w=0.10/v=0.17) 0.02 | -0.03 (w=-0.16/v=0.16) - not proper          
             do what - (w=0.17/v=0.09) 0.02 | -0.03 (w=-0.35/v=0.07) - action              
             they be - (w=0.28/v=0.05) 0.01 | -0.03 (w=-0.39/v=0.06) - party               
                like - (w=0.34/v=0.04) 0.01 | -0.02 (w=-0.37/v=0.07) - question            
                 and - (w=0.29/v=0.05) 0.01 | -0.02 (w=-0.40/v=0.06) - point               
          her action - (w=0.10/v=0.13) 0.01 | -0.02 (w=-0.31/v=0.07) - national            
         national tv - (w=0.09/v=0.14) 0.01 | -0.02 (w=-0.19/v=0.11) - constitutional right
               to do - (w=0.18/v=0.06) 0.01 | -0.02 (w=-0.27/v=0.08) - he do               
            in front - (w=0.11/v=0.10) 0.01 | -0.02 (w=-0.29/v=0.07) - act                 
            audience - (w=0.10/v=0.11) 0.01 | -0.02 (w=-0.20/v=0.10) - what she            
                what - (w=0.26/v=0.04) 0.01 | -0.02 (w=-0.11/v=0.18) - action act          
            right to - (w=0.11/v=0.08) 0.01 | -0.02 (w=-0.17/v=0.10) - represent the       
            have the - (w=0.15/v=0.06) 0.01 | -0.02 (w=-0.13/v=0.13) - and hawaii          
         audience be - (w=0.06/v=0.14) 0.01 | -0.01 (w=-0.56/v=0.03) - in                  
            not have - (w=0.13/v=0.06) 0.01 | -0.01 (w=-0.21/v=0.07) - bring               
           party and - (w=0.06/v=0.10) 0.01 | -0.01 (w=-0.18/v=0.07) - say that            
                they - (w=0.14/v=0.04) 0.01 | -0.01 (w=-0.15/v=0.08) - front               
              nobody - (w=0.05/v=0.08) 0.00 | -0.01 (w=-0.13/v=0.09) - miss the            
               elect - (w=0.05/v=0.07) 0.00 | -0.01 (w=-0.13/v=0.08) - represent           
         of national - (w=0.01/v=0.12) 0.00 | -0.01 (w=-0.07/v=0.15) - she bring           
      elect delegate - (w=0.00/v=0.17) 0.00 | -0.01 (w=-0.19/v=0.05) - us                  
 the appropriateness - (w=0.00/v=0.16) 0.00 | -0.01 (w=-0.18/v=0.05) - right               
  appropriateness of - (w=0.00/v=0.16) 0.00 | -0.01 (w=-0.11/v=0.08) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.09/v=0.09) - of her              
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.08/v=0.10) - she do              
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.10/v=0.08) - that he             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.21/v=0.03) - for                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.04/v=0.16) - hawaii she          
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.06/v=0.10) - the democratic      
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.07/v=0.08) - hawaii              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.04/v=0.12) - be question         
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.10/v=0.04) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.16) - tv audience         
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.15/v=0.02) - have                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.04/v=0.08) - democratic          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.05) - say                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.09) - constitutional      
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.03) - that                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.12) - delegate            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.08) - the point           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.05) - of                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.15) - appropriateness     


Category : N-Tox. Ass to tox. Words
Category2: 
Words    : ['jerk in, dishonor, bring shame']

------------------------------------------------------------------

candidate come across a a adult and the other be pathetically juvenile . 
True label: 1

-                                   ---- Result: 0.53 ----
                          sum toxic : 0.74  |  -0.22 : sum non-toxic                      

        pathetically - (w=1.14/v=0.32) 0.36 | -0.07 (w=-0.26/v=0.26) - juvenile            
         come across - (w=0.51/v=0.27) 0.13 | -0.04 (w=-0.10/v=0.36) - be pathetically     
           adult and - (w=0.32/v=0.28) 0.09 | -0.03 (w=-0.07/v=0.38) - candidate come      
            other be - (w=0.36/v=0.23) 0.08 | -0.03 (w=-0.14/v=0.18) - candidate           
                  be - (w=0.76/v=0.04) 0.03 | -0.02 (w=-0.21/v=0.12) - and the             
                 and - (w=0.29/v=0.05) 0.01 | -0.02 (w=-0.17/v=0.11) - other               
               adult - (w=0.05/v=0.20) 0.01 | -0.01 (w=-0.03/v=0.41) - pathetically juvenile
              across - (w=0.04/v=0.19) 0.01 | -0.00 (w=-0.11/v=0.04) - the                 
                come - (w=0.03/v=0.12) 0.00 | 0.00 (w=0.00/v=0.00) -                     
           the other - (w=0.02/v=0.16) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : Lack of tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

Screw you and your destruction of wildlife all for taint beef . 
True label: 1

-                                   ---- Result: 0.85 ----
                          sum toxic : 1.36  |  -0.51 : sum non-toxic                      

           screw you - (w=1.90/v=0.30) 0.57 | -0.11 (w=-0.49/v=0.23) - all for             
               screw - (w=2.62/v=0.21) 0.55 | -0.10 (w=-0.39/v=0.26) - beef                
                your - (w=1.35/v=0.09) 0.13 | -0.10 (w=-0.42/v=0.23) - wildlife            
                 you - (w=0.65/v=0.07) 0.04 | -0.06 (w=-0.24/v=0.26) - taint               
         destruction - (w=0.11/v=0.23) 0.03 | -0.04 (w=-0.15/v=0.30) - of wildlife         
                 all - (w=0.19/v=0.09) 0.02 | -0.03 (w=-0.17/v=0.20) - and your            
                 and - (w=0.29/v=0.05) 0.01 | -0.03 (w=-0.13/v=0.25) - destruction of      
             you and - (w=0.07/v=0.18) 0.01 | -0.01 (w=-0.21/v=0.07) - for                 
        wildlife all - (w=0.01/v=0.40) 0.00 | -0.01 (w=-0.03/v=0.39) - your destruction    
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.05) - of                  


Category : N-Tox. Ass. Neutral words
Category2: N-Tox. Ass. Stopword in Bigrams
Words    : ['all for, beef, wildlife']

------------------------------------------------------------------

Way to send that IS scoundrel to his maker . He get access to his virgin soon than he ask for . 
True label: 1

-                                   ---- Result: 0.68 ----
                          sum toxic : 1.04  |  -0.37 : sum non-toxic                      

           scoundrel - (w=1.73/v=0.21) 0.36 | -0.08 (w=-0.41/v=0.20) - he ask              
                 his - (w=0.88/v=0.15) 0.13 | -0.06 (w=-0.44/v=0.13) - access              
              virgin - (w=0.60/v=0.20) 0.12 | -0.06 (w=-0.38/v=0.15) - ask for             
            maker he - (w=0.32/v=0.27) 0.09 | -0.05 (w=-0.37/v=0.15) - access to           
                  he - (w=0.49/v=0.14) 0.07 | -0.03 (w=-0.15/v=0.20) - soon than           
                  is - (w=0.40/v=0.11) 0.05 | -0.03 (w=-0.26/v=0.11) - ask                 
              to his - (w=0.16/v=0.28) 0.04 | -0.03 (w=-0.33/v=0.08) - way                 
           his maker - (w=0.15/v=0.28) 0.04 | -0.01 (w=-0.10/v=0.10) - to                  
               maker - (w=0.16/v=0.17) 0.03 | -0.01 (w=-0.21/v=0.05) - for                 
             to send - (w=0.12/v=0.17) 0.02 | -0.01 (w=-0.02/v=0.28) - scoundrel to        
                soon - (w=0.16/v=0.12) 0.02 | -0.01 (w=-0.04/v=0.12) - way to              
          get access - (w=0.08/v=0.22) 0.02 | -0.00 (w=-0.06/v=0.04) - that                
           send that - (w=0.07/v=0.22) 0.02 | 0.00 (w=0.00/v=0.00) -                     
             that is - (w=0.06/v=0.20) 0.01 | 0.00 (w=0.00/v=0.00) -                     
                send - (w=0.10/v=0.12) 0.01 | 0.00 (w=0.00/v=0.00) -                     
                than - (w=0.13/v=0.08) 0.01 | 0.00 (w=0.00/v=0.00) -                     
              he get - (w=0.05/v=0.15) 0.01 | 0.00 (w=0.00/v=0.00) -                     
          his virgin - (w=0.02/v=0.28) 0.00 | 0.00 (w=0.00/v=0.00) -                     
                 get - (w=0.03/v=0.07) 0.00 | 0.00 (w=0.00/v=0.00) -                     
             than he - (w=0.00/v=0.18) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : Lack of tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

Lord have Mercy on us Trump be run amok . 
True label: 1

-                                   ---- Result: 0.40 ----
                          sum toxic : 0.94  |  -0.54 : sum non-toxic                      

            trump be - (w=1.41/v=0.17) 0.24 | -0.14 (w=-0.65/v=0.22) - be run              
            run amok - (w=0.56/v=0.33) 0.18 | -0.12 (w=-0.36/v=0.34) - mercy on            
                amok - (w=0.56/v=0.32) 0.18 | -0.10 (w=-0.29/v=0.33) - have mercy          
               mercy - (w=0.43/v=0.27) 0.12 | -0.08 (w=-0.24/v=0.33) - us trump            
               trump - (w=0.90/v=0.11) 0.10 | -0.05 (w=-0.19/v=0.25) - lord                
           lord have - (w=0.25/v=0.34) 0.08 | -0.02 (w=-0.19/v=0.12) - us                  
                  be - (w=0.76/v=0.04) 0.03 | -0.02 (w=-0.22/v=0.08) - on                  
               on us - (w=0.02/v=0.26) 0.01 | -0.01 (w=-0.15/v=0.06) - have                
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.05/v=0.15) - run                 


Category : Failed to learn tox. Words
Category2: 
Words    : ['amok, run amok']

------------------------------------------------------------------

And Congress and state Houses and Senates be chuck full of Republican walk dead zombie . 
True label: 1

-                                   ---- Result: 0.60 ----
                          sum toxic : 0.76  |  -0.16 : sum non-toxic                      

                dead - (w=1.27/v=0.14) 0.17 | -0.07 (w=-0.31/v=0.22) - houses              
              zombie - (w=0.69/v=0.20) 0.14 | -0.02 (w=-0.13/v=0.19) - of republican       
             full of - (w=0.82/v=0.15) 0.12 | -0.02 (w=-0.19/v=0.13) - walk                
         dead zombie - (w=0.19/v=0.28) 0.05 | -0.02 (w=-0.22/v=0.09) - state               
               chuck - (w=0.26/v=0.19) 0.05 | -0.01 (w=-0.04/v=0.28) - senates             
        congress and - (w=0.26/v=0.18) 0.05 | -0.01 (w=-0.02/v=0.27) - be chuck            
           walk dead - (w=0.15/v=0.25) 0.04 | -0.00 (w=-0.01/v=0.20) - and congress        
          republican - (w=0.32/v=0.11) 0.04 | -0.00 (w=-0.02/v=0.13) - congress            
                full - (w=0.30/v=0.12) 0.04 | -0.00 (w=-0.01/v=0.28) - houses and          
                 and - (w=0.29/v=0.11) 0.03 | -0.00 (w=-0.00/v=0.04) - of                  
                  be - (w=0.76/v=0.03) 0.02 | 0.00 (w=0.00/v=0.00) -                     
           and state - (w=0.10/v=0.16) 0.02 | 0.00 (w=0.00/v=0.00) -                     
          chuck full - (w=0.00/v=0.29) 0.00 | 0.00 (w=0.00/v=0.00) -                     
     republican walk - (w=0.00/v=0.29) 0.00 | 0.00 (w=0.00/v=0.00) -                     
        state houses - (w=0.00/v=0.29) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : disagree
Category2: 
Words    : 

------------------------------------------------------------------

My bad I do not see this initially when I say the same thing . F John McCain and this b**tch . 
True label: 1

-                                   ---- Result: 0.38 ----
                          sum toxic : 0.86  |  -0.48 : sum non-toxic                      

                 tch - (w=1.46/v=0.26) 0.38 | -0.08 (w=-0.36/v=0.21) - initially           
            this tch - (w=0.50/v=0.33) 0.17 | -0.07 (w=-0.42/v=0.17) - see this            
                this - (w=0.69/v=0.12) 0.08 | -0.05 (w=-0.34/v=0.15) - not see             
            when say - (w=0.29/v=0.23) 0.07 | -0.04 (w=-0.18/v=0.24) - mccain and          
                 bad - (w=0.34/v=0.12) 0.04 | -0.03 (w=-0.71/v=0.05) - not                 
              do not - (w=0.55/v=0.07) 0.04 | -0.03 (w=-0.32/v=0.10) - thing               
              mccain - (w=0.18/v=0.19) 0.03 | -0.03 (w=-0.57/v=0.06) - do                  
             say the - (w=0.12/v=0.15) 0.02 | -0.03 (w=-0.13/v=0.23) - my bad              
                 and - (w=0.29/v=0.04) 0.01 | -0.03 (w=-0.34/v=0.08) - when                
                  my - (w=0.10/v=0.09) 0.01 | -0.02 (w=-0.09/v=0.22) - john mccain         
                same - (w=0.06/v=0.11) 0.01 | -0.02 (w=-0.20/v=0.09) - see                 
            and this - (w=0.02/v=0.16) 0.00 | -0.01 (w=-0.07/v=0.17) - same thing          
      this initially - (w=0.00/v=0.32) 0.00 | -0.01 (w=-0.10/v=0.11) - the same            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.08) - say                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.11/v=0.03) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.31) - thing john          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.27) - bad do              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.15) - john                


Category : tox. Word misspelled
Category2: 
Words    : ['b**tch']

------------------------------------------------------------------

Hope they kill him Good Job . -PRON- be sure lot of innocent live save . I like Spanish police have style . 
True label: 1

-                                   ---- Result: 0.73 ----
                          sum toxic : 1.17  |  -0.44 : sum non-toxic                      

                kill - (w=3.16/v=0.12) 0.38 | -0.08 (w=-0.35/v=0.21) - they kill           
            kill him - (w=0.97/v=0.22) 0.21 | -0.06 (w=-0.26/v=0.22) - him good            
                 him - (w=0.72/v=0.10) 0.07 | -0.05 (w=-0.35/v=0.13) - be sure             
            job pron - (w=0.35/v=0.20) 0.07 | -0.04 (w=-0.41/v=0.10) - sure                
                pron - (w=0.94/v=0.06) 0.05 | -0.04 (w=-0.24/v=0.15) - innocent            
           hope they - (w=0.31/v=0.18) 0.05 | -0.03 (w=-0.13/v=0.25) - sure lot            
            good job - (w=0.31/v=0.17) 0.05 | -0.03 (w=-0.44/v=0.07) - good                
          have style - (w=0.21/v=0.24) 0.05 | -0.03 (w=-0.22/v=0.12) - police              
         police have - (w=0.20/v=0.20) 0.04 | -0.02 (w=-0.24/v=0.10) - live                
         of innocent - (w=0.19/v=0.20) 0.04 | -0.02 (w=-0.11/v=0.19) - spanish             
       innocent live - (w=0.15/v=0.24) 0.04 | -0.02 (w=-0.16/v=0.11) - lot                 
              lot of - (w=0.25/v=0.12) 0.03 | -0.01 (w=-0.06/v=0.26) - live save           
                like - (w=0.34/v=0.07) 0.02 | -0.01 (w=-0.15/v=0.04) - have                
                  be - (w=0.76/v=0.03) 0.02 | -0.00 (w=-0.03/v=0.11) - hope                
                save - (w=0.16/v=0.13) 0.02 | -0.00 (w=-0.01/v=0.30) - spanish police      
               style - (w=0.08/v=0.16) 0.01 | -0.00 (w=-0.00/v=0.30) - save like           
                they - (w=0.14/v=0.06) 0.01 | -0.00 (w=-0.01/v=0.10) - job                 
             pron be - (w=0.02/v=0.08) 0.00 | -0.00 (w=-0.00/v=0.04) - of                  


Category : Failed to learn tox. Words
Category2: N-Tox. Ass to tox. Words
Words    : ['kill him | they kill']

------------------------------------------------------------------

you forget republicans stupidism . 
True label: 1

-                                   ---- Result: 0.65 ----
                          sum toxic : 0.66  |  -0.01 : sum non-toxic                      

         republicans - (w=0.83/v=0.59) 0.49 | -0.01 (w=-0.02/v=0.46) - forget              
                 you - (w=0.65/v=0.19) 0.12 | 0.00 (w=0.00/v=0.00) -                     
          you forget - (w=0.08/v=0.64) 0.05 | 0.00 (w=0.00/v=0.00) -                     


Category : tox. Word misspelled
Category2: 
Words    : ['stupidism']

------------------------------------------------------------------

They be much animal than the goat disgust . 
True label: 1

-                                   ---- Result: 0.97 ----
                          sum toxic : 1.33  |  -0.36 : sum non-toxic                      

             disgust - (w=4.38/v=0.26) 1.16 | -0.21 (w=-0.49/v=0.42) - the goat            
             they be - (w=0.28/v=0.16) 0.04 | -0.07 (w=-0.20/v=0.35) - goat                
                  be - (w=0.76/v=0.05) 0.04 | -0.04 (w=-0.22/v=0.19) - be much             
              animal - (w=0.10/v=0.26) 0.03 | -0.03 (w=-0.07/v=0.45) - animal than         
            than the - (w=0.12/v=0.22) 0.03 | -0.01 (w=-0.11/v=0.05) - the                 
                than - (w=0.13/v=0.14) 0.02 | -0.00 (w=-0.01/v=0.44) - much animal         
                they - (w=0.14/v=0.11) 0.02 | -0.00 (w=-0.03/v=0.11) - much                


Category : N-Tox. Ass. Neutral words
Category2: 
Words    : ['the goat']

------------------------------------------------------------------

of the major issue with drone killing be that it change war since it require no courage to pilot and kill . It also dehumanize the battle so that they be similar to a video game . I do not say they be bad in every possible way . I say they be coward . 
True label: 1


-                                   ---- Result: 0.88 ----
                          sum toxic : 1.74  |  -0.86 : sum non-toxic                      

              coward - (w=6.21/v=0.11) 0.66 | -0.06 (w=-0.55/v=0.12) - drone               
                kill - (w=3.16/v=0.07) 0.24 | -0.06 (w=-0.41/v=0.14) - bad in              
             killing - (w=1.57/v=0.12) 0.19 | -0.05 (w=-0.44/v=0.10) - in every            
           be coward - (w=0.73/v=0.13) 0.10 | -0.04 (w=-0.41/v=0.11) - similar to          
          dehumanize - (w=0.52/v=0.15) 0.08 | -0.04 (w=-0.31/v=0.12) - the battle          
             kill it - (w=0.54/v=0.13) 0.07 | -0.04 (w=-0.33/v=0.11) - pilot               
          courage to - (w=0.50/v=0.12) 0.06 | -0.03 (w=-0.30/v=0.11) - since it            
                  be - (w=0.76/v=0.08) 0.06 | -0.03 (w=-0.17/v=0.19) - say they            
             they be - (w=0.28/v=0.17) 0.05 | -0.03 (w=-0.45/v=0.06) - be that             
          no courage - (w=0.26/v=0.16) 0.04 | -0.03 (w=-0.35/v=0.07) - that they           
          be similar - (w=0.34/v=0.12) 0.04 | -0.02 (w=-0.16/v=0.15) - war since           
           pilot and - (w=0.18/v=0.14) 0.02 | -0.02 (w=-0.37/v=0.06) - issue               
                 bad - (w=0.34/v=0.07) 0.02 | -0.02 (w=-0.21/v=0.11) - and kill            
              do not - (w=0.55/v=0.04) 0.02 | -0.02 (w=-0.35/v=0.07) - since               
              be bad - (w=0.20/v=0.10) 0.02 | -0.02 (w=-0.22/v=0.10) - battle              
                they - (w=0.14/v=0.11) 0.02 | -0.02 (w=-0.28/v=0.08) - game                
               every - (w=0.24/v=0.06) 0.02 | -0.02 (w=-0.25/v=0.08) - major               
          it require - (w=0.07/v=0.13) 0.01 | -0.02 (w=-0.12/v=0.16) - way say             
                 and - (w=0.29/v=0.02) 0.01 | -0.02 (w=-0.71/v=0.03) - not                 
                with - (w=0.12/v=0.04) 0.00 | -0.02 (w=-0.17/v=0.11) - issue with          
            possible - (w=0.04/v=0.08) 0.00 | -0.02 (w=-0.57/v=0.03) - do                  
             it also - (w=0.03/v=0.11) 0.00 | -0.02 (w=-0.33/v=0.05) - way                 
             so that - (w=0.03/v=0.09) 0.00 | -0.02 (w=-0.28/v=0.06) - change              
      every possible - (w=0.02/v=0.15) 0.00 | -0.02 (w=-0.16/v=0.11) - courage             
             that it - (w=0.04/v=0.08) 0.00 | -0.02 (w=-0.13/v=0.14) - video game          
          killing be - (w=0.02/v=0.16) 0.00 | -0.02 (w=-0.38/v=0.04) - of the              
                  no - (w=0.06/v=0.04) 0.00 | -0.02 (w=-0.56/v=0.03) - in                  
          with drone - (w=0.01/v=0.16) 0.00 | -0.02 (w=-0.20/v=0.08) - require             
             game do - (w=0.01/v=0.14) 0.00 | -0.01 (w=-0.10/v=0.15) - to video            
           battle so - (w=0.00/v=0.19) 0.00 | -0.01 (w=-0.14/v=0.09) - it                  
      dehumanize the - (w=0.00/v=0.18) 0.00 | -0.01 (w=-0.08/v=0.14) - major issue         
       drone killing - (w=0.00/v=0.18) 0.00 | -0.01 (w=-0.11/v=0.09) - video               
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.06/v=0.15) - require no          
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.07/v=0.09) - not say             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.05/v=0.11) - the major           
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.06/v=0.10) - say                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.15) - possible way        
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.10/v=0.05) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.11/v=0.04) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.09) - similar             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.05) - that                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.04) - so                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.16) - to pilot            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.06) - also                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.08) - war                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.13) - it change           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.02) - of                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.17) - change war          


Category : disagree
Category2: 
Words    : 

------------------------------------------------------------------

I hear much and much criminal use the I make a mistake defense Hillary be a pioneer amongst criminal . 
True label: 1

-                                   ---- Result: 0.56 ----
                          sum toxic : 0.88  |  -0.32 : sum non-toxic                      

            criminal - (w=1.37/v=0.32) 0.44 | -0.07 (w=-0.40/v=0.17) - mistake             
          hillary be - (w=0.79/v=0.21) 0.17 | -0.06 (w=-0.19/v=0.33) - criminal use        
             pioneer - (w=0.24/v=0.26) 0.06 | -0.05 (w=-0.28/v=0.16) - and much            
             hillary - (w=0.37/v=0.15) 0.06 | -0.03 (w=-0.15/v=0.19) - much and            
            the make - (w=0.17/v=0.24) 0.04 | -0.02 (w=-0.14/v=0.18) - defense             
                  be - (w=0.76/v=0.04) 0.03 | -0.02 (w=-0.08/v=0.29) - much criminal       
             use the - (w=0.17/v=0.17) 0.03 | -0.02 (w=-0.05/v=0.33) - be pioneer          
             amongst - (w=0.10/v=0.23) 0.02 | -0.02 (w=-0.06/v=0.26) - hear much           
                 use - (w=0.15/v=0.11) 0.02 | -0.01 (w=-0.13/v=0.09) - make                
                 and - (w=0.29/v=0.05) 0.01 | -0.01 (w=-0.04/v=0.14) - hear                
        make mistake - (w=0.01/v=0.24) 0.00 | -0.01 (w=-0.03/v=0.15) - much                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.11/v=0.04) - the                 


Category : Lack of tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

Bloody hell your column should be suck up by the waste disposal truck . That be all it be worth and stink just a bad . No reporter have ever mention the support of the atrocity commit by Batista support and sanction by the U.S. government Done to protect callous landowner of sugar plantation from their own worker . The States send in their Marines several time to put down sugar plantation worker . The U. S. dominate and direct all aspect of life in Cuba from the end of the Spanish War to Castro have overthrow of Batista Castro would not take direction from the American to put Cuba under American thumb so they make attempt to kill him . A doctrine that many many people over look be the Monroe Doctrine . States basically that only the U. S. can interfere with government in the Western Hemisphere . 
True label: 1

-                                   ---- Result: 0.79 ----
                          sum toxic : 2.01  |  -1.22 : sum non-toxic                      

                suck - (w=6.13/v=0.05) 0.31 | -0.10 (w=-1.56/v=0.07) - suck up             
               stink - (w=2.73/v=0.06) 0.16 | -0.07 (w=-0.60/v=0.12) - sugar               
                kill - (w=3.16/v=0.04) 0.13 | -0.07 (w=-0.77/v=0.09) - bloody hell         
              bloody - (w=1.95/v=0.06) 0.12 | -0.06 (w=-0.78/v=0.07) - be suck             
                hell - (w=2.05/v=0.05) 0.10 | -0.04 (w=-0.45/v=0.08) - bad no              
             to kill - (w=1.45/v=0.06) 0.08 | -0.04 (w=-0.25/v=0.15) - worker the          
            kill him - (w=0.97/v=0.07) 0.07 | -0.04 (w=-0.76/v=0.05) - many people         
             batista - (w=0.27/v=0.17) 0.04 | -0.04 (w=-0.54/v=0.07) - atrocity            
              end of - (w=0.69/v=0.05) 0.03 | -0.03 (w=-0.58/v=0.06) - sanction            
             callous - (w=0.45/v=0.07) 0.03 | -0.03 (w=-0.52/v=0.06) - mention the         
                your - (w=1.35/v=0.02) 0.03 | -0.02 (w=-0.56/v=0.04) - in                  
                  be - (w=0.76/v=0.04) 0.03 | -0.02 (w=-0.35/v=0.07) - by                  
      basically that - (w=0.35/v=0.09) 0.03 | -0.02 (w=-0.37/v=0.06) - the western         
           the waste - (w=0.41/v=0.07) 0.03 | -0.02 (w=-0.43/v=0.05) - direction           
              all it - (w=0.47/v=0.06) 0.03 | -0.02 (w=-0.34/v=0.06) - the can             
            just bad - (w=0.37/v=0.07) 0.03 | -0.02 (w=-0.27/v=0.08) - your column         
                 him - (w=0.72/v=0.03) 0.02 | -0.02 (w=-0.30/v=0.07) - american to         
            doctrine - (w=0.17/v=0.13) 0.02 | -0.02 (w=-0.32/v=0.06) - government in       
             so they - (w=0.45/v=0.05) 0.02 | -0.02 (w=-0.27/v=0.07) - disposal            
      interfere with - (w=0.34/v=0.06) 0.02 | -0.02 (w=-0.28/v=0.06) - the support         
               their - (w=0.44/v=0.05) 0.02 | -0.02 (w=-0.25/v=0.07) - support             
        several time - (w=0.31/v=0.06) 0.02 | -0.02 (w=-0.47/v=0.04) - under               
               waste - (w=0.44/v=0.04) 0.02 | -0.02 (w=-0.37/v=0.05) - the end             
           hell your - (w=0.20/v=0.10) 0.02 | -0.02 (w=-0.38/v=0.04) - of the              
           many many - (w=0.31/v=0.06) 0.02 | -0.02 (w=-0.21/v=0.08) - overthrow of        
           and stink - (w=0.20/v=0.09) 0.02 | -0.02 (w=-0.24/v=0.06) - thumb               
            not take - (w=0.35/v=0.05) 0.02 | -0.01 (w=-0.24/v=0.06) - many                
              to put - (w=0.18/v=0.10) 0.02 | -0.01 (w=-0.49/v=0.03) - that be             
              direct - (w=0.35/v=0.05) 0.02 | -0.01 (w=-0.11/v=0.13) - the                 
            american - (w=0.24/v=0.07) 0.02 | -0.01 (w=-0.20/v=0.07) - from                
            put down - (w=0.25/v=0.07) 0.02 | -0.01 (w=-0.24/v=0.06) - be worth            
             send in - (w=0.23/v=0.07) 0.02 | -0.01 (w=-0.21/v=0.05) - column              
              states - (w=0.18/v=0.09) 0.02 | -0.01 (w=-0.25/v=0.04) - protect             
                ever - (w=0.44/v=0.04) 0.02 | -0.01 (w=-0.12/v=0.09) - worker              
          from their - (w=0.29/v=0.05) 0.02 | -0.01 (w=-0.14/v=0.08) - all aspect          
         support and - (w=0.24/v=0.06) 0.01 | -0.01 (w=-0.71/v=0.01) - not                 
           would not - (w=0.37/v=0.04) 0.01 | -0.01 (w=-0.46/v=0.02) - can                 
             time to - (w=0.32/v=0.04) 0.01 | -0.01 (w=-0.37/v=0.03) - it be               
          to protect - (w=0.28/v=0.05) 0.01 | -0.01 (w=-0.13/v=0.07) - landowner           
         the spanish - (w=0.18/v=0.07) 0.01 | -0.01 (w=-0.15/v=0.06) - aspect of           
                done - (w=0.20/v=0.07) 0.01 | -0.01 (w=-0.24/v=0.04) - life                
             look be - (w=0.16/v=0.08) 0.01 | -0.01 (w=-0.11/v=0.08) - make attempt        
                 bad - (w=0.34/v=0.04) 0.01 | -0.01 (w=-0.20/v=0.04) - mention             
           basically - (w=0.24/v=0.05) 0.01 | -0.01 (w=-0.14/v=0.06) - government          
       doctrine that - (w=0.14/v=0.08) 0.01 | -0.01 (w=-0.15/v=0.05) - of life             
                look - (w=0.38/v=0.03) 0.01 | -0.01 (w=-0.14/v=0.06) - up by               
        dominate and - (w=0.13/v=0.09) 0.01 | -0.01 (w=-0.09/v=0.09) - and sanction        
                only - (w=0.39/v=0.03) 0.01 | -0.01 (w=-0.09/v=0.09) - waste disposal      
                 and - (w=0.29/v=0.04) 0.01 | -0.01 (w=-0.08/v=0.09) - atrocity commit     
        the atrocity - (w=0.14/v=0.08) 0.01 | -0.01 (w=-0.11/v=0.06) - spanish             
      take direction - (w=0.12/v=0.09) 0.01 | -0.01 (w=-0.09/v=0.08) - monroe              
           have ever - (w=0.21/v=0.05) 0.01 | -0.01 (w=-0.21/v=0.03) - own                 
          plantation - (w=0.07/v=0.14) 0.01 | -0.01 (w=-0.07/v=0.09) - no reporter         
         people over - (w=0.14/v=0.07) 0.01 | -0.01 (w=-0.07/v=0.10) - to castro           
            reporter - (w=0.17/v=0.05) 0.01 | -0.01 (w=-0.07/v=0.08) - sanction by         
              be all - (w=0.23/v=0.04) 0.01 | -0.01 (w=-0.23/v=0.03) - time                
              should - (w=0.35/v=0.03) 0.01 | -0.01 (w=-0.10/v=0.06) - to                  
          truck that - (w=0.11/v=0.08) 0.01 | -0.01 (w=-0.12/v=0.05) - the american        
           should be - (w=0.25/v=0.04) 0.01 | -0.01 (w=-0.10/v=0.05) - that many           
                 all - (w=0.19/v=0.04) 0.01 | -0.01 (w=-0.08/v=0.06) - the states          
           interfere - (w=0.14/v=0.06) 0.01 | -0.01 (w=-0.08/v=0.07) - from the            
           they make - (w=0.13/v=0.05) 0.01 | -0.00 (w=-0.06/v=0.08) - worth and           
                down - (w=0.22/v=0.03) 0.01 | -0.00 (w=-0.08/v=0.06) - aspect              
             marines - (w=0.08/v=0.07) 0.01 | -0.00 (w=-0.06/v=0.08) - in cuba             
                just - (w=0.24/v=0.02) 0.01 | -0.00 (w=-0.10/v=0.05) - worth               
             life in - (w=0.09/v=0.06) 0.01 | -0.00 (w=-0.19/v=0.02) - would               
     with government - (w=0.08/v=0.07) 0.01 | -0.00 (w=-0.10/v=0.05) - several             
              castro - (w=0.04/v=0.13) 0.01 | -0.00 (w=-0.06/v=0.07) - war to              
         castro have - (w=0.06/v=0.08) 0.01 | -0.00 (w=-0.15/v=0.03) - have                
                send - (w=0.10/v=0.04) 0.00 | -0.00 (w=-0.06/v=0.07) - put                 
              by the - (w=0.06/v=0.07) 0.00 | -0.00 (w=-0.04/v=0.09) - done to             
              people - (w=0.17/v=0.02) 0.00 | -0.00 (w=-0.08/v=0.04) - the government      
            only the - (w=0.07/v=0.05) 0.00 | -0.00 (w=-0.04/v=0.08) - hemisphere          
                  up - (w=0.14/v=0.03) 0.00 | -0.00 (w=-0.13/v=0.02) - make                
          and direct - (w=0.04/v=0.07) 0.00 | -0.00 (w=-0.04/v=0.08) - reporter have       
                they - (w=0.14/v=0.02) 0.00 | -0.00 (w=-0.02/v=0.10) - cuba under          
                 end - (w=0.08/v=0.04) 0.00 | -0.00 (w=-0.14/v=0.02) - it                  
            in their - (w=0.06/v=0.04) 0.00 | -0.00 (w=-0.06/v=0.04) - that                
                with - (w=0.12/v=0.02) 0.00 | -0.00 (w=-0.02/v=0.10) - thumb so            
              in the - (w=0.09/v=0.02) 0.00 | -0.00 (w=-0.08/v=0.03) - take                
      direction from - (w=0.02/v=0.08) 0.00 | -0.00 (w=-0.02/v=0.09) - under american      
           overthrow - (w=0.03/v=0.07) 0.00 | -0.00 (w=-0.04/v=0.05) - western             
            dominate - (w=0.03/v=0.06) 0.00 | -0.00 (w=-0.04/v=0.04) - their own           
          attempt to - (w=0.03/v=0.05) 0.00 | -0.00 (w=-0.06/v=0.02) - so                  
                  no - (w=0.06/v=0.02) 0.00 | -0.00 (w=-0.02/v=0.09) - have overthrow      
           commit by - (w=0.02/v=0.07) 0.00 | -0.00 (w=-0.01/v=0.10) - put cuba            
              commit - (w=0.03/v=0.05) 0.00 | -0.00 (w=-0.02/v=0.05) - truck               
              be the - (w=0.05/v=0.03) 0.00 | -0.00 (w=-0.01/v=0.10) - monroe doctrine     
            of sugar - (w=0.01/v=0.08) 0.00 | -0.00 (w=-0.01/v=0.09) - column should       
        ever mention - (w=0.01/v=0.08) 0.00 | -0.00 (w=-0.01/v=0.10) - the monroe          
          support of - (w=0.01/v=0.06) 0.00 | -0.00 (w=-0.01/v=0.04) - attempt             
           that only - (w=0.01/v=0.06) 0.00 | -0.00 (w=-0.01/v=0.04) - war                 
          stink just - (w=0.00/v=0.09) 0.00 | -0.00 (w=-0.00/v=0.08) - over look           
                cuba - (w=0.00/v=0.13) 0.00 | -0.00 (w=-0.01/v=0.03) - over                
           cuba from - (w=0.00/v=0.09) 0.00 | -0.00 (w=-0.00/v=0.07) - of                  
  western hemisphere - (w=0.00/v=0.09) 0.00 | 0.00 (w=0.00/v=0.00) -                     
    sugar plantation - (w=0.00/v=0.20) 0.00 | 0.00 (w=0.00/v=0.00) -                     
     plantation from - (w=0.00/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     
    states basically - (w=0.00/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     
        landowner of - (w=0.00/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     
          own worker - (w=0.00/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     
       can interfere - (w=0.00/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     
        castro would - (w=0.00/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     
          direct all - (w=0.00/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     
   plantation worker - (w=0.00/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     
        the dominate - (w=0.00/v=0.09) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : N-Tox. Ass to tox. Words
Category2: 
Words    : ['suck up, be suck, bloody hell']

------------------------------------------------------------------

If you be not a socialist in your 20s you have no heart if you be still a socialist in your 40s you have no brain . 
True label: 1

-                                   ---- Result: 0.56 ----
                          sum toxic : 1.02  |  -0.45 : sum non-toxic                      

               brain - (w=1.87/v=0.12) 0.23 | -0.11 (w=-0.55/v=0.19) - 40s                 
                your - (w=1.35/v=0.12) 0.16 | -0.05 (w=-0.23/v=0.22) - in your             
              you be - (w=0.91/v=0.16) 0.14 | -0.04 (w=-0.75/v=0.06) - be not              
            no brain - (w=0.60/v=0.20) 0.12 | -0.04 (w=-0.56/v=0.08) - in                  
                 you - (w=0.65/v=0.17) 0.11 | -0.04 (w=-0.19/v=0.21) - not socialist       
            no heart - (w=0.23/v=0.21) 0.05 | -0.04 (w=-0.31/v=0.12) - heart               
             have no - (w=0.24/v=0.19) 0.04 | -0.03 (w=-0.24/v=0.11) - if                  
                  be - (w=0.76/v=0.05) 0.04 | -0.03 (w=-0.15/v=0.17) - if you              
           socialist - (w=0.13/v=0.27) 0.03 | -0.03 (w=-0.71/v=0.04) - not                 
        socialist in - (w=0.08/v=0.42) 0.03 | -0.02 (w=-0.19/v=0.11) - be still            
     still socialist - (w=0.07/v=0.25) 0.02 | -0.01 (w=-0.04/v=0.25) - 40s you             
            you have - (w=0.09/v=0.18) 0.02 | -0.01 (w=-0.13/v=0.08) - still               
                  no - (w=0.06/v=0.12) 0.01 | -0.01 (w=-0.15/v=0.07) - have                
                 20s - (w=0.01/v=0.19) 0.00 | -0.00 (w=-0.02/v=0.22) - heart if            
            your 20s - (w=0.00/v=0.25) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : Lack of tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

I will not hold my breath wait for the rebellion . Too many of us vote the way our parent do base on hair color last name or who you be relate to . SA you forget much choice for the poll Is this idiotic just like everything else that come out of Honolulu Hale . 
True label: 1

-                                   ---- Result: 0.91 ----
                          sum toxic : 1.89  |  -0.98 : sum non-toxic                      

             idiotic - (w=11.49/v=0.10) 1.17 | -0.09 (w=-0.78/v=0.11) - who you             
        this idiotic - (w=1.13/v=0.14) 0.16 | -0.06 (w=-0.59/v=0.10) - relate to           
              you be - (w=0.91/v=0.06) 0.05 | -0.05 (w=-0.34/v=0.13) - rebellion           
           my breath - (w=0.37/v=0.13) 0.05 | -0.04 (w=-0.43/v=0.10) - is this             
                 you - (w=0.65/v=0.06) 0.04 | -0.04 (w=-0.29/v=0.15) - honolulu hale       
            vote the - (w=0.35/v=0.11) 0.04 | -0.04 (w=-0.36/v=0.10) - hair                
                  is - (w=0.40/v=0.07) 0.03 | -0.04 (w=-0.29/v=0.12) - last name           
            not hold - (w=0.26/v=0.11) 0.03 | -0.03 (w=-0.42/v=0.08) - choice              
           that come - (w=0.27/v=0.10) 0.03 | -0.03 (w=-0.22/v=0.14) - our parent          
          color last - (w=0.14/v=0.18) 0.03 | -0.03 (w=-0.21/v=0.14) - hale                
            come out - (w=0.29/v=0.09) 0.03 | -0.03 (w=-0.22/v=0.13) - parent do           
            the poll - (w=0.24/v=0.10) 0.02 | -0.03 (w=-0.19/v=0.14) - like everything     
                this - (w=0.69/v=0.03) 0.02 | -0.03 (w=-0.29/v=0.09) - relate              
           else that - (w=0.17/v=0.13) 0.02 | -0.03 (w=-0.26/v=0.10) - honolulu            
            too many - (w=0.18/v=0.09) 0.02 | -0.02 (w=-0.35/v=0.07) - hold                
          everything - (w=0.21/v=0.07) 0.02 | -0.02 (w=-0.29/v=0.08) - many of             
                like - (w=0.34/v=0.04) 0.01 | -0.02 (w=-0.16/v=0.14) - much choice         
                  be - (w=0.76/v=0.02) 0.01 | -0.02 (w=-0.18/v=0.12) - of honolulu         
                else - (w=0.16/v=0.07) 0.01 | -0.02 (w=-0.13/v=0.15) - forget much         
        idiotic just - (w=0.06/v=0.18) 0.01 | -0.02 (w=-0.71/v=0.03) - not                 
                just - (w=0.24/v=0.04) 0.01 | -0.02 (w=-0.57/v=0.03) - do                  
                  sa - (w=0.09/v=0.10) 0.01 | -0.02 (w=-0.13/v=0.14) - name or             
                wait - (w=0.11/v=0.07) 0.01 | -0.02 (w=-0.33/v=0.05) - way                 
          you forget - (w=0.08/v=0.11) 0.01 | -0.02 (w=-0.15/v=0.11) - everything else     
             do base - (w=0.05/v=0.15) 0.01 | -0.02 (w=-0.20/v=0.08) - just like           
             way our - (w=0.05/v=0.14) 0.01 | -0.02 (w=-0.21/v=0.08) - of us               
              out of - (w=0.12/v=0.06) 0.01 | -0.02 (w=-0.10/v=0.15) - us vote             
              or who - (w=0.06/v=0.12) 0.01 | -0.01 (w=-0.22/v=0.06) - last                
          choice for - (w=0.06/v=0.12) 0.01 | -0.01 (w=-0.19/v=0.07) - name                
            wait for - (w=0.06/v=0.09) 0.01 | -0.01 (w=-0.24/v=0.05) - many                
                  my - (w=0.10/v=0.05) 0.01 | -0.01 (w=-0.12/v=0.10) - breath              
               to sa - (w=0.03/v=0.16) 0.00 | -0.01 (w=-0.21/v=0.06) - for                 
             the way - (w=0.03/v=0.07) 0.00 | -0.01 (w=-0.12/v=0.09) - poll                
             base on - (w=0.02/v=0.08) 0.00 | -0.01 (w=-0.19/v=0.05) - us                  
                come - (w=0.03/v=0.05) 0.00 | -0.01 (w=-0.11/v=0.08) - parent              
           be relate - (w=0.01/v=0.12) 0.00 | -0.01 (w=-0.15/v=0.06) - vote                
                base - (w=0.02/v=0.07) 0.00 | -0.01 (w=-0.06/v=0.15) - breath wait         
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.22/v=0.03) - on                  
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.11/v=0.06) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.12/v=0.05) - our                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.04/v=0.17) - poll is             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.04/v=0.15) - sa you              
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.03/v=0.17) - hair color          
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.13/v=0.04) - or                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.17) - on hair             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.10) - color               
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.04/v=0.06) - will not            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.12) - hold my             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.16) - the rebellion       
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.10) - for the             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.05/v=0.05) - out                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.10/v=0.02) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.08) - forget              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.03) - that                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.04) - much                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.04) - who                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.04) - will                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.06) - too                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.05) - of                  


Category : N-Tox. Ass. Stopword in Bigrams
Category2: 
Words    : ['who you, relate to, is this']

------------------------------------------------------------------

Merkel be a idoit . Vote for term limit then I will listen . 
True label: 1

-                                   ---- Result: 0.06 ----
                          sum toxic : 0.40  |  -0.34 : sum non-toxic                      

              merkel - (w=0.66/v=0.29) 0.19 | -0.15 (w=-0.42/v=0.35) - merkel be           
            vote for - (w=0.51/v=0.19) 0.09 | -0.05 (w=-0.27/v=0.18) - term                
          limit then - (w=0.12/v=0.40) 0.05 | -0.03 (w=-0.10/v=0.31) - then will           
                  be - (w=0.76/v=0.05) 0.04 | -0.03 (w=-0.09/v=0.31) - term limit          
                then - (w=0.17/v=0.13) 0.02 | -0.02 (w=-0.15/v=0.15) - vote                
            for term - (w=0.03/v=0.35) 0.01 | -0.02 (w=-0.09/v=0.20) - limit               
                     - (w=0.00/v=0.00) 0.00 | -0.02 (w=-0.21/v=0.07) - for                 
                     - (w=0.00/v=0.00) 0.00 | -0.02 (w=-0.05/v=0.34) - will listen         
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.05/v=0.21) - listen              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.10) - will                


Category : tox. Word misspelled
Category2: 
Words    : ['idoit']

------------------------------------------------------------------

You donkey I be face the same choice too . Everyone face this choice before they actually make the choice . And people who buy year ago be well off . Or year ago . Or just about anytime . What a donkey . lol . 
True label: 1

-                                   ---- Result: 0.75 ----
                          sum toxic : 1.40  |  -0.64 : sum non-toxic                      

              donkey - (w=2.80/v=0.28) 0.77 | -0.11 (w=-0.42/v=0.26) - choice              
                face - (w=0.59/v=0.16) 0.10 | -0.06 (w=-0.34/v=0.17) - year ago            
         what donkey - (w=0.38/v=0.19) 0.07 | -0.04 (w=-0.27/v=0.16) - this choice         
             or just - (w=0.46/v=0.11) 0.05 | -0.04 (w=-0.40/v=0.11) - year                
       about anytime - (w=0.25/v=0.19) 0.05 | -0.03 (w=-0.26/v=0.13) - they actually       
             who buy - (w=0.33/v=0.13) 0.04 | -0.03 (w=-0.30/v=0.11) - before they         
                  be - (w=0.76/v=0.04) 0.03 | -0.03 (w=-0.46/v=0.07) - actually            
       choice before - (w=0.17/v=0.18) 0.03 | -0.03 (w=-0.16/v=0.19) - donkey be           
            everyone - (w=0.35/v=0.08) 0.03 | -0.03 (w=-0.26/v=0.12) - well off            
            face the - (w=0.21/v=0.12) 0.03 | -0.02 (w=-0.20/v=0.12) - just about          
                 off - (w=0.39/v=0.07) 0.03 | -0.02 (w=-0.24/v=0.09) - make the            
                this - (w=0.69/v=0.04) 0.02 | -0.02 (w=-0.10/v=0.19) - too everyone        
         same choice - (w=0.14/v=0.18) 0.02 | -0.02 (w=-0.15/v=0.12) - the choice          
                 you - (w=0.65/v=0.03) 0.02 | -0.02 (w=-0.14/v=0.13) - choice and          
              ago or - (w=0.09/v=0.16) 0.01 | -0.02 (w=-0.10/v=0.17) - buy year            
          and people - (w=0.13/v=0.11) 0.01 | -0.01 (w=-0.19/v=0.08) - buy                 
                just - (w=0.24/v=0.05) 0.01 | -0.01 (w=-0.06/v=0.20) - you donkey          
                what - (w=0.26/v=0.04) 0.01 | -0.01 (w=-0.13/v=0.09) - or                  
             be well - (w=0.09/v=0.09) 0.01 | -0.01 (w=-0.08/v=0.15) - off or              
              people - (w=0.17/v=0.05) 0.01 | -0.01 (w=-0.19/v=0.05) - about               
                 and - (w=0.29/v=0.03) 0.01 | -0.01 (w=-0.05/v=0.15) - face this           
                they - (w=0.14/v=0.04) 0.01 | -0.01 (w=-0.06/v=0.12) - be face             
              before - (w=0.08/v=0.07) 0.01 | -0.01 (w=-0.10/v=0.07) - the same            
                 lol - (w=0.06/v=0.09) 0.01 | -0.01 (w=-0.12/v=0.06) - well                
       actually make - (w=0.04/v=0.13) 0.01 | -0.01 (w=-0.13/v=0.05) - make                
                same - (w=0.06/v=0.06) 0.00 | -0.01 (w=-0.05/v=0.12) - anytime             
             or year - (w=0.01/v=0.12) 0.00 | -0.01 (w=-0.06/v=0.08) - people who          
       everyone face - (w=0.00/v=0.20) 0.00 | -0.00 (w=-0.11/v=0.04) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.13) - ago be              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.16) - ago                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.18) - choice too          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.05) - who                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.06) - too                 


Category : Lack of tox. Words
Category2: 
Words    : ['donkey']

------------------------------------------------------------------

In day Trump be hold a rally in Florida to soothe his ego perhaps he can find a sucker in the crowd . 
True label: 1

-                                   ---- Result: 0.09 ----
                          sum toxic : 0.91  |  -0.81 : sum non-toxic                      

              sucker - (w=2.29/v=0.18) 0.40 | -0.15 (w=-0.66/v=0.23) - soothe              
            trump be - (w=1.41/v=0.12) 0.16 | -0.09 (w=-0.46/v=0.19) - the crowd           
               trump - (w=0.90/v=0.08) 0.07 | -0.07 (w=-0.56/v=0.13) - in                  
                 his - (w=0.88/v=0.08) 0.07 | -0.07 (w=-0.46/v=0.15) - be hold             
                  he - (w=0.49/v=0.07) 0.04 | -0.06 (w=-0.27/v=0.22) - rally in            
           sucker in - (w=0.14/v=0.25) 0.03 | -0.06 (w=-0.49/v=0.12) - perhaps             
                 ego - (w=0.16/v=0.17) 0.03 | -0.05 (w=-0.50/v=0.10) - find                
                  be - (w=0.76/v=0.03) 0.02 | -0.04 (w=-0.35/v=0.11) - hold                
            can find - (w=0.14/v=0.16) 0.02 | -0.03 (w=-0.14/v=0.23) - day trump           
          hold rally - (w=0.08/v=0.25) 0.02 | -0.03 (w=-0.46/v=0.06) - can                 
          perhaps he - (w=0.10/v=0.20) 0.02 | -0.03 (w=-0.27/v=0.10) - day                 
              he can - (w=0.07/v=0.13) 0.01 | -0.03 (w=-0.10/v=0.26) - to soothe           
              in the - (w=0.09/v=0.07) 0.01 | -0.03 (w=-0.09/v=0.28) - soothe his          
             his ego - (w=0.01/v=0.21) 0.00 | -0.02 (w=-0.13/v=0.19) - in florida          
          florida to - (w=0.01/v=0.25) 0.00 | -0.02 (w=-0.12/v=0.17) - florida             
                     - (w=0.00/v=0.00) 0.00 | -0.02 (w=-0.13/v=0.15) - crowd               
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.05/v=0.19) - in day              
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.04/v=0.16) - rally               
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.10/v=0.04) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.11/v=0.03) - the                 


Category : Lack of tox. Words
Category2: 
Words    : ['sucker']

------------------------------------------------------------------

Columnists like her be part of the reason people be not read the Globe . or the Star for that mater It s like GM year ago . make million of car car nobody want to buy because they be such poor quality . She be talk about use laugh wit and homour to skewer Trump but be not the little bite funny while do it . People vote with their money and they be not support this moribund drivel . 
True label: 1

-                                   ---- Result: 0.25 ----
                          sum toxic : 1.20  |  -0.96 : sum non-toxic                      

              drivel - (w=1.17/v=0.10) 0.12 | -0.09 (w=-0.75/v=0.12) - be not              
             be such - (w=1.23/v=0.09) 0.11 | -0.07 (w=-0.90/v=0.08) - be talk             
                  be - (w=0.76/v=0.10) 0.08 | -0.06 (w=-0.48/v=0.13) - car                 
                 wit - (w=0.70/v=0.11) 0.07 | -0.06 (w=-0.70/v=0.08) - star                
         their money - (w=0.62/v=0.09) 0.06 | -0.05 (w=-0.50/v=0.11) - while do            
            like her - (w=0.36/v=0.11) 0.04 | -0.05 (w=-0.71/v=0.07) - not                 
               trump - (w=0.90/v=0.04) 0.04 | -0.04 (w=-0.38/v=0.11) - her be              
                 she - (w=0.69/v=0.06) 0.04 | -0.03 (w=-0.22/v=0.12) - it people           
           about use - (w=0.28/v=0.12) 0.03 | -0.02 (w=-0.34/v=0.07) - year ago            
           money and - (w=0.38/v=0.08) 0.03 | -0.02 (w=-0.39/v=0.06) - reason              
                such - (w=0.53/v=0.06) 0.03 | -0.02 (w=-0.25/v=0.08) - the globe           
                 her - (w=0.52/v=0.06) 0.03 | -0.02 (w=-0.18/v=0.11) - vote with           
              she be - (w=0.41/v=0.07) 0.03 | -0.02 (w=-0.39/v=0.05) - money               
              little - (w=0.59/v=0.05) 0.03 | -0.02 (w=-0.24/v=0.08) - be part             
             they be - (w=0.28/v=0.10) 0.03 | -0.02 (w=-0.29/v=0.06) - not the             
                poor - (w=0.41/v=0.07) 0.03 | -0.02 (w=-0.40/v=0.04) - year                
                like - (w=0.34/v=0.08) 0.03 | -0.02 (w=-0.16/v=0.10) - little bite         
                  gm - (w=0.26/v=0.10) 0.03 | -0.02 (w=-0.57/v=0.03) - do                  
              to buy - (w=0.31/v=0.08) 0.02 | -0.02 (w=-0.26/v=0.06) - part                
               mater - (w=0.18/v=0.13) 0.02 | -0.01 (w=-0.17/v=0.08) - million of          
           people be - (w=0.31/v=0.07) 0.02 | -0.01 (w=-0.25/v=0.06) - support             
            the star - (w=0.21/v=0.10) 0.02 | -0.01 (w=-0.13/v=0.11) - of car              
              skewer - (w=0.15/v=0.13) 0.02 | -0.01 (w=-0.38/v=0.04) - of the              
                this - (w=0.69/v=0.03) 0.02 | -0.01 (w=-0.17/v=0.08) - the reason          
         nobody want - (w=0.17/v=0.11) 0.02 | -0.01 (w=-0.09/v=0.14) - wit and             
               while - (w=0.33/v=0.06) 0.02 | -0.01 (w=-0.12/v=0.10) - trump but           
               their - (w=0.44/v=0.04) 0.02 | -0.01 (w=-0.35/v=0.03) - but                 
        support this - (w=0.14/v=0.10) 0.01 | -0.01 (w=-0.19/v=0.06) - buy                 
              people - (w=0.17/v=0.08) 0.01 | -0.01 (w=-0.15/v=0.08) - with their          
        make million - (w=0.11/v=0.12) 0.01 | -0.01 (w=-0.16/v=0.07) - funny               
          the little - (w=0.15/v=0.08) 0.01 | -0.01 (w=-0.06/v=0.15) - ago make            
                 and - (w=0.29/v=0.04) 0.01 | -0.01 (w=-0.12/v=0.07) - read the            
                bite - (w=0.15/v=0.07) 0.01 | -0.01 (w=-0.15/v=0.05) - vote                
            not read - (w=0.11/v=0.09) 0.01 | -0.01 (w=-0.09/v=0.09) - not support         
                they - (w=0.14/v=0.07) 0.01 | -0.01 (w=-0.06/v=0.13) - star for            
               laugh - (w=0.12/v=0.08) 0.01 | -0.01 (w=-0.14/v=0.06) - it                  
             it like - (w=0.09/v=0.10) 0.01 | -0.01 (w=-0.06/v=0.13) - such poor           
                 use - (w=0.15/v=0.05) 0.01 | -0.01 (w=-0.19/v=0.04) - about               
             because - (w=0.14/v=0.05) 0.01 | -0.01 (w=-0.11/v=0.07) - the                 
                talk - (w=0.10/v=0.06) 0.01 | -0.01 (w=-0.05/v=0.15) - buy because         
                want - (w=0.11/v=0.05) 0.00 | -0.01 (w=-0.04/v=0.15) - car car             
             million - (w=0.07/v=0.07) 0.00 | -0.01 (w=-0.08/v=0.07) - or the              
        because they - (w=0.07/v=0.07) 0.00 | -0.01 (w=-0.21/v=0.03) - for                 
              but be - (w=0.05/v=0.08) 0.00 | -0.01 (w=-0.04/v=0.12) - reason people       
        poor quality - (w=0.03/v=0.13) 0.00 | -0.01 (w=-0.13/v=0.04) - make                
                with - (w=0.12/v=0.03) 0.00 | -0.01 (w=-0.07/v=0.07) - do it               
              nobody - (w=0.05/v=0.08) 0.00 | -0.00 (w=-0.13/v=0.04) - or                  
             quality - (w=0.04/v=0.08) 0.00 | -0.00 (w=-0.03/v=0.16) - funny while         
            for that - (w=0.03/v=0.07) 0.00 | -0.00 (w=-0.08/v=0.05) - want to             
             gm year - (w=0.00/v=0.16) 0.00 | -0.00 (w=-0.07/v=0.06) - read                
             like gm - (w=0.00/v=0.16) 0.00 | -0.00 (w=-0.10/v=0.04) - to                  
          that mater - (w=0.00/v=0.16) 0.00 | -0.00 (w=-0.03/v=0.14) - moribund            
          bite funny - (w=0.00/v=0.16) 0.00 | -0.00 (w=-0.05/v=0.07) - and they            
           to skewer - (w=0.00/v=0.16) 0.00 | -0.00 (w=-0.02/v=0.16) - columnists          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.02) - that                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.08) - globe               
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.06) - ago                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.07) - talk about          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.10) - people vote         
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.16) - use laugh           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.14) - globe or            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.06) - part of             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.04) - of                  


Category : Lack of tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

How childish of Rickie boy . Some see the American flag display in a inappropriate manner and simply let the other person know . And what doe Rickie boy do . Responds with something say the equivalent of at little I have . How moronic . 
True label: 1

-                                   ---- Result: 0.97 ----
                          sum toxic : 1.46  |  -0.49 : sum non-toxic                      

             moronic - (w=5.92/v=0.13) 0.74 | -0.05 (w=-0.43/v=0.13) - the equivalent      
            childish - (w=1.06/v=0.12) 0.12 | -0.05 (w=-0.35/v=0.14) - manner and          
                 boy - (w=0.45/v=0.18) 0.08 | -0.03 (w=-0.46/v=0.07) - at little           
        how childish - (w=0.30/v=0.18) 0.05 | -0.03 (w=-0.27/v=0.13) - equivalent of       
         how moronic - (w=0.25/v=0.19) 0.05 | -0.03 (w=-0.22/v=0.15) - boy do              
              person - (w=0.53/v=0.07) 0.04 | -0.03 (w=-0.27/v=0.11) - have how            
              little - (w=0.59/v=0.06) 0.03 | -0.02 (w=-0.19/v=0.13) - other person        
    in inappropriate - (w=0.16/v=0.18) 0.03 | -0.02 (w=-0.22/v=0.10) - what doe            
       something say - (w=0.16/v=0.16) 0.03 | -0.02 (w=-0.11/v=0.19) - flag display        
            know and - (w=0.21/v=0.11) 0.02 | -0.02 (w=-0.11/v=0.17) - simply let          
             display - (w=0.22/v=0.11) 0.02 | -0.02 (w=-0.17/v=0.11) - equivalent          
                 let - (w=0.35/v=0.06) 0.02 | -0.02 (w=-0.57/v=0.03) - do                  
       inappropriate - (w=0.18/v=0.12) 0.02 | -0.02 (w=-0.56/v=0.03) - in                  
             see the - (w=0.25/v=0.08) 0.02 | -0.02 (w=-0.24/v=0.07) - something           
               of at - (w=0.14/v=0.13) 0.02 | -0.01 (w=-0.15/v=0.10) - flag                
      with something - (w=0.14/v=0.13) 0.02 | -0.01 (w=-0.20/v=0.05) - see                 
              simply - (w=0.23/v=0.08) 0.02 | -0.01 (w=-0.12/v=0.09) - the american        
            american - (w=0.24/v=0.07) 0.02 | -0.01 (w=-0.03/v=0.38) - rickie              
             let the - (w=0.16/v=0.10) 0.02 | -0.01 (w=-0.16/v=0.06) - doe                 
                 and - (w=0.29/v=0.05) 0.01 | -0.01 (w=-0.17/v=0.05) - other               
            some see - (w=0.09/v=0.16) 0.01 | -0.01 (w=-0.11/v=0.06) - the                 
                some - (w=0.24/v=0.05) 0.01 | -0.01 (w=-0.05/v=0.13) - and simply          
                 how - (w=0.12/v=0.10) 0.01 | -0.01 (w=-0.05/v=0.10) - manner              
                what - (w=0.26/v=0.04) 0.01 | -0.01 (w=-0.04/v=0.14) - american flag       
             say the - (w=0.12/v=0.09) 0.01 | -0.00 (w=-0.04/v=0.13) - little have         
         childish of - (w=0.05/v=0.19) 0.01 | -0.00 (w=-0.15/v=0.03) - have                
                with - (w=0.12/v=0.04) 0.00 | -0.00 (w=-0.06/v=0.05) - say                 
                know - (w=0.08/v=0.05) 0.00 | -0.00 (w=-0.06/v=0.04) - at                  
           the other - (w=0.02/v=0.07) 0.00 | -0.00 (w=-0.01/v=0.09) - and what            
         person know - (w=0.01/v=0.15) 0.00 | -0.00 (w=-0.01/v=0.15) - display in          
inappropriate manner - (w=0.00/v=0.19) 0.00 | -0.00 (w=-0.00/v=0.05) - of                  
            boy some - (w=0.00/v=0.19) 0.00 | 0.00 (w=0.00/v=0.00) -                     
            responds - (w=0.00/v=0.19) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : N-Tox. Ass. Neutral words
Category2: 
Words    : ['the equivalent, manner and']

------------------------------------------------------------------

Trump need to step aside . His treason and incompetence combine with his mental derangement make him a danger to America . 
True label: 1


-                                   ---- Result: 0.99 ----
                          sum toxic : 1.39  |  -0.39 : sum non-toxic                      

              mental - (w=2.45/v=0.16) 0.40 | -0.09 (w=-0.54/v=0.17) - aside               
                 his - (w=0.88/v=0.16) 0.14 | -0.08 (w=-0.57/v=0.14) - step                
          trump need - (w=0.51/v=0.21) 0.11 | -0.08 (w=-0.30/v=0.26) - his mental          
    and incompetence - (w=0.41/v=0.23) 0.10 | -0.07 (w=-0.31/v=0.24) - treason and         
          step aside - (w=0.38/v=0.24) 0.09 | -0.03 (w=-0.17/v=0.18) - make him            
          to america - (w=0.45/v=0.20) 0.09 | -0.01 (w=-0.09/v=0.15) - with his            
        incompetence - (w=0.42/v=0.18) 0.08 | -0.01 (w=-0.13/v=0.08) - make                
               trump - (w=0.90/v=0.08) 0.07 | -0.01 (w=-0.10/v=0.08) - to                  
                 him - (w=0.72/v=0.10) 0.07 | -0.01 (w=-0.03/v=0.21) - danger to           
         derangement - (w=0.23/v=0.23) 0.05 | -0.00 (w=-0.02/v=0.17) - combine             
             to step - (w=0.24/v=0.19) 0.04 | -0.00 (w=-0.00/v=0.28) - his treason         
           aside his - (w=0.14/v=0.28) 0.04 | -0.00 (w=-0.00/v=0.21) - combine with        
             treason - (w=0.15/v=0.19) 0.03 | 0.00 (w=0.00/v=0.00) -                     
             america - (w=0.19/v=0.12) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                need - (w=0.19/v=0.09) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                 and - (w=0.29/v=0.04) 0.01 | 0.00 (w=0.00/v=0.00) -                     
              danger - (w=0.05/v=0.17) 0.01 | 0.00 (w=0.00/v=0.00) -                     
                with - (w=0.12/v=0.06) 0.01 | 0.00 (w=0.00/v=0.00) -                     
             need to - (w=0.04/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : Failed to learn tox. Words
Category2: N-Tox. Ass. Neutral words
Words    : ['treason, mental derangement | step, aside']

------------------------------------------------------------------

hmm and Trump be try to depopulate the USA by try to cut off people health care and their right and freedom and leave them to die off so he can try to hold onto power . Grab Trumpy by the ballz and twist them real hard . 
True label: 1

-                                   ---- Result: 0.50 ----
                          sum toxic : 1.25  |  -0.76 : sum non-toxic                      

                grab - (w=1.44/v=0.10) 0.15 | -0.06 (w=-0.72/v=0.09) - be try              
                 die - (w=1.60/v=0.09) 0.14 | -0.06 (w=-0.38/v=0.17) - usa by              
            trump be - (w=1.41/v=0.08) 0.11 | -0.06 (w=-0.39/v=0.15) - power grab          
          off people - (w=0.61/v=0.16) 0.09 | -0.06 (w=-0.44/v=0.13) - and freedom         
               twist - (w=0.81/v=0.11) 0.09 | -0.05 (w=-0.41/v=0.13) - leave them          
           and twist - (w=0.51/v=0.15) 0.08 | -0.05 (w=-0.48/v=0.11) - to hold             
                them - (w=0.59/v=0.11) 0.06 | -0.04 (w=-0.24/v=0.15) - off so              
              to die - (w=0.51/v=0.12) 0.06 | -0.03 (w=-0.31/v=0.11) - to cut              
                 off - (w=0.39/v=0.13) 0.05 | -0.03 (w=-0.29/v=0.11) - care and            
               trump - (w=0.90/v=0.05) 0.05 | -0.03 (w=-0.35/v=0.09) - by                  
             die off - (w=0.25/v=0.15) 0.04 | -0.03 (w=-0.30/v=0.11) - so he               
                 and - (w=0.29/v=0.12) 0.04 | -0.03 (w=-0.30/v=0.10) - hmm                 
             the usa - (w=0.33/v=0.09) 0.03 | -0.03 (w=-0.35/v=0.08) - hold                
          twist them - (w=0.16/v=0.17) 0.03 | -0.02 (w=-0.23/v=0.11) - onto                
                 cut - (w=0.36/v=0.08) 0.03 | -0.02 (w=-0.31/v=0.07) - power               
           and their - (w=0.29/v=0.09) 0.03 | -0.02 (w=-0.46/v=0.04) - can                 
         their right - (w=0.22/v=0.11) 0.03 | -0.02 (w=-0.20/v=0.07) - hard                
              try to - (w=0.11/v=0.21) 0.02 | -0.01 (w=-0.15/v=0.09) - usa                 
                  he - (w=0.49/v=0.05) 0.02 | -0.01 (w=-0.11/v=0.10) - and trump           
               their - (w=0.44/v=0.05) 0.02 | -0.01 (w=-0.09/v=0.12) - cut off             
           and leave - (w=0.18/v=0.11) 0.02 | -0.01 (w=-0.10/v=0.10) - right and           
                real - (w=0.24/v=0.07) 0.02 | -0.01 (w=-0.18/v=0.06) - right               
                  be - (w=0.76/v=0.02) 0.02 | -0.01 (w=-0.10/v=0.09) - to                  
           hold onto - (w=0.06/v=0.14) 0.01 | -0.01 (w=-0.06/v=0.14) - by try              
              people - (w=0.17/v=0.05) 0.01 | -0.01 (w=-0.05/v=0.13) - can try             
              trumpy - (w=0.05/v=0.15) 0.01 | -0.01 (w=-0.06/v=0.09) - health care         
              he can - (w=0.07/v=0.09) 0.01 | -0.00 (w=-0.04/v=0.12) - freedom and         
                care - (w=0.08/v=0.07) 0.01 | -0.00 (w=-0.11/v=0.04) - the                 
              by the - (w=0.06/v=0.06) 0.00 | -0.00 (w=-0.02/v=0.18) - people health       
               leave - (w=0.05/v=0.06) 0.00 | -0.00 (w=-0.06/v=0.04) - so                  
                 try - (w=0.01/v=0.19) 0.00 | -0.00 (w=-0.02/v=0.09) - freedom             
              health - (w=0.03/v=0.08) 0.00 | -0.00 (w=-0.01/v=0.15) - real hard           
      depopulate the - (w=0.00/v=0.19) 0.00 | -0.00 (w=-0.00/v=0.17) - hmm and             
          depopulate - (w=0.00/v=0.18) 0.00 | -0.00 (w=-0.01/v=0.09) - them to             
           them real - (w=0.00/v=0.18) 0.00 | -0.00 (w=-0.00/v=0.18) - onto power          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.19) - to depopulate       


Category : Lack of tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

And Kellyanne Conway be his 1st starter every time . The press should just shut her down ignore her lie mouth . 
True label: 1

-                                   ---- Result: 0.91 ----
                          sum toxic : 1.49  |  -0.57 : sum non-toxic                      

                shut - (w=1.60/v=0.13) 0.21 | -0.13 (w=-0.86/v=0.15) - be his              
               mouth - (w=1.43/v=0.14) 0.20 | -0.13 (w=-0.63/v=0.20) - just shut           
           lie mouth - (w=0.67/v=0.25) 0.17 | -0.10 (w=-0.46/v=0.22) - her lie             
                 lie - (w=1.20/v=0.11) 0.13 | -0.07 (w=-0.46/v=0.16) - 1st                 
            her down - (w=0.54/v=0.23) 0.12 | -0.05 (w=-0.40/v=0.13) - press               
                 her - (w=0.52/v=0.19) 0.10 | -0.03 (w=-0.21/v=0.14) - time the            
          ignore her - (w=0.35/v=0.23) 0.08 | -0.02 (w=-0.13/v=0.17) - starter             
    kellyanne conway - (w=0.37/v=0.21) 0.08 | -0.02 (w=-0.23/v=0.07) - time                
                 his - (w=0.88/v=0.07) 0.06 | -0.01 (w=-0.06/v=0.25) - press should        
           conway be - (w=0.24/v=0.23) 0.06 | -0.01 (w=-0.02/v=0.26) - and kellyanne       
            shut her - (w=0.19/v=0.25) 0.05 | -0.00 (w=-0.11/v=0.03) - the                 
              conway - (w=0.25/v=0.19) 0.05 | -0.00 (w=-0.00/v=0.23) - his 1st             
              ignore - (w=0.30/v=0.12) 0.04 | 0.00 (w=0.00/v=0.00) -                     
              should - (w=0.35/v=0.07) 0.03 | 0.00 (w=0.00/v=0.00) -                     
               every - (w=0.24/v=0.09) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                  be - (w=0.76/v=0.03) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                down - (w=0.22/v=0.09) 0.02 | 0.00 (w=0.00/v=0.00) -                     
           kellyanne - (w=0.09/v=0.20) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                just - (w=0.24/v=0.07) 0.02 | 0.00 (w=0.00/v=0.00) -                     
           the press - (w=0.07/v=0.15) 0.01 | 0.00 (w=0.00/v=0.00) -                     
                 and - (w=0.29/v=0.04) 0.01 | 0.00 (w=0.00/v=0.00) -                     
         should just - (w=0.01/v=0.17) 0.00 | 0.00 (w=0.00/v=0.00) -                     
          every time - (w=0.01/v=0.15) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : N-Tox. Ass. Stopword in Bigrams
Category2: 
Words    : ['be his, just shut, her lie']

------------------------------------------------------------------

Stupidiity become you nicely . 
True label: 1

-                                   ---- Result: 0.31 ----
                          sum toxic : 0.33  |  -0.02 : sum non-toxic                      

          become you - (w=0.23/v=0.57) 0.13 | -0.02 (w=-0.07/v=0.25) - become              
              nicely - (w=0.30/v=0.42) 0.13 | -0.00 (w=-0.01/v=0.65) - you nicely          
                 you - (w=0.65/v=0.11) 0.07 | 0.00 (w=0.00/v=0.00) -                     


Category : tox. Word misspelled
Category2: 
Words    : ['Stupidiity ']

------------------------------------------------------------------

She need to close both nostril and shut her mouth very relax for us . 
True label: 1

-                                   ---- Result: 0.47 ----
                          sum toxic : 0.89  |  -0.43 : sum non-toxic                      

                shut - (w=1.60/v=0.15) 0.24 | -0.15 (w=-0.79/v=0.19) - relax               
               mouth - (w=1.43/v=0.15) 0.22 | -0.11 (w=-0.47/v=0.24) - her mouth           
        both nostril - (w=0.30/v=0.29) 0.09 | -0.06 (w=-0.28/v=0.21) - she need            
                 she - (w=0.69/v=0.11) 0.07 | -0.05 (w=-0.38/v=0.12) - close               
                 her - (w=0.52/v=0.11) 0.06 | -0.02 (w=-0.13/v=0.19) - to close            
             nostril - (w=0.20/v=0.27) 0.06 | -0.02 (w=-0.19/v=0.09) - us                  
            shut her - (w=0.19/v=0.28) 0.05 | -0.01 (w=-0.21/v=0.05) - for                 
              for us - (w=0.30/v=0.16) 0.05 | -0.00 (w=-0.10/v=0.04) - to                  
            and shut - (w=0.08/v=0.21) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                need - (w=0.19/v=0.09) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                 and - (w=0.29/v=0.04) 0.01 | 0.00 (w=0.00/v=0.00) -                     
                very - (w=0.11/v=0.10) 0.01 | 0.00 (w=0.00/v=0.00) -                     
             need to - (w=0.04/v=0.10) 0.00 | 0.00 (w=0.00/v=0.00) -                     
                both - (w=0.01/v=0.11) 0.00 | 0.00 (w=0.00/v=0.00) -                     
         nostril and - (w=0.00/v=0.31) 0.00 | 0.00 (w=0.00/v=0.00) -                     
           relax for - (w=0.00/v=0.31) 0.00 | 0.00 (w=0.00/v=0.00) -                     
          very relax - (w=0.00/v=0.31) 0.00 | 0.00 (w=0.00/v=0.00) -                     
          close both - (w=0.00/v=0.30) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : Lack of tox. Words
Category2: Trigram needed
Words    : 

------------------------------------------------------------------

Trump be also correct when he say his inauguration crowd be the large . And that Ted Cruz have father be responsible for Kennedy have assassination . A person who support a liar be . 
True label: 1

-                                   ---- Result: 0.30 ----
                          sum toxic : 1.44  |  -1.14 : sum non-toxic                      

                liar - (w=6.40/v=0.12) 0.74 | -0.23 (w=-1.22/v=0.19) - liar be             
            trump be - (w=1.41/v=0.09) 0.13 | -0.08 (w=-0.48/v=0.17) - say his             
                  be - (w=0.76/v=0.10) 0.08 | -0.08 (w=-0.42/v=0.18) - his inauguration    
         have father - (w=0.37/v=0.17) 0.06 | -0.08 (w=-0.36/v=0.21) - support liar        
               trump - (w=0.90/v=0.06) 0.06 | -0.06 (w=-0.56/v=0.11) - be also             
                 his - (w=0.88/v=0.06) 0.06 | -0.06 (w=-0.45/v=0.13) - be responsible      
              person - (w=0.53/v=0.09) 0.05 | -0.05 (w=-0.31/v=0.15) - inauguration        
              father - (w=0.35/v=0.12) 0.04 | -0.05 (w=-0.46/v=0.10) - correct             
        correct when - (w=0.20/v=0.19) 0.04 | -0.04 (w=-0.46/v=0.10) - large               
           cruz have - (w=0.18/v=0.19) 0.03 | -0.04 (w=-0.40/v=0.11) - responsible         
           father be - (w=0.20/v=0.16) 0.03 | -0.04 (w=-0.23/v=0.17) - ted cruz            
                  he - (w=0.49/v=0.06) 0.03 | -0.04 (w=-0.20/v=0.20) - inauguration crowd  
            that ted - (w=0.11/v=0.21) 0.02 | -0.04 (w=-0.32/v=0.11) - when he             
     responsible for - (w=0.17/v=0.12) 0.02 | -0.03 (w=-0.16/v=0.19) - kennedy have        
             kennedy - (w=0.08/v=0.15) 0.01 | -0.03 (w=-0.21/v=0.14) - ted                 
                 and - (w=0.29/v=0.03) 0.01 | -0.03 (w=-0.25/v=0.12) - he say              
                cruz - (w=0.05/v=0.15) 0.01 | -0.03 (w=-0.16/v=0.16) - assassination       
            and that - (w=0.07/v=0.09) 0.01 | -0.02 (w=-0.17/v=0.13) - the large           
         who support - (w=0.04/v=0.15) 0.01 | -0.02 (w=-0.25/v=0.08) - support             
  have assassination - (w=0.02/v=0.21) 0.00 | -0.02 (w=-0.34/v=0.06) - when                
              be the - (w=0.05/v=0.06) 0.00 | -0.02 (w=-0.13/v=0.12) - crowd               
            crowd be - (w=0.00/v=0.17) 0.00 | -0.01 (w=-0.11/v=0.13) - person who          
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.15/v=0.06) - have                
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.21/v=0.04) - for                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.05/v=0.16) - large and           
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.04/v=0.20) - also correct        
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.02/v=0.23) - for kennedy         
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.06) - say                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.11/v=0.03) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.08) - also                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.03) - that                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.06) - who                 


Category : N-Tox. Ass to tox. Words
Category2: N-Tox. Ass. Neutral words
Words    : ['liar be, support liar, his inauguration , say his']

------------------------------------------------------------------

The vast majority of the vote eligible population here be I*D*I*O*T*S a good . It take to tango . 
True label: 1

-                                   ---- Result: -0.32 ----
                          sum toxic : 0.28  |  -0.60 : sum non-toxic                      

             take to - (w=0.53/v=0.21) 0.11 | -0.09 (w=-0.36/v=0.24) - good it             
             here be - (w=0.47/v=0.18) 0.08 | -0.08 (w=-0.24/v=0.32) - to tango            
                vast - (w=0.18/v=0.20) 0.04 | -0.07 (w=-0.25/v=0.30) - tango               
                  be - (w=0.76/v=0.04) 0.03 | -0.05 (w=-0.23/v=0.23) - eligible            
          population - (w=0.12/v=0.16) 0.02 | -0.04 (w=-0.21/v=0.21) - vast majority       
                     - (w=0.00/v=0.00) 0.00 | -0.04 (w=-0.24/v=0.18) - majority of         
                     - (w=0.00/v=0.00) 0.00 | -0.04 (w=-0.44/v=0.10) - good                
                     - (w=0.00/v=0.00) 0.00 | -0.03 (w=-0.38/v=0.08) - of the              
                     - (w=0.00/v=0.00) 0.00 | -0.02 (w=-0.14/v=0.15) - be good             
                     - (w=0.00/v=0.00) 0.00 | -0.02 (w=-0.06/v=0.34) - population here     
                     - (w=0.00/v=0.00) 0.00 | -0.02 (w=-0.15/v=0.13) - vote                
                     - (w=0.00/v=0.00) 0.00 | -0.02 (w=-0.09/v=0.19) - it take             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.07/v=0.21) - the vast            
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.09/v=0.16) - majority            
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.03/v=0.35) - eligible population 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.14/v=0.06) - it                  
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.08/v=0.11) - take                
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.11/v=0.08) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.04/v=0.12) - here                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.10/v=0.05) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.20) - the vote            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.05) - of                  


Category : tox. Word misspelled
Category2: 
Words    : ['I*D*I*O*T']

------------------------------------------------------------------

McCraken be a fake doctor a specialist in diversity medcine . 
True label: 1

-                                   ---- Result: 0.08 ----
                          sum toxic : 0.39  |  -0.31 : sum non-toxic                      

             be fake - (w=0.59/v=0.27) 0.16 | -0.17 (w=-0.66/v=0.25) - diversity           
         fake doctor - (w=0.25/v=0.42) 0.11 | -0.05 (w=-0.18/v=0.29) - specialist          
                fake - (w=0.45/v=0.20) 0.09 | -0.04 (w=-0.56/v=0.07) - in                  
                  be - (w=0.76/v=0.05) 0.04 | -0.03 (w=-0.09/v=0.37) - specialist in       
   doctor specialist - (w=0.00/v=0.45) 0.00 | -0.02 (w=-0.08/v=0.21) - doctor              
        in diversity - (w=0.00/v=0.42) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : No tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

hate be a belief if he call for violence or such fine charge him with incite but until then he have the right to speak regardless of how much a fool he be . the leave doe not get to decide . 
True label: 1

-                                   ---- Result: 0.85 ----
                          sum toxic : 1.84  |  -0.99 : sum non-toxic                      

                fool - (w=10.08/v=0.11) 1.07 | -0.20 (w=-1.51/v=0.13) - to decide           
             fool he - (w=0.73/v=0.18) 0.13 | -0.08 (w=-0.55/v=0.15) - he call             
                  he - (w=0.49/v=0.17) 0.08 | -0.06 (w=-0.33/v=0.18) - charge him          
                hate - (w=0.82/v=0.10) 0.08 | -0.05 (w=-0.46/v=0.11) - regardless of       
                 him - (w=0.72/v=0.07) 0.05 | -0.05 (w=-0.51/v=0.10) - fine                
           the leave - (w=0.40/v=0.11) 0.04 | -0.04 (w=-0.37/v=0.11) - belief              
                such - (w=0.53/v=0.08) 0.04 | -0.04 (w=-0.27/v=0.15) - incite              
                  be - (w=0.76/v=0.05) 0.04 | -0.04 (w=-0.40/v=0.10) - how much            
            violence - (w=0.32/v=0.11) 0.04 | -0.04 (w=-0.41/v=0.10) - get to              
         violence or - (w=0.16/v=0.18) 0.03 | -0.04 (w=-0.21/v=0.19) - leave doe           
        for violence - (w=0.15/v=0.18) 0.03 | -0.04 (w=-0.39/v=0.10) - decide              
              of how - (w=0.22/v=0.12) 0.03 | -0.04 (w=-0.39/v=0.09) - speak               
               he be - (w=0.32/v=0.08) 0.02 | -0.03 (w=-0.26/v=0.14) - then he             
             he have - (w=0.24/v=0.09) 0.02 | -0.03 (w=-0.22/v=0.15) - until then          
             or such - (w=0.11/v=0.19) 0.02 | -0.02 (w=-0.25/v=0.09) - charge              
             hate be - (w=0.10/v=0.17) 0.02 | -0.02 (w=-0.12/v=0.19) - such fine           
           the right - (w=0.14/v=0.09) 0.01 | -0.02 (w=-0.71/v=0.03) - not                 
               until - (w=0.14/v=0.09) 0.01 | -0.02 (w=-0.12/v=0.15) - him with            
                then - (w=0.17/v=0.07) 0.01 | -0.02 (w=-0.35/v=0.05) - but                 
            right to - (w=0.11/v=0.10) 0.01 | -0.01 (w=-0.24/v=0.05) - if                  
            have the - (w=0.15/v=0.08) 0.01 | -0.01 (w=-0.18/v=0.06) - right               
           belief if - (w=0.04/v=0.20) 0.01 | -0.01 (w=-0.16/v=0.07) - doe                 
                 how - (w=0.12/v=0.06) 0.01 | -0.01 (w=-0.21/v=0.04) - for                 
           but until - (w=0.04/v=0.17) 0.01 | -0.01 (w=-0.04/v=0.20) - be belief           
                call - (w=0.08/v=0.08) 0.01 | -0.01 (w=-0.06/v=0.12) - to speak            
                with - (w=0.12/v=0.04) 0.01 | -0.01 (w=-0.13/v=0.05) - or                  
             not get - (w=0.05/v=0.10) 0.00 | -0.01 (w=-0.06/v=0.11) - regardless          
               leave - (w=0.05/v=0.08) 0.00 | -0.01 (w=-0.10/v=0.05) - to                  
              be the - (w=0.05/v=0.06) 0.00 | -0.01 (w=-0.04/v=0.12) - call for            
               if he - (w=0.02/v=0.10) 0.00 | -0.01 (w=-0.11/v=0.05) - the                 
                 get - (w=0.03/v=0.05) 0.00 | -0.00 (w=-0.15/v=0.03) - have                
         with incite - (w=0.00/v=0.23) 0.00 | -0.00 (w=-0.05/v=0.09) - doe not             
           much fool - (w=0.00/v=0.22) 0.00 | -0.00 (w=-0.03/v=0.05) - much                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.23) - incite but          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.03) - of                  


Category : N-Tox. Ass. Neutral words
Category2: 
Words    : ['to decide']

------------------------------------------------------------------

I do not think they eat them just kill them chop them up and sell off the part . 
True label: 1

-                                   ---- Result: 0.65 ----
                          sum toxic : 1.24  |  -0.59 : sum non-toxic                      

                kill - (w=3.16/v=0.13) 0.42 | -0.08 (w=-0.31/v=0.25) - just kill           
           kill them - (w=1.50/v=0.22) 0.33 | -0.06 (w=-0.42/v=0.15) - off the             
                them - (w=0.59/v=0.27) 0.16 | -0.06 (w=-0.25/v=0.25) - they eat            
              up and - (w=0.38/v=0.15) 0.06 | -0.06 (w=-0.30/v=0.20) - and sell            
                 off - (w=0.39/v=0.11) 0.04 | -0.05 (w=-0.23/v=0.23) - them just           
              do not - (w=0.55/v=0.07) 0.04 | -0.05 (w=-0.24/v=0.22) - sell off            
          think they - (w=0.24/v=0.16) 0.04 | -0.05 (w=-0.58/v=0.09) - think               
                chop - (w=0.15/v=0.22) 0.03 | -0.04 (w=-0.28/v=0.15) - eat                 
                sell - (w=0.21/v=0.13) 0.03 | -0.03 (w=-0.71/v=0.05) - not                 
           not think - (w=0.16/v=0.14) 0.02 | -0.03 (w=-0.57/v=0.06) - do                  
                just - (w=0.24/v=0.08) 0.02 | -0.03 (w=-0.26/v=0.12) - part                
            the part - (w=0.10/v=0.18) 0.02 | -0.02 (w=-0.07/v=0.25) - eat them            
                 and - (w=0.29/v=0.04) 0.01 | -0.01 (w=-0.02/v=0.32) - chop them           
                  up - (w=0.14/v=0.08) 0.01 | -0.00 (w=-0.11/v=0.03) - the                 
                they - (w=0.14/v=0.07) 0.01 | 0.00 (w=0.00/v=0.00) -                     
             them up - (w=0.00/v=0.20) 0.00 | 0.00 (w=0.00/v=0.00) -                     
           them chop - (w=0.00/v=0.33) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : Failed to learn tox. Words
Category2: N-Tox. Ass. Neutral words
Words    : ['kill them | just kill, off the, they eat, and sell']

------------------------------------------------------------------

You really be a bundle of ignorance . Vancouver be on the Coast and -PRON- have in the South of the Province . Too difficult for you . 
True label: 1

-                                   ---- Result: 0.97 ----
                          sum toxic : 1.51  |  -0.54 : sum non-toxic                      

           ignorance - (w=3.95/v=0.17) 0.66 | -0.07 (w=-0.35/v=0.21) - the coast           
        of ignorance - (w=0.90/v=0.23) 0.21 | -0.07 (w=-0.31/v=0.23) - coast and           
           really be - (w=0.50/v=0.16) 0.08 | -0.06 (w=-0.38/v=0.16) - vancouver           
                 you - (w=0.65/v=0.11) 0.07 | -0.06 (w=-0.44/v=0.13) - be on               
       too difficult - (w=0.27/v=0.25) 0.07 | -0.06 (w=-0.37/v=0.15) - difficult           
                pron - (w=0.94/v=0.06) 0.06 | -0.03 (w=-0.19/v=0.15) - south               
        vancouver be - (w=0.26/v=0.22) 0.06 | -0.03 (w=-0.38/v=0.07) - of the              
           the south - (w=0.30/v=0.18) 0.05 | -0.03 (w=-0.56/v=0.05) - in                  
       difficult for - (w=0.26/v=0.20) 0.05 | -0.02 (w=-0.16/v=0.15) - province            
                  be - (w=0.76/v=0.07) 0.05 | -0.02 (w=-0.20/v=0.10) - really              
           bundle of - (w=0.15/v=0.27) 0.04 | -0.01 (w=-0.22/v=0.06) - on                  
        the province - (w=0.19/v=0.17) 0.03 | -0.01 (w=-0.14/v=0.09) - on the              
             for you - (w=0.23/v=0.14) 0.03 | -0.01 (w=-0.07/v=0.17) - coast               
          you really - (w=0.09/v=0.16) 0.01 | -0.01 (w=-0.21/v=0.05) - for                 
                 and - (w=0.29/v=0.04) 0.01 | -0.01 (w=-0.11/v=0.10) - the                 
              in the - (w=0.09/v=0.07) 0.01 | -0.01 (w=-0.03/v=0.29) - be bundle           
              bundle - (w=0.02/v=0.23) 0.00 | -0.01 (w=-0.08/v=0.08) - pron have           
            south of - (w=0.02/v=0.20) 0.00 | -0.01 (w=-0.15/v=0.04) - have                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.16) - have in             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.13) - and pron            
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.10) - too                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.09) - of                  


Category : N-Tox. Ass. Neutral words
Category2: N-Tox. Ass. Stopword in Bigrams
Words    : ['the coast, coast and, vancouver']

------------------------------------------------------------------

And Notley make sure her Public Sector and other union member get their money while cut back be hit all other non union Albertans just like Trudeau and Horgan Weaver in BC . Typical leave wing crook . 
True label: 1

-                                   ---- Result: 0.63 ----
                          sum toxic : 1.43  |  -0.80 : sum non-toxic                      

               crook - (w=3.45/v=0.12) 0.42 | -0.09 (w=-0.61/v=0.14) - horgan              
       typical leave - (w=0.70/v=0.17) 0.12 | -0.06 (w=-0.34/v=0.18) - her public          
           all other - (w=0.64/v=0.12) 0.08 | -0.06 (w=-0.45/v=0.13) - notley              
              weaver - (w=0.53/v=0.15) 0.08 | -0.06 (w=-0.66/v=0.09) - and other           
         their money - (w=0.62/v=0.12) 0.08 | -0.06 (w=-0.29/v=0.19) - union               
             typical - (w=0.54/v=0.11) 0.06 | -0.05 (w=-0.33/v=0.14) - cut back            
          leave wing - (w=0.44/v=0.11) 0.05 | -0.05 (w=-0.38/v=0.12) - trudeau and         
                wing - (w=0.43/v=0.10) 0.04 | -0.04 (w=-0.24/v=0.16) - money while         
                 hit - (w=0.43/v=0.10) 0.04 | -0.03 (w=-0.41/v=0.07) - sure                
                 her - (w=0.52/v=0.07) 0.04 | -0.03 (w=-0.19/v=0.15) - like trudeau        
             trudeau - (w=0.43/v=0.08) 0.04 | -0.03 (w=-0.18/v=0.15) - back be             
           make sure - (w=0.33/v=0.11) 0.04 | -0.03 (w=-0.39/v=0.07) - money               
              be hit - (w=0.24/v=0.14) 0.03 | -0.03 (w=-0.15/v=0.17) - and notley          
                 cut - (w=0.36/v=0.08) 0.03 | -0.02 (w=-0.22/v=0.09) - member              
           other non - (w=0.18/v=0.16) 0.03 | -0.02 (w=-0.20/v=0.10) - just like           
               while - (w=0.33/v=0.07) 0.02 | -0.02 (w=-0.17/v=0.11) - other               
                 and - (w=0.29/v=0.08) 0.02 | -0.02 (w=-0.11/v=0.18) - other union         
        union member - (w=0.15/v=0.15) 0.02 | -0.02 (w=-0.56/v=0.03) - in                  
               their - (w=0.44/v=0.05) 0.02 | -0.02 (w=-0.14/v=0.12) - public sector       
                 non - (w=0.24/v=0.09) 0.02 | -0.02 (w=-0.08/v=0.19) - sure her            
              sector - (w=0.20/v=0.10) 0.02 | -0.02 (w=-0.09/v=0.17) - hit all             
                  bc - (w=0.20/v=0.10) 0.02 | -0.01 (w=-0.19/v=0.07) - back                
           while cut - (w=0.11/v=0.17) 0.02 | -0.01 (w=-0.10/v=0.11) - get their           
                like - (w=0.34/v=0.05) 0.02 | -0.01 (w=-0.14/v=0.07) - public              
          member get - (w=0.09/v=0.18) 0.02 | -0.01 (w=-0.13/v=0.05) - make                
                  be - (w=0.76/v=0.02) 0.02 | -0.01 (w=-0.04/v=0.16) - non union           
                just - (w=0.24/v=0.05) 0.01 | -0.00 (w=-0.02/v=0.21) - albertans just      
                 all - (w=0.19/v=0.05) 0.01 | -0.00 (w=-0.02/v=0.19) - and horgan          
               in bc - (w=0.07/v=0.12) 0.01 | -0.00 (w=-0.02/v=0.14) - albertans           
               leave - (w=0.05/v=0.07) 0.00 | 0.00 (w=0.00/v=0.00) -                     
                 get - (w=0.03/v=0.05) 0.00 | 0.00 (w=0.00/v=0.00) -                     
          sector and - (w=0.01/v=0.15) 0.00 | 0.00 (w=0.00/v=0.00) -                     
       horgan weaver - (w=0.00/v=0.21) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : Lack of tox. Words
Category2: 
Words    : ['crook']

------------------------------------------------------------------

I think Trump have unwittingly advance the march toward a single payer here . From your lip to God have ear and through his Spirit to your heart . But damn it how long Lord . 
True label: 1

-                                   ---- Result: 1.04 ----
                          sum toxic : 1.77  |  -0.73 : sum non-toxic                      

                damn - (w=8.75/v=0.11) 0.99 | -0.07 (w=-0.47/v=0.16) - the march           
             damn it - (w=1.29/v=0.18) 0.23 | -0.06 (w=-0.36/v=0.15) - and through         
                your - (w=1.35/v=0.10) 0.14 | -0.05 (w=-0.32/v=0.15) - your heart          
           heart but - (w=0.37/v=0.18) 0.06 | -0.04 (w=-0.31/v=0.14) - think trump         
               trump - (w=0.90/v=0.06) 0.05 | -0.04 (w=-0.58/v=0.07) - long                
                 his - (w=0.88/v=0.06) 0.05 | -0.04 (w=-0.25/v=0.17) - have ear            
                 ear - (w=0.31/v=0.13) 0.04 | -0.03 (w=-0.27/v=0.12) - how long            
                 god - (w=0.39/v=0.09) 0.04 | -0.03 (w=-0.58/v=0.06) - think               
          trump have - (w=0.31/v=0.09) 0.03 | -0.03 (w=-0.31/v=0.11) - heart               
              spirit - (w=0.22/v=0.12) 0.03 | -0.03 (w=-0.40/v=0.08) - through             
            but damn - (w=0.12/v=0.20) 0.02 | -0.03 (w=-0.19/v=0.16) - ear and             
              it how - (w=0.14/v=0.15) 0.02 | -0.03 (w=-0.17/v=0.17) - your lip            
         through his - (w=0.12/v=0.15) 0.02 | -0.03 (w=-0.23/v=0.11) - advance             
           from your - (w=0.13/v=0.12) 0.02 | -0.02 (w=-0.19/v=0.13) - lord                
            god have - (w=0.07/v=0.13) 0.01 | -0.02 (w=-0.21/v=0.11) - payer               
                 and - (w=0.29/v=0.03) 0.01 | -0.02 (w=-0.11/v=0.17) - unwittingly         
                 how - (w=0.12/v=0.06) 0.01 | -0.02 (w=-0.14/v=0.13) - single payer        
              to god - (w=0.04/v=0.15) 0.01 | -0.02 (w=-0.09/v=0.19) - lip to              
         advance the - (w=0.00/v=0.16) 0.00 | -0.02 (w=-0.12/v=0.14) - lip                 
              toward - (w=0.00/v=0.12) 0.00 | -0.02 (w=-0.35/v=0.05) - but                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.10/v=0.10) - to your             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.20/v=0.05) - from                
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.05/v=0.21) - have unwittingly    
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.15/v=0.06) - have                
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.06/v=0.12) - march               
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.04/v=0.15) - here from           
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.14/v=0.04) - it                  
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.10/v=0.05) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.20) - toward single       
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.04/v=0.07) - here                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.18) - spirit to           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.11/v=0.02) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.20) - his spirit          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.10) - single              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.22) - payer here          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.20) - march toward        


Category : N-Tox. Ass. Neutral words
Category2: N-Tox. Ass. Stopword in Bigrams
Words    : ['the march, and through']

------------------------------------------------------------------

Immigration reform be long overdue . Keep the trash out . 
True label: 1

-                                   ---- Result: 0.82 ----
                          sum toxic : 1.37  |  -0.54 : sum non-toxic                      

               trash - (w=5.32/v=0.24) 1.27 | -0.10 (w=-0.44/v=0.23) - reform              
                keep - (w=0.31/v=0.15) 0.05 | -0.08 (w=-0.58/v=0.15) - long                
                  be - (w=0.76/v=0.05) 0.04 | -0.08 (w=-0.25/v=0.31) - the trash           
         immigration - (w=0.05/v=0.21) 0.01 | -0.07 (w=-0.15/v=0.44) - trash out           
                     - (w=0.00/v=0.00) 0.00 | -0.06 (w=-0.20/v=0.32) - reform be           
                     - (w=0.00/v=0.00) 0.00 | -0.05 (w=-0.23/v=0.22) - keep the            
                     - (w=0.00/v=0.00) 0.00 | -0.03 (w=-0.13/v=0.25) - be long             
                     - (w=0.00/v=0.00) 0.00 | -0.02 (w=-0.07/v=0.33) - immigration reform  
                     - (w=0.00/v=0.00) 0.00 | -0.02 (w=-0.08/v=0.29) - overdue             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.02/v=0.30) - long overdue        
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.05/v=0.11) - out                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.11/v=0.05) - the                 


Category : N-Tox. Ass. Neutral words
Category2: 
Words    : ['keep out, reform, long, the trash']

------------------------------------------------------------------

No need a lecture on disrespectful action from a Trump Traitor . be not there a Gold Star family you can be attack right now . 
True label: 1

-                                   ---- Result: 0.45 ----
                          sum toxic : 1.22  |  -0.78 : sum non-toxic                      

             traitor - (w=4.46/v=0.16) 0.72 | -0.13 (w=-0.83/v=0.16) - no need             
          traitor be - (w=0.89/v=0.25) 0.22 | -0.10 (w=-0.70/v=0.14) - star                
               trump - (w=0.90/v=0.08) 0.07 | -0.06 (w=-0.31/v=0.18) - lecture             
                  be - (w=0.76/v=0.06) 0.04 | -0.05 (w=-0.24/v=0.23) - action from         
       trump traitor - (w=0.13/v=0.27) 0.03 | -0.05 (w=-0.24/v=0.22) - family you          
                 you - (w=0.65/v=0.05) 0.03 | -0.05 (w=-0.75/v=0.07) - be not              
           be attack - (w=0.14/v=0.18) 0.02 | -0.04 (w=-0.35/v=0.12) - action              
                gold - (w=0.15/v=0.16) 0.02 | -0.04 (w=-0.17/v=0.23) - star family         
                need - (w=0.19/v=0.08) 0.02 | -0.03 (w=-0.14/v=0.24) - lecture on          
        attack right - (w=0.04/v=0.26) 0.01 | -0.03 (w=-0.17/v=0.18) - from trump          
disrespectful action - (w=0.04/v=0.27) 0.01 | -0.03 (w=-0.71/v=0.04) - not                 
           not there - (w=0.05/v=0.16) 0.01 | -0.03 (w=-0.46/v=0.06) - can                 
                 now - (w=0.08/v=0.08) 0.01 | -0.02 (w=-0.27/v=0.07) - there               
                  no - (w=0.06/v=0.07) 0.00 | -0.02 (w=-0.08/v=0.21) - gold star           
           right now - (w=0.01/v=0.14) 0.00 | -0.02 (w=-0.12/v=0.12) - attack              
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.18/v=0.08) - right               
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.20/v=0.07) - from                
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.22/v=0.05) - on                  
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.10/v=0.11) - family              
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.10/v=0.10) - you can             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.09/v=0.10) - can be              
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.05/v=0.18) - disrespectful       
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.02/v=0.28) - need lecture        


Category : Failed to learn tox. Words
Category2: 
Words    : ['traitor']

------------------------------------------------------------------

More Donald Trumps and in Canada who know well about Intelligence Spying I mean than the professional do . Your credibility suck guy . Take a deep breath . 
True label: 1

-                                   ---- Result: 0.55 ----
                          sum toxic : 1.21  |  -0.66 : sum non-toxic                      

                suck - (w=6.13/v=0.14) 0.83 | -0.11 (w=-0.53/v=0.21) - well about          
                your - (w=1.35/v=0.06) 0.09 | -0.08 (w=-0.62/v=0.13) - deep                
                more - (w=0.51/v=0.12) 0.06 | -0.06 (w=-0.41/v=0.14) - who know            
                 guy - (w=0.32/v=0.10) 0.03 | -0.05 (w=-0.36/v=0.15) - credibility         
          canada who - (w=0.15/v=0.20) 0.03 | -0.05 (w=-0.22/v=0.24) - mean than           
              trumps - (w=0.19/v=0.15) 0.03 | -0.04 (w=-0.19/v=0.23) - guy take            
     professional do - (w=0.10/v=0.25) 0.03 | -0.04 (w=-0.18/v=0.19) - the professional    
        intelligence - (w=0.16/v=0.14) 0.02 | -0.03 (w=-0.22/v=0.13) - professional        
           know well - (w=0.13/v=0.16) 0.02 | -0.03 (w=-0.57/v=0.05) - do                  
           take deep - (w=0.10/v=0.20) 0.02 | -0.02 (w=-0.11/v=0.21) - your credibility    
            than the - (w=0.12/v=0.12) 0.01 | -0.02 (w=-0.56/v=0.04) - in                  
             do your - (w=0.08/v=0.16) 0.01 | -0.02 (w=-0.12/v=0.16) - breath              
                 and - (w=0.29/v=0.03) 0.01 | -0.02 (w=-0.14/v=0.12) - in canada           
                than - (w=0.13/v=0.07) 0.01 | -0.02 (w=-0.14/v=0.12) - donald              
                know - (w=0.08/v=0.07) 0.01 | -0.02 (w=-0.06/v=0.24) - trumps and          
                mean - (w=0.02/v=0.09) 0.00 | -0.01 (w=-0.19/v=0.06) - about               
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.12/v=0.08) - well                
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.04/v=0.20) - deep breath         
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.05/v=0.13) - and in              
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.08/v=0.08) - take                
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.02/v=0.25) - about intelligence  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.05/v=0.09) - canada              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.11/v=0.03) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.24) - spying              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.06) - who                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.23) - donald trumps       


Category : N-Tox. Ass. Neutral words
Category2: 
Words    : ['well about, who know']

------------------------------------------------------------------

PLEASE JUST LET THEM DIE THERE IS NO HOPE FOR THEM . 
True label: 1

-                                   ---- Result: 0.42 ----
                          sum toxic : 0.84  |  -0.42 : sum non-toxic                      

                 die - (w=1.60/v=0.18) 0.29 | -0.14 (w=-0.59/v=0.23) - hope for            
            them die - (w=0.49/v=0.33) 0.16 | -0.07 (w=-0.19/v=0.34) - die there           
                them - (w=0.59/v=0.22) 0.13 | -0.06 (w=-0.21/v=0.27) - just let            
            let them - (w=0.30/v=0.22) 0.06 | -0.06 (w=-0.20/v=0.28) - no hope             
                  is - (w=0.40/v=0.16) 0.06 | -0.03 (w=-0.27/v=0.10) - there               
                 let - (w=0.35/v=0.13) 0.05 | -0.02 (w=-0.07/v=0.31) - please just         
            there is - (w=0.15/v=0.29) 0.04 | -0.02 (w=-0.06/v=0.32) - is no               
                just - (w=0.24/v=0.10) 0.02 | -0.02 (w=-0.09/v=0.19) - for them            
                  no - (w=0.06/v=0.09) 0.01 | -0.01 (w=-0.21/v=0.07) - for                 
              please - (w=0.01/v=0.16) 0.00 | -0.00 (w=-0.03/v=0.15) - hope                


Category : Maybe trigram
Category2: 
Words    : ['let them die']

------------------------------------------------------------------

Bullet to the head . End of story . 
True label: 1

-                                   ---- Result: 1.05 ----
                          sum toxic : 1.39  |  -0.34 : sum non-toxic                      

              bullet - (w=1.36/v=0.32) 0.44 | -0.18 (w=-0.33/v=0.53) - head end            
           bullet to - (w=0.70/v=0.45) 0.31 | -0.15 (w=-0.77/v=0.20) - story               
                head - (w=1.02/v=0.21) 0.22 | -0.01 (w=-0.10/v=0.06) - to                  
            of story - (w=0.58/v=0.34) 0.20 | -0.01 (w=-0.11/v=0.06) - the                 
              end of - (w=0.69/v=0.25) 0.18 | -0.00 (w=-0.00/v=0.07) - of                  
            the head - (w=0.11/v=0.29) 0.03 | 0.00 (w=0.00/v=0.00) -                     
                 end - (w=0.08/v=0.19) 0.02 | 0.00 (w=0.00/v=0.00) -                     
              to the - (w=0.03/v=0.14) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : N-Tox. Ass. Neutral words
Category2: 
Words    : ['bullet head, story']

------------------------------------------------------------------

That kind intrusive abusive behavior be simply hand down Creepy cop all around . 
True label: 1

-                                   ---- Result: 0.76 ----
                          sum toxic : 0.91  |  -0.15 : sum non-toxic                      

             cop all - (w=0.59/v=0.35) 0.21 | -0.05 (w=-0.35/v=0.14) - around              
             abusive - (w=0.83/v=0.24) 0.20 | -0.03 (w=-0.18/v=0.18) - behavior            
              creepy - (w=0.60/v=0.25) 0.15 | -0.03 (w=-0.17/v=0.19) - be simply           
          all around - (w=0.36/v=0.24) 0.09 | -0.02 (w=-0.07/v=0.28) - intrusive           
         behavior be - (w=0.21/v=0.25) 0.05 | -0.01 (w=-0.03/v=0.33) - simply hand         
                 cop - (w=0.22/v=0.19) 0.04 | -0.01 (w=-0.02/v=0.26) - hand down           
              simply - (w=0.23/v=0.15) 0.03 | -0.00 (w=-0.06/v=0.05) - that                
                  be - (w=0.76/v=0.04) 0.03 | 0.00 (w=0.00/v=0.00) -                     
                hand - (w=0.20/v=0.15) 0.03 | 0.00 (w=0.00/v=0.00) -                     
                down - (w=0.22/v=0.12) 0.03 | 0.00 (w=0.00/v=0.00) -                     
    abusive behavior - (w=0.07/v=0.34) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                 all - (w=0.19/v=0.08) 0.02 | 0.00 (w=0.00/v=0.00) -                     
                kind - (w=0.09/v=0.15) 0.01 | 0.00 (w=0.00/v=0.00) -                     
           that kind - (w=0.02/v=0.22) 0.00 | 0.00 (w=0.00/v=0.00) -                     


Category : Lack of tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

I be no defender of this moronic prepper enclave that be populate certain region of the west . But Randy Weaver be alive and good . His spouse and year old son be kill at Ruby Ridge and Weaver and his survive daughter be late award several dollar for the wrongful death of their mother and brother . 
True label: 1

-                                   ---- Result: 0.90 ----
                          sum toxic : 1.86  |  -0.96 : sum non-toxic                      

             moronic - (w=5.92/v=0.11) 0.64 | -0.13 (w=-1.42/v=0.09) - be kill             
                kill - (w=3.16/v=0.07) 0.21 | -0.06 (w=-0.54/v=0.11) - alive and           
              weaver - (w=0.53/v=0.23) 0.12 | -0.05 (w=-0.39/v=0.13) - kill at             
               death - (w=1.18/v=0.07) 0.09 | -0.05 (w=-0.45/v=0.10) - be alive            
                 his - (w=0.88/v=0.09) 0.08 | -0.04 (w=-0.35/v=0.11) - be late             
            daughter - (w=0.78/v=0.09) 0.07 | -0.04 (w=-0.43/v=0.09) - brother             
            defender - (w=0.63/v=0.11) 0.07 | -0.03 (w=-0.29/v=0.12) - daughter be         
                  be - (w=0.76/v=0.09) 0.06 | -0.03 (w=-0.51/v=0.07) - dollar              
               randy - (w=0.51/v=0.11) 0.06 | -0.03 (w=-0.24/v=0.14) - west but            
        this moronic - (w=0.40/v=0.15) 0.06 | -0.03 (w=-0.31/v=0.09) - award               
            and year - (w=0.55/v=0.10) 0.06 | -0.03 (w=-0.23/v=0.12) - wrongful            
                 old - (w=0.71/v=0.06) 0.04 | -0.03 (w=-0.30/v=0.08) - survive             
        their mother - (w=0.31/v=0.12) 0.04 | -0.02 (w=-0.49/v=0.05) - that be             
         be populate - (w=0.26/v=0.14) 0.04 | -0.02 (w=-0.20/v=0.12) - ridge               
                 and - (w=0.29/v=0.11) 0.03 | -0.02 (w=-0.23/v=0.10) - spouse              
          dollar for - (w=0.24/v=0.11) 0.03 | -0.02 (w=-0.17/v=0.12) - region of           
                west - (w=0.29/v=0.08) 0.02 | -0.02 (w=-0.16/v=0.13) - good his            
              mother - (w=0.25/v=0.08) 0.02 | -0.02 (w=-0.20/v=0.10) - death of            
            the west - (w=0.23/v=0.09) 0.02 | -0.02 (w=-0.13/v=0.14) - weaver and          
                this - (w=0.69/v=0.03) 0.02 | -0.02 (w=-0.21/v=0.09) - and good            
          mother and - (w=0.15/v=0.12) 0.02 | -0.02 (w=-0.44/v=0.04) - good                
               their - (w=0.44/v=0.04) 0.02 | -0.02 (w=-0.40/v=0.04) - year                
           weaver be - (w=0.09/v=0.14) 0.01 | -0.02 (w=-0.25/v=0.07) - late                
        the wrongful - (w=0.08/v=0.14) 0.01 | -0.02 (w=-0.21/v=0.08) - certain             
                 son - (w=0.09/v=0.08) 0.01 | -0.02 (w=-0.14/v=0.11) - son be              
             and his - (w=0.11/v=0.07) 0.01 | -0.01 (w=-0.12/v=0.12) - old son             
          spouse and - (w=0.06/v=0.12) 0.01 | -0.01 (w=-0.11/v=0.13) - and brother         
            populate - (w=0.05/v=0.11) 0.01 | -0.01 (w=-0.12/v=0.12) - defender of         
                  no - (w=0.06/v=0.04) 0.00 | -0.01 (w=-0.38/v=0.04) - of the              
          and weaver - (w=0.01/v=0.14) 0.00 | -0.01 (w=-0.35/v=0.04) - but                 
               be no - (w=0.01/v=0.06) 0.00 | -0.01 (w=-0.09/v=0.14) - his spouse          
           but randy - (w=0.00/v=0.17) 0.00 | -0.01 (w=-0.08/v=0.13) - enclave             
      certain region - (w=0.00/v=0.17) 0.00 | -0.01 (w=-0.14/v=0.07) - of their            
        enclave that - (w=0.00/v=0.17) 0.00 | -0.01 (w=-0.07/v=0.13) - several dollar      
         no defender - (w=0.00/v=0.16) 0.00 | -0.01 (w=-0.10/v=0.07) - several             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.08/v=0.08) - year old            
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.21/v=0.03) - for                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.04/v=0.13) - ruby                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.05/v=0.09) - alive               
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.11/v=0.03) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.06) - of this             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.16) - randy weaver        
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.04) - at                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.14) - ridge and           
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.16) - at ruby             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.02) - that                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.02/v=0.05) - for the             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.15) - ruby ridge          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.09) - region              
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.07) - of                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.14) - wrongful death      


Category : N-Tox. Ass to tox. Words
Category2: 
Words    : ['be kill, kill at']

------------------------------------------------------------------

I never say it never happen . You lie . 
True label: 1

-                                   ---- Result: 0.27 ----
                          sum toxic : 0.48  |  -0.21 : sum non-toxic                      

                 lie - (w=1.20/v=0.22) 0.26 | -0.11 (w=-0.54/v=0.20) - happen              
        never happen - (w=0.21/v=0.35) 0.07 | -0.05 (w=-0.14/v=0.36) - never               
                 you - (w=0.65/v=0.10) 0.06 | -0.02 (w=-0.06/v=0.35) - never say           
             you lie - (w=0.11/v=0.38) 0.04 | -0.01 (w=-0.14/v=0.10) - it                  
            it never - (w=0.10/v=0.35) 0.04 | -0.01 (w=-0.05/v=0.27) - say it              
          happen you - (w=0.02/v=0.41) 0.01 | -0.01 (w=-0.06/v=0.15) - say                 


Category : Lack of tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

a decision that represent a optimistic ¬moment in a dark time a ¬lawyer for the plaintiff say . How ironic that this dimwitted lawyer would engage in such silly conflation . The only reason I vote for Trump a man I consider obnoxious and woefully uninformed be because he promise to rein in our rogue federal judiciary . 
True label: 1

-                                   ---- Result: 1.05 ----
                          sum toxic : 1.78  |  -0.73 : sum non-toxic                      

               silly - (w=7.52/v=0.09) 0.71 | -0.06 (w=-0.64/v=0.09) - be because          
           dimwitted - (w=1.08/v=0.15) 0.16 | -0.06 (w=-0.48/v=0.12) - ironic that         
           obnoxious - (w=1.26/v=0.12) 0.15 | -0.05 (w=-0.31/v=0.15) - such silly          
                 man - (w=1.89/v=0.07) 0.13 | -0.04 (w=-0.56/v=0.08) - in                  
         time lawyer - (w=0.28/v=0.17) 0.05 | -0.04 (w=-0.42/v=0.10) - engage in           
       obnoxious and - (w=0.30/v=0.15) 0.05 | -0.04 (w=-0.35/v=0.10) - in such             
               trump - (w=0.90/v=0.05) 0.04 | -0.03 (w=-0.21/v=0.17) - uninformed be       
         only reason - (w=0.39/v=0.11) 0.04 | -0.03 (w=-0.32/v=0.11) - ironic              
          uninformed - (w=0.39/v=0.11) 0.04 | -0.03 (w=-0.41/v=0.07) - decision            
            vote for - (w=0.51/v=0.07) 0.04 | -0.03 (w=-0.34/v=0.09) - because he          
                such - (w=0.53/v=0.06) 0.03 | -0.03 (w=-0.19/v=0.14) - how ironic          
             rein in - (w=0.22/v=0.13) 0.03 | -0.03 (w=-0.39/v=0.07) - reason              
               rogue - (w=0.22/v=0.12) 0.03 | -0.02 (w=-0.16/v=0.15) - dark time           
                rein - (w=0.19/v=0.12) 0.02 | -0.02 (w=-0.20/v=0.12) - optimistic          
                this - (w=0.69/v=0.03) 0.02 | -0.02 (w=-0.26/v=0.08) - that this           
                  he - (w=0.49/v=0.04) 0.02 | -0.02 (w=-0.26/v=0.07) - consider            
              engage - (w=0.22/v=0.09) 0.02 | -0.02 (w=-0.11/v=0.14) - to rein             
           trump man - (w=0.13/v=0.15) 0.02 | -0.01 (w=-0.17/v=0.08) - in our              
                only - (w=0.39/v=0.05) 0.02 | -0.01 (w=-0.15/v=0.09) - for trump           
   federal judiciary - (w=0.12/v=0.15) 0.02 | -0.01 (w=-0.21/v=0.06) - for                 
        lawyer would - (w=0.13/v=0.15) 0.02 | -0.01 (w=-0.14/v=0.08) - promise             
        man consider - (w=0.11/v=0.17) 0.02 | -0.01 (w=-0.07/v=0.15) - reason vote         
                dark - (w=0.17/v=0.10) 0.02 | -0.01 (w=-0.09/v=0.13) - woefully            
                  be - (w=0.76/v=0.02) 0.01 | -0.01 (w=-0.23/v=0.05) - time                
       the plaintiff - (w=0.08/v=0.13) 0.01 | -0.01 (w=-0.13/v=0.08) - represent           
          lawyer for - (w=0.08/v=0.14) 0.01 | -0.01 (w=-0.10/v=0.10) - promise to          
              lawyer - (w=0.06/v=0.17) 0.01 | -0.01 (w=-0.15/v=0.06) - vote                
              moment - (w=0.09/v=0.09) 0.01 | -0.01 (w=-0.07/v=0.13) - moment in           
             because - (w=0.14/v=0.05) 0.01 | -0.01 (w=-0.19/v=0.04) - would               
                 and - (w=0.29/v=0.02) 0.01 | -0.01 (w=-0.09/v=0.07) - federal             
           plaintiff - (w=0.04/v=0.12) 0.01 | -0.01 (w=-0.12/v=0.05) - our                 
                 how - (w=0.12/v=0.05) 0.01 | -0.00 (w=-0.07/v=0.07) - the only            
      that represent - (w=0.02/v=0.13) 0.00 | -0.00 (w=-0.11/v=0.04) - the                 
          conflation - (w=0.01/v=0.15) 0.00 | -0.00 (w=-0.06/v=0.05) - that                
             in dark - (w=0.01/v=0.14) 0.00 | -0.00 (w=-0.06/v=0.05) - say                 
       decision that - (w=0.01/v=0.12) 0.00 | -0.00 (w=-0.10/v=0.02) - to                  
           judiciary - (w=0.00/v=0.12) 0.00 | -0.00 (w=-0.01/v=0.12) - say how             
        would engage - (w=0.00/v=0.16) 0.00 | -0.00 (w=-0.02/v=0.05) - for the             
 woefully uninformed - (w=0.00/v=0.17) 0.00 | -0.00 (w=-0.01/v=0.12) - he promise          
       rogue federal - (w=0.00/v=0.18) 0.00 | -0.00 (w=-0.00/v=0.17) - and woefully        
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.17) - our rogue           


Category : N-Tox. Ass. Neutral words
Category2: N-Tox. Ass. Stopword in Bigrams
Words    : ['be because, such silly']

------------------------------------------------------------------

Even the slow voter will come to term with the fact that beyond the nice hair and sock be a politician behave good like a politician phoney mislead and when off script inept . 
True label: 1

-                                   ---- Result: 0.58 ----
                          sum toxic : 1.18  |  -0.60 : sum non-toxic                      

               inept - (w=3.74/v=0.15) 0.57 | -0.08 (w=-0.47/v=0.17) - be politician       
          politician - (w=0.52/v=0.20) 0.10 | -0.06 (w=-0.43/v=0.14) - behave              
           nice hair - (w=0.37/v=0.18) 0.07 | -0.05 (w=-0.36/v=0.13) - hair                
         mislead and - (w=0.31/v=0.18) 0.06 | -0.05 (w=-0.27/v=0.17) - hair and            
                slow - (w=0.36/v=0.12) 0.04 | -0.04 (w=-0.21/v=0.21) - when off            
          voter will - (w=0.26/v=0.16) 0.04 | -0.04 (w=-0.26/v=0.14) - beyond the          
         behave good - (w=0.17/v=0.21) 0.04 | -0.03 (w=-0.17/v=0.20) - sock be             
                 off - (w=0.39/v=0.08) 0.03 | -0.03 (w=-0.35/v=0.10) - nice                
            and when - (w=0.24/v=0.12) 0.03 | -0.03 (w=-0.13/v=0.20) - like politician     
           will come - (w=0.19/v=0.13) 0.02 | -0.03 (w=-0.14/v=0.18) - phoney              
            and sock - (w=0.12/v=0.20) 0.02 | -0.03 (w=-0.44/v=0.06) - good                
                like - (w=0.34/v=0.06) 0.02 | -0.03 (w=-0.27/v=0.09) - term                
                  be - (w=0.76/v=0.02) 0.02 | -0.02 (w=-0.34/v=0.06) - when                
                 and - (w=0.29/v=0.06) 0.02 | -0.02 (w=-0.12/v=0.16) - the nice            
           term with - (w=0.10/v=0.17) 0.02 | -0.01 (w=-0.08/v=0.18) - to term             
             mislead - (w=0.11/v=0.13) 0.01 | -0.01 (w=-0.15/v=0.08) - fact                
          off script - (w=0.06/v=0.21) 0.01 | -0.01 (w=-0.15/v=0.07) - with the            
           good like - (w=0.07/v=0.17) 0.01 | -0.01 (w=-0.07/v=0.15) - script              
           fact that - (w=0.10/v=0.10) 0.01 | -0.01 (w=-0.11/v=0.07) - the                 
            the slow - (w=0.04/v=0.17) 0.01 | -0.01 (w=-0.04/v=0.15) - sock                
            the fact - (w=0.06/v=0.09) 0.01 | -0.01 (w=-0.06/v=0.10) - voter               
                with - (w=0.12/v=0.05) 0.01 | -0.00 (w=-0.07/v=0.07) - even                
              beyond - (w=0.05/v=0.11) 0.01 | -0.00 (w=-0.10/v=0.03) - to                  
             come to - (w=0.04/v=0.10) 0.00 | -0.00 (w=-0.02/v=0.12) - even the            
                come - (w=0.03/v=0.07) 0.00 | -0.00 (w=-0.06/v=0.03) - that                
         that beyond - (w=0.00/v=0.23) 0.00 | -0.00 (w=-0.02/v=0.05) - will                


Category : No tox. Words
Category2: 
Words    : 

------------------------------------------------------------------

Nor the prior to that . -PRON- have a vicious cycle of mediocrity . 
True label: 1

-                                   ---- Result: 0.23 ----
                          sum toxic : 0.63  |  -0.40 : sum non-toxic                      

          mediocrity - (w=0.86/v=0.29) 0.25 | -0.10 (w=-0.46/v=0.21) - prior               
             vicious - (w=0.53/v=0.27) 0.14 | -0.07 (w=-0.25/v=0.29) - cycle of            
       of mediocrity - (w=0.27/v=0.34) 0.09 | -0.06 (w=-0.21/v=0.29) - the prior           
                pron - (w=0.94/v=0.08) 0.07 | -0.06 (w=-0.27/v=0.22) - cycle               
           that pron - (w=0.26/v=0.18) 0.05 | -0.05 (w=-0.14/v=0.34) - have vicious        
            prior to - (w=0.09/v=0.22) 0.02 | -0.02 (w=-0.12/v=0.18) - nor                 
             to that - (w=0.06/v=0.18) 0.01 | -0.01 (w=-0.03/v=0.33) - vicious cycle       
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.08/v=0.10) - pron have           
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.15/v=0.05) - have                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.10/v=0.05) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.11/v=0.04) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.26) - nor the             
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.06/v=0.06) - that                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.05) - of                  


Category : disagree
Category2: 
Words    : 

------------------------------------------------------------------

You can not erase history . Why doe the medium give coverage to the much foolish idea . Check out study by Princeton a to the criterium for remove name statue etc . 
True label: 1


-                                   ---- Result: 0.86 ----
                          sum toxic : 1.43  |  -0.57 : sum non-toxic                      

             foolish - (w=7.23/v=0.14) 0.99 | -0.08 (w=-0.60/v=0.13) - check out           
         history why - (w=0.46/v=0.20) 0.09 | -0.06 (w=-0.41/v=0.14) - statue              
       erase history - (w=0.38/v=0.19) 0.07 | -0.04 (w=-0.41/v=0.11) - the medium          
        much foolish - (w=0.34/v=0.20) 0.07 | -0.04 (w=-0.28/v=0.15) - erase               
             can not - (w=0.77/v=0.07) 0.05 | -0.04 (w=-0.21/v=0.19) - not erase           
              remove - (w=0.37/v=0.11) 0.04 | -0.03 (w=-0.12/v=0.22) - idea check          
            coverage - (w=0.24/v=0.12) 0.03 | -0.03 (w=-0.28/v=0.09) - history             
                 you - (w=0.65/v=0.04) 0.03 | -0.02 (w=-0.71/v=0.03) - not                 
                 etc - (w=0.26/v=0.09) 0.02 | -0.02 (w=-0.46/v=0.05) - can                 
            the much - (w=0.10/v=0.09) 0.01 | -0.02 (w=-0.11/v=0.19) - princeton           
         medium give - (w=0.04/v=0.19) 0.01 | -0.02 (w=-0.22/v=0.09) - idea                
       the criterium - (w=0.04/v=0.17) 0.01 | -0.02 (w=-0.35/v=0.05) - by                  
              medium - (w=0.07/v=0.09) 0.01 | -0.02 (w=-0.18/v=0.10) - check               
              to the - (w=0.03/v=0.12) 0.00 | -0.02 (w=-0.19/v=0.09) - name                
             doe the - (w=0.03/v=0.12) 0.00 | -0.02 (w=-0.23/v=0.07) - why                 
               study - (w=0.02/v=0.11) 0.00 | -0.02 (w=-0.11/v=0.13) - why doe             
            study by - (w=0.00/v=0.18) 0.00 | -0.01 (w=-0.08/v=0.18) - coverage to         
        by princeton - (w=0.00/v=0.23) 0.00 | -0.01 (w=-0.16/v=0.07) - doe                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.21/v=0.04) - for                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.10/v=0.08) - you can             
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.11/v=0.07) - the                 
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.04/v=0.15) - criterium           
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.10/v=0.06) - to                  
                     - (w=0.00/v=0.00) 0.00 | -0.01 (w=-0.02/v=0.23) - foolish idea        
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.18) - criterium for       
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.05/v=0.06) - out                 
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.23) - give coverage       
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.03/v=0.05) - much                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.01/v=0.07) - give                
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.20) - for remove          
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.22) - remove name         
                     - (w=0.00/v=0.00) 0.00 | -0.00 (w=-0.00/v=0.23) - statue etc          


Category : Lack of tox. Words
Category2: N-Tox. Ass to tox. Words
Words    : ['foolish idea']

------------------------------------------------------------------


```