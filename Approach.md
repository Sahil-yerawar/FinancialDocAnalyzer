
### Approach used for this problem

- The first step was to annotate the financial documents as [CR]\(Company Report\)
  ,[Sector] and the rest being treated as [Others].
- Then we work on each pdf according to its type and process for the different fields
  separately.

  - *Recommending company*: We find the company name by searching for email ids, searching for
    phrases having "Private|Pvt Limited|Ltd" in it. This works for majority of cases.

  - *Object company*: For this, we search for Bloomberg, BSE or NSE codes to get
    to know the company being talked about. It may be the case that some of the
    pdfs may have incorrect codes or having no codes.

  - *Company Rating*: The vocabulary of the rating keywords are limited, searching For
    them using regex and not being part of another word would do the work. If not rated,
    they are marked as 'NA'.

  - *Target Price*: A complex regex expression has been developed to capture the Target
    price phrase containing the value. Using the above, we get the value data.
    It may not be available in the case of unrated reports or target prices being
    mentioned in vertical tables

  - *Persons*: To get the person names, we use the Stanford NER module to tag all the words
    in the first page of pdf as most of the financial reports would have authors at
    front. Out of all the tags, we collect those marked as `'PERSON'` and intelligently
    combine them to form a list of their full names


### Schema of result table
- The result table will consist of the following columns:
    - Filename: The filename being processed
    - Recommending Company: The company who made the report. If it's not a company
      or sector report, this field is marked as `Others`

    - is_sector: marking whether the file is a sector report or not. It is valued as
      `NO` if it is a company report of single company or a file marked as `Others` Stanford
      `YES` otherwise.

    - Object Company: The company/companies being talked about in the report.
    - target_price: the target price recommended by the report
    - rating: the rating recommended by the company
    - persons: the authors of this report.

#### Note
To work with Stanford NER module, you need to get the zip file and store in your local machine. The trained models for NER task can be fed to `nltk` to proceed further.
[Link](https://nlp.stanford.edu/software/CRF-NER.html#Download) to know more about installation process.

For the training data and all the other material, please contact the author of this repository.