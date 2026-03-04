# utl-altair-slc-ai-text-mining-sentiment-analysis-did-users-like-my-product
Altair slc ai text mining sentiment analysis did users like my product
    %let pgm=utl-altair-slc-ai-text-mining-sentiment-analysis-did-users-like-my-product;

    %stop_submission;

    Altair slc ai text mining sentiment analysis did users like my product

    Too long to post on a list, see github
    https://github.com/rogerjdeangelis/utl-altair-slc-ai-text-mining-sentiment-analysis-did-users-like-my-product

    Related to RapidMiner
    https://community.altair.com/discussion/42562

    PROBLEM ADD SENTIMENT TO RESPONSES


    NEGATIVE
           Poor quality for the price.
           Stopped working after a few uses.
           Would not recommend to others.
           Very disappointed with the results.
           Hard to use and unclear instructions.
           Product arrived damaged or defective.
           Regret buying this will not purchase again.
    NEUTRAL
           Did not meet my expectations.
           Too expensive for what you get.
           Exceeded my expectations!
    POSITIVE
           Fantastic quality and value.
           Would definitely buy again.
           Highly recommend this to others.
           Just what I needed perfect!
           Worked flawlessly right out of the box.
           Excellent customer service and fast delivery.
           I love this product it is worth every penny.
           Amazing design and easy to use.
           Five stars couldn’t be happier.
           Customer support was unhelpful.

                              SENTIMENT
                                          Cumulative    Cumulative
    SENTIMENT    Frequency     Percent     Frequency      Percent
    --------------------------------------------------------------
    NEGATIVE            7       35.00             7        35.00
    NEUTRAL             3       15.00            10        50.00
    POSITIVE           10       50.00            20       100.00

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=v7;
    data workx.product;
     retain id;
     informat sentences $64.;
     input sentences &;
     id=_n_;
    cards4;
    Exceeded my expectations!
    Fantastic quality and value.
    Would definitely buy again.
    Highly recommend this to others.
    Just what I needed perfect!
    Worked flawlessly right out of the box.
    Excellent customer service and fast delivery.
    I love this product it is worth every penny.
    Amazing design and easy to use.
    Five stars couldn’t be happier.
    Did not meet my expectations.
    Poor quality for the price.
    Stopped working after a few uses.
    Would not recommend to others.
    Very disappointed with the results.
    Hard to use and unclear instructions.
    Product arrived damaged or defective.
    Customer support was unhelpful.
    Too expensive for what you get.
    Regret buying this will not purchase again.
    ;;;;
    run;

    /**************************************************************************************************************************/
    /*  WORKX.PRODUCT total obs=20                                                                                            */
    /*                                                                                                                        */
    /*    id    sentences                                                                                                     */
    /*                                                                                                                        */
    /*     1    Exceeded my expectations!                                                                                     */
    /*     2    Fantastic quality and value.                                                                                  */
    /*     3    Would definitely buy again.                                                                                   */
    /*     4    Highly recommend this to others.                                                                              */
    /*     5    Just what I needed perfect!                                                                                   */
    /*     6    Worked flawlessly right out of the box.                                                                       */
    /*     7    Excellent customer service and fast delivery.                                                                 */
    /*     8    I love this product it is worth every penny.                                                                  */
    /*     9    Amazing design and easy to use.                                                                               */
    /*    10    Five stars couldn’t be happier.                                                                               */
    /*    11    Did not meet my expectations.                                                                                 */
    /*    12    Poor quality for the price.                                                                                   */
    /*    13    Stopped working after a few uses.                                                                             */
    /*    14    Would not recommend to others.                                                                                */
    /*    15    Very disappointed with the results.                                                                           */
    /*    16    Hard to use and unclear instructions.                                                                         */
    /*    17    Product arrived damaged or defective.                                                                         */
    /*    18    Customer support was unhelpful.                                                                               */
    /*    19    Too expensive for what you get.                                                                               */
    /*    20    Regret buying this will not purchase again.                                                                   */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    options validvarname=v7;
    options set=PYTHONHOME "D:\py314";
    proc python;
    submit;
    import pyreadstat as ps
    import pandas as pd
    from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer

    # One-line initialization
    analyzer = SentimentIntensityAnalyzer()

    product,meta = ps.read_sas7bdat('d:/wpswrkx/product.sas7bdat')
    responses = product['sentences']

    # Score them
    rows = []
    for response in responses:
        scores = analyzer.polarity_scores(response)

        # Classify
        if scores['compound'] >= 0.05:
            sentiment = "POSITIVE"
        elif scores['compound'] <= -0.05:
            sentiment = "NEGATIVE"
        else:
            sentiment = "NEUTRAL"

        rows.append({'response': response, 'sentiment': sentiment})

    results = pd.DataFrame(rows)
    print(results)
    endsubmit;
    import python=results data=workx.results;
    run;

    proc sort data=workx.results;
    by sentiment;
    run;

    proc print data=workx.results;
    by sentiment;
    run;

    proc freq data=workx.results;
    tables sentiment;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Altair SLC                                                                                                             */
    /*                                                                                                                        */
    /* sentiment=NEGATIVE                                                                                                     */
    /*                                                                                                                        */
    /* Obs                     response                                                                                       */
    /*                                                                                                                        */
    /*   1    Poor quality for the price.                                                                                     */
    /*   2    Stopped working after a few uses.                                                                               */
    /*   3    Would not recommend to others.                                                                                  */
    /*   4    Very disappointed with the results.                                                                             */
    /*   5    Hard to use and unclear instructions.                                                                           */
    /*   6    Product arrived damaged or defective.                                                                           */
    /*   7    Regret buying this will not purchase again.                                                                     */
    /*                                                                                                                        */
    /* sentiment=NEUTRAL                                                                                                      */
    /*                                                                                                                        */
    /* Obs                    response                                                                                        */
    /*                                                                                                                        */
    /*   8    Exceeded my expectations!                                                                                       */
    /*   9    Did not meet my expectations.                                                                                   */
    /*  10    Too expensive for what you get.                                                                                 */
    /*                                                                                                                        */
    /* sentiment=POSITIVE                                                                                                     */
    /*                                                                                                                        */
    /* Obs                   response                                                                                         */
    /*                                                                                                                        */
    /*  11    Fantastic quality and value.                                                                                    */
    /*  12    Would definitely buy again.                                                                                     */
    /*  13    Highly recommend this to others.                                                                                */
    /*  14    Just what I needed perfect!                                                                                     */
    /*  15    Worked flawlessly right out of the box.                                                                         */
    /*  16    Excellent customer service and fast delivery.                                                                   */
    /*  17    I love this product it is worth every penny.                                                                    */
    /*  18    Amazing design and easy to use.                                                                                 */
    /*  19    Five stars couldn?t be happier.                                                                                 */
    /*  20    Customer support was unhelpful.                                                                                 */
    /*                                                                                                                        */
    /* Altair SLC                                                                                                             */
    /*                                                                                                                        */
    /* The FREQ Procedure                                                                                                     */
    /*                                                                                                                        */
    /*                                      Cumulative    Cumulative                                                          */
    /* sentiment    Frequency    Percent     Frequency       Percent                                                          */
    /* -------------------------------------------------------------                                                          */
    /* NEGATIVE             7      35.00             7         35.00                                                          */
    /* NEUTRAL              3      15.00            10         50.00                                                          */
    /* POSITIVE            10      50.00            20        100.00                                                          */
    /**************************************************************************************************************************/

    /*
    | | ___   __ _
    | |/ _ \ / _` |
    | | (_) | (_| |
    |_|\___/ \__, |
             |___/
    */
    1                                          Altair SLC       09:42 Wednesday, March  4, 2026

    NOTE: Copyright 2002-2025 World Programming, an Altair Company
    NOTE: Altair SLC 2026 (05.26.01.00.000758)
          Licensed to Roger DeAngelis
    NOTE: This session is executing on the X64_WIN11PRO platform and is running in 64 bit mode

    NOTE: AUTOEXEC processing beginning; file is C:\wpsoto\autoexec.sas
    NOTE: AUTOEXEC source line
    1       +  ï»¿ods _all_ close;
               ^
    ERROR: Expected a statement keyword : found "?"
    NOTE: Library workx assigned as follows:
          Engine:        SAS7BDAT
          Physical Name: d:\wpswrkx

    NOTE: Library slchelp assigned as follows:
          Engine:        WPD
          Physical Name: C:\Progra~1\Altair\SLC\2026\sashelp

    NOTE: Library worksas assigned as follows:
          Engine:        SAS7BDAT
          Physical Name: d:\worksas

    NOTE: Library workwpd assigned as follows:
          Engine:        WPD
          Physical Name: d:\workwpd


    LOG:  9:42:52
    NOTE: 1 record was written to file PRINT

    NOTE: The data step took :
          real time : 0.038
          cpu time  : 0.000


    NOTE: AUTOEXEC processing completed

    1         options validvarname=v7;
    2         options set=PYTHONHOME "D:\py314";
    3         proc python;
    4         submit;
    5         import pyreadstat as ps
    6         import pandas as pd
    7         from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
    8
    9         # One-line initialization
    10        analyzer = SentimentIntensityAnalyzer()
    11
    12        product,meta = ps.read_sas7bdat('d:/wpswrkx/product.sas7bdat')
    13        responses = product['sentences']
    14
    15        # Score them
    16        rows = []
    17        for response in responses:
    18            scores = analyzer.polarity_scores(response)
    19
    20            # Classify
    21            if scores['compound'] >= 0.05:
    22                sentiment = "POSITIVE"
    23            elif scores['compound'] <= -0.05:
    24                sentiment = "NEGATIVE"
    25            else:
    26                sentiment = "NEUTRAL"
    27
    28            rows.append({'response': response, 'sentiment': sentiment})
    29
    30        results = pd.DataFrame(rows)
    31        print(results)
    32        endsubmit;

    NOTE: Submitting statements to Python:


    33        import python=results data=workx.results;
    NOTE: Creating data set 'WORKX.results' from Python data frame 'results'
    NOTE: Data set "WORKX.results" has 20 observation(s) and 2 variable(s)

    34        run;
    NOTE: Procedure python step took :
          real time : 0.876
          cpu time  : 0.015


    35
    36        proc sort data=workx.results;
    37        by sentiment;
    38        run;
    NOTE: Automatically set SORTSIZE to 10240MiB
    NOTE: 20 observations were read from "WORKX.results"
    NOTE: Data set "WORKX.results" has 20 observation(s) and 2 variable(s)
    NOTE: Procedure sort step took :
          real time : 0.039
          cpu time  : 0.015


    39
    40        proc print data=workx.results;
    41        by sentiment;
    42        run;
    NOTE: 20 observations were read from "WORKX.results"
    NOTE: Procedure print step took :
          real time : 0.023
          cpu time  : 0.000


    43
    44        proc freq data=workx.results;
    45        tables sentiment;
    46        run;quit;
    NOTE: 20 observations were read from "WORKX.results"
    NOTE: Procedure freq step took :
          real time : 0.008
          cpu time  : 0.000


    ERROR: Error printed on page 1

    NOTE: Submitted statements took :
          real time : 1.149
          cpu time  : 0.078

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
