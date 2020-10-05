

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

V. Now we create a temp table to hold the results from our newly predicting stored proc and join our predicted data back to the original data set and see how it performed.

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

VI. Now join the results back to the original to assess model performance!

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
![Model Results](/assets/img/Iris Results.PNG)

<br>
<br>

![Model Results](/assets/img/Python ML Results.PNG)

<br>
<br>

There we have it our first ML model trained in Python, stored in SQL server and executed via a stored procedure. So further interesting applications for this approach would be to build a SSRS report to capture the models predictions to stake holders. Also to keep advancing the models and "check them into" the sql database. We can easily keep a running talley of model performance because all of the models are stored in that table Ex Iris Model V1, Iris Model Winter, Iris Model 2020. 
