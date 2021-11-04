"# MultiWOZ-Intent-Classification" 
<!-----
NEW: Check the "Suppress top comment" option to remove this info from the output.

Conversion time: 2.8 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β31
* Thu Nov 04 2021 02:05:47 GMT-0700 (PDT)
* Source doc: MultiWOZ
* This is a partial selection. Check to make sure intra-doc links work.
* Tables are currently converted to HTML tables.
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server. NOTE: Images in exported zip file from Google Docs may not appear in  the same order as they do in your doc. Please check the images!

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 6.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>
<a href="#gdcalert4">alert4</a>
<a href="#gdcalert5">alert5</a>
<a href="#gdcalert6">alert6</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>


Data:

Creating CSV files. 



1. I started by extracting text/dialogue and intents from JSON files of train and test folders of the MultiWOZ 2.1 dataset. I got 51244 instances for training and 6843  for testing.
2. The dataset contains the following intents:- book_hotel, book_restaurant, book_train, find_attraction, find_bus, find_hospital, find_hotel, find_police, find_restaurant, find_taxi, find_train
3. Some entries had more than 2 intents. I removed them and got  50838 text/dialogue with intents for training and 6802 text/dialogue with intents for testing. 
4. Then I One -Hot encoded the intents using sklearn.preprocessing.MultiLabelBinarizer 
5. I added no_intents where text/dialogue didn’t have any intent
6. I translated the texts of the test dataset to Russian for evaluation using googletrans
7. So, finally, I got the following 3 CSV files-

    train_intent.csv




<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")


test_intents.csv



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")


Russian_intents_test.csv



<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")


 

The intents distribution in the training dataset is like following-



<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.png "image_tooltip")


I made a subset of the training data by sampling intents each for all the single entries using the Sampling with replacement method and then combined it with all the multi-intent dialogues. For the Bert model, I sampled 2000 intents for each of the single intent entries but for LaBSE, due to resources constraint I had to sample 1000 intents. 

The intents distribution for Bert Model:



<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image5.png "image_tooltip")


The intents distribution for LaBSEModel:



<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image6.png "image_tooltip")


I split this sample data into train and test dataframes.

I built a data module using **<code>LightningDataModule</code></strong> of PyTorch-Lightning. I used <code>AutoTokenizer</code>

/`BertTokenizer `to tokenize the data.

Then I have built an IntentClassifier model using Bert/LaBSE. I have used AdamW as an optimizer.


<table>
  <tr>
   <td>
   </td>
   <td>BERT
   </td>
   <td>LaBSE
   </td>
  </tr>
  <tr>
   <td>Model Name
   </td>
   <td>bert-base-cased
   </td>
   <td>sentence-transformers/LaBSE
   </td>
  </tr>
  <tr>
   <td>Size of Training Data
   </td>
   <td>27255
   </td>
   <td>16805
   </td>
  </tr>
  <tr>
   <td>Size of Validation Data
   </td>
   <td>1435
   </td>
   <td>885
   </td>
  </tr>
  <tr>
   <td>Number of Epochs
   </td>
   <td>3
   </td>
   <td>6
   </td>
  </tr>
  <tr>
   <td>Batch Size
   </td>
   <td>12
   </td>
   <td>24
   </td>
  </tr>
  <tr>
   <td>Max Token Count
   </td>
   <td>128
   </td>
   <td>128
   </td>
  </tr>
  <tr>
   <td>Threshold
   </td>
   <td>.5
   </td>
   <td>.5
   </td>
  </tr>
  <tr>
   <td>Accuracy on English Test Data
   </td>
   <td>96.95%
   </td>
   <td>95.85%
   </td>
  </tr>
  <tr>
   <td>Accuracy on Russian Test Data
   </td>
   <td>87.77%
   </td>
   <td>95.69%
   </td>
  </tr>
</table>


Model Comparison for English Text


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

