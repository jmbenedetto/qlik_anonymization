# Qlik_Anonymization

# The Problem
More and more data protection and privacy are key to our organizations. We need to be able to anonymize data in order to comply with the law, and to scramble it to protect our company sensitive data.

# The proposed solution
A series of scripts using SUBs to scramble field from in memory tables in Qlik Sense.

- Each sub does one task (more details below).
- Some subs use inline tables stored in different qvs files. In this way we can evolve the underlying dataset without having to change the script.
- To simplify the use of the subs, a master script is provided. It contains a sub to load both inline tables and script files.

# How to use it
1. From qlik-giants GCS bucket
No download required.

- To create a data connection in Qlik Cloud to our GCS bucket, check our organization profile in Github (https://github.com/qlik-giants#how-to-use-our-solutions).
- With the data connection created, load 00.fInitializeAnonymization.qvs with Must_Include and call 00.fInitializeAnonymization using 'default' for both parameters.

```qlik
$(Must_Include=[$(data_connection_to_gcs)00.fInitializeAnonymization.qvs])
CALL fInitializeAnonymization('default','default')

- After that, feel free to call any anonymization function available.

1. From a different location

- Make sure you downloaded all qvs files from the folders v1 and Datasets to a location available to Qlick Cloud.
- With the files available:load 00.fInitializeAnonymization.qvs with Must_Include and call 00.fInitializeAnonymization. Please pass the full path for the anonymization scripts as the first parameter, and the full path for the datasets (inline tables in qvs) as the second parameter.

'''qlik
$(Must_Include=[$(data_connection_to_gcs)00.fInitializeAnonymization.qvs])
CALL fInitializeAnonymization('$(vPathToScript)','vPathToDatasets')
'''

- After that, feel free to call any anonymization function available.


## Scripts and Parameters :

	1 - Anon_FakeGenericDimension ( Delete Original Field (Y/N) /Table Name / Field Name / Inline Table Name )
    2 - Anon_FakeWord			  ( Delete Original Field (Y/N) /Table Name / Field Name )
    3 - Anon_Exists				  ( Delete Original Field (Y/N) /Table Name / Field Name )
    4 - Anon_Resampling			  ( Delete Original Field (Y/N) /Table Name / Field Name )
    5 - Anon_Scrambling			  ( Delete Original Field (Y/N) /Table Name / Field Name )
    6 - Anon_Tokenize			  ( Delete Original Field (Y/N) /Table Name / Field Name / tokens)
    7 - Anon_ValueReduction		  ( Delete Original Field (Y/N) /Table Name / Field Name )
    8 - Anon_FakeItSelfDimension  ( Delete Original Field (Y/N) /Table Name / Field Name )
    
    INLINES DataSet avaiables :
    
    1 - INLINE_PERSON_NAME
    2 - INLINE_COMPANY_NAME
    3 - INLINE_COMPANY_DEPARTMENT
    4 - INLINE_CURRENCY_NAME
    5 - INLINE_JOB_TITLE
    6 - INLINE_COLOR_NAME
    7 - INLINE_EMAIL
    8 - INLINE_US_STATES
    
    EXAMPLES :

    CALL Anon_FakeGenericDimension		('Y','Product Lines','Product Line 3','INLINE_US_STATES');
    CALL Anon_FakeWord					('N','Items','Bookings Group 1');
    CALL Anon_Tokenize    			    ('Y','Invoices','PO Number',10);
    
    

*/
