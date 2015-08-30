# ExtraTreesRegressor-in-Python---First-Go

<i>Data source here: http://archive.ics.uci.edu/ml/datasets/Online+News+Popularity</i><br>
<i>Target column (what want to predict) is last column "shares"</i><br>
<i>Removed url column and saved as new file</i><br>
<i>This code is a work in progress (I'm new to Python)</i><br>

import csv<br>
import numpy as np<br>
from sklearn.ensemble import ExtraTreesRegressor<br>
from sklearn.cross_validation import train_test_split<br>
<br>
def import_dataset(filename, all_data):<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;with open(filename,'rb') as csvfile:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;csv_has_header = csv.Sniffer().has_header(csvfile.read(10*1024))<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;csvfile.seek(0)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if csv_has_header:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;csvfile.next()<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;csvlines = csv.reader(csvfile, delimiter=',', quoting=csv.QUOTE_NONNUMERIC)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dataset=list(csvlines)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for x in range (len(dataset)):<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;all_data.append(dataset[x])<br>

def export_dataset(filename, prediction_result):<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;with open(filename, 'wb') as output:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mywriter = csv.writer(output, lineterminator='\n')<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;for val in prediction_result:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mywriter.writerow(val)<br>
<br>
def main():<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;all_data = []<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;split = 0.30<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;export_file_predictions = '/home/becky/Documents/OnlineNewsPopularity_predictions.csv'<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;import_dataset('/home/becky/Documents/OnlineNewsPopularity_fixed_nourl.csv', all_data)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dataset = np.array(all_data)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;training_data, test_data = train_test_split(dataset, test_size = split)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;regression_task = ExtraTreesRegressor(n_estimators=15, max_features="auto", max_depth=None, min_samples_split=2)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;regression_task.fit(training_data, training_data[:,59], sample_weight=None)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;predicted_values = regression_task.predict(test_data)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actual_and_test = np.column_stack([test_data, predicted_values])<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;export_dataset(export_file_predictions, actual_and_test)<br>
<br>	
main()
