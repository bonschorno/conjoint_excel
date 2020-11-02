# Setting up a conjoint experiment on Qualtrics with externally randomized data from Excel

This is a step-by-step guide to embedding externally randomized data into Qualtrics. The process for a conjoint analysis is described below, but the procedure is also applicable to other types of experiments. To get a better idea of the whole workflow, the following helper files are included in the repo:

| File                 | Description                                          |
|----------------------|------------------------------------------------------|
| `conj_attr.xlsx`       | Variable values sheet and lookup table, respectively |
| `conj_attr.csv`        | Conjoint attributes to be uploaded to Qualtrics      |
| `conjoint_example.qsf` | Skeleton of survey setup within Qualtrics            |

## Creating the excel-file

Create two sheets within the same excel file: One with the variable values (`vars_num`), one with the variable labels (`vars_labels`). Let's start with the `vars_num`-sheet. Here, you randomly assign the corresponding variable's numeric codes with the RANDBETWEEN(x,y) where x represents the lower and y the upper bound of the desired numeric range.<sup id="a1">[1](#f1)</sup>

Regarding the column names, the number represents the nth attribute and the letter the position of the attribute. For instance, `conjoint_attr1a` means that it is the first attribute in the left column.

Once you have defined the numeric values, you have to assign the corresponding labels. Now the second sheet, `vars_labels`, comes into play. Why on the second sheet, you might ask? First, so that you have a neat overview of all your variables' labels. Second, you'll have to export the file as `.csv`. Hence, your data should be tidy and not contain any values other than the actual embedded data. Once you've assigned a label to every value for every variable, you can now fill in the columns called ending with *_label*. You do this with the function VLOOKUP(). 

VLOOKUP() works as follows: The first argument is the numeric value you want to label. Say, 1 would translate to *Tall*, 0 to *Short*. The second argument (`table_array`) defines your "codebook" where you assigned a label to every value. In this argument, you have to refer to the "labels" sheet. Also, very important, you have to close the values with dollar signs! Otherwise, as the function is dynamic, the values to be looked up in `vars_num` will be outside of the defined table_array, leading to NAs. The third argument refers to the column in table_array, where the variable's labels are stored. In our case, that's always the second column, hence the 2. Finally, you have to set the last argument to FALSE as you want to have exact matches, meaning that the output will be NA if there is no corresponding label to the value. Specifying everything accordingly should give you the correct label for every value. 

Lastly, to add a new contact list to Qualtrics, you have to create a fake e-mail address. This is done the following way: 

=LOWER(CHAR(RANDBETWEEN(97,122))&CHAR(RANDBETWEEN(97,122))&"@testmail.com")<sup id="a2">[2](#f2)</sup>

Finally, you can also create an identifier (id), but that's optional.

That's it. Now you have your excel file ready!

## Adding the contact list to Qualtrics

From now on, it's relatively straightforward: You go to Contacts > Create Contact List > name the file and assign it to the folder where you want to store it on Qualtrics > Import from a File... (default) > upload your csv-file.

Then you add all the data from the contact list to the embedded data in the survey flow, create an Html table in the question field where you want to implement the conjoint analysis, and add the embedded data to the Html table.

As a final step, you have to create the links to see the embedded data.<sup id="a3">[3](#f3)</sup> Hence, you create personal links, and if everything was done correctly, you should now see the embedded data in your conjoint table once you click on a generated link!

---

### Footnotes

<b id="f1">1</b> Be aware that Excel updates the random values every time you add new columns. Hence, if you want your values to be static, you have to select "keep values only" once they were created with the RANDBETWEEN-function. [↩](#a1)

<b id="f2">2</b> Source: [http://manonastreet.blogspot.com/2016/12/fake-identity-generator-style-email.html](http://manonastreet.blogspot.com/2016/12/fake-identity-generator-style-email.html) [↩](#a2)

<b id="f3">3</b> As the data was created externally, it's only displayed once it's assigned its labels, which happens by generating the links. [↩](#a1)

