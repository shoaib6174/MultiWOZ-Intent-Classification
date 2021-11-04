<!-----
NEW: Check the "Suppress top comment" option to remove this info from the output.

Conversion time: 1.514 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β31
* Thu Nov 04 2021 02:46:18 GMT-0700 (PDT)
* Source doc: MultiWOZ
* This is a partial selection. Check to make sure intra-doc links work.
* Tables are currently converted to HTML tables.
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server. NOTE: Images in exported zip file from Google Docs may not appear in  the same order as they do in your doc. Please check the images!

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 3.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>


Here I have compared the performance of LaBSE and BERT models on the MultiWOZ dataset. Then I have also translated the English text to Russian and evaluated both models on it without tuning the models onto the Russian Language. 

**MultiWOZ Dataset:** Multi-Domain Wizard-of-Oz dataset (MultiWOZ) is a fully-labeled collection of human-human written conversations spanning over multiple domains and topics.

**Preparing the data:**

I have created 3 CSV files using the following steps:-



1. I started by extracting text/dialogue and intents from JSON files of train and test folders of the MultiWOZ 2.1 dataset. I got 51244 instances for training and 6843  for testing.
2. The dataset contains the following intents:- book_hotel, book_restaurant, book_train, find_attraction, find_bus, find_hospital, find_hotel, find_police, find_restaurant, find_taxi, find_train
3. Some entries had more than 2 intents. I removed them and got  50838 text/dialogue with intents for training and 6802 text/dialogue with intents for testing. 
4. Then I One -Hot encoded the intents using sklearn.preprocessing.MultiLabelBinarizer 
5. I added no_intents where text/dialogue didn’t have any intent
6. I translated the texts of the test dataset to Russian for evaluation using googletrans
7. So, finally, I got the following 3 CSV files-
    1. Train_intent.csv
    2. Test_intents.csv
    3. Russian_intents_test.csv

The intents distribution in the training dataset-



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")


To get a balanced dataset, I have made a subset of the training data by sampling intents each for all the single entries using the Sampling with replacement method and then combined it with all the multi-intent dialogues. For the Bert model, I have sampled 2000 intents for each of the single intent entries but for LaBSE, due to resources constraint I had to sample 1000 intents. 

The intents distribution for the Bert Model-



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")


The intents distribution for the LaBSEMode-



<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")


Then I have split the training data into train_df and val_df.

Finally, I have built a data module using **<code>LightningDataModule</code></strong> class of PyTorch-Lightning. I used <code>AutoTokenizer</code>/<code>BertTokenizer </code>to tokenize the data.

**Models: **

I have built IntentClassifier models using Bert and LaBSE models from the transformers library which was developed by Hugging Face. I have used AdamW as an optimizer.

Due to resource constraints, I had to use a smaller data set for training. 


<table>
  <tr>
   <td>
   </td>
   <td><strong>BERT</strong>
   </td>
   <td><strong>LaBSE</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Model Name</strong>
   </td>
   <td>bert-base-cased
   </td>
   <td>sentence-transformers/LaBSE
   </td>
  </tr>
  <tr>
   <td><strong>Size of Training Data</strong>
   </td>
   <td>27255
   </td>
   <td>16805
   </td>
  </tr>
  <tr>
   <td><strong>Size of Validation Data</strong>
   </td>
   <td>1435
   </td>
   <td>885
   </td>
  </tr>
  <tr>
   <td><strong>Number of Epochs</strong>
   </td>
   <td>3
   </td>
   <td>6
   </td>
  </tr>
  <tr>
   <td><strong>Number of Parameters</strong>
   </td>
   <td>108 M
   </td>
   <td>470 M
   </td>
  </tr>
  <tr>
   <td><strong>Batch Size</strong>
   </td>
   <td>12
   </td>
   <td>24
   </td>
  </tr>
  <tr>
   <td><strong>Max Token Count</strong>
   </td>
   <td>128
   </td>
   <td>128
   </td>
  </tr>
  <tr>
   <td><strong>Threshold</strong>
   </td>
   <td>0.5
   </td>
   <td>0.5
   </td>
  </tr>
</table>


**Model Evaluation:**


<table>
  <tr>
   <td colspan="2" >→ Model Name →
   </td>
   <td colspan="2" ><strong>BERT</strong>
   </td>
   <td colspan="2" ><strong>LaBSE</strong>
   </td>
  </tr>
  <tr>
   <td>↓ Metric ↓
   </td>
   <td>→ Language →
<p>
↓ Method ↓
   </td>
   <td>English (Original)
   </td>
   <td>Russian (Translated)
   </td>
   <td>English (Original)
   </td>
   <td>Russian (Translated)
   </td>
  </tr>
  <tr>
   <td><strong>Accuracy</strong>
   </td>
   <td>-
   </td>
   <td>0.9695
   </td>
   <td>0.8777
   </td>
   <td>0.9585
   </td>
   <td>0.9569
   </td>
  </tr>
  <tr>
   <td rowspan="4" ><strong>Precision</strong>
   </td>
   <td>Sample Average
   </td>
   <td>0.86
   </td>
   <td>0.05
   </td>
   <td>0.81
   </td>
   <td>0.78
   </td>
  </tr>
  <tr>
   <td>Weighted Average
   </td>
   <td>0.88
   </td>
   <td>0.70
   </td>
   <td>0.78
   </td>
   <td>0.82
   </td>
  </tr>
  <tr>
   <td>Micro Average
   </td>
   <td>0.88
   </td>
   <td>0.18
   </td>
   <td>0.77
   </td>
   <td>0.81
   </td>
  </tr>
  <tr>
   <td>Macro Average
   </td>
   <td>0.83
   </td>
   <td>0.53
   </td>
   <td>0.73
   </td>
   <td>0.74
   </td>
  </tr>
  <tr>
   <td rowspan="4" ><strong>Recall</strong>
   </td>
   <td>Sample Average
   </td>
   <td>0.84
   </td>
   <td>0.05
   </td>
   <td>0.86
   </td>
   <td>0.78
   </td>
  </tr>
  <tr>
   <td>Weighted Average
   </td>
   <td>0.81
   </td>
   <td>0.05
   </td>
   <td>0.84
   </td>
   <td>0.75
   </td>
  </tr>
  <tr>
   <td>Micro Average
   </td>
   <td>0.81
   </td>
   <td>0.05
   </td>
   <td>0.84
   </td>
   <td>0.75
   </td>
  </tr>
  <tr>
   <td>Macro Average
   </td>
   <td>0.82
   </td>
   <td>0.07
   </td>
   <td>0.84
   </td>
   <td>0.75
   </td>
  </tr>
  <tr>
   <td rowspan="4" ><strong>F1 Score</strong>
   </td>
   <td>Sample Average
   </td>
   <td>0.84
   </td>
   <td>0.05
   </td>
   <td>0.82
   </td>
   <td>0.76
   </td>
  </tr>
  <tr>
   <td>Weighted Average
   </td>
   <td>0.84
   </td>
   <td>0.06
   </td>
   <td>0.81
   </td>
   <td>0.78
   </td>
  </tr>
  <tr>
   <td>Micro Average
   </td>
   <td>0.82
   </td>
   <td>0.08
   </td>
   <td>0.81
   </td>
   <td>0.78
   </td>
  </tr>
  <tr>
   <td>Macro Average
   </td>
   <td>0.85
   </td>
   <td>0.06
   </td>
   <td>0.75
   </td>
   <td>0.74
   </td>
  </tr>
</table>


Here, we can see that the BERT model has performed well on English texts but it has done really poorly on Russian Texts and the LaBSE model has done well on both English and Russian texts. BERT model has done better on English text than LaBSE because we trained LaBSE with fewer data. 

It’s possible to get better results on this dataset than this with more computation power. I also couldn’t optimize the hyperparameters due to resource constraints. 
