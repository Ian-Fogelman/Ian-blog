---
layout: post
title:  SQL Server and Python Machine Learning
date:   2020-05-19
description: 2020-05-19-SQL Server and Python Machine Learning
img: py.png # Add image post (optional)
tags: [Machine Learning, SQL Server, Python, Pandas, SciKitLearn]

datatable: true
author: Ian Fogelman # Add name author (optional)
---

<meta property="og:title" content="Dynamic Python">
<meta property="og:description" content="A blog by Ian Fogelman.">
<meta property="og:image" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">
<meta property="og:url" content="https://repository-images.githubusercontent.com/190807493/a3610e80-bed1-11e9-87ac-2a4f0aa3b2ee">

<br>
<br>
Today we will be utalizing a new feature in 2017+ SQL Server enviornments, machine learning via Python native script execution. In this tutorial we will recap how to load a Pickle file into a SQL server database stored as binary data. Retreive that file and load it as a machine learning model and apply the model predictions to our SQL Server data. So the steps to achieve our predicts are as follows:

<br>
<br>

I. Create a Table to hold our binary Models<br>
II. Create a table to hold our machine learning data<br>
III. Create a procedure to load our pickled model into the ML table<br>
IV. Fire stored procedure to load the pickle file<br>
V. Create another stored procedure to take in our SQL server data and model and return a result set<br>
VI. Compare the result set from the model to our data

<br>
<br>


<a href="https://github.com/Ian-Fogelman/ian-blog/raw/master/assets/files/ML%20Init.zip" target="_blank">ML Init.zip</a>
Files : <br>
Iris.csv<br>
IrisClassifier.pkl<br>
ML Init.sql

<br>
<br>

I. First step is we create a database and a table to store our machine learning logic
{% highlight SQL %}

CREATE DATABASE [ML]
GO

USE [ML]
--TABLE TO HOLD BINARY MODEL
DROP TABLE IF EXISTS ML_Models;
GO
CREATE TABLE ML_Models (
                model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY,
                model VARBINARY(MAX) NOT NULL
);
GO

{% endhighlight %}

<br>
<br>

{% highlight SQL %}
{% endhighlight %}
II. Next we need to load the Iris data from our csv file into a table in our new database, lets create the table in TSQL and utalize a bulk insert command.

<br>
<br>

{% highlight SQL %}

CREATE TABLE IRIS
(
Id INT,
SepalLengthCm FLOAT,
SepalWidthCm FLOAT,
PetalLengthCm FLOAT,
PetalWidthCm FLOAT,
Species VARCHAR(MAX)

)

BULK INSERT  IRIS
FROM 'C:\Test\Iris.csv'
WITH (FIELDTERMINATOR = ',', ROWTERMINATOR = '\n',FIRSTROW=2);
GO

--CHECK TO MAKE SURE DATA MADE IT IN 
SELECT * FROM IRIS
{% endhighlight %}

<br>
<br>

III. Next we must load our persisted model logic from a pickle file. Pickle is a python library that allows you to persist anything to a file on your work station. In this case I trained the model beforehand in a jupyter notebook enviornment and created the pickle file already.

<br>
<br>


{% highlight SQL %}
--PROC TO LOAD PKL AS BINARY
CREATE PROCEDURE SPX_LOAD_MODEL_FROM_PICKLE (@PicklePath VARCHAR(MAX),@trained_model varbinary(max) OUTPUT)
AS
BEGIN

DECLARE @PYTHON NVARCHAR(MAX)
SET @PYTHON = '
from sklearn import linear_model
import pickle
with open(str('''+ @PicklePath +'''), ''rb'') as pickle_file:
    model = pickle.load(pickle_file)
trained_model = pickle.dumps(model)'

    EXECUTE sp_execute_external_script
      @language = N'Python'
    , @script = @PYTHON
    , @input_data_1 = N''
    , @input_data_1_name = N''
    , @params = N'@trained_model varbinary(max) OUTPUT'
    , @trained_model = @trained_model OUTPUT;
END

--LOAD THE TABLE WITH A PICKLE FILE

DECLARE @model VARBINARY(MAX);
EXEC SPX_LOAD_MODEL_FROM_PICKLE 'C:\Test\IrisClassifier.pkl',@model OUTPUT;
INSERT INTO ML_Models (model_name, model) VALUES('Iris Model', @model);

SELECT * FROM ML_Models

{% endhighlight %}

<br>
<br>

V. Next we need a stored procedure that will pick up the binary stored in our table and apply the model prediction to our data.
To do this we use a combination of the input_data_1 input_data_name and params keyword arguments for our stored procedure.


<br>
<br>


{% highlight SQL %}
DROP PROCEDURE IF EXISTS SPX_PY_PREDICT_IRIS;
GO
CREATE PROCEDURE SPX_PY_PREDICT_IRIS (@model varchar(100))
AS
BEGIN
	DECLARE @py_model varbinary(max) = (select model from ML_Models where model_name = @model);

	EXEC sp_execute_external_script
					@language = N'Python'
				  , @script = N'
def convertName(i):
	retList = ["Iris-setosa","Iris-versicolor","Iris-virginica"]
	return retList[i]

import pickle
import pandas as pd
model = pickle.loads(py_model)
df = input_data
mylist = []
for index, row in df.iterrows():
	x = model.predict([[row["SepalLengthCm"], row["SepalWidthCm"],row["PetalLengthCm"],row["PetalWidthCm"]]])
	mylist.append(x)
OutputDataSet = pd.DataFrame(mylist)
'
					, @input_data_1 = N'Select "SepalLengthCm", "SepalWidthCm","PetalLengthCm","PetalWidthCm" from IRIS'
					, @input_data_1_name = N'input_data'
					, @params = N'@py_model varbinary(max)'
					, @py_model = @py_model

END;
GO

{% endhighlight %}

<br>
<br>

VI. Lastly we create a temp table to hold the results from our newly predicting stored proc and join our predicted data back to the original data set and see how it performed.

<br>
<br>

{% highlight SQL %}
IF OBJECT_ID('TEMPDB..#PRED_RESULTS') IS NOT NULL DROP TABLE #PRED_RESULTS
CREATE TABLE #PRED_RESULTS
(
ID INT IDENTITY(1,1),
PREDICTEDVAL INT
)

INSERT INTO #PRED_RESULTS
EXEC SPX_PY_PREDICT_IRIS 'Iris Model'
{% endhighlight %}

<br>
<br>

Now join the results back to the original to assess model performance!

<br>
<br>

{% highlight SQL %}

	--GRANULAR RESUTS
	SELECT PR.ID,
	CASE WHEN PR.PREDICTEDVAL = 0 THEN 'Iris-setosa' 
		 WHEN PR.PREDICTEDVAL = 1 THEN 'Iris-versicolor'
		 WHEN PR.PREDICTEDVAL = 2 THEN 'Iris-virginica'
		 END AS PredictedSpecies,
	   SPECIES as Species,
	   IR.SEPALLENGTHCM,
	   IR.SEPALWIDTHCM,
	   PETALLENGTHCM,
	   PETALWIDTHCM

	   FROM #PRED_RESULTS AS PR
	JOIN IRIS AS IR
		ON PR.ID = IR.ID
	
	--AGGREGATED RESULTS
	SELECT 
	SUM(CASE WHEN PredictedSpecies = Species THEN 1 ELSE 0 END) AS CORRECT,
	SUM(CASE WHEN PredictedSpecies != Species THEN 1 ELSE 0 END) AS INCORRECT,
	CAST(SUM(CASE WHEN PredictedSpecies = Species THEN 1 ELSE 0 END) AS FLOAT)  / COUNT(*) AS ACCURACY
	FROM(
	SELECT PR.ID,
	CASE WHEN PR.PREDICTEDVAL = 0 THEN 'Iris-setosa' 
		 WHEN PR.PREDICTEDVAL = 1 THEN 'Iris-versicolor'
		 WHEN PR.PREDICTEDVAL = 2 THEN 'Iris-virginica'
		 END AS PredictedSpecies,
	   SPECIES as Species,
	   IR.SEPALLENGTHCM,
	   IR.SEPALWIDTHCM,
	   PETALLENGTHCM,
	   PETALWIDTHCM
	FROM #PRED_RESULTS AS PR
	JOIN IRIS AS IR
		ON PR.ID = IR.ID) AS X
		
{% endhighlight %}

<br>
<br>

There we have it our first ML model trained in Python, stored in SQL server and executed via a stored procedure. So further interesting applications for this approach would be to build a SSRS report to capture the models predictions to stake holders. Also to keep advancing the models and "check them into" the sql database. We can easily keep a running talley of model performance because all of the models are stored in that table Ex Iris Model V1, Iris Model Winter, Iris Model 2020. 
