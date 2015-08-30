# ExtraTreesRegressor-in-Python---First-Go

Data source here: http://archive.ics.uci.edu/ml/datasets/Online+News+Popularity<br>
Target column (what want to predict) is last column "shares"<br>
Removed url column and saved as new file<br>
This code is a work in progress (I'm new to Python)<br>

import csv<br>
import numpy as np<br>
from sklearn.ensemble import ExtraTreesRegressor<br>
from sklearn.cross_validation import train_test_split<br>
<br>
def import_dataset(filename, all_data):<br>
&nbsp;&nbsp;	with open(filename,'rb') as csvfile:<br>
		csv_has_header = csv.Sniffer().has_header(csvfile.read(10*1024))<br>
		csvfile.seek(0)<br>
		if csv_has_header:<br>
			csvfile.next()<br>
		csvlines = csv.reader(csvfile, delimiter=',', quoting=csv.QUOTE_NONNUMERIC)<br>
		dataset=list(csvlines)<br>
		for x in range (len(dataset)):<br>
			all_data.append(dataset[x])<br>

def export_dataset(filename, prediction_result):<br>
	with open(filename, 'wb') as output:<br>
		mywriter = csv.writer(output, lineterminator='\n')<br>
		for val in prediction_result:<br>
			mywriter.writerow(val)<br>
<br>
def main():<br>
	all_data = []<br>
	split = 0.30<br>
	export_file_predictions = '/home/becky/Documents/OnlineNewsPopularity_predictions.csv'<br>
	import_dataset('/home/becky/Documents/OnlineNewsPopularity_fixed_nourl.csv', all_data)<br>
	dataset = np.array(all_data)<br>
	training_data, test_data = train_test_split(dataset, test_size = split)<br>
	regression_task = ExtraTreesRegressor(n_estimators=15, max_features="auto", max_depth=None, min_samples_split=2)<br>
	regression_task.fit(training_data, training_data[:,59], sample_weight=None)<br>
	predicted_values = regression_task.predict(test_data)<br>
	actual_and_test = np.column_stack([test_data, predicted_values])<br>
	export_dataset(export_file_predictions, actual_and_test)<br>
<br>	
main()
