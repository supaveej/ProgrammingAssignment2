
# [Coursera JHU] Getting and Cleaning Data: Course Project

## Purpose


```R
source("projUtils.R")

## [1] Merge the training and the test sets to create one data set.
files <- c()
for(datatype in datatypes){ 
  files <- c(files,paste0("./",datatype,"/",list.files(pattern="\\.txt$", 
                              path=paste0("./",datatype))))
}

tables <- lapply(files,importTable)
combinedtable <- combineTables(tables)
combinedtable <- getActivityLabels(combinedtable)
colnames(combinedtable) <- updatedColumnNames(colnames(combinedtable))

avgtable <-createAvgByActivityandSubject(combinedtable)
```

    
    Attaching package: ‘dplyr’
    
    The following objects are masked from ‘package:stats’:
    
        filter, lag
    
    The following objects are masked from ‘package:base’:
    
        intersect, setdiff, setequal, union
    


    [1] "./test/subject_test.txt test subject"
    [1] "./test/X_test.txt test X"
    [1] "./test/y_test.txt test y"
    [1] "./train/subject_train.txt train subject"
    [1] "./train/X_train.txt train X"
    [1] "./train/y_train.txt train y"



```R
ls()
```


<ol class=list-inline>
	<li>'a'</li>
	<li>'activities'</li>
	<li>'avgtable'</li>
	<li>'columnNames'</li>
	<li>'combinedtable'</li>
	<li>'combineTables'</li>
	<li>'createAvgByActivityandSubject'</li>
	<li>'datatype'</li>
	<li>'datatypes'</li>
	<li>'files'</li>
	<li>'getActivityLabels'</li>
	<li>'getFeatureColumnNames'</li>
	<li>'importTable'</li>
	<li>'tables'</li>
	<li>'updatedColumnNames'</li>
</ol>




```R
head(combinedtable)
```


<table>
<thead><tr><th scope=col>subject</th><th scope=col>datatype</th><th scope=col>time_BodyAcceleration_Mean_X</th><th scope=col>time_BodyAcceleration_Mean_Y</th><th scope=col>time_BodyAcceleration_Mean_Z</th><th scope=col>time_BodyAcceleration_StandardDeviation_X</th><th scope=col>time_BodyAcceleration_StandardDeviation_Y</th><th scope=col>time_BodyAcceleration_StandardDeviation_Z</th><th scope=col>time_GravityAcceleration_Mean_X</th><th scope=col>time_GravityAcceleration_Mean_Y</th><th scope=col>⋯</th><th scope=col>frequency_BodyGyro_StandardDeviation_Z</th><th scope=col>frequency_BodyAcceleration_Magnitude_Mean</th><th scope=col>frequency_BodyAcceleration_Magnitude_StandardDeviation</th><th scope=col>frequency_BodyAcceleration_JerkMagnitude_Mean</th><th scope=col>frequency_BodyAcceleration_JerkMagnitude_StandardDeviation</th><th scope=col>frequency_BodyGyroMagnitude_Mean</th><th scope=col>frequency_BodyGyroMagnitude_StandardDeviation</th><th scope=col>frequency_BodyGyroJerkMagnitude_Mean</th><th scope=col>frequency_BodyGyroJerkMagnitude_StandardDeviation</th><th scope=col>activity</th></tr></thead>
<tbody>
	<tr><td>1          </td><td>train      </td><td>0.2885845  </td><td>-0.02029417</td><td>-0.1329051 </td><td>-0.9952786 </td><td>-0.9831106 </td><td>-0.9135264 </td><td>0.9633961  </td><td>-0.1408397 </td><td>⋯          </td><td>-0.9940349 </td><td>-0.9521547 </td><td>-0.9561340 </td><td>-0.9937257 </td><td>-0.9937550 </td><td>-0.9801349 </td><td>-0.9613094 </td><td>-0.9919904 </td><td>-0.9906975 </td><td>STANDING   </td></tr>
	<tr><td>1          </td><td>train      </td><td>0.2784188  </td><td>-0.01641057</td><td>-0.1235202 </td><td>-0.9982453 </td><td>-0.9753002 </td><td>-0.9603220 </td><td>0.9665611  </td><td>-0.1415513 </td><td>⋯          </td><td>-0.9897847 </td><td>-0.9808566 </td><td>-0.9758658 </td><td>-0.9903355 </td><td>-0.9919603 </td><td>-0.9882956 </td><td>-0.9833219 </td><td>-0.9958539 </td><td>-0.9963995 </td><td>STANDING   </td></tr>
	<tr><td>1          </td><td>train      </td><td>0.2796531  </td><td>-0.01946716</td><td>-0.1134617 </td><td>-0.9953796 </td><td>-0.9671870 </td><td>-0.9789440 </td><td>0.9668781  </td><td>-0.1420098 </td><td>⋯          </td><td>-0.9873282 </td><td>-0.9877948 </td><td>-0.9890155 </td><td>-0.9892801 </td><td>-0.9908667 </td><td>-0.9892548 </td><td>-0.9860277 </td><td>-0.9950305 </td><td>-0.9951274 </td><td>STANDING   </td></tr>
	<tr><td>1          </td><td>train      </td><td>0.2791739  </td><td>-0.02620065</td><td>-0.1232826 </td><td>-0.9960915 </td><td>-0.9834027 </td><td>-0.9906751 </td><td>0.9676152  </td><td>-0.1439765 </td><td>⋯          </td><td>-0.9886776 </td><td>-0.9875187 </td><td>-0.9867420 </td><td>-0.9927689 </td><td>-0.9916998 </td><td>-0.9894128 </td><td>-0.9878358 </td><td>-0.9952207 </td><td>-0.9952369 </td><td>STANDING   </td></tr>
	<tr><td>1          </td><td>train      </td><td>0.2766288  </td><td>-0.01656965</td><td>-0.1153619 </td><td>-0.9981386 </td><td>-0.9808173 </td><td>-0.9904816 </td><td>0.9682244  </td><td>-0.1487502 </td><td>⋯          </td><td>-0.9879443 </td><td>-0.9935909 </td><td>-0.9900635 </td><td>-0.9955228 </td><td>-0.9943890 </td><td>-0.9914330 </td><td>-0.9890594 </td><td>-0.9950928 </td><td>-0.9954648 </td><td>STANDING   </td></tr>
	<tr><td>1          </td><td>train      </td><td>0.2771988  </td><td>-0.01009785</td><td>-0.1051373 </td><td>-0.9973350 </td><td>-0.9904868 </td><td>-0.9954200 </td><td>0.9679482  </td><td>-0.1482100 </td><td>⋯          </td><td>-0.9853661 </td><td>-0.9948360 </td><td>-0.9952833 </td><td>-0.9947329 </td><td>-0.9951562 </td><td>-0.9905000 </td><td>-0.9858609 </td><td>-0.9951433 </td><td>-0.9952387 </td><td>STANDING   </td></tr>
</tbody>
</table>




```R

```
